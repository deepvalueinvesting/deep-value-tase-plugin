---
name: analyze-report
description: Deep-dive analysis of a company's latest financial report with QoQ/YoY trends
arguments:
  - name: company_name
    description: Company name in Hebrew or English
    required: true
---

# Analyze Financial Report

You are performing a deep-dive analysis of the latest financial report using DeepValue's financial data tools.

## Pre-Flight Questions

Before starting, ask the user:
1. Which reporting period? (latest by default, or specify Q1/Q2/Q3/Q4/Annual)
2. Any specific metrics or sections to focus on?

## Skill Context

Apply the **hebrew-quality** skill for Hebrew language quality, grammar, formatting, and financial terminology.
Apply the **tase-analysis** skill for IFRS conventions and TASE market structure.
Apply the **deep-value-methodology** skill for identifying red flags and quality indicators.

## Writing Style

The output must be written as a professional narrative for a fund manager analyst. No emojis. No bullet points. Each paragraph focuses on one key idea. Sections connected with transition words. Flowing, cohesive text — like a chapter from a book.

## Workflow

Use the MCP prompt `analyze_financial_report` with the company name to execute the analysis workflow.

The workflow will:
1. Search and identify the company
2. Find the most recent report in the database and verify with the user
3. Gather 20 quarters and 10 years of financial data (income statements, balance sheets, cash flows). If latest quarter data not yet ingested, fall back to main_events or report PDF
4. Special Q4 handling: for annual reports, derive Q4 results (annual minus Q1+Q2+Q3) and analyze both annual and implied Q4 separately
5. Get current ratios via screen_stocks (100+ metrics) and TTM historical ratios for 10 years
6. Calculate all relevant ratios vs past values (QoQ, YoY) even if not in the report
7. Generate 3 branded charts (revenue/profit over 20 quarters, margin trends, working capital)
8. Write the narrative analysis in Hebrew

## Output Format

The output pipeline is: **HTML (source) → PDF (presentation)**.

1. **Build HTML**: Create a complete HTML file with RTL direction (`dir="rtl"`, `lang="he"`), professional CSS styling (fonts, spacing, tables, colors), and charts embedded as base64: `<img src="data:image/png;base64,{image_base64}" />` using the `image_base64` field from `generate_chart`. **Never use `image_url` in the HTML** — external URLs are blocked in sandbox. **Never regenerate charts with matplotlib** — always use `image_base64` from `generate_chart`. Save this HTML file — it is the editable source document.
2. **Convert to PDF**: Programmatically convert the HTML file to PDF (using weasyprint or equivalent). Do NOT regenerate content — convert the exact HTML file as-is.
3. **Present PDF**: Show the PDF to the user as the final output. The user sees only the PDF.
4. **Edits**: If the user requests changes, edit the HTML source file and re-convert to PDF. Never regenerate from scratch.

The HTML is the source of truth. The PDF is the presentation layer.

## Error Handling

If company not found, suggest similar names. If financial data not yet in DB for latest report, check main_events first, then fall back to PDF URL from reporting_files. Always note data source used.
