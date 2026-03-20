---
name: analyze-announcements
description: Analyze a company's Maya (TASE) regulatory announcements with materiality scoring
arguments:
  - name: company_name
    description: Company name in Hebrew or English
    required: true
---

# Analyze Announcements

You are analyzing regulatory announcements from the Maya (TASE) filing system using DeepValue's financial data tools.

## Pre-Flight Questions

Before starting, ask the user:
1. Any specific time period? (default: last 30 announcements)
2. Any specific topics of interest? (e.g., dividends, M&A, personnel changes)

## Skill Context

Apply the **hebrew-quality** skill for Hebrew language quality, grammar, formatting, and financial terminology.
Apply the **tase-analysis** skill for Maya form types and TASE market structure.

## Writing Style

The output must be written as a professional narrative for a fund manager analyst. No emojis. No bullet points. Each paragraph focuses on one key idea. Sections flow naturally with transition words. The goal is a cohesive analysis that reads as a single document, not a list of items.

## Workflow

Use the MCP prompt `analyze_announcements` with the company name to execute the analysis workflow.

The workflow will:
1. Search and identify the company
2. Fetch recent Maya announcements (30 most recent)
3. Get form type reference data for categorization
4. Search for topic-specific announcements if relevant
5. Get current market data via screen_stocks for context
6. Write categorized narrative analysis with materiality assessment

## Output Format

The output pipeline is: **HTML (source) → PDF (presentation)**.

1. **Build HTML**: Create a complete HTML file with RTL direction (`dir="rtl"`, `lang="he"`), professional CSS styling (fonts, spacing, tables, colors). Save this HTML file — it is the editable source document.
2. **Convert to PDF**: Programmatically convert the HTML file to PDF (using weasyprint or equivalent). Do NOT regenerate content — convert the exact HTML file as-is.
3. **Present PDF**: Show the PDF to the user as the final output. The user sees only the PDF.
4. **Edits**: If the user requests changes, edit the HTML source file and re-convert to PDF. Never regenerate from scratch.

The HTML is the source of truth. The PDF is the presentation layer.

## Error Handling

If company not found, suggest similar names. If no announcements found, note the absence and check if company recently listed.
