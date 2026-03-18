---
name: analyze-orders
description: Analyze order flow patterns, compare to prior periods, and assess revenue impact for Israeli public companies
arguments:
  - name: company_name
    description: Company name in Hebrew or English
    required: true
---

# Analyze Orders

You are analyzing order flow patterns and their revenue implications for an Israeli public company using DeepValue's financial data tools.

## Pre-Flight Questions

Before starting, ask the user:
1. What comparison period? (default: YTD vs same period last year and full prior year)
2. Any specific order types of interest? (e.g., satellite, defense, framework agreements, specific clients)

## Skill Context

Apply the **tase-analysis** skill for Maya form types and Hebrew formatting.

## Writing Style

The output must be written as a professional narrative for a fund manager analyst. No emojis. No bullet points. Each paragraph focuses on one key idea. Sections flow naturally with transition words. The goal is a cohesive analysis that reads as a single document, not a list of items. Tables are permitted only for the order timeline mapping — all commentary must be flowing text.

## Workflow

### Step 1 — Identify the Company

Search for the company using `search_companies` (always search in Hebrew). Confirm the company name and sector before proceeding.

### Step 2 — Establish the Timeline

Determine the analysis periods based on the user's request (default: YTD current year). Always establish a comparison baseline:
- **Primary period**: The user's requested period (e.g., YTD 2026)
- **Same period prior year**: Same calendar window one year earlier (e.g., Jan 1 – Mar 16, 2025)
- **Full prior year**: Complete prior calendar year (e.g., full 2025)

### Step 3 — Gather Order Data (run in parallel)

Execute all of the following in parallel:

1. **YTD Maya announcements**: `search_maya_announcements` for the company, from Jan 1 of current year to today. Fetch all (page_size=100).
2. **Order-specific Maya search (current period)**: `search_maya_announcements` with terms="הזמנה,הזמנות,חוזה,הסכם,התקשרות,פרויקט" operator="OR", filtered to company and current period.
3. **Order-specific Maya search (same period last year)**: Same search terms, filtered to same calendar window one year prior.
4. **Order-specific Maya search (full prior year)**: Same search terms, filtered to full prior year.
5. **Order backlog**: `get_order_backlog` for the company (no date filter — returns all available periods). This gives the reported backlog from financial statements, including firm vs. framework orders, segment breakdown, and revenue conversion timeline.
6. **Income statements (quarterly)**: `get_income_statements` with report_type="quarterly" for revenue trend data.
7. **Market context**: `screen_stocks` with company_ids for current valuation, revenue growth rates (YoY, QoQ, CAGR 5Y), and margins.

### Step 4 — Build the Order Timeline

From the Maya announcements, extract every order-related announcement. For each order, capture:
- Date
- Description (client type: Israeli/international, new/expansion/framework)
- Amount in NIS
- Duration if stated
- Whether it's an expansion of a previously announced order (watch for "המשך", "הרחבה", "הגדלה", "עודכן לסך כולל")

**Critical**: When orders reference prior announcements (e.g., "המשך מ-5.1.26" or "סכום ההזמנה גדל ועודכן לסך כולל"), identify the chain and avoid double-counting. Present both the incremental additions and the cumulative total clearly.

Present the timeline as a table with columns: Date | Description | Amount (NIS M) | Notes (new/expansion/cumulative).

### Step 5 — Quantify and Compare

Calculate:
- **Total announced orders** for the current period (with and without overlapping expansions)
- **Total announced orders** for same period last year
- **Total announced orders** for full prior year
- **Ratio**: Current period vs. same period last year, and vs. full prior year
- **Order frequency**: Average days between order announcements in each period
- **Average order size**: Mean and median per period
- **Largest single order** in each period

### Step 6 — Revenue Impact Assessment

Using income statement data:
- Calculate TTM revenue (sum of last 4 reported quarters)
- Calculate last full-year annual revenue
- Express total announced orders as a **percentage of TTM revenue** and **percentage of annual revenue**
- If order backlog data is available, show how backlog has evolved over recent quarters and what percentage of it converts to revenue within 12 months
- Note the revenue recognition timeline: are these orders for immediate delivery or spread over months/years?

### Step 7 — Investigate Causes (DATA-DRIVEN ONLY)

**CRITICAL: Only include this section if supporting data is found. Never speculate or hallucinate reasons.**

Search for context behind the order patterns using:
1. `search_main_events` with terms like "הזמנות,צבר,backlog" to find management commentary from financial reports
2. `search_main_events` with terms like "ביקוש,לקוחות,שוק" for demand/market context
3. `search_maya_announcements` for strategic agreements or partnerships that may explain order acceleration (e.g., infrastructure deals, new contracts)
4. The general YTD announcements from Step 3 may contain relevant non-order announcements (strategic agreements, new markets, regulatory changes)

For each cause identified, cite the specific source (report period, announcement date, or main event). If no clear causes are found in the data, state explicitly: "לא נמצא מידע מפורש בדיווחי החברה המסביר את השינוי בקצב ההזמנות" — and move on.

Potential cause categories to search for (only if data supports):
- Geopolitical/security environment driving demand
- New strategic partnerships or infrastructure agreements
- Expansion into new markets or client segments
- Regulatory changes creating demand
- Competitor exits or market consolidation
- Seasonal patterns visible in multi-year data

### Step 8 — Generate Charts

Generate 3 branded charts using `generate_chart`:

1. **YTD orders comparison** (grouped_bar): Total announced orders in current YTD vs same period last year. Labels: ["YTD נוכחי", "YTD אשתקד"], series with the sum values.
2. **Order backlog trend** (line): If backlog data is available, show backlog over recent quarters. If no backlog data, substitute with a bar chart of orders by quarter.
3. **Quarterly revenue trend** (bar): Revenue over the last 20 quarters — for cross-referencing with order pace.

Use `image_name` in English only (e.g., "ytd_orders_comparison", "order_backlog_trend", "quarterly_revenue_trend").

Embed each chart in the HTML document using: `<img src="data:image/png;base64,{image_base64}">` with the `image_base64` field from `generate_chart`. Never use `image_url` — it is blocked in sandbox environments.

### Step 9 — Write the Analysis

Structure the output as follows:

**תמונת שוק נוכחית** — One paragraph with current price, market cap, key multiples (P/E, P/S, EV/EBITDA), and revenue growth rate.

**מיפוי הזמנות — [תקופה נוכחית]** — The order timeline table for the current period, followed by a narrative summary of total orders and notable patterns.

**השוואה ל[תקופה קודמת]** — The comparison period order table, followed by quantitative comparison (ratios, frequency, average size). This is the core analytical insight — make the comparison sharp and specific. Embed the YTD orders comparison chart here using `image_base64` from `generate_chart`.

**ניתוח צבר הזמנות** — If order backlog data is available from financial reports, analyze the backlog trend, composition (firm vs. framework), and conversion rate. Embed the backlog/orders trend chart here using `image_base64` from `generate_chart`. If not available, skip this section silently.

**השלכה על ההכנסות** — Revenue impact analysis: orders as percentage of TTM/annual revenue, implied revenue growth, margin implications. Include the revenue recognition timeline if discernible from the order descriptions. Embed the quarterly revenue chart here using `image_base64` from `generate_chart`.

**גורמים מניעים** — Only if Step 7 found data-backed causes. Otherwise omit this section entirely.

**הערכת מהותיות** — Closing assessment: is the order acceleration/deceleration a structural shift or noise? What does it imply for forward estimates? How does current valuation reflect (or not reflect) the order trend?

**Disclaimer** — Brief note that analysis is based on regulatory filings only and does not constitute investment advice.

## Output Format

The output pipeline is: **HTML (source) → PDF (presentation)**.

1. **Build HTML**: Create a complete HTML file with RTL direction (`dir="rtl"`, `lang="he"`), professional CSS styling (fonts, spacing, tables, colors), and charts embedded as base64: `<img src="data:image/png;base64,{image_base64}" />` using the `image_base64` field from `generate_chart`. **Never use `image_url` in the HTML** — external URLs are blocked in sandbox. **Never regenerate charts with matplotlib** — always use `image_base64` from `generate_chart`. Save this HTML file — it is the editable source document.
2. **Convert to PDF**: Programmatically convert the HTML file to PDF (using weasyprint or equivalent). Do NOT regenerate content — convert the exact HTML file as-is.
3. **Present PDF**: Show the PDF to the user as the final output. The user sees only the PDF.
4. **Edits**: If the user requests changes, edit the HTML source file and re-convert to PDF. Never regenerate from scratch.

The HTML is the source of truth. The PDF is the presentation layer.

## Error Handling

If company not found, suggest similar names. If no order announcements found, check whether the company typically reports orders (not all sectors do) and note accordingly. If order backlog tool returns empty, the company may not report backlog — skip that section silently. If income statement data is unavailable for recent quarters, use the most recent available and note the data lag.
