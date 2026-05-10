# shinobidata — Claude Code plugin

Portfolio analytics and US-equity market research, in Claude Code. Backed by the [ShinobiData](https://shinobidata.com) MCP server (`https://mcp.shinobidata.com/api/mcp/mcp`).

## Install

From within Claude Code:

```shell
/plugin marketplace add niyamvora/shinobidata
/plugin install shinobidata@shinobidata
```

The first tool call opens an OAuth consent screen in your browser. Sign in with Google, click Allow, and you're done. 32 tools become available across five groups.

## What's in the box

- **Portfolio CRUD (6)** — `create_portfolio`, `add_holdings`, `parse_portfolio_text`, `add_transaction`, `update_holding`, `delete_holding`.
- **Portfolio analytics (6)** — `get_portfolio_overview`, `get_portfolio_performance` (vs SPY/QQQ), `analyze_portfolio_fundamentals`, `get_growth_vs_stagnant`, `get_dividend_summary`, `get_risk_summary`.
- **Single-company research (7)** — `search_companies`, `get_company_snapshot`, `get_financials`, `get_growth_stats`, `get_peers`, `get_analyst_view`, `get_price_history`.
- **Market & sector (10)** — `compare_companies`, `get_sector_overview`, `get_industry_overview`, `get_market_overview`, `get_top_movers`, `get_earnings_calendar`, `get_dividend_calendar`, `get_valuation_history`, `get_quality_scores`, `get_news_sentiment`.
- **Discovery (3)** — `screen` (cursor-paginated DSL), `search`, `fetch`.

Full tool reference: [`docs/tools.md`](../../docs/tools.md).

## What you can ask

- "How concentrated am I in tech?"
- "Did my portfolio beat the S&P this year?"
- "Compare AAPL, MSFT, NVDA, GOOGL, META on valuation and growth."
- "Is AAPL cheap vs its 10-year history?"
- "Which earnings reports are coming up next 14 days, mid-cap and above?"

The model orchestrates the tool calls; you ask the question.

## Auth, scopes, privacy

OAuth 2.1 with PKCE (S256). Three scopes: `portfolio:read`, `portfolio:write`, `market:read`. Tokens rotate. Revocable from your account. See the consent screen for the plain-English breakdown of what each scope grants.

- Privacy policy: <https://shinobidata.com/en/legal/privacy-policy>
- Terms: <https://shinobidata.com/en/legal/terms>
- Contact: <support@shinobidata.com>

## Troubleshooting

- **Plugin not installing**: refresh with `/plugin marketplace update shinobidata`.
- **OAuth loop**: clear cached creds with `rm -rf ~/.claude/plugins/cache/shinobidata` and reinstall.
- **No tools after install**: run `/reload-plugins` to activate.

For more, see the top-level [installation guide](../../docs/installation.md) and [troubleshooting](../../docs/troubleshooting.md).

## License

MIT. Brand assets are © 2026 ShinobiData — see [`assets/README.md`](../../assets/README.md).
