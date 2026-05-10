# Installation

3 paths: Claude Desktop, ChatGPT (Plus / Teams / Enterprise), and any other MCP-compatible client.

The connector URL is the same in all cases:

```
https://mcp.shinobidata.com/api/mcp/mcp
```

---

## Claude Desktop

1. Open Claude Desktop's config file:
   - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`

2. Add the `shinobidata` entry under `mcpServers`. If the file is empty, paste this whole thing:

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

   If you already have other MCP servers configured, just add the `shinobidata` key inside `mcpServers`.

3. Save the file.

4. **Cmd-Q** to fully quit Claude Desktop (not just close the window — quit the process).

5. Reopen Claude Desktop.

6. The first tool call triggers the OAuth dance:
   - Browser opens to `https://mcp.shinobidata.com/oauth/authorize`
   - Sign in with Google
   - Click **Allow** on the consent screen
   - Browser closes; you're back in Claude

7. Try a quick prompt: *"List my MCP tools."* You should see all 32 tools across the 5 groups.

### Resetting the OAuth flow

If something goes wrong during auth and you need to start over:

```bash
rm -rf ~/.mcp-auth/
```

Then Cmd-Q + reopen Claude. Next tool call re-triggers the dance.

---

## claude.ai (web)

Currently MCP support in `claude.ai` (the web app) is rolling out behind the connectors UI. Once it's available in your account:

1. Click the connector icon in the chat composer.
2. Choose **Add custom connector** (or similar — the exact label changes).
3. Paste `https://mcp.shinobidata.com/api/mcp/mcp`.
4. Authorize via the OAuth dance.

If you don't see the connector option yet, this feature is still being gated. Use Claude Desktop (above) in the meantime — same backend, same tools.

---

## ChatGPT (Plus / Teams / Enterprise)

1. **Settings** → **Connectors** → **Add Custom**.
2. Paste the URL: `https://mcp.shinobidata.com/api/mcp/mcp`.
3. ChatGPT will discover the OAuth flow via `/.well-known/oauth-authorization-server` automatically.
4. Authorize when prompted.

For **Deep Research mode**, the connector's `search` and `fetch` tools are picked up automatically once it's installed. Switch to Deep Research and ask a research-style prompt:

> *Pick a few US stocks with strong revenue growth and reasonable valuation. Use ShinobiData to research them.*

Expect 3–5 tool calls (`search` → `get_growth_stats` → `get_valuation_history` → `compare_companies` → `fetch`) returning a grounded, citation-rich answer.

---

## Any other MCP-compatible client

If your client supports the **Streamable HTTP transport** + **OAuth 2.1 with PKCE** (most do), point it at the connector URL directly:

```
https://mcp.shinobidata.com/api/mcp/mcp
```

The well-known metadata at `https://mcp.shinobidata.com/.well-known/oauth-authorization-server` covers everything the client needs to set up auth (endpoints, scopes, supported flows). The protocol-level handshake is standard MCP.

OAuth scopes you'll be asked to grant:

- `portfolio:read` — view your portfolios + holdings + transactions.
- `portfolio:write` — create / edit / delete portfolios on your behalf.
- `market:read` — read public market data (companies, fundamentals, returns) — no personal data involved.

You can choose to grant only the scopes you need at the consent screen.

---

## Common install issues

See [`troubleshooting.md`](troubleshooting.md) for OAuth flow failures, "no tools showing", rate limit errors, and similar.

If your specific issue isn't there, file a [bug report](https://github.com/niyamvora/shinobidata/issues/new?template=bug.yml) or post in [Discussions → Q&A](https://github.com/niyamvora/shinobidata/discussions/categories/q-a).
