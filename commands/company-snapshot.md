---
name: company-snapshot
description: Get a one-screen snapshot of a public company with Financial Datasets, covering price, valuation, recent results, and news.
---

# Company snapshot

1. Take the ticker from the user's request. If they gave a company name that could match more than one listing, ask which one.
2. Confirm the `financial-datasets` MCP server is enabled and signed in. If it is not, use the `connect-financial-datasets` skill.
3. Call these tools for the ticker:
   - `get_company_facts`
   - `get_stock_price`
   - `get_financial_metrics_snapshot`
   - `get_income_statement` with `period: "quarterly"` and `limit: 4`
   - `get_news` with `limit: 5`
4. Reply in this order, using only returned values:
   - Company name, sector, industry, and exchange
   - Latest price and market cap
   - Three to five valuation and profitability ratios from the snapshot
   - A table of the last four quarters with the report period, revenue, and net income
   - Up to five headlines with their dates and links
5. Label every figure with its report period. If a tool returns an error or no data, say which part is missing; do not fill it from memory.
