# Stock Site — AI-First Equity Research Portal Builder

Build institutional-quality equity research portals for any publicly traded stock using Claude Code. Give it a ticker and it autonomously researches the company from primary sources (SEC filings, earnings transcripts, IR materials), forms a proprietary thesis, builds a multi-year financial model, and deploys 5 interconnected HTML pages.

**No sell-side PDFs required.** The skill researches autonomously. Sell-side notes are optional enrichment.

## Live Examples

- **Amazon (AMZN):** [cafecito-ai.com/lcs/amzn/](https://cafecito-ai.com/lcs/amzn/)
- **ServiceNow (NOW):** [cafecito-ai.com/lcs/now/](https://cafecito-ai.com/lcs/now/)

## What It Builds

Each portal has 5 pages:

| Page | What's On It |
|------|-------------|
| **Landing** | Hero, 7-metric bar with source tooltips, BLUF thesis, thesis cards, earnings setup box, sell-side quotes |
| **Memo** | 3,000+ word Tufte-style SCQA memo with 15+ source tags, sidenotes, TOC, SOTP analysis, print CSS, PDF export |
| **Deck** | 14-slide carousel with UMG-style cover, source lines on data slides, keyboard navigation |
| **Model** | 12 interactive sliders, multi-year DCF + EV/EBITDA cross-check, interactive SOTP, reverse-DCF implied growth, sensitivity tables, Chart.js charts, keyboard shortcuts, URL scenario sharing |
| **Consensus** | Analyst table, peer comp, 8-quarter beat/miss history, quarterly estimates, revision momentum, catalyst calendar, management quotes |

## Key Features

- **AI-First Research** — Autonomous data collection from SEC EDGAR, earnings transcripts, IR pages, and web search
- **Anti-Hallucination Protocol** — Every number tagged with source: `[10-K]`, `[Transcript]`, `[Market]`, `[Consensus]`, `[Estimated]`
- **Skill Chaining** — Orchestrates `/3-statements`, `/dcf`, `/comps`, `/competitive-analysis` as building blocks
- **10 Sector Templates** — SaaS, Mega-Cap Tech, Financials, Healthcare, Consumer, REIT, Energy, Industrial, Trucking, Music
- **Dark Mode** — Toggle on all pages with localStorage persistence
- **Interactive SOTP** — Per-segment multiple sliders, equity stakes broken out, valuation year toggle
- **Reverse-DCF** — Shows what growth rate the current stock price implies
- **34-Item Quality Checklist** — Verified before every deployment

## Install

### Option A: As a Claude Code Skill (simplest)

```bash
# Clone into your Claude skills directory
git clone https://github.com/eratner15/stock-site-skill.git ~/.claude/skills/stock-site
```

Then in Claude Code, the skill triggers automatically when you say:
- "Build a portal for GOOGL"
- "Create a stock site for NVDA"
- "Stock site META"

### Option B: As a Plugin Command

```bash
# Clone into an existing financial-analysis plugin
git clone https://github.com/eratner15/stock-site-skill.git
cp -r stock-site-skill ~/.claude/plugins/YOUR_PLUGIN/skills/stock-site
cp stock-site-skill/commands/*.md ~/.claude/plugins/YOUR_PLUGIN/commands/
```

Then use: `/stock-site TICKER`

## Usage

```
/stock-site GOOGL
```

Or with sell-side PDFs for enrichment:

```
/stock-site GOOGL /path/to/research/pdfs/
```

### Update an Existing Portal

```
/stock-site-update AMZN
```

Refreshes price, estimates, and earnings data on an existing portal.

## Configuration

### Deployment Target

The skill deploys to a Cloudflare Worker by default. Edit `references/deployment.md` to configure your own deployment target:

- **Cloudflare Workers** (default) — TypeScript template literals served via Hono
- **Static HTML** — Generate standalone HTML files for any hosting
- **GitHub Pages** — Push to a repo with GitHub Pages enabled

### Design Customization

Edit `references/design-system.md` to change:
- Colors (gold accent, navy, surface colors)
- Typography (Inter for UI, Merriweather for memo prose)
- Brand name in nav
- Unsplash image preferences

## Autoresearch

The skill includes a `program.md` for autonomous improvement using the [Karpathy autoresearch](https://github.com/karpathy/autoresearch) pattern. Run it to continuously evaluate and improve your portals:

```
/loop 30m Run one autoresearch experiment cycle on the stock-site skill...
```

The included `results.tsv` shows 30+ experiments that took the skill from 7.4 to 10.0+ quality score with zero discards.

## Requirements

- [Claude Code](https://claude.ai/claude-code) (CLI or desktop)
- A deployment target (Cloudflare Workers recommended)
- Optional: [Financial Services Plugins](https://github.com/anthropics/financial-services-plugins) for skill chaining (`/comps`, `/dcf`, `/3-statements`)

## File Structure

```
stock-site-skill/
├── SKILL.md                    # Main skill definition + quality checklist
├── commands/
│   ├── stock-site.md           # /stock-site slash command
│   └── stock-site-update.md    # /stock-site-update slash command
├── references/
│   ├── design-system.md        # CSS variables, typography, components
│   ├── page-structures.md      # 5-page spec with all features
│   ├── model-templates.md      # 10 sector-specific slider configs
│   └── deployment.md           # Routing and deploy instructions
├── program.md                  # Autoresearch improvement loop
└── results.tsv                 # Experiment history (30+ runs)
```

## License

MIT
