# Stock-Site Autoresearch

Autonomous improvement loop for the stock-site equity research portal skill.
Adapted from [karpathy/autoresearch](https://github.com/karpathy/autoresearch).

## Setup

1. **Agree on a run tag** with the user (e.g. `apr9`).
2. **Read the in-scope files:**
   - `SKILL.md` — the skill definition (modifiable)
   - `references/design-system.md` — CSS variables, typography, component patterns
   - `references/page-structures.md` — 5-page spec
   - `references/model-templates.md` — sector-specific slider configs
   - `references/deployment.md` — routing and deploy
3. **Read the reference portals** (the "training data"):
   - AMZN: `/home/eratner/lcs-portfolio-intel/src/pages/amzn-*.ts` (gold standard)
   - NOW: `/home/eratner/lcs-portfolio-intel/src/pages/now-*.ts` (latest AI-first build)
4. **Establish baseline** by scoring both portals.
5. **Initialize results.tsv** with header row.

## Evaluation

Each portal page is scored 0-10 on 6 dimensions. The composite score is the average.

### Scoring Rubric

| Dimension | 0-3 (Poor) | 4-6 (Adequate) | 7-8 (Good) | 9-10 (Excellent) |
|-----------|------------|-----------------|-------------|-------------------|
| **Data Accuracy** | Hallucinated numbers | Most numbers correct | All numbers sourced | Every number tagged with source |
| **Completeness** | Missing sections | Core sections present | All sections + extras | Comprehensive with depth |
| **Model Functionality** | Broken JS / no interactivity | Sliders work, basic calcs | Multi-year + DCF + presets | SOTP + scenarios + balance sheet |
| **Design Quality** | Broken layout | Readable, basic styling | Matches design system | Pixel-perfect, responsive |
| **Analytical Depth** | Surface-level summary | Thesis present | Proprietary view + risks | SCQA + variant perception + catalysts |
| **Anti-Hallucination** | No sources cited | Some sources | Most numbers tagged | Full source protocol |

### How to Score

Use `/browse` to load each page and evaluate:

```bash
$B goto https://cafecito-ai.com/lcs/{ticker}/
$B screenshot /tmp/{ticker}-index-eval.png
$B console --errors
# Check model page specifically
$B goto https://cafecito-ai.com/lcs/{ticker}/model.html
$B js "typeof updateAll"  # Must be "function"
$B js "document.getElementById('v-blend').textContent"  # Must show a price
```

Score each of the 5 pages on each dimension. Record the average.

## What You CAN Modify

- `SKILL.md` — skill workflow, prompts, quality checklist
- `references/*.md` — design system, page structures, model templates
- Individual portal page files (`amzn-*.ts`, `now-*.ts`) — fix bugs, improve content
- CSS patterns, JS model logic, HTML structure

## What You CANNOT Modify

- The deployment infrastructure (`index.ts` routing, wrangler config)
- Other non-stock-site pages in the worker
- The scoring rubric itself (it's the ground truth)

## The Goal

**Maximize the composite quality score across all live portals.**

Improvements that raise the score without adding unnecessary complexity are ideal.
Simplifications that maintain the score are valuable.
Changes that lower the score on any dimension are discarded.

## The Experiment Loop

LOOP FOREVER:

1. **Score current state** — evaluate all live portals, record baseline
2. **Identify the weakest dimension** — what's dragging the score down?
3. **Form a hypothesis** — what change would improve it?
4. **Implement the change** — edit the skill file, reference, or page template
5. **Deploy** — `npx wrangler deploy`
6. **Re-score** — evaluate the change with `/browse`
7. **Keep or discard:**
   - If composite score improved → keep (advance)
   - If composite score unchanged but simpler → keep (simplification win)
   - If composite score decreased → revert (`git checkout -- <file>`)
8. **Log to results.tsv** — commit hash, score, status, description
9. **Go to step 1**

### Experiment Ideas (Seed List)

- [ ] Add source tags to all historical financials (anti-hallucination)
- [ ] Verify all management quotes against actual transcripts
- [ ] Cross-check consensus estimates with current data
- [ ] Improve responsive design on mobile
- [ ] Add more sector templates to model-templates.md
- [ ] Deepen the SOTP methodology in SKILL.md
- [ ] Add earnings surprise calculation to consensus page
- [ ] Improve chart colors and formatting
- [ ] Add print-friendly CSS for memo page
- [ ] Validate all links between pages work
- [ ] Ensure FCF/share appears in all model bridge tables
- [ ] Add Rule of 40 metric to SaaS portals
- [ ] Standardize the "Where Street is Wrong" format
- [ ] Add footnotes with source citations to memo tables
- [ ] Test all model presets produce reasonable valuations

### Logging

```
commit	score	delta	status	description
a1b2c3d	7.2	+0.0	keep	baseline (AMZN + NOW portals)
b2c3d4e	7.4	+0.2	keep	added source tags to AMZN memo financials
c3d4e5f	7.3	-0.1	discard	moved charts above sensitivity tables
```

### NEVER STOP

Once the loop begins, do NOT pause to ask the human. Run experiments indefinitely.
If you run out of ideas: re-read the portals, check the rubric for low-scoring areas,
browse competitor research portals for inspiration, or try combining improvements.
