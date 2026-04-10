---
description: Build and deploy an AI-first equity research portal — autonomously researches from SEC filings, transcripts, and web data
argument-hint: "[ticker]"
---

# Stock Site — AI-First Equity Research Portal

Build a complete equity research portal for the specified ticker. The skill autonomously researches the company from primary public sources, forms a proprietary thesis, and generates 5 interconnected pages deployed to cafecito-ai.com/lcs/{ticker}/.

## Workflow

### Step 1: Load the stock-site skill

Load `skill: "stock-site"` from the financial-analysis plugin and follow its full workflow:

1. **Phase 1: Autonomous Data Collection** — SEC EDGAR (10-K/10-Q), earnings transcripts, IR materials, live market data via web search. Sell-side PDFs are optional enrichment if provided.

2. **Phase 2: AI Analysis with Skill Chaining**
   - Chain `skill: "3-statements"` for the financial model
   - Chain `skill: "dcf-model"` for DCF valuation
   - Chain `skill: "comps-analysis"` for peer comp table
   - Chain `skill: "competitive-analysis"` for competitive landscape
   - Form proprietary thesis from primary sources

3. **Phase 3: Build 5 HTML Pages** — Landing (with earnings setup box), Investment Memo (with SOTP and sidenotes), Presentation Deck, Interactive Model (multi-year + DCF + SOTP), Consensus (with peer comps, beat/miss history, revision momentum)

4. **Phase 4: Deploy** — Wire routing in LCS worker and deploy to Cloudflare

### Anti-Hallucination Rule

Every number must be tagged with its source: [10-K], [10-Q], [Transcript], [IR], [Market], [Consensus], [Sell-Side], [Computed], or [Estimated]. No AI estimates presented as facts.

### If user provides source materials

Sell-side PDFs, earnings decks, or specific URLs provided by the user become enrichment that supplements (not replaces) the autonomous research. Extract analyst price targets, estimates, and quotes to cross-check the AI's own thesis.
