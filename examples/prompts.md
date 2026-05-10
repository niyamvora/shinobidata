# Prompts

Real prompts that exercise the full surface. Paste any into Claude Desktop or ChatGPT once you've [installed the connector](../docs/installation.md).

Each one typically triggers 1-4 tool calls. Tools the AI calls are noted under each block.

## Setup

Run once.

```
Create a portfolio called "Test Portfolio".

Then parse this and add the holdings to it:

Symbol, Quantity, AvgCost
AAPL, 25, 180
NVDA, 50, 75
JPM, 25, 200
JNJ, 30, 160
KO, 70, 60
XOM, 40, 100
PLTR, 80, 18
CRWD, 10, 270
DDOG, 25, 110
SHOP, 40, 70
HOOD, 80, 25
DKNG, 100, 40
RBLX, 50, 45
AFRM, 50, 50
RKLB, 150, 10
SOFI, 250, 8
RIVN, 200, 50
OPEN, 1500, 20
LUNR, 300, 8
ACHR, 350, 7
```

Tools: `create_portfolio`, `parse_portfolio_text`, `add_holdings`.

20 holdings spanning mega/large/mid/small/micro across 9 sectors. Two intentional underwater positions (RIVN, OPEN) so the winners/losers tables show up clearly.

## How is my portfolio doing

```
How is my portfolio doing? Give me the full overview.
```

Tools: `get_portfolio_overview`. Market value, cost basis, unrealized P&L, sector / country / market-cap allocation, top 5 winners + 5 losers by 1Y return.

## Growing vs stagnating

```
Which of my holdings are growing fastest, and which are stagnating?
```

Tools: `get_growth_vs_stagnant`. Bucket counts, top growers, top stagnators, weighted revenue + profit CAGRs.

## Did I beat the S&P

```
Did my portfolio beat the S&P 500 over the last year?
```

Tools: `get_portfolio_performance` with period `1Y`. Total return, excess vs SPY + QQQ, latest vol / drawdown / Sharpe.

## Dividends

```
How much dividend income did I get over the past year, and what's coming up in the next 30 days?
```

Tools: `get_dividend_summary` (TTM income, top payers, current yield) + `get_dividend_calendar` (upcoming ex-dates).

## Risk and concentration

```
How risky is my portfolio? Any concentration concerns?
```

Tools: `get_risk_summary`. Weighted beta, vol, drawdown, Sharpe, top-N concentration weights, sector + holding HHI, threshold flags.

## Compare 5 megacaps

```
Compare AAPL, MSFT, NVDA, GOOGL, and META side by side.
```

Tools: `compare_companies`. Valuation, profitability, growth, leverage, quality, beta, recent returns.

## Lay of the market

```
What's the US market doing today?

Then drill into the technology sector.
Then into the semiconductors industry specifically.
```

Tools: `get_market_overview` → `get_sector_overview` → `get_industry_overview`.

## What's coming up

```
What earnings reports are coming up in the next 14 days?
What ex-dividend dates are coming up in the next 30 days?
Who are today's top movers?
```

Tools: `get_earnings_calendar`, `get_dividend_calendar`, `get_top_movers`.

## Cheap vs its history

```
Is AAPL cheap or expensive vs its own 5- and 10-year history?
```

Tools: `get_valuation_history`. Current P/E + P/S + EV/EBITDA TTM, 5y / 10y percentile rank.

## Quality scores

```
Get the quality scores for AAPL, NVDA, JPM, JNJ, MSFT.
```

Tools: `get_quality_scores`. Piotroski F + Altman Z + Beneish M + Magic Formula + Sloan-style cash, with thresholds.

## News on a name

```
What's the news sentiment on NVDA right now?
```

Tools: `get_news_sentiment`.

## Single-company drilldown (the cascade)

```
Search for "data" companies.
Give me a snapshot of NVDA.
Show NVDA's last 5 years of income statements.
What are NVDA's growth stats across all horizons?
Who are NVDA's peers?
What do analysts think about NVDA?
Show NVDA's price history from 2025-01-01 to 2025-06-01.
```

Tools: `search_companies` → `get_company_snapshot` → `get_financials` → `get_growth_stats` → `get_peers` → `get_analyst_view` → `get_price_history`.

## Screen the universe

```
Screen for technology stocks with P/E < 30 and Piotroski F-score >= 7. Sort by 5Y sales CAGR descending.
```

Tools: `screen` with the DSL filter.

## ChatGPT Deep Research

In ChatGPT, switch to Deep Research and try:

```
Pick a few US stocks with strong recent revenue growth and reasonable valuation. Use ShinobiData to research them. Look at fundamentals, valuation history, peers, and analyst views. Give me a recommendation.
```

ChatGPT will orchestrate `search` → `get_growth_stats` → `get_valuation_history` → `compare_companies` → `fetch` and stitch a real answer with citations.

## Mutations

```
Update my AAPL holding to 30 shares at $185 avg cost.
Add a transaction: I bought 10 more NVDA at $130 today.
Delete my OPEN position. (Yes, confirm.)
```

Tools: `update_holding`, `add_transaction`, `delete_holding`. `delete_holding` requires `confirm: true`.

## Edge cases

These exercise the validation layer:

```
Compare AAPL, MSFT, NVDA, GOOGL, META, AMZN.   # 6 symbols, gets rejected (cap is 5)
Show NVDA's price history from 2023-01-01 to 2026-01-01.   # 1095 days, rejected (cap is 730)
Delete my AAPL position.   # missing confirm: true, refused
Get my portfolio without authorizing first.   # 401 with WWW-Authenticate
```

Each returns a structured error explaining what's wrong.
