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
2. **Hebrew Review**: Before generating the HTML, re-read ALL the Hebrew text you wrote and run the **hebrew-quality** self-review checklist against it. Fix every violation before proceeding. The checklist:
   - [ ] **Gibberish check**: Read every sentence. Does each one make grammatical sense in Hebrew? Could a native speaker understand it?
   - [ ] **Vocabulary check**: Are there any English words that have Hebrew equivalents? (e.g., "All-Time-High" → "שיא כל הזמנים")
   - [ ] **Invented words check**: Does every Hebrew word actually exist? If unsure, replace with a known synonym.
   - [ ] **Calque check**: Are there phrases that sound like translated English? Rewrite them naturally.
   - [ ] **Verb agreement check**: Does every verb match its subject in gender and number?
   - [ ] **Financial terms check**: Are financial terms consistent with ISA/TASE conventions? (מכפיל רווח not P/E ratio, שיעור רווח גולמי not שוליים)
   - [ ] **Formatting check**: Currency on the left (₪1,234)? Dates as dd/MM/YYYY? Negative numbers wrapped in `<span dir="ltr">`?
   - [ ] **Fluency check**: Read 3 random paragraphs aloud. Do they flow naturally, or do they sound robotic/translated?
   Fix all issues found, then proceed to HTML generation.
3. **Design Review**: After generating the HTML file and before converting to PDF, verify the HTML against the **document-design.md** reference. Check every item:
   - [ ] `@page` rules: size A4, correct margins (15mm top for pages 2+, 0 for first page, 8mm bottom), white background, page numbers via `@bottom-center`
   - [ ] Header: gradient `#1a5276` → `#2c3e50`, padding `35px 75px`, `border-radius: 0`, `margin-bottom: 12px`
   - [ ] Content-body: single white box wrapping all sections, padding `50px 75px 40px`, `border-radius: 0`
   - [ ] Sections: separated by `margin-bottom: 28px` only — NO divider lines, NO individual white boxes
   - [ ] Section h2: `color: #1a5276`, `border-bottom: 2px solid #eaecee`
   - [ ] Metric boxes: `display: inline-block`, padding `6px 12px`, font-size `13px` — NOT grid cards
   - [ ] Tables: `thead th` background `#1a5276`, numeric cells use `class="num"`
   - [ ] Charts: embedded as `data:image/png;base64,{image_base64}` — NOT `image_url`
   - [ ] Footer: `<div class="footer">` with gray background `#f8f9fa`, padding `20px 75px`, OUTSIDE content-body
   - [ ] Disclaimer + powered-by inside footer div
   - [ ] No `box-shadow` (unsupported by weasyprint)
   Fix all issues found, then proceed to PDF conversion.
4. **Convert to PDF**: Programmatically convert the HTML file to PDF (using weasyprint or equivalent). Do NOT regenerate content — convert the exact HTML file as-is.
5. **Present PDF**: Show the PDF to the user as the final output. The user sees only the PDF.
6. **Edits**: If the user requests changes, edit the HTML source file and re-convert to PDF. Never regenerate from scratch.

The HTML is the source of truth. The PDF is the presentation layer.

## Error Handling

If company not found, suggest similar names. If financial data not yet in DB for latest report, check main_events first, then fall back to PDF URL from reporting_files. Always note data source used.
