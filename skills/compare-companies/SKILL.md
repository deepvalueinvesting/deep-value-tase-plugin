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

1. **Build HTML**: Before writing any HTML, read **skills/tase-analysis/references/document-design.md** and copy the exact CSS from its reference example. Also read **skills/tase-analysis/references/heebo-font.css** and paste its `@font-face` rules into the `<style>` block (this embeds the Heebo font as base64 — do NOT use `@import url('https://fonts.googleapis.com/...')`). Then create the HTML file with RTL direction (`dir="rtl"`, `lang="he"`), using the copied CSS, and charts embedded as base64: `<img src="data:image/png;base64,{image_base64}" />` using the `image_base64` field from `generate_chart`. **Never use `image_url` in the HTML** — external URLs are blocked in sandbox. **Never regenerate charts with matplotlib** — always use `image_base64` from `generate_chart`. Save this HTML file — it is the editable source document.
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

If a company not found, suggest similar names. If companies are in very different sectors, note the caveat but proceed. If one company has significantly less data, note the limitation.
