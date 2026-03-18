# DeepValue TASE Plugin

Professional financial analysis tools for the Israeli stock market (TASE).

## What's Included

### Commands
- `/build-thesis <company>` — Build a comprehensive Hebrew investment thesis
- `/analyze-report <company>` — Deep-dive into the latest financial report
- `/analyze-announcements <company>` — Analyze Maya regulatory announcements
- `/compare-companies <company1> <company2>` — Side-by-side company comparison

### Skills
- **tase-analysis** — TASE market structure, Israeli IFRS, Maya system, Hebrew formatting
- **deep-value-methodology** — Value investing principles, screening metrics, red flags, scoring

### MCP Server
Connects to DeepValue's remote MCP server with 48+ financial data tools including:
- Company search and AI insights
- Financial statements (income, balance sheet, cash flow)
- Stock screening with 100+ metrics
- Chart generation with Hebrew RTL support
- Maya announcements and corporate events

## Installation

### From GitHub Marketplace (recommended)

```
/plugin marketplace add deepvalueinvesting/deep-value-tase-plugin
/plugin install deepvalue-tase@deepvalueinvesting
```

To enable auto-updates: open `/plugin` → Marketplaces → Enable auto-update for `deepvalueinvesting`.

### Direct MCP Server (works in any MCP client)

```
URL: https://financials.deepvalue.co.il/mcp
```

## Usage Examples

```
/build-thesis אלביט
/analyze-report בנק לאומי
/analyze-announcements טבע
/compare-companies אלביט רפאל
```
