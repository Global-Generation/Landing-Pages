# Landing Pages — global-generations.us

## What is this

Static Tilda export — remarketing landing page for Google Ads. Hosted on S3 + CloudFront.

## Deploy

```bash
# From repo root:
DYLD_LIBRARY_PATH=/opt/homebrew/opt/expat/lib aws s3 sync . s3://global-generations-us-site/ --exclude ".git/*" --exclude "*.bak" --exclude "README.md" --exclude "CLAUDE.md"
DYLD_LIBRARY_PATH=/opt/homebrew/opt/expat/lib aws cloudfront create-invalidation --distribution-id EXJ3W80LP8A65 --paths "/*"
```

## Key files

- `page62702763.html` — the only real page (main landing at `/`)
- `404.html` — error page
- `robots.txt` / `sitemap.xml` — SEO (sitemap currently has 6 dead URLs — see `seo/STATUS.md`)
- `css/`, `js/`, `images/` — shared Tilda assets (do not delete randomly — main page references them)
- `.claude/commands/seo-*.md` — slash commands for the SEO toolkit
- `seo/` — все SEO docs / state / reports / templates / MCP setup. **Точка входа: `seo/README.md`**

## Infrastructure

- S3: `global-generations-us-site`
- CloudFront: `EXJ3W80LP8A65` (`dixmq96bmzcjx.cloudfront.net`)
- Route 53 zone: `Z054617150JR83K8E03X`
- Domain: `global-generations.us` (AWS Route 53 Domains, auto-renew, expires 2027-05-15)
- GTM: `GTM-W3FBQXRC`

## Domain migration status

- **2026-04-27**: Transfer SiteGround → Route 53 complete, NS switched same day
- **NS at registrar**: `ns-1903.awsdns-45.co.uk`, `ns-345.awsdns-43.com`, `ns-816.awsdns-38.net`, `ns-1295.awsdns-33.org`
- **TODO**: cancel SiteGround hosting subscription after DNS fully propagates (24-48h from 2026-04-27)
- **60-day registrar lock** active until ~2026-06-26 (`clientTransferProhibited`/`serverTransferProhibited`) — standard post-transfer security

## SEO toolkit

Документация и state — в `seo/`. Точка входа: **`seo/README.md`**.

5 slash-команд: `/seo-crawl`, `/seo-gsc`, `/seo-semrush`, `/seo-audit`, `/seo-report`.

**Source-of-truth:** `seo/mapping.md`. **Run order:** `crawl → gsc → semrush → audit → report`.

### Status (2026-04-27)

- ✅ работает без MCP: `/seo-crawl`, `/seo-audit`, `/seo-report`
- ⏳ `/seo-gsc` — нужен OAuth (см. `seo/references/MCP-SETUP.md`)
- ⏳ `/seo-semrush` — Lev купит API позже

### При работе над SEO

1. `seo/STATUS.md` — что работает, что заблокировано.
2. `seo/mapping.md` — конкретно что чинить (Top Priorities + Open Issues + H1 fix path).
3. `seo/references/run-cookbook.md` — типовые сценарии.
4. `seo/DECISIONS.md` — посмотреть прежде чем переоткрывать решённый вопрос.

## GOTCHA

- `DYLD_LIBRARY_PATH=/opt/homebrew/opt/expat/lib` prefix required for `aws` CLI (Python 3.14 + macOS Tahoe expat bug)
- This is a Tilda export — HTML is machine-generated, not hand-written. Edit carefully.
- CSS/JS files include assets for pages that no longer exist in the repo — they're shared Tilda bundles, safe to keep.
- **Push access**: `LevAvdoshin-Truv` has no push rights — `gh auth switch --user LevAvdoshin` before pushing.
- **Sitemap mismatch**: `sitemap.xml` ссылается на 7 URL но 6 из них (`/usa`, `/services`, `/premium`, `/faq`, `/reviews`, `/company`) дают 404. Будет почищено первым прогоном `/seo-crawl`.
- **Title currently `РЕМАРКЕТИНГ`** — это test placeholder (см. историю миграции 2026-04-27). Перед публичным релизом NS-пропагации вернуть нормальный title.
