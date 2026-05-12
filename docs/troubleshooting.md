# Troubleshooting

The usual gotchas, roughly in order of how often they come up.

If yours isn't here, search [Discussions → Q&A](https://github.com/dark-horse-stocks/shinobidata/discussions/categories/q-a) or file a [bug](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=bug.yml).

## OAuth

### The OAuth dance loops back to the consent screen

You're stuck between an old cached token and a new flow. Reset:

```bash
rm -rf ~/.mcp-auth/
```

Cmd-Q Claude Desktop (full quit, not just close the window). Reopen. Try again.

### I clicked Allow and the browser tab just hangs

Two usual causes:

1. A pop-up blocker ate the redirect. Disable for `mcp.shinobidata.com` and retry.
2. The `redirect_uri` your client registered doesn't match what we expect. Open Console.app, search for "Claude Helper", look for an OAuth error. Fix is usually `rm -rf ~/.mcp-auth/` plus reinstall — Claude registers a fresh client_id on next launch.

### The consent screen says "Invalid request"

Usually an expired authorization code (10-minute TTL). Happens if you walk away mid-flow. Cancel, reset the cache, restart.

If it keeps happening, the OAuth client your AI client registered may have been deleted on our side. `rm -rf ~/.mcp-auth/` forces a fresh registration.

### Every tool call returns 401 with WWW-Authenticate: Bearer error=invalid_token

Access token expired (1-hour TTL). Refresh tokens rotate automatically — the AI client should handle it. If it's happening over and over, the refresh token is stale. Wipe `~/.mcp-auth/` and re-authorize.

## Tools

### I see 0 tools after install

Most common cause: you didn't fully quit and reopen Claude Desktop after editing the config. MCPs load at app start — config edits need a full quit (Cmd-Q on macOS).

If you've quit-and-reopened and still see nothing:

1. Validate the config JSON at <https://jsonlint.com>.
2. Check `command` is `npx` and the args include `mcp-remote` plus the URL.
3. In Claude Desktop, hover over the connector indicator if there's an error count.

### Tools list, but every call hits a rate limit

You're hitting the per-token rate limit. Default is generous; if you're consistently hitting it, you may have a tool loop (the AI calling the same tool over and over). File a [bug](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=bug.yml) and we'll dig in.

Limits also reset after a short window. Wait a minute and retry.

### Some tools work, others return 401

Scope mismatch. If you denied `portfolio:write` at the consent screen, every CRUD tool will 401. Re-authorize and grant the missing scope.

### Numbers don't match what I expect

Check the response's `units` block. Two conventions live side by side:

- Growth is in percentages (10 means 10%) where the `units.growth` field says so.
- Returns are in decimals (0.10 means 10%).
- Currency is USD. Market cap fields are absolute USD, not millions.

If you're sure it's a real bug (cost basis off, P&L sign flipped), file with the prompt + response.

### ChatGPT Deep Research doesn't call my ShinobiData tools

Deep Research wants two specifically-named tools, `search` and `fetch`. Both are present in v1.0.0+. If they're missing, your client may be on a stale schema cache:

1. Disconnect and reconnect the connector.
2. Or revoke the OAuth token from your account settings and reauthorize.

## Specific errors

### `503 Screener cache is not ready yet`

Internal Redis cache is warming. Retry in 5-10 seconds. If it's still 503 after a minute, file a bug.

### `400 Range too wide (X days). Hard cap is 730 days`

`get_price_history` caps at 730 days per call. Split the range or narrow it.

### `Refusing to delete without confirm: true`

You called `delete_holding` without `confirm: true`. Deliberate guardrail — the AI has to explicitly opt in to a destructive action. Most clients ask the user before passing it.

### `Holding X not found in portfolio Y`

Symbol may be spelled differently from how we resolved it. Use `search_companies` to confirm the canonical form, or call `get_portfolio_overview` to see exactly what's stored.

### `No company found with symbol "X"`

Symbol isn't in the `companies` table. Usually means: it's a non-US listing (we're US-only in v1), it's delisted or recently IPO'd and we haven't indexed it, or it's a typo. Use `search_companies` with a partial name or symbol prefix to look it up.

## Performance

### Tool calls feel slow

p50 today is ~700ms; p99 ~1.2s. If you're seeing >3s consistently on a single tool:

- `get_market_overview` and `get_top_movers` are the slowest (full-universe LATERAL joins). Caching is on the roadmap.
- `add_holdings` with >50 rows runs 1-2s for the bulk upsert.
- `get_price_history` with 730 days returns ~500 bars + ~95KB JSON; transport overhead dominates.

If everything feels slow, your network is the bottleneck. `mcp-remote` proxies through your local machine.

### How do I see actual tool latency

No public-facing dashboard. We track per-tool latency internally for security and abuse purposes. If you're investigating a perf issue, file a bug with the affected tool + a timestamp; we can pull the audit-log entries.

## Last resort

```bash
rm -rf ~/.mcp-auth/
# Cmd-Q Claude Desktop
# Reopen
# Re-authorize
```

90% of intermittent issues clear after this. If yours doesn't, [file a bug](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=bug.yml) with what you tried, your client + version, and any logs you can grab.
