# Installation

Three paths: Claude Desktop, ChatGPT, and anything else that speaks MCP. The connector URL is the same in all cases:

```
https://mcp.shinobidata.com/api/mcp/mcp
```

## Claude Desktop

1. Open the config file.
   - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`

2. Add the `shinobidata` block under `mcpServers`. If the file is empty, paste the whole thing:

   ```json
   {
     "mcpServers": {
       "shinobidata": {
         "command": "npx",
         "args": ["-y", "mcp-remote", "https://mcp.shinobidata.com/api/mcp/mcp"]
       }
     }
   }
   ```

   If you already have other MCPs, just add the `shinobidata` key alongside them.

3. Save.

4. Quit Claude Desktop fully. On macOS that's Cmd-Q, not closing the window. MCPs are loaded at app start.

5. Reopen Claude. The first tool call kicks off the OAuth flow:
   - Browser opens to the consent screen
   - Sign in with Google
   - Click Allow
   - Browser closes, you're back in Claude

6. Try `list my MCP tools`. You should see all 32 across the five groups.

### Resetting OAuth

If something goes weird mid-flow:

```bash
rm -rf ~/.mcp-auth/
```

Cmd-Q Claude, reopen, the next tool call starts fresh.

## claude.ai (web)

The web app is rolling out connector support behind a feature flag. When you have access:

1. Click the connector icon in the chat composer.
2. "Add custom connector" (the label varies).
3. Paste `https://mcp.shinobidata.com/api/mcp/mcp`.
4. Authorize.

If the option isn't there yet, use Claude Desktop. Same backend, same tools, same data.

## ChatGPT (Plus / Teams / Enterprise)

1. Settings → Connectors → Add Custom.
2. Paste the URL: `https://mcp.shinobidata.com/api/mcp/mcp`.
3. ChatGPT auto-discovers OAuth from the well-known endpoint.
4. Authorize when prompted.

For Deep Research mode, the `search` and `fetch` tools get picked up automatically once it's connected. Try:

> Pick a few US stocks with strong recent revenue growth and reasonable valuation. Use ShinobiData to research them.

You'll see ChatGPT call `search` → `get_growth_stats` → `get_valuation_history` → `compare_companies` → `fetch` and stitch a real answer with citations.

## Anything else

If your client supports Streamable HTTP and OAuth 2.1 with PKCE (most do), point it at the connector URL directly:

```
https://mcp.shinobidata.com/api/mcp/mcp
```

The well-known endpoints at `https://mcp.shinobidata.com/.well-known/oauth-authorization-server` and `.../oauth-protected-resource` cover everything else (auth + token endpoints, scopes, supported flows).

Scopes you'll see at the consent screen:

- `portfolio:read` — view your portfolios, holdings, transactions.
- `portfolio:write` — create / edit / delete portfolios.
- `market:read` — read public market data. No personal data.

Grant only what you want.

## When something doesn't work

See [troubleshooting.md](troubleshooting.md). If your specific issue isn't there, file a [bug](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=bug.yml) or post in [Discussions → Q&A](https://github.com/dark-horse-stocks/shinobidata/discussions/categories/q-a).
