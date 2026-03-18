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
- Pre-opening: 09:00-09:45 (random opening phase)
- Continuous trading: 09:45-17:14
- Closing auction: 17:14-17:25 (random close)
- Post-market: 17:25-17:30
- Sunday through Thursday (no weekend trading)

### Currency
All financial data is in Israeli New Shekel (₪ / ILS).
Format numbers as: ₪1,234,567 (thousands separator: comma).
Large amounts: מיליוני ₪ (millions), מיליארדי ₪ (billions).

## Israeli IFRS Reporting

### Reporting Calendar
- Annual reports: Due within 3 months of fiscal year-end (typically March)
- Quarterly reports (Q1, Q2, Q3): Due within 2 months
- Q4 is included in the annual report
- Banks report separately under Bank of Israel guidelines

### Key IFRS Differences for Israeli Companies
- Real estate: Investment property often at fair value (IAS 40)
- Defense: Long-term contract revenue recognition (IFRS 15)
- Banks: Expected credit loss model (IFRS 9)
- Insurance: IFRS 17 for insurance contracts
- Dual-listed companies may also file with SEC (20-F)

### Financial Statement Structure (Hebrew Terms)
| Hebrew | English |
|--------|---------|
| דוח רווח והפסד | Income Statement |
| מאזן | Balance Sheet |
| דוח תזרים מזומנים | Cash Flow Statement |
| הכנסות | Revenue |
| רווח גולמי | Gross Profit |
| רווח תפעולי | Operating Profit |
| EBITDA | EBITDA |
| רווח נקי | Net Profit |
| נכסים שוטפים | Current Assets |
| נכסים לא שוטפים | Non-Current Assets |
| התחייבויות שוטפות | Current Liabilities |
| הון עצמי | Shareholders' Equity |
| תזרים מפעילות שוטפת | Operating Cash Flow |
| תזרים מפעילות השקעה | Investing Cash Flow |
| תזרים מפעילות מימון | Financing Cash Flow |
| רווח למניה | Earnings Per Share (EPS) |
| דיבידנד | Dividend |

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

## Hebrew Formatting Rules

### Text Direction
- All analysis output must be in Hebrew (RTL)
- Numbers remain LTR within Hebrew text
- Percentages: 15.2% (not %15.2)
- Currency: ₪1,234 (shekel sign before number)

### Professional Tone
- Use formal Hebrew (שפה עילית)
- Financial terminology should match ISA/TASE conventions
- Dates: DD.MM.YYYY or DD בחודש YYYY
- Quarters: Q1, Q2, Q3, Q4 (English abbreviations are standard)

### Analysis Structure
Hebrew financial analysis follows this standard order:
1. תקציר מנהלים (Executive Summary)
2. סקירת החברה (Company Overview)
3. ניתוח פיננסי (Financial Analysis)
4. מיצוב תחרותי (Competitive Position)
5. תמחור ושווי (Valuation)
6. סיכונים (Risks)
7. סיכום והמלצה (Summary & Recommendation)
