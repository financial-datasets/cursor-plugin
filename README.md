# Financial Datasets for Cursor

The official [Financial Datasets](https://www.financialdatasets.ai) plugin for Cursor. It gives Cursor stock market data for 27,000+ active and delisted U.S. tickers: financial statements, stock prices, SEC filings, insider and institutional ownership, KPIs, news, macro data, and a stock screener.

One install adds the Financial Datasets MCP server, with 28 read-only tools, and the skills, commands, and rule that tell Cursor which tool fits each question.

## Install and connect

1. Install **Financial Datasets** from the [Cursor Marketplace](https://cursor.com/marketplace).
2. Open **Customize** in the Cursor sidebar (or **Cursor Settings → Tools & MCP**) and enable the `financial-datasets` server.
3. Select **Connect** or **Authenticate**, then sign in to Financial Datasets in your browser and approve access.

The plugin connects to `https://mcp.financialdatasets.ai/` and signs you in with OAuth. You do not paste an API key into Cursor.

Usage is billed to the account you sign in with, at the same rates as the API. See [pricing](https://www.financialdatasets.ai/pricing).

If you added the server by hand before, remove `financial-datasets` from `~/.cursor/mcp.json` so that it is not listed twice.

## Try it

- "What was Apple's revenue and net income over the last 4 quarters?"
- "Pull the risk factors from Tesla's latest 10-K."
- "Show me insider trades at Nvidia this year."
- "Which companies have revenue above $100B and a P/E ratio under 20?"
- "Are there any activist investors in BlackBerry?"
- "What does the US Treasury yield curve look like today?"

## Included

| Component | What it provides |
| --- | --- |
| MCP server | The hosted Financial Datasets MCP server, with 28 read-only tools and OAuth sign-in |
| Skills | Workflows for company research, SEC filings, ownership, screening, and macro data, plus API guidance and connection help |
| Commands | `/company-snapshot`, `/read-filing`, and `/screen-stocks` |
| Rule | Sends market and company questions to the right tool, so that Cursor fetches figures instead of recalling them |

### Skills

| Skill | When Cursor uses it |
| --- | --- |
| `fd-company-research` | Fundamentals, valuation, prices, segments, KPIs, earnings, and news for a company |
| `fd-sec-filings` | Finding filings and reading sections of a 10-K, 10-Q, or 8-K |
| `fd-ownership` | Insider trades and holdings, 13F institutional positions, holders above 5%, and ETF constituents |
| `fd-screen-stocks` | Finding companies that match financial criteria |
| `fd-macro` | Treasury yields, policy rates, inflation, the labor market, GDP, and GPU rental prices |
| `fd-api` | Writing application code against the Financial Datasets REST API |
| `connect-financial-datasets` | Sign-in and MCP troubleshooting |

### Commands

| Command | What it does |
| --- | --- |
| `/company-snapshot` | Price, valuation, the last four quarters, and recent news for one company |
| `/read-filing` | Answers from a section of a company's 10-K, 10-Q, or 8-K |
| `/screen-stocks` | Turns criteria into screener filters and returns the matches |

### Tools

| Group | Tools |
| --- | --- |
| Company | `get_company_facts` |
| Financial statements | `get_income_statement`, `get_balance_sheet`, `get_cash_flow_statement`, `get_segmented_financials` |
| Metrics | `get_financial_metrics`, `get_financial_metrics_snapshot` |
| Earnings | `get_earnings` |
| KPIs | `get_kpi_metrics`, `get_kpi_guidance`, `get_kpi_non_gaap` |
| Prices | `get_stock_price`, `get_stock_prices` |
| SEC filings | `get_filings`, `get_filing_items`, `list_filing_item_types` |
| Insiders | `get_insider_trades`, `get_insider_ownership` |
| Institutions (13F) | `get_institutional_investors`, `get_institutional_holdings` |
| Beneficial owners (13D/13G) | `get_beneficial_owners`, `get_beneficial_ownership` |
| Index funds | `get_index_fund` |
| Macro | `get_macro_data`, `get_interest_rates` |
| News | `get_news` |
| Screener | `screen_stocks`, `list_stock_screener_filters` |

Full tool reference: [docs.financialdatasets.ai/mcp-server](https://docs.financialdatasets.ai/mcp-server).

## Building with the Financial Datasets API

Sign-in connects Cursor to the MCP server. Code that you write against the REST API uses an API key instead. Create one in the dashboard at [financialdatasets.ai](https://financialdatasets.ai), keep it in a server-side `FINANCIAL_DATASETS_API_KEY` environment variable, and send it in the `X-API-KEY` header. Never put the key in browser code or commit it to source control.

The `fd-api` skill guides Cursor through this. API reference: [docs.financialdatasets.ai](https://docs.financialdatasets.ai).

## Security

- Every tool is read-only.
- The plugin holds no code. It is Markdown and JSON, with no hooks and no scripts.
- It configures one connection, to `mcp.financialdatasets.ai`.
- It stores no API key and no secret. Cursor handles the OAuth sign-in.

## Local development

Load the plugin from a checkout of this repository:

```bash
mkdir -p ~/.cursor/plugins/local
rsync -a --delete --exclude '.git/' ./ ~/.cursor/plugins/local/financial-datasets/
```

Then run **Developer: Reload Window** in Cursor and check that the `financial-datasets` server, seven skills, three commands, and one rule appear under Customize. Copy the directory; Cursor does not load a symlink that points outside `~/.cursor/plugins/local`.

## Support

Email [support@financialdatasets.ai](mailto:support@financialdatasets.ai).

## License

MIT
