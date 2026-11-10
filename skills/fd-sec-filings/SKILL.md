---
name: fd-sec-filings
description: Find SEC filings and read their sections with Financial Datasets. Use when the user asks for a company's 10-K, 10-Q, or 8-K, wants the Risk Factors, Business description, MD&A, or another filing section, asks what a company disclosed in a filing, or needs a filing's accession number or URL.
---

# Find and read SEC filings

Two tools from the `financial-datasets` MCP server do this work:

- `get_filings` lists a company's filings: type, report date, accession number, and document URLs.
- `get_filing_items` returns the text of named sections from a 10-K, 10-Q, or 8-K.

## Workflow

1. If you do not know which filing the user means, call `get_filings` with the `ticker` (or `cik`) and a `filing_type` such as `10-K`, `10-Q`, `8-K`, or `DEF 14A`. Lower `limit` from its default of 100 when you only need the most recent few.
2. Call `get_filing_items` with what that filing type requires:

   | `filing_type` | Required |
   | --- | --- |
   | `10-K` | `year` |
   | `10-Q` | `year` and `quarter` (1 to 4) |
   | `8-K` | `accession_number`, taken from `get_filings` |

3. Pass `item` as an array of item names and ask only for the sections the question needs. Sections are long.
4. Answer from the returned text. Name the filing type and period, and give the filing URL from `get_filings` when the user needs a source.

## Item names

Item names look like `Item-1A`. Common 10-K sections:

| Item | Section |
| --- | --- |
| `Item-1` | Business |
| `Item-1A` | Risk Factors |
| `Item-7` | Management's Discussion and Analysis |
| `Item-7A` | Quantitative and Qualitative Disclosures About Market Risk |
| `Item-8` | Financial Statements and Supplementary Data |

Common 8-K items: `Item-1.01` (material agreement), `Item-2.02` (results of operations), `Item-5.02` (officer and director changes), `Item-8.01` (other events).

For 10-Q items, or any name you are unsure of, call `list_filing_item_types`. It returns every item name grouped by filing type.

## Limits

- Section text is available for 10-K, 10-Q, and 8-K filings. For other forms, give the user the document URL from `get_filings`.
- For numbers, prefer the statement tools (`get_income_statement` and the others) over reading Item 8. They return the figures as structured data.
- Quote the filing; do not paraphrase a disclosure into something stronger than what it says.
