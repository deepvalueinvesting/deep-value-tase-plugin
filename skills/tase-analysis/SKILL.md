# TASE Analysis Skill

Expert knowledge for analyzing Israeli public companies listed on the Tel Aviv Stock Exchange (TASE).

## TASE Market Structure

### Key Indices (Hebrew names as listed on TASE)
- **ת"א-35** (tase_index_id=142): Top 35 companies by market cap (blue chips)
- **ת"א-125** (137): Top 125 companies (broad market)
- **ת"א-90** (143): Companies ranked 36-125 (mid-cap)
- **ת"א-SME60** (147): Small and medium enterprises (60 companies)
- **ת"א-200** (199): Top 200 companies
- **ת"א All-Share** (168): All traded stocks
- Sector indices: ת"א-טכנולוגיה (169), ת"א-פיננסים (148), ת"א-נדל"ן (149), ת"א-ביומד (167), ת"א בנקים-5 (164), ת"א בטחוניות (207), ת"א-תעשייה (178), ת"א-בנייה (181), ת"א-נפט וגז (170), ת"א-רשתות שיווק (188)
- Dividend indices: תל-דיב (166), תל דיב אריסטוקרט (200)

### Trading Hours
- Monday through Thursday: 10:00-17:35
- Friday: 10:00-14:00
- No weekend trading (Saturday-Sunday)
- Source: https://www.tase.co.il/he/content/about/tradingdays_change

### Currency
Most financial data is in Israeli New Shekel (₪ / ILS), but some companies report in foreign currencies.
The tool returns a `currency` field per metric value — always check it before displaying data.
Examples of USD reporters: אלביט מערכות, נקסט ויז'ן. Other companies may report in EUR.
Format ILS as: ₪1,234,567 | USD as: $1,234,567 | EUR as: €1,234,567.
Large amounts: מיליוני ₪/$/€ (millions), מיליארדי ₪/$/€ (billions).

## Israeli IFRS Reporting

### Reporting Calendar
- Annual reports: Due within 3 months of fiscal year-end (typically March)
- Quarterly reports (Q1, Q2, Q3): Due within 2 months
- Q4 is included in the annual report
- Banks report separately under Bank of Israel guidelines

### Key IFRS Differences for Israeli Companies
- Real estate: Investment property often at fair value (IAS 40)
- Defense: Long-term contract revenue recognition (IFRS 15)
- Banks: Exempt from IFRS — follow Bank of Israel reporting requirements based on US GAAP
- Insurance: IFRS 17 for insurance contracts (effective January 2025, alongside IFRS 9)
- High-tech companies: May apply US GAAP (with IFRS reconciliation) if meeting specified revenue/ownership conditions
- Dual-listed companies: May file using US GAAP, UK-adopted IAS, or other recognized standards (e.g., SEC 20-F)

## Maya Regulatory System

Maya (מאי"ה) is the TASE electronic filing system. Key form types:
- **דוח כספי שנתי** — Annual financial report
- **דוח כספי רבעוני** — Quarterly financial report
- **דוח מיידי** — Immediate report (material events)
- **הצעת רכש** — Tender offer
- **עסקת בעלי עניין** — Related party transaction
- **חלוקת דיבידנד** — Dividend distribution
- **הנפקת מניות** — Share issuance
- **הודעה על אסיפה כללית** — General meeting notice

## Document Design

When generating HTML documents, follow the **document-design** reference (`references/document-design.md`) for the complete CSS design system, component library, and layout rules. Key principles:

- White card-based sections with subtle shadows
- Gradient header banner with title, subtitle, date
- Inline metric boxes for KPIs
- Colored badges for categorization in tables
- Charts centered with rounded corners
- Clean disclaimer (no borders, no callout boxes)
- Heebo font from Google Fonts
- `dir="rtl"` and `lang="he"` on `<html>`

## Hebrew Language & Formatting

For all Hebrew language, formatting, and terminology guidelines, see the **hebrew-quality** skill.
