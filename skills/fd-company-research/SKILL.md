---
name: fd-company-research
description: Research a public company's fundamentals with Financial Datasets. Use when the user asks about a company's revenue, earnings, margins, cash flow, balance sheet, valuation ratios, market cap, stock price or price history, business segments, operating KPIs, guidance, latest earnings, or recent news, or wants to compare companies on those numbers.
---

# Research a company

Use the `financial-datasets` MCP tools. Fetch the numbers; do not recall them.

## Pick the tool

| Need | Tool |
| --- | --- |
| Confirm the company, sector, exchange, or whether it is still active | `get_company_facts` |
| Latest price | `get_stock_price` |
| Price history | `get_stock_prices` |
| Current market cap, P/E, margins, and other ratios | `get_financial_metrics_snapshot` |
| The same ratios over time | `get_financial_metrics` |
| Revenue, expenses, net income, EPS | `get_income_statement` |
| Assets, liabilities, equity, debt, cash | `get_balance_sheet` |
| Operating cash flow, capex, buybacks, dividends | `get_cash_flow_statement` |
| Revenue and operating income by segment | `get_segmented_financials` |
| Operating numbers the company reports, such as subscribers or units sold | `get_kpi_metrics` |
| Company-defined measures, such as Adjusted EBITDA | `get_kpi_non_gaap` |
| Management's forward guidance | `get_kpi_guidance` |
| The most recent reported results | `get_earnings` |
| Recent headlines | `get_news` |

## Workflow

1. Resolve the ticker. If a name could match several listings, call `get_company_facts` or ask.
2. Set `period` on purpose. The statement and metrics tools default to `ttm`. Use `quarterly` for "the last four quarters" and `annual` for a multi-year trend.
3. Set `limit` to the number of periods the question needs (default 4, max 100). Use the `report_period_gte` and `report_period_lte` filters for a date range.
4. Call only the tools the question needs. One statement is usually enough.
5. Answer with the ticker, the report period, the period type, and the currency next to each figure.

## Details that change the answer

- `get_stock_prices` returns daily bars for the trailing year by default. Set `interval` (`second`, `minute`, `day`, `week`, `month`, `year`), `interval_multiplier`, `start_date`, and `end_date` for anything else.
- `as_reported: true` on a statement tool returns the line items exactly as the company filed them, instead of the standardized fields. It supports `annual` and `quarterly` only.
- Companies outside the US: pass `market: "UK"` or `market: "FR"` with the ticker, or pass an `isin`. These listings carry annual data only.
- `get_segmented_financials` defaults to `annual`. The KPI tools default to `quarterly` and take a `metric_name` filter.
- `get_earnings` with a `ticker` returns that company's recent results. The same `report_period` can appear twice, once from the 8-K announcement and once from the 10-Q or 10-K; do not count it as two quarters. Without a `ticker` it returns the feed of companies that reported most recently.
- `get_news` returns at most 10 articles. Omit `ticker` for market-wide news.

## Comparing companies

Use the same tool, `period`, and `limit` for each company so the periods line up. Fiscal years end in different months; show each company's `report_period` instead of assuming calendar years.

Do not mix trailing-twelve-month figures with quarterly or annual figures in one calculation. Label any ratio or growth rate you compute yourself as calculated.
