---
name: fd-api
description: Write application code against the Financial Datasets REST API with API-key authentication. Use when the user asks to integrate Financial Datasets into a codebase, script, notebook, or backend, to call an endpoint from code, to debug a Financial Datasets API request, or to connect their own agent to the Financial Datasets MCP server with an API key.
---

# Build with the Financial Datasets API

There are two ways to reach the data. Keep them apart.

| Task | Use | Auth |
| --- | --- | --- |
| Answer a question in this chat | The `financial-datasets` MCP tools | The plugin's sign-in. No API key. |
| Write code that calls the API | The REST API at `https://api.financialdatasets.ai` | An API key in the `X-API-KEY` header |

This skill covers the second row.

## Authentication

1. The user creates an API key in the dashboard at https://financialdatasets.ai.
2. Read the key from a server-side environment variable, `FINANCIAL_DATASETS_API_KEY`.
3. Send it in the `X-API-KEY` header on every request.

Never write the key into source code, a committed file, or browser code, and never ask the user to paste it into chat. In a web app, call the API from the server and return only the data to the client.

## A first request

```bash
curl "https://api.financialdatasets.ai/financials/income-statements?ticker=AAPL&period=annual&limit=4" \
  -H "X-API-KEY: $FINANCIAL_DATASETS_API_KEY"
```

```python
import os
import requests

headers = {"X-API-KEY": os.environ["FINANCIAL_DATASETS_API_KEY"]}
url = "https://api.financialdatasets.ai/financials/income-statements"
params = {"ticker": "AAPL", "period": "annual", "limit": 4}

response = requests.get(url, headers=headers, params=params)
response.raise_for_status()
statements = response.json()["income_statements"]
```

Each endpoint wraps its rows in a named key, such as `income_statements`, `snapshot`, or `company_facts`.

## Look up the endpoint before writing the call

Do not write parameters or response fields from memory. Read the source:

- OpenAPI spec, the source of truth for every request and response schema: https://financialdatasets.ai/openapi.json
- Index of every docs page: https://docs.financialdatasets.ai/llms.txt
- Any docs page as Markdown: append `.md` to its URL

Common endpoints:

| Data | Endpoint |
| --- | --- |
| Income statements, balance sheets, cash flow statements | `GET /financials/income-statements`, `/financials/balance-sheets`, `/financials/cash-flow-statements` |
| Financial metrics | `GET /financial-metrics`, `GET /financial-metrics/snapshot` |
| Prices | `GET /prices`, `GET /prices/snapshot` |
| Company facts | `GET /company/facts` |
| SEC filings and filing sections | `GET /filings`, `GET /filings/items` |
| Insider trades | `GET /insider-trades` |
| Institutional holdings | `GET /institutional-holdings` |
| News | `GET /news` |
| Stock screener | `POST /financials/search/screener` |

## Pagination

List endpoints return pages. When more rows match than one page holds, the response includes `next_page_url`. Request that URL exactly as given, with the same header, and stop when the field is absent.

```python
rows = []
while url:
    data = requests.get(url, headers=headers).json()
    rows.extend(data["income_statements"])
    url = data.get("next_page_url")
```

## Connecting an agent over MCP with an API key

A script or production agent that speaks MCP connects to `https://mcp.financialdatasets.ai/api` and sends the same `X-API-KEY` header. That endpoint serves the same tools as the plugin without a browser sign-in. See https://docs.financialdatasets.ai/mcp-server.

## Handling responses

- Check the HTTP status before reading the body, and surface the API's error message instead of swallowing it.
- Handle a missing or `null` field explicitly. Never substitute zero for it.
- Requests are billed to the key's account. Cache results the code reuses instead of fetching them on every run.
