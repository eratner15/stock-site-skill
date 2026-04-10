# Deployment — LCS Worker Pattern

## File Creation

Each page is a TypeScript template literal exported from `src/pages/{ticker}-{page}.ts`:

```typescript
export const {ticker}IndexHTML = `<!DOCTYPE html>...`;
```

Files:
- `src/pages/{ticker}-index.ts` → `{ticker}IndexHTML`
- `src/pages/{ticker}-memo.ts` → `{ticker}MemoHTML`
- `src/pages/{ticker}-presentation.ts` → `{ticker}PresentationHTML`
- `src/pages/{ticker}-model.ts` → `{ticker}ModelHTML`
- `src/pages/{ticker}-consensus.ts` → `{ticker}ConsensusHTML`

**CRITICAL ESCAPING RULES for template literals:**
- Backticks (`` ` ``) MUST be escaped as `` \` `` — otherwise the template literal terminates early
- Dollar signs (`$`) do NOT need escaping UNLESS followed by `{` — a bare `$50` is fine, only `${` triggers interpolation
- **NEVER use `\$` for dollar signs** — this renders as literal `\$` on the page. Just use `$` directly.
- The `<\/script>` closing tag must use `<\/script>` to avoid premature script block termination

## Routing

Edit `/home/eratner/lcs-portfolio-intel/src/index.ts`:

1. Add imports at top:
```typescript
import { {ticker}IndexHTML } from './pages/{ticker}-index';
// ... etc
```

2. Add routes in the `app.get('/*', ...)` catch-all, BEFORE the SPA fallback:
```typescript
if (path === '/lcs/{ticker}' || path === '/lcs/{ticker}/') return c.html({ticker}IndexHTML);
if (path === '/lcs/{ticker}/memo.html') return c.html({ticker}MemoHTML);
if (path === '/lcs/{ticker}/presentation.html') return c.html({ticker}PresentationHTML);
if (path === '/lcs/{ticker}/model.html') return c.html({ticker}ModelHTML);
if (path === '/lcs/{ticker}/consensus.html') return c.html({ticker}ConsensusHTML);
```

## Deploy Command

```bash
cd /home/eratner/lcs-portfolio-intel && source ~/.nvm/nvm.sh && nvm use 20 && \
NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt \
CLOUDFLARE_API_TOKEN=_aWVT9W6jGvJvfzdRER67eDxmGxrCxILZhqOCdHp \
CLOUDFLARE_ACCOUNT_ID=f7a9b24f679e1d3952921ee5e72e677e \
npx wrangler deploy
```

## D1 Research Entry

```bash
npx wrangler d1 execute lcs-portfolio-db --command "INSERT INTO research (ticker, company_name, sector, analyst, thesis, rating, target_price, portal_url, status, tags, firm_holding) VALUES ('{TICKER}', '{Company Name}', '{Sector}', 'LCS Research', '{thesis text}', 'BUY', {pt_number}, 'https://cafecito-ai.com/lcs/{ticker}/', 'active', '[\"tag1\",\"tag2\"]', 1)" --remote
```

## Verification

```bash
for page in "" "memo.html" "presentation.html" "model.html" "consensus.html"; do
  code=$(curl -s -o /dev/null -w "%{http_code}" "https://cafecito-ai.com/lcs/{ticker}/${page}")
  echo "${code} /lcs/{ticker}/${page}"
done
```

All should return 200.
