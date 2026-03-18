---
name: build-thesis
description: Build a comprehensive Hebrew investment thesis for an Israeli public company
arguments:
  - name: company_name
    description: Company name in Hebrew or English
    required: true
---

# Build Investment Thesis

You are building a professional Hebrew investment thesis using DeepValue's financial data tools.

## Pre-Flight Questions

Before starting, ask the user:
1. What is the investment horizon? (short/medium/long — default: long)
2. What is the analysis focus? (value/growth/dividend — default: value)
3. Any specific concerns or angles to explore?

## Skill Context

Apply the **tase-analysis** skill for Hebrew formatting, IFRS conventions, and TASE market structure.
Apply the **deep-value-methodology** skill for valuation frameworks, red flags, and scoring.

## Writing Style

The output must be written as a professional narrative for a fund manager analyst evaluating a buy/sell decision. No emojis. No bullet points. Each paragraph focuses on one key idea. Sections flow naturally with transition words (מילות קישור), as if reading a chapter from a book. Precise and data-driven, but readable.

## Workflow

Use the MCP prompt `build_investment_thesis` with the company name and user preferences to execute the full 15-step analysis workflow.

The workflow will:
1. Search and identify the company
2. Business overview — what the company does, operations, management, market position
3. Competitive advantages (moat) — type, strength, sustainability
4. Risk factors — type, severity, business impact
5. Gather 20 quarters and 10 years of income statements, balance sheets, and cash flows
6. Get current ratios via screen_stocks (100+ metrics, more comprehensive than get_current_ratios)
7. Get TTM historical ratios for 10 years (best source for valuation multiples like P/E, P/B, EV/EBITDA)
8. Review dividend history and order backlog
9. Compare with sector peers via stock screening
10. Get current market data
11. Generate 4 branded charts (revenue trend, margins, balance sheet composition, revenue vs margin)
12. Write the complete thesis in Hebrew narrative style

## Output Format

The output pipeline is: **HTML (source) → PDF (presentation)**.

1. **Build HTML**: Create a complete HTML file with RTL direction (`dir="rtl"`, `lang="he"`), professional CSS styling (fonts, spacing, tables, colors), and charts embedded as base64: `<img src="data:image/png;base64,{image_base64}" />` using the `image_base64` field from `generate_chart`. **Never use `image_url` in the HTML** — external URLs are blocked in sandbox. **Never regenerate charts with matplotlib** — always use `image_base64` from `generate_chart`. Save this HTML file — it is the editable source document.
2. **Convert to PDF**: Programmatically convert the HTML file to PDF (using weasyprint or equivalent). Do NOT regenerate content — convert the exact HTML file as-is.
3. **Present PDF**: Show the PDF to the user as the final output. The user sees only the PDF.
4. **Edits**: If the user requests changes, edit the HTML source file and re-convert to PDF. Never regenerate from scratch.

The HTML is the source of truth. The PDF is the presentation layer.

## Error Handling

If company not found, suggest similar names and ask user to clarify. If data is incomplete, note which data is missing and analyze what's available. If charts fail, continue with text-only analysis.
