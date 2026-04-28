# Run Cookbook — типовые сценарии

> Только сценарии под существующие команды (5 штук): `/seo-crawl`, `/seo-audit`, `/seo-report`, `/seo-gsc` (после OAuth), `/seo-semrush` (после покупки API).

---

## Cold start (новый сайт, MCP подключены)

```
1. /seo-crawl       ← реальные meta + sitemap проверка
2. /seo-gsc         ← baseline impressions/clicks/queries (28d)
3. /seo-semrush     ← baseline traffic/keywords/AS + 12mo trend
4. /seo-audit       ← приоритизация фиксов
5. /seo-report      ← первый снапшот для истории
```

**Output:** `mapping.md` заполнен реальными данными, `trends.md` получил первую строку, `reports/` — 5 dated файлов.

## Cold start (без MCP — только то что работает сегодня)

```
1. /seo-crawl       ← single-source проверка meta + sitemap 404s
2. /seo-audit       ← приоритизация на основе того что есть
```

Дальше — настроить MCP (см. `MCP-SETUP.md`).

## После деплоя

Когда меняли `<title>`, `<meta>`, JSON-LD, robots, sitemap, или контент:

```
/seo-crawl /
```

Проверит что live соответствует ожиданиям, флагнет регрессии.

## Месячный цикл (1-е число)

```
1. /seo-crawl       ← убедиться что mapping актуален
2. /seo-gsc         ← месячные данные (после OAuth)
3. /seo-semrush     ← месячный snapshot + trend (после покупки API)
4. /seo-audit       ← пересмотр приоритетов
5. /seo-report      ← полный отчёт
```

**Output:** `seo/reports/seo-report-YYYY-MM-DD.md` готов копировать в Slack/Notion.

## Brief mode (быстрый ответ «как у нас дела»)

```
/seo-report brief
```

3-строчное summary в чат, файл не пишется. Полезно для ad-hoc вопросов.

## После смены primary keyword

(Когда поменяли primary kw в `mapping.md` руками или после SEMrush research.)

```
1. — обновить page62702763.html: <title>, og:title, og:description, h1 (см. mapping.md → H1 fix) —
2. — обновить sitemap.xml если изменилась структура URL —
3. — деплой: aws s3 sync + cloudfront invalidate —
4. /seo-crawl       ← убедиться что live изменения видны
5. /seo-audit       ← обновить Top Priorities
```

## Если страница пропала из индекса

```
1. /seo-crawl /                  ← проверить noindex / canonical / 5xx
2. — если crawl показал noindex → проверить page62702763.html
3. — в GSC UI → URL Inspection → Request indexing
```

## После трансфера домена (как у нас 2026-04-27)

```
1. — дождаться NS пропагации (24-48h, проверять `dig +short NS domain @8.8.8.8`)
2. /seo-crawl       ← после пропагации, проверить что новый origin отдаёт корректно
3. — отменить старый hosting (SiteGround в нашем случае)
4. — через неделю запустить cold start full cycle
```

## Если SEMrush показывает падение позиций

```
1. /seo-crawl       ← может что-то сломалось технически
2. — открыть последний /seo-gsc отчёт — есть ли там тренд
3. /seo-audit       ← пересмотреть Issues
4. — глазами в SEMrush UI: какой конкурент пробился? что у него изменилось?
```

(Здесь нет `/seo-diagnose` — был удалён как over-engineered. Анализ конкурентов делается руками в SEMrush UI.)

## Если Google уведомил о Manual Action

⚠️ Серьёзно. По шагам:

```
1. — прочитать письмо из GSC внимательно (что именно нарушено) —
2. — НЕ запускать /seo-* до понимания —
3. — устранить причину (thin content / spammy backlinks / hidden text / etc.) —
4. — submit Reconsideration Request в GSC —
5. — после снятия: /seo-audit чтобы перепрофилировать
```
