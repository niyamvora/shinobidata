# Changelog

All notable changes to the **public-facing surface** of the ShinobiData MCP server (tool catalog, OAuth contract, response shapes). The hosted service deploys continuously; this log captures what's user-observable.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions follow [SemVer](https://semver.org/) — major bumps mean breaking changes to a tool's contract; minor for new tools or new optional fields; patch for non-breaking response improvements.

## [1.0.0] — 2026-05-10

First version eligible for public directory listings ([MCP Registry](https://registry.modelcontextprotocol.io/?search=shinobidata), Anthropic + OpenAI directories).

### Added

- **32 OAuth-protected MCP tools** across 5 capability groups:
  - **Portfolio CRUD** (6): `create_portfolio`, `add_holdings`, `parse_portfolio_text`, `add_transaction`, `update_holding`, `delete_holding`.
  - **Portfolio analytics** (6): `get_portfolio_overview`, `get_portfolio_performance`, `analyze_portfolio_fundamentals`, `get_growth_vs_stagnant`, `get_dividend_summary`, `get_risk_summary`.
  - **Single-company research** (7): `search_companies`, `get_company_snapshot`, `get_financials`, `get_growth_stats`, `get_peers`, `get_analyst_view`, `get_price_history`.
  - **360° market & sector** (10): `compare_companies`, `get_sector_overview`, `get_industry_overview`, `get_market_overview`, `get_top_movers`, `get_earnings_calendar`, `get_dividend_calendar`, `get_valuation_history`, `get_quality_scores`, `get_news_sentiment`.
  - **Discovery + ChatGPT compat** (3): `screen`, `search`, `fetch`.
- **OAuth 2.1 with PKCE S256** end-to-end. Refresh-token rotation per RFC 6749 §10.4. RFC 7591 dynamic client registration.
- **Three OAuth scopes**: `portfolio:read`, `portfolio:write`, `market:read`.
- **MCP Registry listing** as `com.shinobidata/research` (DNS-verified namespace).
- **OAuth metadata branding** — `service_documentation`, `op_policy_uri`, `op_tos_uri`, `resource_*` siblings.
- **Server-level briefing** (`MCP_SERVER_INSTRUCTIONS`) delivered to AI clients on connect — orients the model to the 5 tool groups, unit conventions, destructive-tool contract, pagination rules.
- **Public install endpoint**: `https://mcp.shinobidata.com/api/mcp/mcp`.
- **Privacy + Terms pages** with MCP-specific sections (data collected, retention windows, AI client data flow).

### Documented limits

- US equities only (v1).
- USD-only (v1).
- 2 portfolios per user (v1 cap).
- 90-day audit-log retention.
- `get_price_history` capped at 730 days per call.
- `compare_companies` capped at 5 symbols.
- `screen` returns 50 rows / page with `nextCursor`.

### Known v2 deferrals

- `analyze_portfolio_fundamentals.marginTrend` — needs additional join to `FinancialRatiosAnnual`.
- `get_dividend_summary` strict held-on-ex-date computation — currently uses Robinhood/Fidelity-style approximation.
- Multi-backend `web_search` fanning out to Claude internal search / Google Gemini grounded search / Perplexity online.
