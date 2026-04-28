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
- `robots.txt` / `sitemap.xml` — SEO (sitemap currently has 6 dead URLs — see `docs/seo_mapping.md → Top Priorities`)
- `css/`, `js/`, `images/` — shared Tilda assets (do not delete randomly — main page references them)
- `.claude/commands/seo-*.md` — slash commands for the SEO toolkit
- `docs/seo_mapping.md` — central SEO source-of-truth (read first when working on SEO)
- `references/MCP-SETUP.md` — how to wire the `gg-search-console` / `gg-semrush` MCPs

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

## SEO commands (TL;DR)

10 slash-команд в `.claude/commands/`:
- `/seo-crawl` — sync live page meta into mapping (после деплоя)
- `/seo-gsc` — pull GSC data (impressions, clicks, top queries) — еженедельно
- `/seo-semrush` — pull SEMrush (traffic, keywords, position) — ежемесячно
- `/seo-keywords` — research / apply primary keywords
- `/seo-diagnose` — root cause анализ (почему не ранжируется)
- `/seo-gaps` — конкуренты + keyword gap analysis (раз в квартал)
- `/seo-audit` — приоритизация фиксов (Impact × Effort)
- `/seo-daily` — daily routine (5 мин) — индексация + top queries
- `/seo-weekly` — пятничный digest для команды
- `/seo-report` — месячный отчёт + brief mode

**Source-of-truth:** `docs/seo_mapping.md`. **Run order для нового сайта:** crawl → gsc → semrush → keywords → diagnose → audit.

### Текущий статус MCP (2026-04-27)

- `gg-search-console` — ⏳ pending OAuth (нужен refresh token, см. `references/MCP-SETUP.md`)
- `gg-semrush` — ⏳ **deferred** — Lev купит SEMrush API позже

**Работает без MCP сразу:** `/seo-crawl`, `/seo-audit`, `/seo-weekly`, `/seo-report`.
**Ждёт OAuth для GSC:** `/seo-gsc`, `/seo-daily`.
**Ждёт SEMrush:** `/seo-semrush`, `/seo-keywords`, `/seo-diagnose`, `/seo-gaps`.

## GOTCHA

- `DYLD_LIBRARY_PATH=/opt/homebrew/opt/expat/lib` prefix required for `aws` CLI (Python 3.14 + macOS Tahoe expat bug)
- This is a Tilda export — HTML is machine-generated, not hand-written. Edit carefully.
- CSS/JS files include assets for pages that no longer exist in the repo — they're shared Tilda bundles, safe to keep.
- **Push access**: `LevAvdoshin-Truv` has no push rights — `gh auth switch --user LevAvdoshin` before pushing.
- **Sitemap mismatch**: `sitemap.xml` ссылается на 7 URL но 6 из них (`/usa`, `/services`, `/premium`, `/faq`, `/reviews`, `/company`) дают 404. Будет почищено первым прогоном `/seo-crawl`.
- **Title currently `РЕМАРКЕТИНГ`** — это test placeholder (см. историю миграции 2026-04-27). Перед публичным релизом NS-пропагации вернуть нормальный title.
