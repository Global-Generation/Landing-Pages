# Landing Pages — global-generations.us

Remarketing landing page for Global Generation Google Ads campaigns.

## Stack

- **Source**: Tilda export (static HTML)
- **Hosting**: S3 `global-generations-us-site` → CloudFront `dixmq96bmzcjx.cloudfront.net`
- **Domain**: `global-generations.us` (AWS Route 53 Domains, auto-renew, expires 2027-05-15)
- **CloudFront ID**: `EXJ3W80LP8A65`
- **Route 53 Zone**: `Z054617150JR83K8E03X`

## Structure

```
page62702763.html   — Main landing page (/)
404.html            — Error page
robots.txt          — Search engine directives
sitemap.xml         — Sitemap (7 URLs — note: 6 of them currently 404, see SEO toolkit)
css/                — Tilda stylesheets
js/                 — Tilda scripts
images/             — All images (~370 files)
files/              — Page body fragments
.claude/commands/   — Slash commands for SEO toolkit (/seo-*)
seo/                — SEO documentation, state files, templates, reports — start here for SEO
```

## Deploy

```bash
# Sync to S3 (from repo root)
DYLD_LIBRARY_PATH=/opt/homebrew/opt/expat/lib \
  aws s3 sync . s3://global-generations-us-site/ \
  --exclude ".git/*" --exclude "*.bak" --exclude "README.md" --exclude "CLAUDE.md"

# Invalidate CloudFront cache
DYLD_LIBRARY_PATH=/opt/homebrew/opt/expat/lib \
  aws cloudfront create-invalidation \
  --distribution-id EXJ3W80LP8A65 --paths "/*"
```

## Google Ads

- **PMax campaign**: `GG-PMax-YT-AllAudiences` (ID: `23782556337`, $10/day, paused)
- **Landing URL**: `https://global-generations.us/`
- **UTM**: `utm_source=google&utm_medium=cpc&utm_campaign=gg-pmax-yt`
- **GTM**: `GTM-W3FBQXRC`

## Domain Migration — Complete

- **2026-04-22**: Transfer initiated SiteGround → AWS Route 53 Domains
- **2026-04-27**: Transfer completed, NS switched to Route 53
- **NS at registrar**: `ns-1903.awsdns-45.co.uk`, `ns-345.awsdns-43.com`, `ns-816.awsdns-38.net`, `ns-1295.awsdns-33.org`
- **Propagation**: 24-48h from 2026-04-27 (old NS TTL was 6h)
- **TODO**: cancel SiteGround hosting subscription after propagation
- **60-day registrar lock** (`clientTransferProhibited`/`serverTransferProhibited`) until ~2026-06-26 — standard post-transfer security

## Push access

The `LevAvdoshin-Truv` GitHub account has no push rights to this repo. Use `gh auth switch --user LevAvdoshin` before pushing.

## SEO

SEO-toolkit живёт в **[`seo/`](seo/README.md)** — это единая точка входа.

5 slash-команд (`.claude/commands/seo-*.md`):

| Команда | Когда | MCP |
|---|---|---|
| `/seo-crawl` | После каждого деплоя | none ✅ |
| `/seo-audit` | Ежемесячно | none ✅ |
| `/seo-report` | 1-е число месяца | none ✅ |
| `/seo-gsc` | Еженедельно | gg-search-console (⏳ OAuth pending) |
| `/seo-semrush` | Ежемесячно | gg-semrush (⏳ Lev купит API) |

Run order: `crawl → gsc → semrush → audit → report`.

Документация в `seo/`: `README.md`, `STATUS.md`, `mapping.md`, `AUDIT.md`, `DECISIONS.md`, `trends.md` + `references/` (MCP-SETUP, audit-checklist, report-template, run-cookbook) + `reports/` для dated outputs.
