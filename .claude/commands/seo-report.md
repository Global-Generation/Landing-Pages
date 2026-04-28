---
description: Месячный SEO-отчёт. Domain health MoM, ranking buckets, wins/regressions, top-5 priorities на следующий месяц. Brief mode для 3-строчного summary.
argument-hint: [optional "brief" = 3 строки + статус]
---

# /seo-report — Monthly SEO report

**Target file:** `docs/seo_mapping.md` (обновляет Domain Metrics + Recovery Status в SEO Health)
**Aux output:** `docs/seo-report-YYYY-MM-DD.md`
**Template:** `references/report-template.md`
**Argument:** `$ARGUMENTS` — `brief` для краткого режима.
**Requires:** ничего внешнего — реюзает existing data.

---

## Step 1: Available sources

Объявить в начале отчёта какие источники доступны:
- ✅ SEMrush — last sync {date}
- ✅ GSC — last sync {date}
- ⚠️ если данные старше 14 дней — флагать

## Step 2: Domain health MoM

Из `docs/seo-trends.md` достать строки этого месяца и прошлого. Посчитать Δ:

| Metric | Этот месяц | Прошлый | Δ |
|---|---|---|---|
| Organic traffic | ... | ... | +X% |
| Organic keywords | ... | ... | ... |
| Domain rank | ... | ... | ... |
| Impressions (28d) | ... | ... | ... |
| Clicks (28d) | ... | ... | ... |
| Avg pos | ... | ... | ... |
| CTR | ... | ... | ... |

## Step 3: Ranking buckets

Из последнего SEMrush-снапшота (`docs/seo-semrush-{latest}.md` либо `docs/seo_mapping.md`):

| Bucket | Кол-во keywords | Δ vs прошлый месяц |
|---|---|---|
| Top-3 | ... | ... |
| Top-10 | ... | ... |
| Page 2 (11–20) | ... | ... |
| Buried (21–50) | ... | ... |
| Not ranking (>50) | ... | ... |

## Step 4: Wins / Regressions

**Wins** (улучшение позиции ≥ 3 пунктов):
- `<keyword>` — pos N → M (Δ +K)
- ...

**Regressions** (просадка ≥ 3 пунктов):
- `<keyword>` — pos N → M (Δ -K) — root cause: ...

## Step 5: Top 5 priorities на месяц

Pull из `### Top Priorities` секции `seo_mapping.md` (заполняется `/seo-audit`). Top 5 нерешённых.

Если `### Top Priorities` пустой → нагенерить из:
- 🔴 страницы с `Diagnosis: 🚫 Not indexed`
- 🥈 page-2 opportunities
- 📉 CTR problems
- 🎯 Intent mismatches
- Quick wins из последнего audit

## Step 6: What changed in the site

`git log --since="1 month ago" --oneline` — короткий список деплоев. Бывшие тех. события (sitemap clean-up, schema added, etc.).

## Step 7: Watch list

Risk сигналы:
- Тренд traffic вниз 2+ месяца подряд
- Coverage drop в GSC
- Cannibalization (несколько pages ранжируются по одному kw — у нас single landing, обычно нерелевантно)

## Step 8: Update mapping

Обновить в `seo_mapping.md → ## SEO Health`:
- `### Domain Metrics` — текущие значения
- `**Report:** {YYYY-MM-DD}`

## Step 9: Brief mode

Если `$ARGUMENTS = brief`:

```
# SEO Brief — {YYYY-MM-DD}

Health: {📈|📉|➖} Traffic {N} (Δ +X%), Keywords {N}, Impressions {N}.

Pages: 1 indexed, 0 noindex, 0 errors.

Top 3 priorities:
1. ...
2. ...
3. ...
```

## Step 10: Save + print

Полный отчёт → `docs/seo-report-YYYY-MM-DD.md`. Brief — только в чат, файл не пишем.

---

## Notes

- Запускать в первый рабочий день месяца.
- `/seo-report` не делает API-вызовов — реюзает данные из dated отчётов и mapping. Поэтому перед run: убедиться что свежий `/seo-gsc` и `/seo-semrush` за этот месяц был.
- Brief mode полезен в чате при ad-hoc вопросе «как у нас дела по SEO».
