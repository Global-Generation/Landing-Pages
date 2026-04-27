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
- `robots.txt` / `sitemap.xml` — SEO
- `css/`, `js/`, `images/` — shared Tilda assets (do not delete randomly — main page references them)

## Infrastructure

- S3: `global-generations-us-site`
- CloudFront: `EXJ3W80LP8A65` (`dixmq96bmzcjx.cloudfront.net`)
- Route 53 zone: `Z054617150JR83K8E03X`
- Domain: `global-generations.us` (transfer from SiteGround pending, expires 2026-05-15)
- GTM: `GTM-W3FBQXRC`

## GOTCHA

- `DYLD_LIBRARY_PATH=/opt/homebrew/opt/expat/lib` prefix required for `aws` CLI (Python 3.14 + macOS Tahoe expat bug)
- This is a Tilda export — HTML is machine-generated, not hand-written. Edit carefully.
- CSS/JS files include assets for pages that no longer exist in the repo — they're shared Tilda bundles, safe to keep.
