# Document Design Reference — DeepValue Analysis Reports

**CRITICAL**: Copy the exact HTML structure and CSS from the example below. Do not invent your own design. Do not use CSS grid for metrics. Do not use border-right indicators for headings. Do not use large card boxes for KPIs.

## Reference Example

Below is a complete working HTML document that demonstrates the exact design. Your output must follow this structure, CSS, and component patterns exactly. Replace the content but keep the structure identical.

```html
<!DOCTYPE html>
<html lang="he" dir="rtl"><head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ניתוח הזמנות — גילת טלקום גלובל (GLTL)</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Heebo:wght@300;400;500;700&display=swap');

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: 'Heebo', sans-serif;
            background: #f8f9fa;
            color: #2c3e50;
            line-height: 1.85;
            font-size: 15.5px;
        }

        .container {
            max-width: 860px;
            margin: 0 auto;
            padding: 40px 30px;
        }

        .header {
            background: linear-gradient(135deg, #1a5276 0%, #2c3e50 100%);
            color: white;
            padding: 35px 40px;
            border-radius: 12px;
            margin-bottom: 35px;
        }

        .header h1 {
            font-size: 26px;
            font-weight: 700;
            margin-bottom: 6px;
        }

        .header .subtitle {
            font-size: 15px;
            opacity: 0.85;
            font-weight: 300;
        }

        .header .date {
            font-size: 13px;
            opacity: 0.7;
            margin-top: 10px;
        }

        .section {
            background: white;
            border-radius: 10px;
            padding: 30px 35px;
            margin-bottom: 24px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.06);
        }

        .section h2 {
            font-size: 20px;
            font-weight: 700;
            color: #1a5276;
            margin-bottom: 16px;
            padding-bottom: 8px;
            border-bottom: 2px solid #eaecee;
        }

        .section p {
            margin-bottom: 14px;
            text-align: justify;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin: 18px 0;
            font-size: 14px;
        }

        thead th {
            background: #1a5276;
            color: white;
            padding: 10px 12px;
            text-align: right;
            font-weight: 500;
        }

        tbody td {
            padding: 9px 12px;
            border-bottom: 1px solid #eaecee;
        }

        tbody tr:nth-child(even) {
            background: #f8f9fa;
        }

        tbody tr.chain-highlight {
            background: #eaf2f8;
        }

        .metric-box {
            display: inline-block;
            background: #eaf2f8;
            border-radius: 8px;
            padding: 8px 16px;
            margin: 4px 6px 4px 0;
            font-size: 14px;
        }

        .metric-box .label {
            font-weight: 300;
            font-size: 12px;
            color: #5d6d7e;
        }

        .metric-box .value {
            font-weight: 700;
            color: #1a5276;
        }

        .chart-container {
            text-align: center;
            margin: 20px 0;
        }

        .chart-container img {
            max-width: 100%;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .disclaimer {
            font-size: 13px;
            color: #7f8c8d;
            text-align: center;
            padding: 20px 0 0;
            margin-top: 10px;
        }

        .badge {
            display: inline-block;
            font-size: 11px;
            padding: 2px 8px;
            border-radius: 4px;
            font-weight: 500;
        }

        .badge-new { background: #d5f5e3; color: #1e8449; }
        .badge-expansion { background: #fdebd0; color: #b9770e; }
        .badge-framework { background: #d6eaf8; color: #2471a3; }
        .badge-intl { background: #f5eef8; color: #7d3c98; }

        .powered-by {
            text-align: center;
            font-size: 12px;
            color: #aab7b8;
            margin-top: 30px;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- HEADER: gradient banner with title, subtitle, date -->
        <div class="header">
            <h1>ניתוח הזמנות — גילת טלקום גלובל בע"מ (GLTL)</h1>
            <div class="subtitle">סקטור תקשורת ומדיה | שירותי תקשורת לוויינית</div>
            <div class="date">תאריך הניתוח: 17 במרץ 2026 | תקופת ניתוח: YTD 2026 מול 2025</div>
        </div>

        <!-- SECTION: market snapshot with INLINE metric boxes (not grid cards!) -->
        <div class="section">
            <h2>תמונת שוק נוכחית</h2>
            <p>מניית גילת טלקום נסחרת במחיר של 178.1 ש"ח, המשקף שווי שוק של כ-191 מיליון ש"ח...</p>
            <div>
                <span class="metric-box"><span class="label">מחיר</span> <span class="value">178.1 ₪</span></span>
                <span class="metric-box"><span class="label">שווי שוק</span> <span class="value">191M ₪</span></span>
                <span class="metric-box"><span class="label">P/E</span> <span class="value">12.4</span></span>
                <span class="metric-box"><span class="label">P/S</span> <span class="value">0.75</span></span>
                <span class="metric-box"><span class="label">EV/EBITDA</span> <span class="value">3.0</span></span>
                <span class="metric-box"><span class="label">CAGR 5Y</span> <span class="value">5.1%</span></span>
            </div>
        </div>

        <!-- SECTION: table with colored BADGES in cells -->
        <div class="section">
            <h2>מיפוי הזמנות — YTD 2026 (1.1–17.3)</h2>
            <table>
                <thead>
                    <tr>
                        <th>תאריך</th>
                        <th>תיאור</th>
                        <th>סכום (מ' ש"ח)</th>
                        <th>סוג</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>01.01</td>
                        <td>הזמנה מלקוח ישראלי (חברה-בת), 12 חודשים</td>
                        <td>5.0</td>
                        <td><span class="badge badge-new">חדשה</span></td>
                    </tr>
                    <tr class="chain-highlight">
                        <td>05.01</td>
                        <td>התקשרות לאספקת שירותי תקשורת לוויינית, לקוח ישראלי</td>
                        <td>52.0</td>
                        <td><span class="badge badge-new">חדשה</span></td>
                    </tr>
                    <tr>
                        <td>28.01</td>
                        <td>הרחבת הזמנה מלקוח ישראלי</td>
                        <td>9.0</td>
                        <td><span class="badge badge-expansion">הרחבה</span></td>
                    </tr>
                    <tr>
                        <td>10.02</td>
                        <td>הזמנת מסגרת מלקוח ישראלי</td>
                        <td>10.0</td>
                        <td><span class="badge badge-framework">מסגרת</span></td>
                    </tr>
                    <tr>
                        <td>02.03</td>
                        <td>הזמנה מלקוח בינלאומי, לשנת 2026</td>
                        <td>3.8</td>
                        <td><span class="badge badge-intl">בינלאומי</span></td>
                    </tr>
                </tbody>
            </table>
            <p>סך ההזמנות שדווחו ב-YTD 2026 עומד על כ-173.4 מיליון ש"ח...</p>
        </div>

        <!-- SECTION: with chart (base64 from generate_chart) -->
        <div class="section">
            <h2>השוואה ל-YTD 2025 ושנת 2025 המלאה</h2>
            <p>הפער בין שתי התקופות חד באופן חריג...</p>
            <div class="chart-container">
                <img src="data:image/png;base64,{image_base64}" alt="השוואת הזמנות YTD">
            </div>
        </div>

        <!-- SECTION: text only -->
        <div class="section">
            <h2>גורמים מניעים</h2>
            <p>ניתוח הדוחות הכספיים מצביע על מספר גורמים...</p>
            <p>גורם שני הוא ההסכם האסטרטגי...</p>
        </div>

        <!-- DISCLAIMER: plain centered gray text. NO borders. NO colored boxes. -->
        <div class="disclaimer">
            ניתוח זה מבוסס על דיווחים רגולטוריים בלבד ואינו מהווה ייעוץ השקעות. יש לבצע בדיקה עצמאית לפני קבלת החלטות השקעה.
        </div>

        <div class="powered-by">
            Powered by DeepValue | נתונים עד 17.3.2026
        </div>
    </div>
</body></html>
```

## Critical Design Rules

**Metric boxes** — ALWAYS use `<span class="metric-box">` as small inline badges in a single flowing row. NEVER use CSS grid, flexbox grids, or large card layouts for metrics. The metric boxes are small, inline, and flow naturally in one row.

**Tables** — ALWAYS have `<thead>` with dark blue background (`#1a5276`) and white text. ALWAYS use colored `<span class="badge badge-TYPE">` in the last column to categorize rows. Badge types: `badge-new` (green), `badge-expansion` (orange), `badge-framework` (blue), `badge-intl` (purple). Use `chain-highlight` class on `<tr>` for related order chains.

**Sections** — EVERY section is wrapped in `<div class="section">` which gives it a white background, rounded corners, and subtle shadow. Section titles use `<h2>` with blue color and bottom border. NEVER use standalone headings outside of section cards. NEVER use vertical side bars or border-right indicators.

**Charts** — Use `<div class="chart-container"><img src="data:image/png;base64,{image_base64}"></div>`. Charts come from `generate_chart` tool only. NEVER use `image_url`. NEVER regenerate with matplotlib.

**Disclaimer** — Plain centered gray text. NO borders, NO background colors, NO colored callout boxes. Just `.disclaimer` class with `color: #7f8c8d; text-align: center`.

**Footer** — `<div class="powered-by">Powered by DeepValue | נתונים עד [date]</div>`

**DO NOT** — No empty sections. No grid card metrics. No border-right on headings. No inventing your own CSS.
