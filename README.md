# DeepValue TASE Plugin

Professional financial analysis tools for the Israeli stock market (TASE).

## What's Included

### Commands
- `/build-thesis <company>` — Build a comprehensive Hebrew investment thesis
- `/analyze-report <company>` — Deep-dive into the latest financial report
- `/analyze-announcements <company>` — Analyze Maya regulatory announcements
- `/analyze-orders <company>` — Analyze order flow patterns, compare to prior periods, and assess revenue impact
- `/compare-companies <company1> <company2>` — Side-by-side company comparison
- `/monitor-watchlist` — Generate a personalized Hebrew morning brief for your watchlist companies

### Skills
- **tase-analysis** — TASE market structure, Israeli IFRS, Maya regulatory system
- **deep-value-methodology** — Value investing principles, screening metrics, red flags, scoring
- **hebrew-quality** — Hebrew language quality, grammar, formatting, financial terminology, self-review checklist

### MCP Server
Connects to DeepValue's remote MCP server with 48+ financial data tools including:
- Company search and AI insights
- Financial statements (income, balance sheet, cash flow)
- Stock screening with 100+ metrics
- Chart generation with Hebrew RTL support
- Maya announcements and corporate events
- Order backlog and dividend history
- Index components and historical prices

## Installation

### Claude Web / Mobile (Connectors)

1. Open **Settings** → **Customize** → **Connectors**
2. Click **Add Custom Connector**
3. Fill in:
   - **Name:** `DeepValue`
   - **URL:** `https://financials.deepvalue.co.il/mcp`
4. Approve OAuth access from DeepValue

![Claude Web Connector Setup](https://mcp.deepvalue.co.il/instructions/how_to_add_to_claude_web.png)

> Mobile works the same way — open Settings → Customize → Connectors.

### Claude Code (Plugin)

```
/plugin marketplace add deepvalueinvesting/deep-value-tase-plugin
/plugin install deepvalue-tase@deepvalueinvesting
```

> **Tip:** To enable auto-updates, open `/plugin` → Marketplaces → Enable auto-update for `deepvalueinvesting`.

### Claude Co-Work (Plugin)

1. Open Claude Desktop and switch to the **Cowork** tab
2. Click **Customize** in the left sidebar
3. Click the **+** button → **Add marketplace from GitHub**
4. Enter: `deepvalueinvesting/deep-value-tase-plugin`
5. Browse the marketplace and click **Install** on the DeepValue plugin

## Usage Examples

```
/build-thesis אלביט
/analyze-report בנק לאומי
/analyze-announcements טבע
/analyze-orders אלביט
/compare-companies אלביט רפאל
/monitor-watchlist
```

## Contact

Questions or feedback? Reach us at deepvalueil@gmail.com
