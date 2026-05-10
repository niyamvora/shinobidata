# Demo prompts

A gallery of real prompts that exercise the full surface. Copy any of these into Claude Desktop or ChatGPT after [installing the connector](../docs/installation.md).

Grouped by what the AI orchestrates underneath. Each prompt typically triggers 1–4 tool calls.

---

## Setup (run once)

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

Tools the AI calls: `create_portfolio` → `parse_portfolio_text` → `add_holdings`.

> 20 holdings spanning mega/large/mid/small/micro across 9 sectors. Two intentional underwater positions (RIVN, OPEN) so the winners/losers tables show up clearly.

---

## "How is my portfolio doing?"

```
How is my portfolio doing? Give me the full overview.
```

Tools: `get_portfolio_overview`. Returns market value, cost basis, unrealized P&L, sector / country / market-cap allocation, top 5 winners + top 5 losers by 1Y return.

---

## "Which holdings are growing fast vs stagnating?"

```
Which of my holdings are growing fastest, and which are stagnating?
```

Tools: `get_growth_vs_stagnant`. Returns bucket counts (`accelerating` / `growing` / `stable` / `stagnant` / `declining` / `noData`), top growers, top stagnators, weighted revenue + profit CAGRs.

---

## "Did my portfolio beat the S&P over the last year?"

```
Did my portfolio beat the S&P 500 over the last year?
```

Tools: `get_portfolio_performance` with period `1Y`. Returns total return, excess vs SPY + QQQ, latest vol/drawdown/Sharpe.

---

## "What dividends am I getting?"

```
How much dividend income did I get over the past year, and what's coming up in the next 30 days?
```

Tools: `get_dividend_summary` (TTM income, top payers, current yield) + `get_dividend_calendar` (upcoming ex-dates).

---

## "How risky is my portfolio?"

```
How risky is my portfolio? Any concentration concerns?
```

Tools: `get_risk_summary`. Returns weighted beta, vol, drawdown, Sharpe, top-N concentration weights, sector + holding HHI, and threshold flags (single-stock >15%, sector >40%).

---

## "Compare these 5 megacaps"

```
Compare AAPL, MSFT, NVDA, GOOGL, and META side by side.
```

Tools: `compare_companies`. Returns valuation, profitability, growth, leverage, quality, beta, and recent returns for all 5.

---

## "Give me the lay of the market today"

```
What's the US market doing today?

Then drill into the technology sector.
Then drill into the semiconductors industry.
```

Tools: `get_market_overview` → `get_sector_overview` → `get_industry_overview`.

---

## "What's coming up?"

```
What earnings reports are coming up in the next 14 days?
What ex-dividend dates are coming up in the next 30 days?
Who are today's top movers?
```

Tools: `get_earnings_calendar`, `get_dividend_calendar`, `get_top_movers`.

---

## "Is X cheap vs its history?"

```
Is AAPL cheap or expensive vs its own 5- and 10-year history?
```

Tools: `get_valuation_history`. Returns current P/E + P/S + EV/EBITDA TTM and their 5y / 10y percentile ranks.

---

## "How healthy are these companies fundamentally?"

```
Get the quality scores for AAPL, NVDA, JPM, JNJ, MSFT.
```

Tools: `get_quality_scores`. Returns Piotroski F + Altman Z + Beneish M + Magic Formula + Sloan-style cash score for each, with threshold interpretation.

---

## "What's the news on X?"

```
What's the news sentiment on NVDA right now?
```

Tools: `get_news_sentiment`. Returns recent headlines + per-ticker sentiment scores.

---

## Single-company drilldown

```
Search for "data" companies.
Then give me a snapshot of NVDA.
Show NVDA's last 5 years of income statements.
What are NVDA's growth stats across all horizons?
Who are NVDA's peers in the same industry?
What do analysts think about NVDA right now?
Show me NVDA's price history from 2025-01-01 to 2025-06-01.
```

Tools: the full `search_companies` → `get_company_snapshot` → `get_financials` → `get_growth_stats` → `get_peers` → `get_analyst_view` → `get_price_history` cascade.

---

## Discovery (screen the universe)

```
Screen for technology stocks with P/E < 30 and Piotroski F-score >= 7. Sort by 5Y sales CAGR descending.
```

Tools: `screen` with the DSL filter.

---

## ChatGPT Deep Research mode

In ChatGPT, switch to **Deep Research** and try:

```
Pick a few US stocks with strong recent revenue growth and reasonable valuation. Use ShinobiData to research them. Look at fundamentals, valuation history, peers, and analyst views. Give me a recommendation.
```

ChatGPT will orchestrate `search` → `get_growth_stats` → `get_valuation_history` → `compare_companies` → `fetch` to compose a grounded, citation-rich answer.

---

## Mutations

```
Update my AAPL holding to 30 shares at $185 avg cost.
Add a transaction: I bought 10 more NVDA at $130 today.
Delete my OPEN position. (Yes, confirm.)
```

Tools: `update_holding`, `add_transaction`, `delete_holding`. Note `delete_holding` requires explicit `confirm: true`.

---

## Edge-case tests

These exercise the validation layer:

```
Compare AAPL, MSFT, NVDA, GOOGL, META, AMZN.    # 6 symbols → rejected (cap 5)
Show me NVDA's price history from 2023-01-01 to 2026-01-01.    # 1095 days → rejected (cap 730)
Delete my AAPL position.    # missing confirm: true → refused
Get my portfolio without authorizing first.    # 401 with WWW-Authenticate
```

Each returns a structured error explaining what's wrong.
