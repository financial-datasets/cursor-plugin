---
name: screen-stocks
description: Find stocks that match financial criteria with the Financial Datasets stock screener.
---

# Screen stocks

1. Turn the user's criteria into a list of conditions. Ask for a threshold when one is missing, for example what "cheap" means.
2. Confirm the `financial-datasets` MCP server is enabled and signed in. If it is not, use the `connect-financial-datasets` skill.
3. Call `list_stock_screener_filters` to get the exact field names and operators, unless they are already in this conversation.
4. Call `screen_stocks` with one filter per condition. Write money amounts in full, so one billion dollars is `1000000000`.
5. Show the matches as a table with the ticker, each filtered field, and the report period.
6. Results are sorted by ticker and cut at `limit`. If the user asked for a ranking, say how the list was built, and tighten the filters when more companies match than `limit` returns.
