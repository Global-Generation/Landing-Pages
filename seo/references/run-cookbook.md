# Run Cookbook — типовые сценарии

> Рецепты «что и в каком порядке запускать» для конкретных ситуаций.

---

## Cold start (первый раз после setup)

Условия: новый сайт, MCP подключены, ничего ещё не прогоняли.

```
1. /seo-crawl                  ← реальные meta + sitemap проверка (5 мин)
2. /seo-gsc                    ← baseline impressions/clicks/queries (10 мин)
3. /seo-semrush                ← baseline traffic/keywords/AS (10 мин)
4. /seo-keywords               ← research primary kw кандидатов (15 мин)
5. — review кандидатов руками —
6. /seo-keywords apply         ← апплай выбранных (5 мин)
7. /seo-diagnose               ← root cause анализ (15 мин)
8. /seo-gaps                   ← конкуренты + opportunities (15 мин)
9. /seo-audit                  ← приоритизация (10 мин)
```

**Итого**: ~ 1.5-2 часа в Claude + ручной review между этапами.

**Output**: `seo/mapping.md` полностью заполнен реальными данными, есть Top Priorities, KEYWORDS.md обновлён.

---

## Daily routine (5 мин, утром с кофе)

```
/seo-daily
```

- Index status, top queries/pages 7d, sitemap coverage
- Apend в `seo/DAILY-LOG.md`
- 3-5 рекомендаций на день

**Когда не запускать:** в выходные / отпуск (всё равно прогон будет, но никто не прочитает).

---

## Weekly cycle (пятница, 30 мин)

```
1. /seo-gsc                    ← обновляем GSC данные за 7-28 дней
2. /seo-weekly                 ← синтез digest для команды
```

**Output**: digest на русском в `seo/reports/seo-weekly-YYYY-MM-DD.md` для копи-пейста в Slack/Notion.

---

## Monthly cycle (1-е число, 1.5 часа)

```
1. /seo-crawl                  ← убедиться что mapping актуален
2. /seo-gsc                    ← месячные данные
3. /seo-semrush                ← месячный snapshot + 12mo trend
4. /seo-diagnose               ← если есть просадки в trends
5. /seo-audit                  ← пересмотр приоритетов
6. /seo-report                 ← полный отчёт
```

**Output**: `seo/reports/seo-report-YYYY-MM-DD.md` для команды/инвесторов.

---

## Quarterly review (раз в 3 месяца, 2 часа)

```
1. весь monthly cycle
2. /seo-gaps                   ← конкуренты + новые opportunities
3. — review COMPETITORS.md руками —
4. — review PLAN.md, обновить milestones —
```

**Output**: COMPETITORS.md обновлён + PLAN.md актуализирован + decision points решены.

---

## После деплоя страницы (5 мин)

Когда меняли `<title>`, `<meta>`, JSON-LD, robots, sitemap, или контент:

```
/seo-crawl /
```

(аргумент `/` или конкретный path для проверки только этой страницы)

Проверит что live соответствует ожиданиям, флагнет регрессии.

---

## После смены primary keyword

```
1. /seo-keywords apply          ← применить новый primary
2. — обновить page62702763.html руками: title, og:title, hero copy —
3. — задеплоить —
4. /seo-crawl                   ← убедиться что live изменения видны
5. /seo-audit                   ← обновить Top Priorities
```

---

## Если страница пропала из индекса

```
1. /seo-crawl /                 ← проверить noindex / canonical / 5xx
2. — если crawl показал noindex → проверить page62702763.html и Tilda settings
3. /seo-daily                   ← coverage state в GSC
4. — в GSC URL Inspection → Request indexing
5. /seo-diagnose /              ← root cause если crawl чистый
```

---

## Если пришёл странный трафик из новой страны

```
1. /seo-gsc                     ← посмотреть country breakdown
2. — оценить относительность (релевантна ли новая аудитория)
3. — если да: добавить hreflang / locale-specific landing —
4. — если нет: фильтровать в анализе, не дёргаться —
```

---

## Если SEMrush показывает падение позиций

```
1. /seo-crawl                   ← может что-то сломалось технически
2. /seo-diagnose                ← top-3 SERP comparison — возможно конкурент пробился
3. /seo-gaps                    ← их новые keywords
4. — если ранжирующая URL изменилась → cannibalization, проверить
5. /seo-audit                   ← пересмотреть Issues — что чинить
```

---

## Если Google уведомил о Manual Action

⚠️ Серьёзный инцидент. По шагам:

```
1. — прочитать письмо из GSC внимательно (что именно нарушено) —
2. — НЕ запускать /seo-* до понимания —
3. — собрать команду: что и когда меняли, что могло вызвать
4. — устранить причину (thin content / spammy backlinks / hidden text / etc.) —
5. — submit Reconsideration Request в GSC —
6. — после snятия: /seo-audit чтобы перепрофилировать
```

---

## Если домен только что трансфернули (как у нас 2026-04-27)

```
1. — проверить NS пропагацию (`dig +short NS domain @8.8.8.8`)
2. — после полной пропагации (24-48ч) — /seo-crawl
3. /seo-daily 7 дней подряд      ← ждём баазa данных в GSC
4. через неделю /seo-gsc + /seo-semrush для baseline
5. — отменить старый hosting (SiteGround / etc.)
```
