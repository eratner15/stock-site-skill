# Page Structures — 5-Page Portal Spec (v2 — Post-Autoresearch)

Updated after 25 experiments on AMZN and NOW portals. Every feature listed below is a standard requirement.

## Global Elements (All Pages)

- **Nav:** Fixed top, glassmorphism blur, brand links to `/lcs/`, 5 page links with active state, ticker+price, CTA button
- **Footer:** Gold line, brand, sources disclaimer, **"Last updated: [date]"** timestamp
- **OG Meta Tags:** `og:title`, `og:description`, `og:image` (Unsplash), `og:type`, plus `<meta name="description">` and chart emoji favicon
- **Scroll listener:** Nav adds `.scrolled` shadow on scroll > 10px
- **Dark Mode:** Toggle button (moon/sun emoji) in nav-right. CSS variables swap via `html.dark` class. localStorage persistence key `lcs-dark`. Covers nav, metrics, cards, tables, panels, slides.

## 1. Landing Page (index)

Sections in order:
1. **Hero** — Unsplash background, gradient overlay, eyebrow, h1 with gold span, tagline, recommendation badge with `title` tooltip showing PT sources
2. **Metrics Bar** — 7-column grid, each `.metric-val` has `title` tooltip with exact data source. **"Hover any metric for data source"** hint below
3. **BLUF** — Surface background, 3 paragraphs of sourced thesis narrative
4. **Thesis Cards** — 3-column grid, numbered (01/02/03), colored top borders, stat badges
5. **Earnings Setup Box** — Dark navy background, 4-stat grid (FY EPS est, Q1 rev est, key segment est, key metric), 2 whisper cards (What to Watch + Key Risks)
6. **Module Cards** — 2-column grid linking to memo/presentation/model/consensus
7. **Sell-Side Quotes** — 3-column grid, quote cards with firm color coding
8. **Footer**

## 2. Investment Memo

- **Font:** Merriweather (serif) body, Inter headings
- **Table of Contents** — `<nav class="memo-toc">` with Roman numeral ordered list in 2-column layout, anchor links to each section
- **SCQA framework** — broken into 4 labeled sub-paragraphs (Situation, Complication, Question, Answer)
- **8-10 sections** with Roman numeral headings, each `<h2>` has an `id` anchor
- **8+ Tufte sidenotes** — `<span class="memo-sidenote">` with sourced facts
- **Source tags** — `<span class="source-tag" title="Source description">[Tag]</span>` on 15+ key financial claims
- **Tables** with methodology footnotes in `.memo-sources` box below each table
- **Management quotes** in `.memo-quote` blockquote styling
- **SOTP section** with equity stakes table (if applicable)
- **Sources & Methodology** box before footer (Primary, Sell-Side, Market Data, Forward Estimates)
- **Back to Top** button — fixed circular button, appears after 800px scroll
- **Print CSS** — nav hidden, sidenotes inline, tables no-break, print font sizes
- **PDF Export** button — fixed bottom-left, triggers `window.print()`, hidden during print
- **~3,000+ words** of narrative prose (no bullet lists)

## 3. Presentation Deck

- **12-15 slides** with carousel navigation (arrows, dots, keyboard left/right)
- `.slide` divs, `.active` class shows current, slide counter at top ("1 / 14")

**Cover slide (Slide 1) — CRITICAL:**
- `.slide-cover` with dark gradient overlay on Unsplash image
- Large title, tagline at 65% white
- 4-column `.cover-stats` grid with transparent blur boxes
- Gold badge with recommendation + price target
- Source line at bottom

**Remaining slides:** Each has eyebrow label, action title, body (tables/stats/prose), source line
- **Source lines** on 8+ data-heavy slides citing specific research reports, filings, or methodology
- **Financial summary** slide extended through FY+3 with growth rate rows
- **Recommendation** slide includes 3-box valuation grid (DCF, Multiples, SOTP prices)

## 4. Interactive Model

**Header:** Section label, title, description of slider count and what they control

**Assumptions Panel:**
- Surface background, 3-column grid, 12 range sliders in 4 groups
- Each slider label has `title` tooltip with **rationale** explaining WHY the default was chosen
- `cursor:help` on labels to indicate hover tooltip
- **Bull / Base / Street / Bear** preset buttons + Reset
- **Keyboard shortcuts** — 1/2/3/4 for presets, R for reset, visible `.kbd-hint`
- **URL Scenario Sharing** — "Share Scenario" button encodes non-default slider values in URL hash. `loadFromHash()` restores on page load. Toast notification on copy.
- **Chart Downloads** — PNG download buttons on both charts via `canvas.toDataURL()`

**Valuation Summary Cards (3):**
- DCF | EV/EBITDA Cross-Check | Blended Fair Value (gold border)
- Each shows: price, upside %, method description
- **Smart Blend:** DCF/multiples weight auto-adjusts based on TV% concentration (50/50 standard, 40/60 at 85-95% TV, 30/70 at >95% TV). Dynamic label shows actual weights. Prevents capex-heavy companies from being unfairly penalized by temporarily negative FCFs.
- Blended card includes **Implied P/E, FCF Yield, and 2-Year Annualized IRR** (color-coded)

**Metrics Output Bar:** 8-column grid, auto-updates on slider change

**Multi-Year Earnings Bridge Table (FY-1A → FY+4E):**
- Segment revenue + growth rates
- Segment operating income + margins
- EBITDA, D&A, Capex, FCF, FCF Margin
- Net Income, EPS, **FCF per Share**
- **Balance Sheet section:** Cash, Debt, Net Debt, Net Debt/EBITDA

**DCF Table (5-year + Terminal):**
- EBITDA, Less Capex, Less Taxes, UFCF, Discount Factor, PV of FCF
- Terminal Value, PV of Terminal
- **Terminal Value as % of EV** — color-coded warning (green <75%, gold 75-90%, red >90%)
- Enterprise Value, Less Net Debt, Implied Share Price

**Sensitivity Tables (2):**
- DCF: WACC vs Terminal Growth — gold cell highlights current selection
- Multiples: EV/EBITDA (or EV/Revenue) vs Growth — gold cell highlights current

**Interactive SOTP Section:**
- **Year toggle** buttons (FY+1 / FY+2 / FY+3)
- **6 sliders** in assumptions panel: 3 business segment multiples + 3 equity stakes
- Table with section headers: Operating Businesses | Equity/Acquired Assets | Valuation Summary
- Individual line items for each equity stake (e.g., Anthropic, Rivian, Moveworks, Armis)

**Reverse-DCF "Market-Implied Growth":**
- Binary-search for growth rate that makes blended = current price
- 3-card display: Market-Implied Growth | Your Base Case | Growth Gap
- Color-coded gap (green >5pp = underpriced, gold 0-5pp, red negative)

**Charts (Chart.js):**
- Stacked bar: Revenue by segment (FY-1A → FY+4E)
- Dual line: EPS + FCF/Share trajectory

**Scenario Table:**
- Bull / Base / Street / Bear rows with DCF, Multiple, Blended prices + upside
- **Probability-Weighted EV** total row with gold-highlighted blended price

## 5. Consensus

**Consensus Strip:** 5 cards (rating, mean PT, PT range, analyst count, % buy)

**Rating Distribution Bar:** Proportional segments for Strong Buy/Buy/Hold/Sell

**Analyst Table:** 8+ rows with firm, analyst, rating badge, PT, prior PT, estimates, date
- **Data source footnote** below

**Segment Quarterly Estimates:** Q-by-Q breakdown with multiple sell-side sources
- **Data source footnote** citing each firm

**Key Takeaways:** 2-column cards summarizing top 2 analyst reports

**"Where the Street Is Wrong":** 2-column cards with proprietary AI view

**Capex ROI / SaaS Economics:** 3-column metric cards (sector-dependent)
- Capex-heavy: Incremental Rev/Capex, Backlog Coverage, Capex Inflection
- SaaS: Rule of 40, Net Revenue Retention, LTV/CAC

**Consensus Estimate Detail Table:** Revenue, EPS, margins, FCF with Low/Consensus/High
- **Data source footnote**

**Valuation vs Peers:** Comp table with 5+ peers, discount/premium calculation
- **Data source footnote**

**Earnings Beat/Miss History:** Last 8 quarters, EPS + Revenue surprise %
- **Data source footnote**

**Catalyst Calendar:** Date, event, significance, impact tags

**Management Quotes:** 3 sourced quotes in card layout

**Estimate Revision Momentum:** 4-card grid (EPS, Revenue, PT, key metric), 90-day deltas
- 2-column cards: Upgrades/Downgrades + Key Estimate Drivers

**Master Data Sources Disclaimer:** Before footer
