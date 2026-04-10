# Model Templates — Sector-Specific Slider Configurations

Each model page uses 12 range sliders organized in 4 groups (3 per group) with Bull/Base/Bear presets.

## Trucking / Transportation (KNX pattern)

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| TL Rev/Loaded Mile Growth | -2 | 15 | 0.5 | 5 | % |
| TL Volume Growth | -5 | 8 | 0.5 | 1 | % |
| LTL Revenue Growth | 0 | 20 | 1 | 8 | % |
| TL Adj Operating Ratio | 86 | 96 | 0.5 | 91 | % |
| LTL Adj Operating Ratio | 88 | 98 | 0.5 | 93 | % |
| Tractor Count | 18 | 24 | 0.5 | 21 | K |
| Interest Rate | 3 | 7 | 0.25 | 5 | % |
| Tax Rate | 20 | 30 | 1 | 25 | % |
| Shares Outstanding | 150 | 170 | 1 | 162 | M |
| Exit P/E | 12 | 25 | 0.5 | 18 | x |
| WACC | 7 | 15 | 0.5 | 10 | % |
| Fuel Impact | -3 | 3 | 0.5 | 0 | % |

**Key calc:** OR drives operating income (lower = better). TL OI = TL Rev * (1 - OR/100).

## REIT (SLG pattern)

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Cap Rate | 4.5 | 8.0 | 0.25 | 6.0 | % |
| Occupancy Rate | 85 | 98 | 0.5 | 92 | % |
| NOI Growth | -2 | 8 | 0.5 | 3 | % |
| Disposition Volume | 0 | 2000 | 50 | 500 | $M |
| Disposition Cap Rate | 4.0 | 7.0 | 0.25 | 5.5 | % |
| Leasing Spread | -5 | 15 | 0.5 | 5 | % |
| Interest Rate | 3 | 7 | 0.25 | 5.5 | % |
| Tax Rate | 0 | 5 | 0.5 | 0 | % |
| Shares Outstanding | — | — | — | — | M |
| P/FFO Multiple | 8 | 18 | 0.5 | 12 | x |
| Discount Rate | 6 | 12 | 0.5 | 9 | % |
| Leverage (Debt/EV) | 30 | 60 | 1 | 45 | % |

**Key calc:** NAV = NOI / Cap Rate. FFO bridge from NOI.

## Music / Media (UMG pattern)

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Subscription Growth | 6 | 14 | 0.5 | 10 | % |
| Total Revenue Growth | 5 | 12 | 0.5 | 8 | % |
| Physical Revenue Growth | -2 | 8 | 0.5 | 4 | % |
| EBITDA Margin (Near) | 22 | 28 | 0.5 | 24 | % |
| EBITDA Margin (Far) | 23 | 30 | 0.5 | 25.5 | % |
| Interest Rate | 3 | 6 | 0.25 | 4 | % |
| Tax Rate | 20 | 30 | 1 | 25 | % |
| Leverage | 0 | 3.5 | 0.25 | 1 | x |
| Buyback Price | 18 | 35 | 1 | 24 | € |
| Dividend Growth | 0 | 5 | 0.5 | 2 | % |
| P/E Near | 20 | 35 | 0.5 | 28 | x |
| Discount Rate | 7 | 15 | 0.5 | 10 | % |

## Industrial / Conglomerate (RRX pattern)

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Segment A Growth | -5 | 15 | 0.5 | 3 | % |
| Segment B Growth | -5 | 15 | 0.5 | 5 | % |
| Segment C Growth | -5 | 15 | 0.5 | 2 | % |
| EBITDA Margin | 18 | 28 | 0.1 | 23 | % |
| D&A (as % rev) | 5 | 9 | 0.1 | 7 | % |
| Interest Rate | 3 | 9 | 0.25 | 5.5 | % |
| Tax Rate | 15 | 30 | 0.5 | 22 | % |
| Debt Paydown/yr | 0 | 600 | 25 | 300 | $M |
| Shares Outstanding | — | — | — | — | M |
| Target P/E | 10 | 30 | 0.5 | 18 | x |
| EV/EBITDA Multiple | 8 | 20 | 0.5 | 14 | x |
| Capex (as % rev) | 3 | 8 | 0.1 | 5 | % |

## Energy / Oilfield Services (BKR pattern)

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Revenue Growth | -5 | 15 | 0.5 | 5 | % |
| EBITDA Margin | 15 | 25 | 0.5 | 18 | % |
| FCF Conversion | 50 | 90 | 1 | 70 | % |
| Capex (as % rev) | 3 | 8 | 0.5 | 5 | % |
| Oil Price (WTI) | 50 | 120 | 5 | 75 | $/bbl |
| Gas Price (HH) | 2 | 6 | 0.25 | 3.5 | $/mcf |
| Interest Rate | 3 | 7 | 0.25 | 5 | % |
| Tax Rate | 18 | 28 | 1 | 22 | % |
| Shares Outstanding | — | — | — | — | M |
| EV/EBITDA Multiple | 6 | 14 | 0.5 | 10 | x |
| FCF Yield Target | 4 | 10 | 0.5 | 6 | % |
| Shareholder Return | 40 | 80 | 5 | 60 | % |

## SaaS / Software (NOW pattern)

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Subscription Rev Growth FY+2 | 12 | 28 | 0.5 | 20 | % |
| Growth Deceleration | 0 | 5 | 0.5 | 2 | %pts/yr |
| Professional Services Growth | -5 | 15 | 1 | 5 | % |
| Non-GAAP Op Margin (Exit) | 28 | 42 | 0.5 | 35 | % |
| FCF Margin (Exit) | 30 | 48 | 0.5 | 40 | % |
| AI/New Product ACV Uplift | 0 | 8 | 0.5 | 3 | % |
| D&A Growth | 5 | 30 | 1 | 15 | %/yr |
| WACC | 7 | 14 | 0.25 | 10 | % |
| Terminal Growth | 2 | 6 | 0.25 | 4 | % |
| EV/Revenue Multiple | 5 | 20 | 0.5 | 10 | x |
| Shares Outstanding | — | — | — | — | B |
| Tax Rate | 10 | 25 | 0.5 | 15 | % |

**Key calc:** Revenue = Sub Rev + PS Rev. FCF = Total Rev * FCF Margin. EPS = (OI * (1-tax)) / shares. Rule of 40 = Rev Growth + FCF Margin.

**SOTP approach:** Value core platform (EV/Revenue) + AI/new product business (ACV * high-growth multiple) separately. SaaS companies often have net cash (add back).

**Presets should include:** Bull, Base, Street (consensus), Bear. Street preset uses company guidance for near-term and consensus for out-years.

## Mega-Cap Tech / Cloud (AMZN pattern)

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Cloud/AWS Peak Growth FY+2 | 20 | 45 | 0.5 | 34 | % |
| Cloud Growth Decel | 2 | 10 | 0.5 | 5 | %pts/yr |
| Retail/Other Rev Growth | 3 | 16 | 0.5 | 9 | % |
| Cloud Op Margin (Exit) | 28 | 45 | 0.5 | 38 | % |
| Retail Op Margin (Exit) | 3 | 12 | 0.25 | 7 | % |
| D&A Growth | 12 | 40 | 1 | 25 | %/yr |
| Capex FY+2 ($B) | — | — | 5 | — | $B |
| Capex FY+3 Growth | -10 | 30 | 1 | 12 | % |
| Tax Rate | 8 | 25 | 0.5 | 13 | % |
| WACC | 7 | 14 | 0.25 | 10 | % |
| Terminal Growth | 2 | 5 | 0.25 | 3.5 | % |
| EV/EBITDA Multiple | 8 | 22 | 0.5 | 14 | x |

**Key calc:** Segment-level revenue and OI. EBITDA = OI + D&A. FCF = EBITDA - Capex - Taxes. Multi-year capex cycle with bend-down in FY+4.

**SOTP approach:** Value cloud (EV/Revenue), retail (OI multiple), advertising (EV/Revenue), equity stakes (broken out individually with sliders). These companies often have significant equity investments in private companies.

**Presets:** Must include "Street" for consensus estimates. Bear case should not be extreme — these are mega-caps with diversified revenue.

## Financials / Banks

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Net Interest Income Growth | -5 | 15 | 0.5 | 4 | % |
| Non-Interest Income Growth | -10 | 20 | 1 | 3 | % |
| Net Interest Margin | 2.0 | 4.5 | 0.05 | 3.2 | % |
| Efficiency Ratio | 50 | 75 | 0.5 | 60 | % |
| Provision for Credit Losses (% loans) | 0.2 | 2.0 | 0.1 | 0.5 | % |
| Loan Growth | -5 | 15 | 0.5 | 5 | % |
| CET1 Ratio | 9 | 15 | 0.25 | 11 | % |
| Dividend Payout Ratio | 20 | 60 | 5 | 35 | % |
| Buyback Yield | 0 | 5 | 0.5 | 2 | % |
| P/TBV Multiple | 0.5 | 3.0 | 0.1 | 1.5 | x |
| P/E Multiple | 6 | 18 | 0.5 | 10 | x |
| Tax Rate | 15 | 28 | 1 | 21 | % |

**Key calc:** NII = Avg Earning Assets * NIM. Pre-provision profit = NII + Non-II - Expenses. Net income = PPP - Provisions - Taxes. ROE = NI / Avg Equity. ROTCE = NI / Avg TCE. Book value accretes with retained earnings.

**SOTP approach:** Value core banking (P/TBV on tangible book), wealth management (P/E premium), insurance (if applicable). Banks are valued on book, not revenue.

## Healthcare / Pharma

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Base Revenue Growth | -5 | 15 | 0.5 | 5 | % |
| Pipeline Probability Adj. | 10 | 80 | 5 | 40 | % |
| Peak Revenue New Drug ($B) | 0 | 15 | 0.5 | 3 | $B |
| Years to Peak Revenue | 2 | 8 | 1 | 5 | yrs |
| Gross Margin | 60 | 90 | 0.5 | 75 | % |
| R&D as % Revenue | 10 | 30 | 1 | 18 | % |
| SG&A as % Revenue | 15 | 35 | 1 | 25 | % |
| Patent Cliff Impact (%) | 0 | 30 | 1 | 10 | % |
| Tax Rate | 10 | 25 | 1 | 16 | % |
| EV/EBITDA Multiple | 8 | 22 | 0.5 | 14 | x |
| P/E Multiple | 10 | 30 | 0.5 | 18 | x |
| WACC | 8 | 13 | 0.25 | 9.5 | % |

**Key calc:** Revenue = Base Business * (1 + growth) - Patent Cliff + Pipeline * Probability. EBITDA = Revenue * (Gross Margin - R&D% - SG&A%). Pipeline probability-adjusted rNPV for new drugs.

**SOTP approach:** Value base business (P/E), pipeline at risk-adjusted NPV per drug, cash/debt separately. Pharma SOTP is standard because pipelines are often worth more than the base business.

## Consumer / Retail

| Slider | Min | Max | Step | Default | Unit |
|--------|-----|-----|------|---------|------|
| Same-Store Sales Growth | -5 | 10 | 0.5 | 3 | % |
| New Store Growth | 0 | 8 | 0.5 | 2 | % |
| Gross Margin | 25 | 55 | 0.5 | 38 | % |
| SG&A as % Revenue | 18 | 35 | 0.5 | 25 | % |
| E-Commerce Mix | 5 | 50 | 1 | 20 | % |
| E-Commerce Growth | 5 | 40 | 1 | 15 | % |
| Capex as % Revenue | 2 | 8 | 0.5 | 4 | % |
| Tax Rate | 18 | 28 | 1 | 23 | % |
| Shares Outstanding | — | — | — | — | M |
| EV/EBITDA Multiple | 6 | 18 | 0.5 | 11 | x |
| P/E Multiple | 10 | 30 | 0.5 | 18 | x |
| Dividend Yield | 0 | 5 | 0.25 | 1.5 | % |

**Key calc:** Revenue = (Existing Stores * SSS) + (New Stores * Avg Revenue) + E-Commerce. EBITDA = Revenue * (Gross Margin - SG&A%). FCF = EBITDA - Capex - Taxes.

**SOTP approach:** Value brick-and-mortar (EV/EBITDA), e-commerce (EV/Revenue at premium for growth), private label brands (if applicable). Retail is valued on earnings, not revenue.

## Default / Generic

For SaaS/software companies, use the SaaS template above. For mega-cap tech with cloud + retail segments, use the Mega-Cap Tech template. The default template below is for sectors that don't match any specific pattern.

For sectors without a specific template, use a generic EPS growth model:
- Revenue growth, margin expansion, multiple expansion as primary drivers
- 12 sliders split across revenue, profitability, balance sheet, and valuation
