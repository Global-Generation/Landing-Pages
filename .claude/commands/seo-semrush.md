---
description: Тянет данные SEMrush для global-generations.us — domain rank, organic keywords, traffic, 12-мес тренд, per-page primary keyword position. Пишет в seo/mapping.md и seo/trends.md.
argument-hint: [optional URL path для одной страницы]
---

# /seo-semrush — Pull SEMrush data into mapping

**Target file:** `seo/mapping.md`
**Aux outputs:** `seo/trends.md`, `seo/reports/seo-semrush-YYYY-MM-DD.md`
**Argument:** `$ARGUMENTS` — optional URL path (default = все страницы).
**Requires:** MCP `gg-semrush`.

**Database:** `ru` по умолчанию. Если для primary keyword нужен другой регион (например, `kz`) — задать override в команде.

---

## Step 1: Schema sanity

```
mcp__gg-semrush__get_report_schema()
```

Чтобы убедиться что MCP жив и какие отчёты доступны.

## Step 2: Domain snapshot

```
mcp__gg-semrush__domain_rank(domain="global-generations.us", database="ru")
mcp__gg-semrush__domain_rank_history(domain="global-generations.us", database="ru", display_limit=12)
```

Извлечь: traffic (visits/month), keywords count, rank/AS, backlinks (если возвращает).

## Step 3: Bulk organic keywords

```
mcp__gg-semrush__domain_organic(domain="global-generations.us", database="ru", display_limit=1000)
```

Сохранить в памяти для шага 4 (один большой вызов вместо N маленьких).

## Step 4: Per-page primary keyword position

Для каждой страницы из mapping:

1. Достать `Primary keyword` из её Keywords table.
2. Найти этот keyword в bulk-результатах из шага 3 (case-insensitive).
3. Если найден → вытащить position, traffic %, search volume, KD, URL (что ранжируется).
4. Если не найден → pos = `> 100 / not ranking`.

## Step 5: Update mapping

Для каждой страницы обновить:

```
**SEMrush:** ({YYYY-MM-DD}, db=ru) Traffic share: X% · Organic keywords: N · Primary kw "<kw>" pos: X (vol: V · KD: K) · Ranking URL: <url>
```

Для primary keyword на странице — заполнить Volume и KD в Keywords table.

В `## SEO Health → Domain Metrics`:
- `Organic traffic`, `Organic keywords`, `Domain rank` — текущие + Δ MoM.

`**SEMrush sync:** {YYYY-MM-DD}` в SEO Health.

## Step 6: Trends

В `seo/trends.md`:

```
| {YYYY-MM} | {traffic} | {keywords} | {rank} | SEMrush |
```

И страничный тренд для primary keyword `/`:

```
| {YYYY-MM} | / pos: X | ... |
```

## Step 7: Dated report

`seo/reports/seo-semrush-YYYY-MM-DD.md`:

```
# SEMrush Sync — {YYYY-MM-DD}

Database: ru
Domain: global-generations.us

## Domain snapshot
- Traffic: N visits/month (Δ ...)
- Keywords: N (Δ ...)
- Authority Score / Rank: X

## 12-month trend
| Month | Traffic | Keywords | Rank |
|---|---|---|---|
| ... | ... | ... | ... |

## Top 20 organic keywords (по traffic)
| Keyword | Pos | Vol | KD | Traffic % | Ranking URL |
|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... |

## Per-page
### /
- Primary kw "<kw>" pos: X (vol: V, KD: K)
- Other ranking keywords: N
- Top 5 ranking keywords: ...
```

---

## Notes

- SEMrush database `ru` — для русскоязычных запросов (RU + соседние страны). Если позже сайт начнёт целить КЗ-аудиторию отдельно — есть `kz`.
- Primary keyword в mapping должен быть на том же языке что и database. Если в mapping primary — английская фраза, а db=ru — pos будет "not ranking" даже если ранжируется в US database. Решение: дополнительная колонка `database` в Keywords table или менять primary.
- Bulk-фетч в шаге 3 ограничен 1000 строк — для большого сайта можно пагинировать. Для текущего лендинга 1 страница — лимита хватает.
