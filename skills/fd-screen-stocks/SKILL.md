---
name: fd-screen-stocks
description: Screen for stocks that match financial criteria with Financial Datasets. Use when the user asks to find, filter, or list companies by revenue, profit, growth, margins, debt, valuation ratios such as P/E, market cap, sector, or industry, or describes an investment screen in plain language.
---

# Screen stocks

Use `screen_stocks` from the `financial-datasets` MCP server. It returns the companies whose latest reported data matches every filter you pass.

## Workflow

1. Call `list_stock_screener_filters` unless its output is already in this conversation. It returns the exact field names and the operators each field accepts. Do not guess field names.
2. Translate the request into filters. Each filter is one condition:

   ```json
   { "field": "revenue", "operator": "gt", "value": 1000000000 }
   ```

3. Call `screen_stocks` with the `filters` array. Set `limit` (default 10, max 100) to the number of results the user wants.
4. Present the matches as a table with the ticker, each filtered field, and the `report_period` of the data.

## Writing filters

- Operators: `gt`, `lt`, `gte`, `lte`, `eq` for numeric fields; `eq` or `in` for `sector` and `industry`.
- Write money amounts in full. One billion dollars is `1000000000`, not `1` or `"1B"`.
- Sector and industry values are strings, for example `{ "field": "sector", "operator": "eq", "value": "Information Technology" }`. `in` takes an array of them. Take the spelling from `get_company_facts` for a company in that sector; do not invent one.
- All filters must hold at once. There is no OR between filters; run two screens for an either-or request.

## Limits

- Results come back sorted by ticker, and `limit` cuts that list. A screen with more matches than `limit` returns the alphabetically first ones, not the largest or cheapest.
- For a ranked request such as "the ten largest", tighten the filters until the matches fit within `limit`, sort the returned rows yourself, and tell the user how the list was built.
- The screener covers US-listed companies and uses each company's latest reported period.

After a screen, use the `fd-company-research` skill to look closer at any match.
