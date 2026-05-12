# Tools

32 tools across 5 groups. Every tool is OAuth-protected; the required scope is shown next to each group.

A few conventions worth knowing before you read the catalog:

- Growth values are percentages (10 means 10%) wherever a tool's response includes a `units.growth` note.
- Returns are decimals (0.10 means 10%).
- Currency is USD. v1 limitation.
- Every response carries:
  - `dataAsOf` — wallclock time the response was assembled
  - `dataSources` — array of underlying tables / providers
  - `snapshotAsOf` — date(s) of the underlying snapshot data, separate from `dataAsOf`

## Portfolio CRUD (6)

Scope: `portfolio:write` for mutations, `portfolio:read` for reads.

| Tool | Args | What it does |
|---|---|---|
| `create_portfolio` | `{ name, baseCurrency? }` | New empty portfolio. v1 cap is 2 portfolios per user. |
| `add_holdings` | `{ portfolioId, holdings: [{symbol, quantity, avgCost, purchaseDate?}] }` | Bulk-add up to 500 rows. Idempotent on `(portfolioId, symbol)`. |
| `parse_portfolio_text` | `{ text }` | CSV / Robinhood-positions parser. Returns matched + unmatched rows. Doesn't write — feed the matched rows to `add_holdings` after the user confirms. |
| `add_transaction` | `{ portfolioId, symbol, type, ... }` | Append one buy / sell / dividend / split / fee. Idempotent on `(portfolioId, externalId)` when `externalId` is set. |
| `update_holding` | `{ portfolioId, symbol, quantity?, avgCost?, purchaseDate? }` | Surgical edit. Only the provided fields change. |
| `delete_holding` | `{ portfolioId, symbol, confirm: true }` | Destructive. Refuses without `confirm: true`. Guards against hallucinated tool calls. |

## Portfolio analytics (6)

Scope: `portfolio:read`.

| Tool | What it returns |
|---|---|
| `get_portfolio_overview` | MV, cost basis, unrealized P&L, allocation by sector / country / market-cap class, top winners + losers. The single-call answer to "how is my portfolio doing?". |
| `get_portfolio_performance` | Period total return (1M / 3M / YTD / 1Y / 3Y / 5Y / MAX) + excess vs SPY + QQQ + latest vol / drawdown / Sharpe. Optional ≤90-pt downsampled time series. |
| `analyze_portfolio_fundamentals` | Per-holding 3y/5y/10y CAGRs (sales, profit), margins, ROE, debt/equity, FCF yield, Piotroski + portfolio-weighted aggregate. |
| `get_growth_vs_stagnant` | Bucketing across `accelerating` / `growing` / `stable` / `stagnant` / `declining` / `noData`. Top growers + stagnators. Weighted revenue + profit CAGRs. |
| `get_dividend_summary` | TTM income, current yield (on market and on cost), weighted payout ratio, top payers, upcoming ex-dividend dates. |
| `get_risk_summary` | Weighted beta, Altman Z, debt/equity. Latest portfolio volatility / max drawdown / Sharpe. Concentration block (top-N weights, sector + holding HHI). Threshold flags (single-stock >15%, sector >40%). |

All analytics tools return constant-size responses regardless of holdings count. The reasoning is in [architecture.md](architecture.md#why-analytics-tools-dont-paginate).

## Single-company research (7)

Scope: `market:read`.

| Tool | Args | Returns |
|---|---|---|
| `search_companies` | `{ query }` | Fuzzy match by symbol or name. Top 25 hits. |
| `get_company_snapshot` | `{ symbol, exchange? }` | Latest annual snapshot — ~30-field projection (valuation, profitability, growth, technicals, analyst view). |
| `get_financials` | `{ symbol, statement, period?, years? }` | Last N periods of one statement (income / balance / cashflow), annual or quarterly, oldest first. |
| `get_growth_stats` | `{ symbol }` | Multi-horizon CAGRs (3y/5y/7y/10y) across sales, profit, EBITDA, EBIT, gross profit. Plus YoY medians + averages. |
| `get_peers` | `{ symbol, n?, peerType? }` | Top-N similar companies (default 10, max 20). `peerType`: `same_industry` (default) or `global`. |
| `get_analyst_view` | `{ symbol }` | Target price + computed upside %, consensus rating, ratings histogram (strongBuy / buy / hold / sell / strongSell), EPS revisions (current vs 7/30/60/90 days ago, up/down counts). |
| `get_price_history` | `{ symbol, from, to }` | Daily OHLC + adjusted close + volume. Hard cap: 730 days per call. |

## Market & sector (10)

Scope: `market:read`.

| Tool | Args | Returns |
|---|---|---|
| `compare_companies` | `{ symbols: [up to 5] }` | Side-by-side: valuation, profitability, growth, leverage, quality, beta, recent returns. Rejects > 5 symbols. |
| `get_sector_overview` | `{ sector }` | Counts + total market cap, P10/P25/median/P75/P90 of 10 metrics, top 10 by cap, leaders + laggards by 1Y return. |
| `get_industry_overview` | `{ industry }` | Same shape as sector at industry granularity, plus parent sector context. |
| `get_market_overview` | `{}` | US-market roll-up: total cap, sector heatmap (count + total cap + median 1D + 1Y returns per sector), top 10 gainers + losers (≥ $2B cap). |
| `get_top_movers` | `{ window, sector?, marketCapClass? }` | Top 10 gainers + losers + volume movers. Window: 1d / 1w / 1m / ytd / 1y. |
| `get_earnings_calendar` | `{ from, to, marketCapClass? }` | Upcoming earnings between [from, to], capped at 90 days. Includes consensus EPS + revenue + prior-quarter surprise. |
| `get_dividend_calendar` | `{ from, to }` | Upcoming ex-dividend dates with single-payment yield. 90-day cap. |
| `get_valuation_history` | `{ symbol }` | Current P/E + P/S + EV/EBITDA TTM + their 5y / 10y percentile rank vs the company's own past. Higher = more expensive than usual. |
| `get_quality_scores` | `{ symbols: [up to 10] }` | Piotroski F + Altman Z + Beneish M + Magic Formula + Sloan-style cash. Includes threshold interpretation notes. |
| `get_news_sentiment` | `{ symbol, limit? }` | Recent headlines + per-ticker sentiment scores. Default 10, max 50. |

## Discovery + ChatGPT compat (3)

Scope: `market:read`.

| Tool | Args | Returns |
|---|---|---|
| `screen` | `{ query?, sortField?, sortDirection?, cursor? }` | Filter + sort the US universe via screener DSL. Cursor-paginated, 50 / page. |
| `search` | `{ query }` | ChatGPT deep-research compat. Returns `{ results: [{ id, title, text, url? }] }`. Pass an `id` to `fetch`. |
| `fetch` | `{ id }` | ChatGPT deep-research compat. Returns `{ id, title, text, url, metadata }`. `text` is a plain-language summary; `metadata` is the structured breakdown. |

The names `search` and `fetch` are mandatory for ChatGPT's deep-research mode to recognize the connector.

## v2 deferrals

Things we know we want but cut from v1 to ship faster:

- `analyze_portfolio_fundamentals.marginTrend` — needs a join into the historical ratios table. v1 returns current operating margin only.
- `get_dividend_summary` strict held-on-ex-date math — v1 uses `current_quantity × Σ(amount over 365d)`, which is what Robinhood and Fidelity show. The strict version walks the per-symbol transaction history.
- `web_search` multi-backend tool — fan out to Claude internal web search, Google Gemini grounded search, Perplexity online. Sibling to `search_companies`. Planned for v2.

[Open a tool request](https://github.com/dark-horse-stocks/shinobidata/issues/new?template=tool-request.yml) for anything else you want.
