---
name: hebrew-quality
description: Hebrew language quality, grammar, formatting, financial terminology, and self-review checklist for all Hebrew output
---

# Hebrew Quality Skill

Single source of truth for all Hebrew output — formatting, grammar, vocabulary, and quality. Every skill that produces Hebrew text must apply this skill.

## Hebrew Formatting Rules

### Text Direction
- All analysis output must be in Hebrew (RTL)
- Numbers remain LTR within Hebrew text
- Percentages: 15.2% (not %15.2)
- Currency: ₪1,234 (shekel sign before number)

### Currency Formatting
- Currency symbol ALWAYS goes to the LEFT of the number: ₪1,234,567 (NOT 1,234,567₪)
- This applies in both Hebrew and English text, in tables, metric boxes, and narratives
- Thousands: אלפי ₪
- Millions: מיליוני ₪ or מלש"ח (מיליון ש"ח)
- Billions: מיליארדי ₪ or מלרד"ש (מיליארד ש"ח)
- **Stock prices from TASE are in agorot (cents), not shekels.** Do NOT add ₪ symbol to stock price columns.

**CRITICAL — Currency + large amounts**: Never mix the ₪ symbol with Hebrew words like "מיליון". Choose ONE of these formats:
- **Compact (for metric boxes / tables)**: `₪4.8M` — symbol + number + English abbreviation
- **Hebrew narrative**: `4.8 מיליון ש"ח` — number + Hebrew "מיליון" + ש"ח
- **WRONG**: ~~₪4.8 מיליון~~ — this hybrid mixes the symbol with Hebrew words. Never use it.

### Percentages
- Always with % sign: 15.2%, -3.5%
- Basis points: נקודות בסיס (bps)
- Change notation: עלייה של 2.3%, ירידה של 1.1%

### Signed Numbers (Negative AND Positive)
- The minus/plus sign MUST always appear to the LEFT of the number, even inside Hebrew sentences.
- In RTL context, wrap ANY signed number (negative or positive) in `<span dir="ltr">` to force correct rendering.
- This applies to metric boxes, inline narrative text, and any other RTL context.
- Example: `הכנסות <span dir="ltr">-27%</span>, צמיחה של <span dir="ltr">+52%</span>`
- **Never use the word "שלילי" after a number** to indicate it's negative — just use the minus sign: `תזרים חופשי של <span dir="ltr">-43</span> מיליון ש"ח` (NOT ~~₪43 מיליון שלילי~~)
- **Never write "מינוס X"** — just write the number with a minus sign: `<span dir="ltr">-4.2</span>` (NOT ~~מינוס 4.2~~)

### Dates
- Date format is ALWAYS dd/MM/YYYY (e.g., 20/03/2026). Never use MM-DD, YYYY-MM-DD, or other formats.
- Quarters: Q1, Q2, Q3, Q4 (English abbreviations are standard in Israeli finance)

### Professional Tone
- Use formal Hebrew (שפה עילית)
- Financial terminology should match ISA/TASE conventions
- No emojis in financial documents

### Financial Ratios (Hebrew Terms)
| English | Hebrew |
|---------|--------|
| P/E | מכפיל רווח |
| P/B | מכפיל הון |
| EV/EBITDA | מכפיל EV/EBITDA |
| ROE | תשואה להון |
| ROA | תשואה לנכסים |
| Current Ratio | יחס שוטף |
| Quick Ratio | יחס מהיר |
| D/E | יחס חוב להון |
| Dividend Yield | תשואת דיבידנד |

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

### Analysis Structure
Hebrew financial analysis follows this standard order:
1. תקציר מנהלים (Executive Summary)
2. סקירת החברה (Company Overview)
3. ניתוח פיננסי (Financial Analysis)
4. מיצוב תחרותי (Competitive Position)
5. תמחור ושווי (Valuation)
6. סיכונים (Risks)
7. סיכום והמלצה (Summary & Recommendation)

### Table Formatting
Use Hebrew headers in tables:
```
| מדד | חברה א' | חברה ב' | מנצחת |
```

### Chart Titles (Hebrew)
- Revenue: מגמת הכנסות רבעונית
- Margins: שולי רווח לאורך זמן
- Balance Sheet: הרכב מאזן
- Valuation: מכפילי תמחור — השוואה
- Comparison: השוואת הכנסות — [חברה 1] מול [חברה 2]

### Disclaimer (Standard)
Always end financial documents with:
```
ניתוח זה אינו מהווה ייעוץ השקעות. יש לבצע בדיקה עצמאית לפני קבלת החלטות השקעה.
```

## Core Language Principles

1. **Write natural Hebrew, not translated English.** Read your sentence aloud — if it sounds like Google Translate, rewrite it.
2. **If unsure about a word — use a simpler synonym.** Never invent words. If you cannot recall the correct Hebrew term, use a descriptive phrase instead.
3. **Match verb gender and number to the subject.** Hebrew verbs must agree with their subject in gender (זכר/נקבה) and number (יחיד/רבים).
4. **Avoid vague pronouns.** Do not use "זו", "זה", "אלה" when the referent is ambiguous. Repeat the actual noun.
5. **Use established financial terminology.** Consult the `references/financial-glossary.md` for authoritative Hebrew financial terms. Do not improvise translations of English financial concepts.
6. **Keep sentences short and direct.** Long, nested sentences break more easily in Hebrew. If a sentence has more than one comma, consider splitting it.

## Anti-Pattern Rules

These are the most common LLM failures when generating Hebrew financial text. Each rule includes wrong→right examples. For a comprehensive list, see `references/common-mistakes.md`.

### Rule 1: No Gibberish Sentences
LLMs sometimes produce syntactically broken Hebrew that is meaningless. Every sentence must be readable and make sense.
- ❌ "החברה נייר ערך ייעודי בתאריך של 2013"
- ✅ "החברה הונפקה בבורסת תל אביב בשנת 2013"

### Rule 2: Use Hebrew Terms, Not English
When a Hebrew equivalent exists, always use it.
- ❌ "ה-All-Time-High של המניה" → ✅ "שיא כל הזמנים של המניה"
- ❌ "going concern" → ✅ "עסק חי"
- ❌ "R&D" → ✅ "מו"פ (מחקר ופיתוח)"

### Rule 3: No Non-Existent Words
Do not invent Hebrew words or use corrupted forms.
- ❌ "בגבי" → ✅ "לגבי" or "בנוגע ל"
- ❌ "הולוקציה" → ✅ "הקצאה"
- ❌ "שוליים כוללים" (for gross margin) → ✅ "שיעור רווח גולמי"
- ❌ "רבעונות" → ✅ "רבעונים" (quarters — masculine plural)

### Rule 4: No Awkward Calques
Do not translate English expressions word-for-word into Hebrew.
- ❌ "קודם-הכנסות" → ✅ "טרום-הכנסות" or "לחברה אין עדיין הכנסות ממשיות"
- ❌ "מנקודת מבט של" (overused) → ✅ "מבחינת" or "לפי גישת"
- ❌ "משמע כי" → ✅ "כלומר" or "דהיינו"

### Rule 5: Correct Verb Conjugation
Match gender and number. Avoid vague pronouns.
- ❌ "היתה תזרים" → ✅ "היה תזרים" (תזרים is masculine)
- ❌ "ירדה זו" → ✅ "ירד הרווח" (use the actual noun)
- ❌ "הפכה זו לחיובית" → ✅ "ההון הפך לחיובי"

### Rule 6: Correct Financial Terminology
Use the precise Hebrew financial term, not a literal translation.
- ❌ "נייר ערך" (when meaning stock) → ✅ "מניה"
- ❌ "שוליים" (for margins) → ✅ "שיעורי רווח" or "שיעור רווח גולמי/תפעולי/נקי"

## References

- `references/financial-glossary.md` — Authoritative Hebrew↔English financial vocabulary by domain
- `references/common-mistakes.md` — Comprehensive wrong→right examples organized by error type
- `references/sentence-templates.md` — Ready-made Hebrew templates for common financial statements

## Self-Review Checklist

**MANDATORY**: After writing ANY Hebrew text, review it against this checklist before finalizing. Go through each item and fix violations.

- [ ] **Gibberish check**: Read every sentence. Does each one make grammatical sense in Hebrew? Could a native speaker understand it without guessing?
- [ ] **Vocabulary check**: Are there any English words that have Hebrew equivalents? (Check `references/financial-glossary.md`)
- [ ] **Invented words check**: Does every Hebrew word actually exist? If unsure, replace with a known synonym.
- [ ] **Calque check**: Are there phrases that sound like translated English? Rewrite them naturally.
- [ ] **Verb agreement check**: Does every verb match its subject in gender and number?
- [ ] **Pronoun check**: Are "זו", "זה", "אלה" used only when the referent is clear? Replace with the actual noun if ambiguous.
- [ ] **Financial terms check**: Are financial terms consistent with ISA/TASE conventions? (Check `references/financial-glossary.md`)
- [ ] **Formatting check**: Currency on the left (₪1,234)? Dates as dd/MM/YYYY? Negative numbers with left-side minus?
- [ ] **Fluency check**: Read 3 random paragraphs aloud. Do they flow naturally, or do they sound robotic/translated?

## Scoring Rubric

Use this scale to evaluate Hebrew quality (1-10):

| Score | Description | Characteristics |
|-------|-------------|-----------------|
| 1-2 | Unreadable | Multiple gibberish sentences, non-existent words, broken grammar throughout |
| 3-4 | Poor | Understandable but clearly machine-generated; frequent English calques, wrong terminology, verb errors |
| 5-6 | Acceptable | Mostly correct Hebrew with occasional awkward phrasing or wrong terms; a native speaker would notice errors |
| 7-8 | Good | Natural, fluent Hebrew; correct terminology; rare minor issues; reads like a professional wrote it |
| 9-10 | Excellent | Indistinguishable from a native Hebrew financial analyst; precise terminology, natural flow, zero errors |

**Target: 7+ on every output.**
