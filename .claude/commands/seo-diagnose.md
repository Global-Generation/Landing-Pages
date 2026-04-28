---
description: Root cause анализ — почему страница не ранжируется. Использует SEMrush + GSC + сравнение с топ-3 SERP-конкурентами. Пишет диагноз в каждую страницу mapping.
argument-hint: [optional URL path = одна страница, default = все 🔴 priority]
---

# /seo-diagnose — Why not ranking?

**Target file:** `seo/mapping.md`
**Aux output:** `seo/reports/seo-diagnose-YYYY-MM-DD.md`
**Argument:** `$ARGUMENTS` — `/path` = одна страница; иначе все 🔴.
**Requires:** MCP `gg-semrush` + WebFetch. Опционально `gg-search-console`.

---

## Step 1: Mode declaration

В начале вывода зафиксировать:

```
Mode: A — full (SEMrush + GSC + WebFetch)
       либо
Mode: B — partial (SEMrush + WebFetch, GSC недоступен)
```

Mode A если `gg-search-console` отвечает. Иначе B.

## Step 2: Per page — classify

Для каждой целевой страницы — определить тип проблемы:

| Тип | Условия |
|---|---|
| 🚫 Not indexed | noindex=Yes ИЛИ (pos > 100 И GSC impressions = 0 И mapping ≥ 7 дней) |
| 🔍 Relevance | pos 11–100, low impressions, top GSC query совпадает с primary kw |
| 📉 CTR | many impressions, few clicks (CTR < domain avg / 2) |
| 🥈 Page 2 opportunity | pos 11–20, vol ≥ 200 |
| 🎯 Intent mismatch | top GSC query ≠ primary keyword (set difference) |
| ✅ Healthy | pos 1–10 + clicks > 0 |

Записать тип в строку `**Diagnosis:**` страницы.

## Step 3: Top-3 SERP fetch

Для primary keyword страницы:

```
mcp__gg-semrush__execute_report(report="phrase_organic", phrase="<primary kw>", database="ru", display_limit=3)
```

Извлечь: top-3 URL.

## Step 4: WebFetch competitors

Для каждого top-3 URL — `WebFetch(url, prompt="extract title, H1, meta desc, FAQ schema, word count, H2 count, page type")`.

Skip если URL = .gov, .edu, hr-портал (не наш игровой профиль), форум.

## Step 5: Compare table

Построить таблицу сравнения нашей страницы с top-3:

| Field | / (мы) | Comp #1 | Comp #2 | Comp #3 |
|---|---|---|---|---|
| Title | ... | ... | ... | ... |
| H1 | ... | ... | ... | ... |
| Word count | ... | ... | ... | ... |
| FAQ | Y/N | Y/N | Y/N | Y/N |
| H2 count | ... | ... | ... | ... |
| Page type | landing | listing/article/landing | ... | ... |
| Position | X | 1 | 2 | 3 |

## Step 6: Why we're losing

Конкретные пункты vs #1:
- "У #1 word count в 3× больше — текстового контента не хватает"
- "У #1 FAQ schema — наш landing без FAQ"
- "У #1 в title есть слово 'стипендия' — у нас нет"
- "У #1 явно informational/listing формат — нашему landing нужно либо мимикрировать под него, либо целить другой keyword"

## Step 7: Update mapping

Каждой странице добавить:

```
**Diagnosis:** {🚫|🔍|📉|🥈|🎯|✅} {описание в 1-2 строки} ({YYYY-MM-DD})

**vs Top-3:** ... (краткий выжим)
```

Перезаписать `### Key Findings` в SEO Health — top 5 причин по всем страницам.

## Step 8: Dated report

`seo/reports/seo-diagnose-YYYY-MM-DD.md`:

```
# SEO Diagnosis — {YYYY-MM-DD}

Mode: A | B

## Per-page

### /
**Diagnosis type:** 🎯 Intent mismatch
**Primary kw:** "поступление в вузы США"
**Position:** 87 (SEMrush, db=ru)
**Top GSC query for this page:** "стоимость учебы в США"

#### Top-3 SERP comparison
| ... | / | comp1 | comp2 | comp3 |

#### Why we're losing
- ...
- ...

#### Recommended fixes
- ...

---
```

---

## Notes

- Diagnose работает на existing data — перед запуском обычно гонят `/seo-crawl`, `/seo-semrush`, `/seo-gsc`.
- Если страница ✅ Healthy — не делать full top-3 анализ, просто отметить и идти дальше.
- Для лендинга обычно главный диагноз — Intent mismatch (single landing с broad-keyword проигрывает специализированным comparison-сайтам).
