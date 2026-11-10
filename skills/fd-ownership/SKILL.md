---
name: fd-ownership
description: Look up who owns and trades a stock with Financial Datasets. Use when the user asks about insider buying or selling, what executives or directors hold, which funds or institutions own a company, what a fund such as Berkshire Hathaway holds, 13F filings, holders above 5%, activist investors, Schedule 13D or 13G stakes, or the holdings and weights of an ETF or index fund.
---

# Ownership and insider activity

Use the `financial-datasets` MCP tools. Each source answers a different question.

| Question | Tool | Source |
| --- | --- | --- |
| What did insiders buy or sell? | `get_insider_trades` | Forms 4 and 5 transactions |
| What do insiders hold? | `get_insider_ownership` | Forms 3 and 5 |
| Which institutions hold this stock? | `get_institutional_holdings` with `ticker` | 13F |
| What does this institution hold? | `get_institutional_holdings` with `filer_cik` | 13F |
| Who holds more than 5%, and are any activists? | `get_beneficial_ownership` | Schedules 13D and 13G |
| What is inside this ETF or index fund? | `get_index_fund` | Fund holdings filings |

## Insiders

- `get_insider_trades` takes a `ticker` and optional `name`, `filing_date_gte`, and `filing_date_lte`. It returns up to 100 rows by default.
- `get_insider_ownership` shows positions as reported per filing, newest first. Filter with `form_type`: `3` is an insider's initial statement, `5` is the annual statement.
- Trades are events and ownership statements are the state. Use both when the user asks whether insiders are accumulating.

## Institutions (13F)

1. To query by institution, first call `get_institutional_investors` with a `name` prefix such as `Berkshire` to get the `filer_cik`.
2. Call `get_institutional_holdings` with exactly one of `filer_cik` or `ticker`.
3. With no `report_period` filter it returns the latest available quarter. Raise `limit` (default 10, max 200) for a fuller list.

13F data is quarterly. A position is the holding at the end of the filer's report period, not today's holding, so give the report period with every position.

## Holders above 5% and activists

1. To query by owner, first call `get_beneficial_owners` with a `name` prefix to get the `filer_cik`.
2. Call `get_beneficial_ownership` with exactly one of `ticker` or `filer_cik`.
3. Set `type: "activist"` for Schedule 13D stakes or `type: "passive"` for Schedule 13G stakes. Omit it for both.
4. By default each stake's current state is returned. Set `history: true` for the full amendment chain.

Coverage of these schedules begins in January 2025. Say so when the user asks about an earlier stake.

## ETFs and index funds

`get_index_fund` takes the fund's `ticker` (for example `SPY`) and returns holdings sorted by weight. Use `as_of` for the composition on a past date, `asset_class` (`equity` or `bond`) to filter, and `limit` with `offset` to page past the first 50 holdings.

## "Who owns this company?"

Combine `get_institutional_holdings` by `ticker`, `get_beneficial_ownership` by `ticker`, and `get_insider_ownership`. Keep the three groups separate in the answer, because the same shares can appear in more than one filing type.
