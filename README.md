# Landing Pages — global-generations.us

Remarketing landing page for Global Generation Google Ads campaigns.

## Stack

- **Source**: Tilda export (static HTML)
- **Hosting**: S3 `global-generations-us-site` → CloudFront `dixmq96bmzcjx.cloudfront.net`
- **Domain**: `global-generations.us` (migrating from SiteGround → Route 53)
- **CloudFront ID**: `EXJ3W80LP8A65`
- **Route 53 Zone**: `Z054617150JR83K8E03X`

## Structure

```
page62702763.html   — Main landing page (/)
404.html            — Error page
robots.txt          — Search engine directives
sitemap.xml         — Sitemap (7 URLs)
css/                — Tilda stylesheets
js/                 — Tilda scripts
images/             — All images (~370 files)
files/              — Page body fragments
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

## Domain Migration Status

- **From**: SiteGround (Tucows/OpenSRS, IP `34.174.111.243`)
- **To**: AWS Route 53
- **Status**: `pendingTransfer` since 2026-04-22
- **Domain expires**: 2026-05-15
- After transfer: NS auto-switch to AWS, verify SSL, cancel SiteGround
