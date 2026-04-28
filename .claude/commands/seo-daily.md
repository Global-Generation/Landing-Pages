---
description: Ежедневная SEO-рутина — индексация, top-5 queries, top-5 pages, sitemap coverage, рекомендации на день. Append в docs/seo-daily-log.md. 5-минутная команда.
argument-hint: [нет]
---

# /seo-daily — Daily SEO routine

**Target file:** `docs/seo-daily-log.md` (append)
**Requires:** MCP `gg-search-console`. SEMrush опционален.

---

## Step 1: Index status check

```
mcp__gg-search-console__get_index_status(site="https://global-generations.us/", url="https://global-generations.us/")
```

Получить:
- Coverage state (Submitted and indexed / Discovered – currently not indexed / Crawled – currently not indexed / blah)
- Last crawl
- Indexing state
- Page fetch state
- Robots verdict

## Step 2: Sitemap check

```
mcp__gg-search-console__get_sitemaps(site="https://global-generations.us/")
```

Для каждой sitemap — submitted vs indexed vs errors.

## Step 3: Last 7 days performance

```
mcp__gg-search-console__advanced_search_analytics(
  site="https://global-generations.us/",
  startDate=today-7d, endDate=today-2d,
  dimensions=[]
)
```

Totals + top-5 queries + top-5 pages (отдельные вызовы с `dimensions=["query"]` и `dimensions=["page"]`).

## Step 4: Сравнить с прошлым днём

Прочитать `docs/seo-daily-log.md` — вытащить вчерашние числа. Посчитать Δ:
- Impressions Δ
- Clicks Δ
- Avg pos Δ
- Coverage state — изменилось?

## Step 5: Recommendations

На основе сегодняшних чисел сгенерить 3-5 коротких рекомендаций:

- 📌 Если top query — branded → добавить «обзор» статью под non-branded
- 📌 Если CTR < 1% при impressions > 100 — переписать title/description
- 📌 Если "Crawled - currently not indexed" → submit to indexing manually
- 📌 Если sitemap errors → fix URLs
- 📌 Если Δ impressions > -20% за день → flag

## Step 6: Append в log

```markdown
---

## {YYYY-MM-DD}

### Index status
- Coverage: ...
- Last crawl: ...
- Robots: ...

### Sitemap
- {sitemap-url}: submitted N, indexed N, errors N

### Performance (7d, ending {date})
- Impressions: N (Δ ...)
- Clicks: N (Δ ...)
- Avg pos: X (Δ ...)
- CTR: X% (Δ ...)

### Top 5 queries
1. ... — impr / clicks / pos
2. ...

### Top 5 pages
1. ...

### Recommendations for today
- 📌 ...
- 📌 ...
```

---

## Notes

- Цель: 5 минут, выпил кофе → знаешь что в SEO происходит.
- Не делает дорогих SEMrush-вызовов — только GSC (бесплатный лимит).
- Если индексация ломается — это первое место где увидишь.
- Cadence: каждый рабочий день. Удобно повесить на cron / utility shortcut.
