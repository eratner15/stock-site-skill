---
description: Update an existing stock-site portal with new earnings data, price changes, or estimate revisions
argument-hint: "[ticker]"
---

# Stock Site Update

Update an existing equity research portal with fresh data. Use after earnings reports, significant price moves, or estimate revisions.

## Workflow

### Step 1: Identify What Changed

WebSearch for the ticker to find:
- Current stock price (compare to portal's hardcoded price)
- Latest earnings results (if new quarter reported)
- Consensus estimate changes
- New analyst ratings/PTs

### Step 2: Update Portal Files

The portal pages are at `/home/eratner/lcs-portfolio-intel/src/pages/{ticker}-*.ts`

**Price updates (all 5 pages):**
- Find and replace the old price constant (e.g., `232.95`) with current price
- Update market cap computation
- Update nav ticker display

**Post-earnings updates (if new quarter):**
- Add new quarter to beat/miss history table (consensus page)
- Update FY actuals in financial tables (memo + model)
- Shift model projections forward if fiscal year rolled
- Update earnings setup box with next quarter estimates (index page)
- Refresh consensus estimates and analyst PTs

**Estimate revision updates:**
- Update consensus strip on consensus page
- Update analyst table with new PTs
- Refresh revision momentum section

### Step 3: Deploy

```bash
cd /home/eratner/lcs-portfolio-intel && source ~/.nvm/nvm.sh && nvm use 20 && \
NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt \
CLOUDFLARE_API_TOKEN=_aWVT9W6jGvJvfzdRER67eDxmGxrCxILZhqOCdHp \
CLOUDFLARE_ACCOUNT_ID=f7a9b24f679e1d3952921ee5e72e677e \
npx wrangler deploy
```

### Step 4: Update Timestamp

Change "Last updated: [old date]" to today's date in all 5 page footers.

### Step 5: Verify

Use `/browse` to confirm model still works (no JS errors, presets valid).
