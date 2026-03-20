---
name: monitor-watchlist
description: Generate a personalized Hebrew morning brief for watchlist companies with market snapshot, announcements, alerts, and valuation ranking
arguments: []
---

# Monitor Watchlist

You are generating a personalized Hebrew morning brief for a user's watchlist companies using DeepValue's financial data tools.

## Pre-Flight Questions

Before starting, ask the user:
1. Which sections to include? (default: all four — Market Snapshot, Announcements Digest, Attention Flags, Valuation Ranking)
2. Announcements lookback period? (default: 7D; options: 1D, 3D, 7D, 14D, 30D)
3. Specific watchlist? (default: all watchlists combined)

## Skill Context

Apply the **hebrew-quality** skill for Hebrew language quality, grammar, formatting, and financial terminology.
Apply the **tase-analysis** skill for IFRS conventions and TASE market structure.
Apply the **deep-value-methodology** skill for valuation frameworks, scoring principles, and red flags.

## Writing Style

The output must be written as a professional narrative for a fund manager analyst reviewing a morning brief. No emojis. No bullet points. Each paragraph focuses on one key idea. Sections flow naturally with transition words (מילות קישור). Tables are used for data presentation — all commentary must be flowing text.

## Workflow

### Step 1 — Fetch Watchlist

If the user requested a specific watchlist, use `get_watchlists` to find it, then `get_watchlist_companies` for that watchlist. Otherwise, use `get_watchlist_companies` without a watchlist filter to get all watched companies.

Store the company IDs as a comma-separated string. Count the total companies (N).

If the watchlist is empty, display a helpful Hebrew message explaining how to add companies to a watchlist, and stop.

### Step 2 — Gather Market Data (single call)

Call `screen_stocks` with all company IDs and the following fields:

```
fields: ["company_name", "security_name", "last_price", "market_cap", "pe_ratio", "pb_ratio", "ev_ebitda", "dividend_yield_ttm", "roe_ttm", "roa_ttm", "debt_to_equity", "current_ratio", "gross_margin_ttm", "operating_margin_ttm", "net_margin_ttm", "revenue_yoy_growth", "revenue_cagr_5y", "eps_yoy_growth", "price_change_52w", "high_52w", "low_52w", "all_time_high", "volume_avg_30d", "free_cash_flow_yield", "enterprise_value", "revenue_ttm", "net_income_ttm"]
output_format: "json"
```

This single call provides data for sections 1, 3, and 4.

### Step 3 — Gather Announcements (date-based, adaptive)

Convert the user's lookback period to dates:
- `from_date` = today minus the lookback period (e.g., 7 days)
- `to_date` = today

**If N <= 10 companies**: Execute parallel `get_company_maya_announcements` calls per company, each with `from_date` and `to_date` parameters.

**If N > 10 companies**: Execute a single `search_maya_announcements(company_ids=..., from_date=..., to_date=..., page_size=200)` call. If the response indicates more pages exist (total > page_size), fetch subsequent pages until all announcements are retrieved.

**Always (regardless of N)**: Make one additional `search_maya_announcements` call with `is_priority=true` for the last 30 days across all company IDs. This catches material announcements even outside the user's lookback window.

**Company name resolution**: The `search_maya_announcements` cross-company endpoint may not return `company_name` in each result. To resolve this, build a company_id → company_name lookup from the `screen_stocks` data (Step 2) and join it onto announcement results before rendering.

### Step 4 — Section 1: Market Snapshot (תמונת שוק)

Aggregate metrics across the watchlist:
- Total market cap of all watched companies
- Median P/E ratio
- Average dividend yield
- Count of companies near 52-week high (within 5%)
- Count of companies near 52-week low (within 5%)

Present as inline metric boxes (`<span class="metric-box">`).

Then render a table with **ALL** watchlist companies, sorted by market cap descending. Do not truncate — the user expects to see every company in their watchlist:

| חברה | מחיר | % משיא 52 שבועות | שווי שוק | מכפיל רווח | תשואת דיבידנד |
|------|------|-------------------|----------|------------|---------------|

For P/E, if earnings are negative show "הפסד" (not "שלילי" or a negative P/E number).

### Step 5 — Section 2: Announcements Digest (תקציר דיווחים)

**Table layout note:** The date column (תאריך) must display full dd/MM/YYYY dates without wrapping. Use `style="width: 15%; white-space: nowrap"` on the date `<th>` and `<td>` cells, and reduce the subject column (נושא) width accordingly.

From the announcements gathered in Step 3:

1. **Deduplicate** announcements by ID across all searches. Also merge corrections/amendments of the same event (e.g., multiple dividend corrections for the same company) into a single entry with the latest values.
2. **Categorize** each announcement into one of: דיבידנד, דוח כספי, עסקה/הסכם, פעילות בעלי עניין, שינוי בהנהלה, אסיפה, אחר.
3. **Score materiality** on a 1-5 scale:
   - 5: M&A, going concern, major contract (>5% of revenue), material lawsuit
   - 4: Dividend declaration, quarterly/annual report, significant insider transaction
   - 3: Board/management changes, medium contracts, regulatory filing
   - 2: Meeting notices, routine updates, dividend corrections
   - 1: Administrative filings, minor corrections, procedural notices

Render a table:

| חברה | תאריך | נושא | קטגוריה | מהותיות |
|------|-------|------|---------|---------|

Use badge classes for categories:
- `badge-new` (green) for דיבידנד and דוח כספי
- `badge-expansion` (orange) for עסקה/הסכם
- `badge-framework` (blue) for פעילות בעלי עניין
- `badge-intl` (purple) for אסיפה and אחר

Show the top 40 announcements by materiality score. If more exist, add a note: "מוצגים 40 הדיווחים המהותיים ביותר מתוך [total]".

After the table, write a 2-3 sentence narrative summary highlighting the most material announcements and their potential implications.

### Step 6 — Section 3: Flags (דגלים)

Scan the screen_stocks data for each company and generate flags. Companies with no flags are omitted from this section entirely.

**Price alerts:**
- Near 52-week high (within 5%): "קרוב לשיא 52 שבועות"
- Near 52-week low (within 5%): "קרוב לשפל 52 שבועות"
- Near all-time high (within 5%): "קרוב לשיא כל הזמנים"

**Valuation alerts:**
- P/E > 30 or P/E < 5 (extreme): "מכפיל רווח קיצוני"
- P/B < 1.0 (below book): "נסחרת מתחת להון העצמי"
- EV/EBITDA > 20: "מכפיל EV/EBITDA גבוה"
- Dividend yield > 8% (sustainability concern): "תשואת דיבידנד גבוהה במיוחד — בדוק קיימות"

**Health alerts:**
- D/E > 2.0: "מינוף גבוה" — but use D/E > 4.0 for real estate companies and skip this flag entirely for banks/insurance (leverage is structural for financials)
- Current ratio < 1.0: "יחס שוטף נמוך" — skip for banks, insurance, and financial services (they structurally operate with low current ratios)
- Revenue YoY growth < -10%: "ירידה בהכנסות" — detail shows the growth rate (e.g., -23%)
- Net margin declining YoY (current < previous): "ירידה ברווחיות" — detail shows the margin (e.g., -0.1%)
- Revenue YoY growth > 0% but net margin declining YoY: "צמיחה ללא רווחיות" — detail shows revenue growth rate

**Momentum alerts:**
- Revenue YoY growth > 20%: "צמיחה חזקה בהכנסות"
- EPS YoY growth > 30%: "צמיחה חזקה ברווח למניה"

Render a table:

| חברה | דגל | סוג | פירוט |
|------|-----|-----|-------|

Use badge classes: `badge-new` (green) for momentum, `badge-expansion` (orange) for valuation, `badge-framework` (blue) for price, `badge-intl` (purple) for health.

Sort the table by flag severity: health flags first, then valuation, then price, then momentum. Within each category, sort alphabetically by company name.

If there are more than 60 flags, show the top 60 and add a note above the table: "מוצגים 60 הדגלים המהותיים ביותר מתוך [total]".

Follow with a narrative paragraph summarizing the most significant flags and what they imply for portfolio attention.

### Step 7 — Section 4: Valuation Ranking (דירוג אטרקטיביות)

Calculate a simplified quantitative deep-value score (1-10) for each company using only the screen_stocks data from Step 2.

**Four dimensions with defined weights:**

**Financial Strength (25%):** ROE, D/E, Current Ratio, Operating Margin
**Growth (20%):** Revenue YoY, Revenue CAGR 5Y, EPS YoY
**Profitability (25%):** Gross Margin, Net Margin, ROA
**Valuation (30%):** P/E, P/B, Dividend Yield, EV/EBITDA

**Metric scoring (each metric maps to 1-10):**

| Metric | 1-2 (Poor) | 3-4 (Below Avg) | 5-6 (Average) | 7-8 (Good) | 9-10 (Excellent) |
|--------|-----------|-----------------|---------------|------------|------------------|
| ROE | < 0% | 0-5% | 5-10% | 10-18% | > 18% |
| D/E | > 3.0 | 2.0-3.0 | 1.0-2.0 | 0.3-1.0 | < 0.3 |
| Current Ratio | < 0.5 | 0.5-1.0 | 1.0-1.5 | 1.5-2.5 | > 2.5 |
| Operating Margin | < 0% | 0-5% | 5-10% | 10-20% | > 20% |
| Revenue YoY | < -10% | -10-0% | 0-5% | 5-15% | > 15% |
| Revenue CAGR 5Y | < -5% | -5-0% | 0-5% | 5-10% | > 10% |
| EPS YoY | < -20% | -20-0% | 0-10% | 10-25% | > 25% |
| Gross Margin | < 15% | 15-25% | 25-35% | 35-50% | > 50% |
| Net Margin | < 0% | 0-3% | 3-7% | 7-15% | > 15% |
| ROA | < 0% | 0-2% | 2-5% | 5-10% | > 10% |
| P/E (inverted) | > 35 | 25-35 | 15-25 | 8-15 | < 8 |
| P/B (inverted) | > 5.0 | 3.0-5.0 | 1.5-3.0 | 0.8-1.5 | < 0.8 |
| Dividend Yield | 0% | 0-1% | 1-3% | 3-5% | > 5% |
| EV/EBITDA (inverted) | > 20 | 15-20 | 10-15 | 6-10 | < 6 |

**Null handling:** If a metric is null/unavailable, exclude it from the dimension average. Do not penalize for missing data. If all metrics in a dimension are null, exclude that dimension and redistribute its weight proportionally across remaining dimensions. **A dimension score must NEVER be 0 due to missing data** — 0 means exclusion, not a score. If a company has fewer than 2 dimensions with data, exclude it from the ranking entirely and note it separately as "אין מספיק נתונים לדירוג".

**Composite score:** Weighted average of the scored dimensions (after redistribution if needed).

Render a ranked table sorted by composite score descending:

| דירוג | חברה | חוסן פיננסי | צמיחה | רווחיות | תמחור | ציון משוקלל |
|-------|------|-------------|-------|---------|-------|------------|

Color-code scores in the table:
- 8-10: green text (`color: #1e8449`)
- 6-7.9: blue text (`color: #2471a3`)
- 4-5.9: orange text (`color: #b9770e`)
- Below 4: red text (`color: #c0392b`)

Follow with a narrative paragraph explaining the ranking, highlighting the top-ranked companies and what drives their scores.

**Cross-section synthesis:** Where relevant, connect insights across sections. For example, if a top-ranked company also has a material announcement this week, or if a company near 52-week lows also has deteriorating fundamentals, call this out explicitly. Flag potential value traps — companies that appear cheap (high valuation score) but have health flags or declining revenue.

### Step 8 — Generate 2 Charts

Generate 2 branded charts using `generate_chart`:

1. **Valuation Map** (grouped_bar): P/E and EV/EBITDA per company. Cap at 12 companies (use the top 12 by market cap if N > 12). Use `image_name: "watchlist_valuation_map"`. Hebrew title: "מפת תמחור — מכפיל רווח ו-EV/EBITDA".

2. **Score Ranking** (bar): Composite deep-value scores sorted descending. Use `image_name: "watchlist_score_ranking"`. Hebrew title: "דירוג אטרקטיביות — ציון משוקלל".

Embed each chart in the HTML using: `<img src="data:image/png;base64,{image_base64}" alt="[Hebrew chart title]">` with the `image_base64` field from `generate_chart`. Always include `alt` text matching the chart title. **Never use `image_url`** — it is blocked in sandbox environments. **Never regenerate charts with matplotlib** — always use `image_base64` from `generate_chart`.

### Step 9 — Assemble HTML Brief

Build the complete HTML document. **Read `skills/tase-analysis/references/document-design.md` and copy its exact CSS and HTML structure** — do not invent your own design. The reference contains the complete working CSS that must be used verbatim (Heebo font, `#1a5276` color scheme, inline `<span class="metric-box">` for metrics, `.section` cards, `.badge-*` classes, `.chart-container`, `.disclaimer`).

**Document structure:**

```
Header (gradient banner)
  → Title: "תקציר בוקר — רשימת מעקב"
  → Subtitle: count of companies, date range
  → Date: today's date

Executive Summary (1 section)
  → 3-4 sentence overview: market snapshot highlights, notable announcements, key flags, top-ranked companies
  → End with 1 actionable sentence: which companies require immediate attention and why (e.g., "שלוש חברות דורשות תשומת לב: [company] בשל [reason]")

Section 1: תמונת שוק (if requested)
  → Metric boxes + market data table
  → Embed valuation map chart here (breaks up table density with a visual)

Section 2: תקציר דיווחים (if requested)
  → Announcements table + narrative

Section 3: דגלים (if requested)
  → Flags table + narrative

Section 4: דירוג אטרקטיביות (if requested)
  → Ranking table + narrative
  → Embed score ranking chart

Disclaimer
  → "ניתוח זה מבוסס על נתוני שוק ודיווחים רגולטוריים בלבד ואינו מהווה ייעוץ השקעות. יש לבצע בדיקה עצמאית לפני קבלת החלטות השקעה."

Footer
  → "Powered by DeepValue | נתונים עד [date]"
```

**HTML requirements:**
- `dir="rtl"`, `lang="he"`
- Use the exact CSS from `skills/tase-analysis/references/document-design.md`
- Metric boxes as inline `<span class="metric-box">` (never CSS grid cards)
- Tables with `<thead>` dark blue background and badge-categorized rows
- Charts as base64 `<img>` inside `<div class="chart-container">`
- Disclaimer as plain centered gray text (no borders, no colored boxes)

## Output Format

The output pipeline is: **HTML (source) → PDF (presentation)**.

1. **Build HTML**: Create a complete HTML file following the structure above. Save this HTML file — it is the editable source document.
2. **Convert to PDF**: Programmatically convert the HTML file to PDF (using weasyprint or equivalent). Do NOT regenerate content — convert the exact HTML file as-is.
3. **Present PDF**: Show the PDF to the user as the final output. The user sees only the PDF.
4. **Edits**: If the user requests changes, edit the HTML source file and re-convert to PDF. Never regenerate from scratch.

The HTML is the source of truth. The PDF is the presentation layer.

## Efficiency Budget

- N <= 10 companies: ~N+5 tool calls (parallel per-company announcements + screen_stocks + watchlist + priority search + 2 charts)
- N > 10 companies: ~6 tool calls total (screen_stocks + 2 announcement searches + watchlist + 2 charts)

## Error Handling

If watchlist is empty, show a helpful Hebrew message and stop. If screen_stocks returns partial data, analyze what's available and note gaps. If announcements return empty, note "לא נמצאו דיווחים בתקופה המבוקשת" and skip Section 2. If chart generation fails, continue with text-only output. If a company has no data at all in screen_stocks, exclude it from rankings and note it.
