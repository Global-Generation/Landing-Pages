---
description: Подбирает keyword candidates для страниц где primary не ранжируется или intent не совпадает. Research mode → пишет 📋 Pending в mapping; apply mode → переключает primary keyword и ставит ✅ Applied.
argument-hint: [optional "apply" чтобы применить уже одобренные кандидаты]
---

# /seo-keywords — Keyword research + apply

**Target file:** `seo/mapping.md`
**Aux output:** `seo/reports/seo-keywords-YYYY-MM-DD.md`
**Argument:** `$ARGUMENTS` — пусто = research mode; `apply` = применить кандидаты со статусом `📋 Pending approval`.
**Requires:** MCP `gg-semrush`.

---

## Mode: Research (default)

### Step 1: Найти страницы для research

Из `seo/mapping.md` — страницы где:
- `Diagnosis:` содержит `🎯 Intent mismatch` или `⚠️ Keyword not searchable`, ИЛИ
- В Keywords table primary status = `🔍 Pending validation`, ИЛИ
- В SEMrush блоке primary kw pos > 100 (not ranking) при том что vol > 0.

### Step 2: Seed-based research per page

Для каждой такой страницы:

1. Из её mapping вытащить тему / описание / текущий keyword.
2. Сформулировать 2-3 seed-фразы. Для лендинга global-generations.us примеры:
   - «поступление в США»
   - «университеты США стипендии»
   - «помощь поступить в вуз США»
3. Для каждого seed:
   ```
   mcp__gg-semrush__execute_report(report="phrase_related", phrase=seed, database="ru", display_limit=50)
   mcp__gg-semrush__execute_report(report="phrase_organic", phrase=seed, database="ru", display_limit=10)
   ```
4. Если есть текущий keyword — взять `phrase_related` для него тоже (понять semantic neighborhood).

### Step 3: Filter + rank candidates

Filter:
- Vol ≥ 100 (можно ниже для long-tail если нет очевидных кандидатов)
- KD ≤ 55 (домен молодой, DR низкий)
- SERP intent: B2C educational. Дисквалифицировать если SERP top-5 = форумы / реддит / абсолютная навигация.
- Не дублирует уже primary/secondary на этой странице.

Rank by:
1. Vol × ICP relevance (high relevance — приоритет)
2. KD (lower = better)
3. SERP openings (есть ли top-10 не из forbes/quora — реальный шанс)

### Step 4: SERP intent check

Для top-5 кандидатов на каждой странице — pull `phrase_organic` top-5 SERP. Глазами оценить:
- Это commercial / informational / navigational?
- Совпадает ли с целью лендинга (B2C, конверсия в консультацию)?

### Step 5: Записать в mapping

В каждой целевой странице добавить блок:

```
**Keyword candidates** ({YYYY-MM-DD}):
- 📋 Pending approval — recommended: "<kw>" (vol: V, KD: K, SERP: commercial)
- 📋 Pending — alt 1: "<kw2>" (vol: V, KD: K)
- 📋 Pending — alt 2: "<kw3>" (vol: V, KD: K)

Reasoning: <1-2 строки почему этот kw подходит лучше текущего>
```

**Не менять `| Primary |` в Keywords table** — research только предлагает.

### Step 6: Dated report

`seo/reports/seo-keywords-YYYY-MM-DD.md` — таблица всех кандидатов по всем страницам, sorted by recommended.

---

## Mode: Apply

Запуск: `/seo-keywords apply`.

### Step 1: Найти `📋 Pending approval`

Сканировать `seo/mapping.md` на строки `📋 Pending approval — recommended:`.

Lev предварительно может отредактировать кандидатов вручную (выбрать другой alt) — apply берёт строку с `recommended` либо помеченную `✅ chosen`.

### Step 2: Apply

Для каждой:
1. Обновить `| Primary |` в Keywords table странички — заменить keyword/vol/KD на новые значения.
2. Заменить статус `📋 Pending approval` → `✅ Applied {YYYY-MM-DD}`.
3. Добавить в `### Open Issues` пункт `[ ] /path: после смены primary kw — обновить <title>, og:title, hero copy в page62702763.html` (это требует Tilda-перекомпиляции либо ручной правки).

### Step 3: Update SEO Health header

`**Keywords:** {YYYY-MM-DD}` в `## SEO Health`.

### Step 4: Output

```
# Keywords Applied — {YYYY-MM-DD}

## /
- Primary: "<old>" → "<new>" (vol: V, KD: K)
- Reasoning: ...
- Next: обновить on-page контент

## ...
```

---

## Notes

- Research НИКОГДА не меняет primary keyword без explicit `apply`.
- Apply триггерит TODO для on-page обновления — keyword change бесполезен без обновления title/H1/контента.
- Для лендинга в основном будет интересна одна группа keywords — research можно делать раз в квартал.
