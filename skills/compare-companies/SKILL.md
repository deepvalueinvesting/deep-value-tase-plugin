---
name: compare-companies
description: Side-by-side comparison of two Israeli public companies with scoring
arguments:
  - name: company1
    description: First company name (Hebrew or English)
    required: true
  - name: company2
    description: Second company name (Hebrew or English)
    required: true
---

# Compare Companies

You are performing a side-by-side comparison of two Israeli public companies using DeepValue's financial data tools.

## Pre-Flight Questions

Before starting, ask the user:
1. What's the comparison purpose? (investment decision, sector analysis, academic)
2. Any specific metrics to emphasize?

## Skill Context

Apply the **hebrew-quality** skill for Hebrew language quality, grammar, formatting, and financial terminology.
Apply the **tase-analysis** skill for IFRS conventions and TASE market structure.
Apply the **deep-value-methodology** skill for the scoring framework and valuation benchmarks.

## Writing Style

The output must be written as a professional narrative for a fund manager analyst. No emojis. No bullet points. Each paragraph focuses on one key idea. Sections connected with transition words. The comparison should flow as a cohesive argument, not a mechanical side-by-side listing.

## Workflow

Use the MCP prompt `compare_companies` with both company names to execute the comparison workflow.

The workflow will:
1. Search and identify both companies
2. Get current ratios via screen_stocks for both (100+ metrics each)
3. Get 20 quarters of income statements for both
4. Get TTM historical ratios for 10 years for both (best source for valuation multiples)
5. Generate 4 branded charts (revenue comparison, margin trends, valuation multiples, returns)
6. Write the narrative comparative analysis with scoring in Hebrew

## Output Format

The output pipeline is: **HTML (source) → PDF (presentation)**.

1. **Build HTML**: Create a complete HTML file with RTL direction (`dir="rtl"`, `lang="he"`), professional CSS styling (fonts, spacing, tables, colors), and charts embedded as base64: `<img src="data:image/png;base64,{image_base64}" />` using the `image_base64` field from `generate_chart`. **Never use `image_url` in the HTML** — external URLs are blocked in sandbox. **Never regenerate charts with matplotlib** — always use `image_base64` from `generate_chart`. Save this HTML file — it is the editable source document.
2. **Convert to PDF**: Programmatically convert the HTML file to PDF (using weasyprint or equivalent). Do NOT regenerate content — convert the exact HTML file as-is.
3. **Present PDF**: Show the PDF to the user as the final output. The user sees only the PDF.
4. **Edits**: If the user requests changes, edit the HTML source file and re-convert to PDF. Never regenerate from scratch.

The HTML is the source of truth. The PDF is the presentation layer.

## Error Handling

If a company not found, suggest similar names. If companies are in very different sectors, note the caveat but proceed. If one company has significantly less data, note the limitation.
