---
name: connect-financial-datasets
description: Connect or troubleshoot Financial Datasets in Cursor. Use when the user has just installed this plugin, asks how to sign in or authenticate, cannot see the Financial Datasets tools, sees the server listed twice, or gets a 401 or authentication error from the Financial Datasets MCP server.
---

# Connect Financial Datasets in Cursor

## Standard connection

1. Open **Customize** in the Cursor sidebar (or **Cursor Settings → Tools & MCP**).
2. Find the `financial-datasets` server installed by this plugin and enable it.
3. Select **Connect** or **Authenticate**.
4. Sign in to Financial Datasets in the browser and approve access. An account can be created at https://financialdatasets.ai.
5. Return to Cursor and confirm that the server lists its tools.

The plugin connects to `https://mcp.financialdatasets.ai/` over Streamable HTTP. Cursor discovers the sign-in flow from the server. Do not add headers, client secrets, or API keys to `mcp.json`.

## Verify

Use a read-only request:

```text
Use Financial Datasets to get the latest stock price for AAPL.
```

The agent should call `get_stock_price`. A result confirms both the connection and the signed-in account.

## Troubleshooting

| Symptom | Action |
| --- | --- |
| `financial-datasets` is missing | Refresh or reinstall the plugin, then reload the Cursor window |
| The server appears twice | An older manual entry exists. Remove `financial-datasets` from `~/.cursor/mcp.json` or the project's `.cursor/mcp.json` and keep the plugin's server |
| 401 or "authentication required" | Disconnect the server, connect it again, and complete sign-in |
| The browser sign-in does not open | Open the server's details and select Authenticate |
| Tools are still unavailable after sign-in | Reload the Cursor window and enable the server again |
| A tool returns an error about the plan, credits, or limits | Relay the message and point the user to https://www.financialdatasets.ai/pricing. Do not retry the same call |

Never ask the user to paste a token or API key into chat.

## API keys

Sign-in covers Cursor's connection to the MCP server. An API key is needed only when the user writes their own code against the REST API, or connects a script to `https://mcp.financialdatasets.ai/api`. For that, use the `fd-api` skill.
