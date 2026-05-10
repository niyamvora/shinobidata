# Troubleshooting

Common install + runtime issues, in order of how often they come up.

If your specific issue isn't here, search [Discussions → Q&A](https://github.com/niyamvora/shinobidata/discussions/categories/q-a) or file a [bug report](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml).

---

## OAuth flow

### "OAuth dance loops back to the consent screen forever"

You're caught between an old cached token and the new flow. Reset the cache:

```bash
rm -rf ~/.mcp-auth/
```

Then **Cmd-Q** Claude Desktop (don't just close the window — quit the process), reopen, and try again.

### "I clicked Allow but the browser tab just hangs"

Two possibilities:

1. **Pop-up blocker** intercepted the redirect back to your AI client. Disable for `mcp.shinobidata.com` and retry.
2. **The `redirect_uri` registered in your client doesn't match what the OAuth server expects.** Open Console.app on macOS, search for "Claude Helper", and look for an OAuth error message. The fix is usually `rm -rf ~/.mcp-auth/` + reinstall (Claude Desktop re-registers a fresh client_id on next launch).

### "Browser opens but the consent screen says 'Invalid request'"

Usually means an expired authorization code (10-minute TTL) — happens if you walk away mid-flow. Cancel, reset the cache, start fresh.

If it persists, the OAuth client your AI client registered may have been deleted on our side. `rm -rf ~/.mcp-auth/` forces a fresh registration.

### "I see WWW-Authenticate: Bearer error=invalid_token on every tool call"

Your access token expired (1-hour TTL). Refresh tokens rotate automatically; the AI client should handle the refresh transparently. If you're seeing this repeatedly, the refresh token may be stale — wipe `~/.mcp-auth/` and re-authorize.

---

## Tool calls

### "I see 0 tools after install"

Most common cause: **you didn't fully quit + reopen Claude Desktop after editing the config.** MCP servers are loaded at app start; config changes need a full quit (Cmd-Q on macOS, not just closing the window).

If you've quit-and-reopened and still see 0 tools:

1. Check the config JSON is valid — paste it into <https://jsonlint.com> to verify.
2. Check the `command` is `npx` and the args include `mcp-remote` + the URL.
3. In Claude Desktop, look for a small connector indicator (usually shows error count if any). Hover for details.

### "Tools list but every call fails with rate limit"

You hit the per-token rate limit. Default is generous; if you're consistently hitting it, you may have a tool loop (the AI calling the same tool repeatedly). Open a [bug](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml) — we'd want to investigate the loop pattern.

Rate limits also reset after a short window. Wait a minute and retry.

### "Some tools work, others return 401"

Likely a scope mismatch. The consent screen lets you deny individual scopes; if you denied `portfolio:write`, all CRUD tools (create/add/edit/delete) will 401. Re-authorize and grant the missing scope.

### "Numbers don't match what I expect"

Check the response's `units` block. ShinobiData mixes two conventions:

- **Growth** is in percentages (10 = 10%) where the `units.growth` field says so.
- **Returns** are in decimals (0.10 = 10%).
- **Currency** is USD; market cap fields are absolute USD (not millions).

If you're sure it's a real bug (e.g., cost basis is wrong, P&L sign is flipped), file a bug with the prompt + response.

### "ChatGPT Deep Research doesn't call my ShinobiData tools"

ChatGPT's deep-research mode requires **two specifically-named tools**: `search` and `fetch`. Both are present in v1.0.0+. If they're missing, your client may be on an outdated cached schema:

1. Disconnect + reconnect the connector.
2. Or rotate the OAuth token: revoke from your account settings, then reauthorize.

---

## Specific errors

### `503 Screener cache is not ready yet`

Internal Redis cache is warming. Retry in 5–10 seconds. If it persists past a minute, the cache may be down — file a bug.

### `400 Range too wide (X days). Hard cap is 730 days`

`get_price_history` has a 730-day-per-call ceiling. Split into two calls or narrow the window.

### `Refusing to delete without confirm: true`

You called `delete_holding` without the `confirm: true` flag. This is a deliberate guardrail against hallucinated tool calls. Add `confirm: true` to actually delete. Most AI clients ask the user for confirmation before they pass it.

### `Holding X not found in portfolio Y`

The symbol may be spelled differently than how we resolved it. Use `search_companies` to confirm the canonical symbol, or call `get_portfolio_overview` to see the exact symbol strings stored.

### `No company found with symbol "X"`

The symbol isn't in our `companies` table — typically because:

- It's a non-US listing (we're US-only in v1).
- It's a delisted or recently-IPO'd ticker we haven't indexed yet.
- It's a typo.

Use `search_companies` with a partial name or symbol prefix to look it up.

---

## Performance

### "Tool calls feel slow (>3s)"

p50 today is ~700ms; p99 ~1.2s. If you're consistently seeing >3s on a single tool:

- **`get_market_overview` and `get_top_movers`** are the slowest tools (full-universe LATERAL joins). Caching is planned to bring these under 200ms.
- **`add_holdings` with >50 rows** can take 1–2s because of bulk upsert overhead.
- **`get_price_history` with 730 days** returns ~500 bars + ~95KB JSON; transport overhead dominates.

If everything feels slow, your network may be the bottleneck — `mcp-remote` proxies through your local machine.

### "How do I see how long tools are actually taking?"

Currently no public-facing latency dashboard. Internal audit logs track per-tool latency for security/abuse purposes. If you're investigating a perf issue, file a bug with the affected tool + your timestamp; we can pull the audit-log entries.

---

## Last resort

```bash
rm -rf ~/.mcp-auth/
# Cmd-Q Claude Desktop (full quit)
# Reopen Claude Desktop
# Re-authorize via the OAuth dance
```

90% of intermittent issues clear after this reset.

If they don't, [file a bug](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml) with the steps you took, the AI client + version, and any logs you can grab. We respond fast on real issues.
