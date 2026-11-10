---
name: fd-macro
description: Get macroeconomic data with Financial Datasets. Use when the user asks about Treasury yields or the yield curve, the 2s10s spread, central bank policy rates, CPI or PCE inflation, the jobs report, payrolls, the unemployment rate, jobless claims, job openings, GDP growth, corporate profits, the saving rate, or GPU rental prices.
---

# Macroeconomic data

Use `get_macro_data` from the `financial-datasets` MCP server. One tool serves every dataset; the `dataset` argument picks it.

| `dataset` | Returns | History needs |
| --- | --- | --- |
| `yield_curve` | Daily US Treasury par yields, one key per tenor from `1_month` to `30_year` | dates only |
| `interest_rates` | Policy rates of major central banks | `bank`, for example `FED`, `ECB`, `BOE`, `BOJ` |
| `inflation` | US CPI and the PCE price index | `series`, for example `cpi_all_sa` or `pce_core_sa` |
| `labor` | Payrolls, unemployment, participation, hourly earnings, JOLTS, weekly jobless claims | `series`, for example `payrolls_sa` |
| `gdp` | Real GDP growth, GDP levels, contributions to growth, corporate profits, personal income, the saving rate | `series`, for example `real_gdp_growth` |
| `gpu_prices` | Daily rental price per GPU hour for `h100`, `h200`, `b200`, `b300` | `series` |

## Latest values or history

- Omit `start_date` and `end_date` to get the latest values for the whole dataset.
- Give `start_date`, `end_date`, or both to get history. For every dataset except `yield_curve`, history serves one bank or one series per call, so pass `bank` or `series` as the table shows.
- The tool description lists every series id. Use an id from that list.

## Reading the values

- A `null` means the value was not published for that date. It is not zero.
- Headline inflation is `change_12m_pct` of `cpi_all_nsa`. The Federal Reserve's preferred measure is `change_12m_pct` of `pce_core_sa`.
- `real_gdp_growth` is quarter over quarter at an annual rate. Year-over-year growth is `change_year_pct` of `real_gdp`.
- The Federal Reserve's rate is the midpoint of its target range.
- Give the reference date with each figure. CPI, PCE, and GDP series are published on different schedules, so their latest rows can refer to different periods.
- The `gpu_prices` response carries a source attribution. Keep it when you show the data.

`get_interest_rates` returns only the current policy rate of each bank. Prefer `get_macro_data` with `dataset: "interest_rates"`, which also serves history.
