# Design System — Stock Site Portals

## Color Palette (CSS Variables)

```css
:root {
  --bg: #FFFFFF;
  --surface: #F8F9FB;
  --surface-2: #F1F3F6;
  --navy: #0F1729;
  --navy-soft: #1E3A5F;
  --border: #E2E5EB;
  --border-light: #ECEEF2;
  --gold: #B8973E;
  --gold-soft: rgba(184,151,62,0.08);
  --gold-hover: #9A7D2E;
  --steel: #2C5F7C;
  --green: #1A7A3A;
  --red: #C0392B;
  --forest: #1A5632;
  --text: #2D3748;
  --text-muted: #6B7280;
  --heading: #111827;
}
```

## Typography

- **Body/UI:** Inter (Google Fonts) — weights 400, 500, 600, 700, 800
- **Memo body:** Merriweather (serif) for long-form reading
- **Code/Numbers:** JetBrains Mono for financial tables
- Wrapper: `max-width: 1080px` (1120px for model page)

## Navigation

Fixed top nav with `backdrop-filter: blur(16px)`, `background: rgba(255,255,255,0.85)`. Adds `.scrolled` class with shadow on scroll > 40px.

```html
<nav id="nav">
  <div class="wrap">
    <div class="nav-brand">LEVIN CAPITAL STRATEGIES</div>
    <div class="nav-links">
      <a href="/lcs/{ticker}/" class="active">Overview</a>
      <a href="/lcs/{ticker}/memo.html">Memo</a>
      <a href="/lcs/{ticker}/presentation.html">Deck</a>
      <a href="/lcs/{ticker}/model.html">Model</a>
      <a href="/lcs/{ticker}/consensus.html">Consensus</a>
    </div>
    <div class="nav-right">
      <span class="nav-ticker">NYSE: <strong>{TICKER}</strong> ${price}</span>
      <a href="/lcs/{ticker}/memo.html" class="nav-cta">View Memo</a>
    </div>
  </div>
</nav>
```

## Hero / Cover Section Pattern

**IMPORTANT: Use a CLEAN WHITE/LIGHT hero -- NOT a dark navy background.**
The design should be light, readable, and professional like a Morgan Stanley fact sheet.
No Unsplash background images. No dark gradient overlays.

```css
.hero {
  background: #FFFFFF;
  border-bottom: 1px solid var(--border);
  padding: 48px 0 32px;
}
.hero .wrap {
  max-width: 1080px;
  margin: 0 auto;
  padding: 0 24px;
}
.hero-eyebrow {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 3px;
  color: var(--gold);
  font-weight: 600;
  margin-bottom: 8px;
}
.hero h1 {
  font-size: 36px;
  font-weight: 800;
  color: var(--heading);
  line-height: 1.15;
  margin-bottom: 12px;
}
.hero-subtitle {
  font-size: 16px;
  color: var(--text-muted);
  line-height: 1.6;
  max-width: 700px;
}
```

Stat boxes on white background:
```css
.cover-stat {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 18px 14px;
  text-align: center;
}
.cover-stat .stat-value {
  font-size: 22px;
  font-weight: 700;
  color: var(--heading);
}
.cover-stat .stat-label {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: var(--text-muted);
  margin-top: 4px;
}
```

Recommendation badge:
```css
.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: rgba(26,122,58,0.08);
  border: 1px solid rgba(26,122,58,0.2);
  color: var(--green);
  border-radius: 6px;
  padding: 8px 18px;
  font-weight: 600;
  font-size: 13px;
}
```

**Overall page feel:** Clean white backgrounds, navy text, gold accent lines/badges,
subtle gray borders, plenty of whitespace. Think institutional fact sheet, not
fintech dashboard. Every page should be comfortable to print.

## Metrics Bar

7-column grid with 1px gap, rounded corners, shadow:
```css
.metrics {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 1px;
  background: var(--border);
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 1px 8px rgba(0,0,0,0.04);
}
```

## Slider Pattern (Model Page)

```css
input[type=range] {
  flex: 1;
  accent-color: var(--gold);
  height: 6px;
  cursor: pointer;
  min-width: 100px;
}
```

Assumptions panel: `.assumptions-panel` with surface background, `.assumptions-grid` in 3-column layout.

## Sensitivity Table

```css
.sensitivity-2d .cell-gold {
  background: var(--gold-soft);
  color: var(--gold);
  font-weight: 800;
  box-shadow: inset 0 0 0 2px var(--gold);
}
```

## Footer

```html
<footer>
  <div class="footer-line"></div><!-- 40px gold bar -->
  <p>Levin Capital Strategies • {Company} ({TICKER}) Equity Research Portal</p>
  <p>Sources: ... For internal use only.</p>
</footer>
```

## Responsive Breakpoints

- 1000px: Assumptions grid → 2 columns
- 900px: Charts → 1 column, metrics → 3 columns, hide nav links
- 600px: Metrics → 2 columns, hide ticker
