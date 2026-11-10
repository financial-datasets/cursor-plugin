---
name: read-filing
description: Read a section of a company's 10-K, 10-Q, or 8-K with Financial Datasets and answer from the filing text.
---

# Read an SEC filing section

1. Take the ticker, the filing type (`10-K`, `10-Q`, or `8-K`), and the section the user wants. Default to the latest 10-K when they name none.
2. Confirm the `financial-datasets` MCP server is enabled and signed in. If it is not, use the `connect-financial-datasets` skill.
3. Call `get_filings` with the ticker, the `filing_type`, and a small `limit` to find the filing, its year, and its accession number.
4. Map the section to an item name, such as `Item-1A` for Risk Factors or `Item-7` for MD&A in a 10-K. Call `list_filing_item_types` when unsure.
5. Call `get_filing_items` with only the items needed. A 10-K needs `year`, a 10-Q needs `year` and `quarter`, and an 8-K needs `accession_number`.
6. Answer from the returned text. Quote the passages that support the answer, and name the filing type, its period, and the filing URL.
