---
name: stock-site
description: |
  Build and deploy a complete equity research investment portal for any publicly
  traded stock. AI-first: autonomously researches the company from primary sources
  (SEC filings, earnings transcripts, IR materials, web data), forms a proprietary
  thesis, builds a multi-year financial model, and generates 5 interconnected HTML
  pages. Sell-side research PDFs are optional enrichment, not required input.
  Chains existing financial analysis skills (/comps, /dcf, /3-statements,
  /competitive-analysis) for institutional-quality analytics.
  Use when asked to "build a portal", "create a stock site", "build an investment
  page for [TICKER]", "create research for [company]", "build another one like
  UMG/SLG/BKR/RRX/KNX/AMZN", or "stock-site [TICKER]".
---

# Stock Site — AI-First Equity Research Portal Builder

## Philosophy

**AI-first, not sell-side-first.** The skill autonomously researches the company
from primary public sources, forms its own thesis, and builds the portal. Sell-side
PDFs are optional validation, not the foundation.

**Zero hallucination tolerance.** Every financial figure must trace to a verifiable
source: SEC filing, company IR page, or live market data. If a number can't be
sourced, flag it as an estimate with the methodology shown.

**Skill chaining over reinvention.** Use the installed financial analysis plugins
as analytical engines rather than rebuilding their logic.

## Anti-Hallucination Protocol

Every data point in the portal must have one of these source tags:

| Tag | Meaning | Example |
|-----|---------|---------|
| `[10-K]` | SEC annual filing | Revenue, segment breakdown, risk factors |
| `[10-Q]` | SEC quarterly filing | Quarterly revenue, recent guidance |
| `[Transcript]` | Earnings call transcript | Management quotes, guidance |
| `[IR]` | Investor relations page | Shareholder letter, press release |
| `[Market]` | Live market data via web search | Price, market cap, shares out |
| `[Consensus]` | Aggregated analyst estimates | Mean EPS, revenue estimates |
| `[Sell-Side]` | Named analyst report (if provided) | Barclays PT $300 |
| `[Computed]` | Derived from sourced inputs | EV/EBITDA = (mkt cap + debt - cash) / EBITDA |
| `[Estimated]` | AI estimate with methodology shown | "Based on 3-year CAGR of segment" |

**Rules:**
- Historical financials (revenue, EPS, margins): MUST come from [10-K] or [10-Q]
- Forward estimates: Must be [Consensus], [Sell-Side], or [Estimated] with method
- Quotes: MUST be [Transcript] or [IR] with attribution
- Never present an AI estimate as a fact — always label it

## Required Inputs

| Required | Purpose |
|----------|---------|
| Ticker | The stock to research |

| Optional (Enrichment) | Purpose |
|------------------------|---------|
| Sell-side research PDFs | Cross-check AI thesis, extract analyst PTs |
| Company earnings deck | Supplement filing data with visual context |
| Earnings transcripts | Management quotes, Q&A color |
| Specific URL (IR page, letter) | Targeted content to incorporate |

## Design Reference

**Design benchmark:** `/home/eratner/cafecito-ai/umg/` (UMG portal)

**Latest / most complete implementation:** AMZN portal at `/home/eratner/lcs-portfolio-intel/src/pages/amzn-*.ts`

The AMZN portal is the current gold standard with:
- Interactive multi-year model (FY27-29) with DCF + EV/EBITDA cross-check
- Interactive SOTP with broken-out equity stakes and per-segment multiple sliders
- Earnings setup box on landing page
- 16 Tufte sidenotes in memo
- Deepened consensus page (AWS quarterly estimates, peer comp, beat/miss history, revision momentum)

**All existing portals (for pattern reference):**
- UMG: `/home/eratner/cafecito-ai/umg/` (music/media)
- SLG: `/home/eratner/cafecito-ai/slg/` (REIT)
- BKR: `/home/eratner/cafecito-ai/bkr/` (energy)
- RRX: `/home/eratner/lcs-portfolio-intel/src/pages/rrx-*.ts` (industrial)
- KNX: `/home/eratner/lcs-portfolio-intel/src/pages/knx-*.ts` (trucking)
- AMZN: `/home/eratner/lcs-portfolio-intel/src/pages/amzn-*.ts` (mega-cap tech/cloud)

**Design system, page structures, and deployment:** See `references/` directory.

## Execution Workflow

### Phase 1: Autonomous Data Collection

Gather ALL data before writing any pages. Every item must be sourced.

#### 1A. SEC Filings (Primary Source of Truth)

**Auto-fetch latest 10-K:**
```bash
# Get CIK number
CIK=$(curl -s "https://efts.sec.gov/LATEST/search-index?q=%22{TICKER}%22&dateRange=custom&startdt=2025-01-01&enddt=2026-12-31&forms=10-K" | python3 -c "import sys,json; print(json.load(sys.stdin)['hits']['hits'][0]['_source']['file_num'])" 2>/dev/null)
```

**WebFetch approach (preferred — works without local tools):**
```
WebFetch https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={TICKER}&type=10-K&dateb=&owner=include&count=1
→ Extract the filing URL from the results
WebFetch {filing_url}
→ Extract financials from the 10-K HTML
```

**What to extract from 10-K:**
- Income Statement: Revenue by segment (3yr), Operating Income by segment, Net Income, EPS
- Balance Sheet: Cash & equivalents, Total debt, Total assets, Stockholders' equity, Shares outstanding
- Cash Flow: Operating CF, Capex, FCF (OCF - Capex), Dividends, Buybacks
- Risk Factors: Top 5 material risks (skip boilerplate)
- Segment descriptions, KPIs, geographic breakdown
- Equity investments / unconsolidated interests (for SOTP)
- Management guidance (if in filing)

**Verification:** Cross-check 3 numbers against a web source (e.g., MacroTrends, SEC filing summary)

#### 1B. Earnings Transcripts & IR Materials
WebSearch for:
- Latest earnings call transcript
- CEO/CFO shareholder letter
- Most recent investor day presentation
- Any URLs provided by the user

Extract:
- Management guidance (revenue, margins, capex)
- Key quotes (with speaker attribution)
- Strategic priorities and initiatives
- Capital allocation framework

#### 1C. Live Market Data
WebSearch for current:
- Stock price, market cap, shares outstanding
- 52-week range, average daily volume
- Dividend yield (if applicable)
- Consensus analyst estimates (EPS, revenue for next 2-3 years)
- Analyst ratings distribution (buy/hold/sell count)
- Mean and median price targets
- Recent analyst upgrades/downgrades

#### 1D. Sell-Side Research (If PDFs Provided)
If user provides research PDFs:
- Extract via PyMuPDF (`fitz`)
- Pull: price targets, rating, key estimates, thesis points, management quotes
- Flag where sell-side estimates differ from AI's own analysis
- These supplement but don't replace primary source data

### Phase 2: AI Analysis (Skill Chaining)

Run the analysis phase using existing skills as building blocks. Each skill
produces structured output that feeds into the portal pages.

#### 2A. Financial Model — Chain `/3-statements`
Build the 3-statement model from 10-K data:
- Income statement: 3yr historical + 3yr projected
- Balance sheet: assets, liabilities, equity
- Cash flow statement with FCF derivation

Use management guidance and historical growth rates for projections.
Label all forward estimates as [Estimated] with methodology.

#### 2B. Peer Comp Table — Chain `/comps`

Invoke the comps-analysis skill to build the peer comparison:

```
/comps {TICKER}
```

The skill will:
1. Identify 5-8 comparable companies (same sector, similar scale)
2. Pull operating metrics (Revenue, EBITDA, margins, growth rates)
3. Pull valuation multiples (EV/Revenue, EV/EBITDA, P/E, FCF Yield)
4. Calculate statistical summary (median, mean, discount/premium)

**Use the comps output to populate:**
- Consensus page → "Valuation vs Peers" table
- Model page → default multiple ranges for sliders
- Memo → valuation section peer benchmarks
- Presentation → valuation slide peer data

**If /comps is unavailable**, use WebSearch to manually gather:
- 5 closest public peers by business model and revenue scale
- NTM EV/EBITDA, NTM P/E, NTM EV/Revenue for each
- Revenue growth rate and key margin for each
- Calculate the peer average and the target company's discount/premium

This data feeds directly into 4 of the 5 portal pages — it's one of the highest-leverage skill chains.

#### 2C. DCF Valuation — Chain `/dcf`
Build a DCF using the 3-statement model outputs:
- 5-year explicit FCF projections
- WACC calculation (with sourced inputs)
- Terminal value via Gordon Growth
- Sensitivity tables (WACC vs terminal growth)

This feeds the interactive model page's DCF section.

#### 2D. Competitive Landscape — Chain `/competitive-analysis`
Analyze the competitive positioning:
- Market share data
- Key competitive advantages / moats
- Threat assessment
- Industry dynamics

This feeds the memo's industry context section.

#### 2E. AI Thesis Formation
Using ALL collected data, form a proprietary thesis:

**The thesis must answer:**
1. What is the market missing? (the variant perception)
2. What are the 3 key catalysts?
3. What is the biggest risk the market is correctly pricing?
4. What is the 12-month risk/reward setup?

**Rating framework:**
- BUY: >20% upside with identifiable catalysts
- HOLD: Fair value within 10%, no clear catalyst
- SELL: >15% downside or deteriorating fundamentals

**Price target derivation:**
- Primary: DCF from Phase 2C
- Cross-check: EV/EBITDA on forward estimates
- Cross-check: Peer-relative valuation from Phase 2B
- If sell-side provided: Compare to consensus PT and explain differences

### Phase 3: Build 5 HTML Pages

Read the reference implementations before building. Each page uses:
- **Design system:** See `references/design-system.md`
- **Page structure:** See `references/page-structures.md`
- **Model template:** See `references/model-templates.md`

#### Page 1: Landing (index)
- Hero with company-relevant Unsplash image
- 7-metric bar (price, mkt cap, revenue, EPS, key segment metric, valuation multiple)
- BLUF thesis (3 paragraphs, all claims sourced)
- 3 thesis cards (the variant perception pillars)
- **Earnings setup box** (next quarter estimates, what to watch, risks into print)
- Module cards linking to other 4 pages
- Sell-side quotes (from PDFs if provided, or from consensus data)
- **Metric tooltips** — every metric-val has `title` showing exact data source
- **"Hover for sources"** hint under metrics bar

#### Page 2: Investment Memo
- Merriweather serif body, SCQA framework
- 3,000+ words of narrative prose (no bullet lists)
- SCQA broken into labeled sub-paragraphs
- **8+ Tufte sidenotes** with sourced facts throughout
- Financial trajectory table with sourced historical + estimated forward data
- **SOTP section** with equity stakes breakdown (if applicable)
- Valuation section with DCF, multiples, and peer-relative
- Risk assessment (from 10-K risk factors, not generic)
- Every number tagged with source
- **Methodology footnotes** after financial tables explaining HOW forward estimates were derived
- **Sources & Methodology** box before footer (Primary, Sell-Side, Market Data categories)
- **Print CSS** — nav hidden, sidenotes inline, tables no-break, print font sizes

#### Page 3: Presentation Deck
- 12-15 slide carousel
- UMG-style cover slide (dark gradient, transparent stat boxes)
- Segment analysis, thesis slides, financial summary
- Valuation and risk assessment with **3-framework price target grid** (DCF, Multiples, SOTP)
- Catalyst calendar
- **Source lines** on 8+ data-heavy slides (financial summary, valuation, recommendation)

#### Page 4: Interactive Model
Follow the AMZN pattern (the most advanced implementation):
- **Multi-year projections** (3 forward years, not just 1)
- **12 sliders** organized in 4 groups, sector-appropriate
  - See `references/model-templates.md` for sector detection
- **Bull / Base / Street / Bear presets**
  - Street = consensus estimates (always include this)
- **DCF valuation** with 5-year FCF + terminal value
- **EV/EBITDA cross-check** on forward EBITDA
- **Blended fair value** with **smart blend** — adjusts DCF/multiples weight based on terminal value concentration:
  - TV% < 85%: standard 50/50 blend
  - TV% 85-95%: 40% DCF / 60% multiples (growth company)
  - TV% > 95%: 30% DCF / 70% multiples (capex-distorted, e.g., AMZN)
  - Dynamic label shows actual weights used. Applies to valuation cards, scenario table, prob-weighted EV, and reverse-DCF
- **Multi-year earnings bridge** with:
  - Segment revenue and growth rates
  - Segment operating income and margins
  - EBITDA, D&A, capex, FCF
  - Net income, EPS, **FCF per share**
  - Balance sheet section (cash, debt, net debt, net debt/EBITDA)
- **Interactive SOTP** with:
  - Per-segment revenue/OI multiple sliders
  - Valuation year toggle (FY+1 / FY+2 / FY+3)
  - Broken-out equity stakes (individual line items with sliders)
- **2D sensitivity tables** (DCF: WACC vs terminal growth; Multiples: EV/EBITDA vs growth)
- **Chart.js charts** (stacked revenue bar + EPS/FCF line)
- **Scenario table** with probability-weighted outcomes + **Prob-Weighted EV row**
- **Blended valuation card** showing: Implied P/E, FCF Yield, and **2-Year Annualized IRR** (color-coded: green >20%, gold 0-20%, red <0%)
- **DCF terminal value transparency** — TV as % of EV row with warning (green <75%, gold 75-90%, red >90% "elevated")
- **Reverse-DCF "Market-Implied Growth"** section — binary-searches for growth rate that justifies current price, shows gap vs base case
- **Keyboard shortcuts** — 1/2/3/4 for presets, R for reset, with visible kbd hint
- **Slider rationale tooltips** — every slider label has `title` explaining WHY the default was chosen, citing analyst estimates and methodology

#### Page 5: Consensus
Follow the AMZN pattern (deepened version):
- Consensus strip (rating, PT, range, analyst count)
- Rating distribution bar
- Analyst table (8+ analysts if data available)
- **Segment quarterly estimates** (key segment Q-by-Q with multiple sources)
- Key takeaways (2-column cards)
- "Where the Street is Wrong" (proprietary AI view)
- **Capex ROI / Capital returns framework** (if capex-heavy company)
- Consensus estimate detail table (revenue, EPS, margins, FCF)
- **Valuation vs peers** (comp table with discount/premium to peer average)
- **Earnings beat/miss history** (last 8 quarters)
- Catalyst calendar
- Management quotes (sourced from transcripts)
- **Estimate revision momentum** (90-day revision cards + upgrade/downgrade timeline)
- **Data source footnotes** after every data table (analyst table, estimate detail, beat/miss, peer comp, quarterly estimates)
- **Master Data Sources disclaimer** before footer

### Phase 4: Wire Routing & Deploy

Same as before — see `references/deployment.md`.

### Phase 5: Verify

```bash
for page in "" "memo.html" "presentation.html" "model.html" "consensus.html"; do
  curl -s -o /dev/null -w "%{http_code}" "https://cafecito-ai.com/lcs/{ticker}/${page}"
done
```

Then use `/browse` to verify:
- All pages return 200
- Model page: no JS console errors, sliders work, charts render
- Memo: sidenotes visible on desktop
- Presentation: carousel navigates correctly

## Sector Detection

Match the company to a sector for model template selection:
- **Mega-Cap Tech/Cloud:** Multi-segment model with AWS/Cloud, Retail, Ads (AMZN pattern)
- **Trucking/Transport:** Operating ratio model (KNX pattern)
- **REIT:** NAV/cap rate model (SLG pattern)
- **Music/Media:** Subscription growth model (UMG pattern)
- **Industrial/Conglomerate:** Segment EBITDA model (RRX pattern)
- **Energy/Oilfield:** FCF yield model (BKR pattern)
- **SaaS/Software:** ARR/NDR model with Rule of 40
- **Financials/Banks:** NIM/ROE model with book value
- **Healthcare/Pharma:** Pipeline probability-adjusted model
- **Consumer/Retail:** Same-store sales + margin expansion model
- **Default:** EPS growth model with revenue/margin/multiple sliders

## Quality Checklist

Before deployment, verify:

**Data Accuracy:**
- [ ] All historical financials match 10-K/10-Q (spot-check 5 numbers)
- [ ] Forward estimates labeled as [Consensus], [Sell-Side], or [Estimated]
- [ ] All management quotes have speaker attribution and source
- [ ] Rating bar totals match analyst count in consensus strip
- [ ] Consensus estimate ranges are internally consistent

**Model Functionality:**
- [ ] Model page: no JS console errors, all sliders functional
- [ ] All 4 presets (Bull/Base/Street/Bear) produce reasonable valuations
- [ ] DCF produces reasonable implied price (within 50% of current price)
- [ ] SOTP equity stakes are individually broken out with sliders
- [ ] Keyboard shortcuts (1/2/3/4/R) work, kbd hint visible
- [ ] Probability-weighted EV row in scenario table
- [ ] Implied P/E, FCF Yield, and 2Y IRR on blended valuation card
- [ ] Terminal value as % of EV row in DCF table with color-coded warning
- [ ] Reverse-DCF "Market-Implied Growth" section binary-searching correctly
- [ ] Charts render (Chart.js loaded, canvas non-zero dimensions)
- [ ] URL scenario sharing: Share button encodes slider state in URL hash, loadFromHash() restores on page load
- [ ] Chart download buttons: PNG export on both charts via canvas.toDataURL()

**Anti-Hallucination:**
- [ ] Memo: 15+ source tags ([10-K], [Transcript], etc.) + methodology footnotes
- [ ] Consensus: data-source footnotes after every table + master disclaimer
- [ ] Index: title tooltips on all 7 metrics + hero badge
- [ ] Model: rationale tooltips on all 12 slider labels
- [ ] Deck: source lines on 8+ data-heavy slides
- [ ] Source attribution covers ALL 5 page types

**Completeness:**
- [ ] Earnings setup box on landing page with next quarter estimates
- [ ] Memo has 8+ sidenotes, SCQA broken into sub-paragraphs, SOTP section
- [ ] Consensus has peer comp, beat/miss history, revision momentum
- [ ] Beat/miss history has last 8 quarters
- [ ] OG meta tags + description + favicon on all pages
- [ ] Clickable brand linking to /lcs/ + "Last updated" timestamp in all footers
- [ ] Print CSS on memo pages
- [ ] Dark mode toggle on all pages with localStorage persistence
- [ ] PDF export button on memo pages (window.print() with print CSS)

**Technical:**
- [ ] No `${` template interpolation bugs in served HTML
- [ ] No unescaped single quotes in JS strings within template literal
- [ ] All nav links correct with proper active states
- [ ] Mobile responsive: tables scroll, metrics reflow, nav collapses
- [ ] Dark mode CSS covers all elements (nav, metrics, cards, tables, slides, panels)
- [ ] All 5 pages return HTTP 200
