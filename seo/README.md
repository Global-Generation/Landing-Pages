# SEO — global-generations.us

Source-of-truth: **`mapping.md`**.
Текущее состояние и блокеры: **`STATUS.md`**.

---

## Команды (slash в Claude Code)

| Команда | Что | MCP | Status сегодня |
|---|---|---|---|
| `/seo-crawl` | WebFetch live-страницу, синк meta/JSON-LD/word count в mapping | none | ✅ работает |
| `/seo-audit` | Приоритизация фиксов из `mapping.md` (Impact × Effort) | none | ✅ работает |
| `/seo-report` | Месячный отчёт + brief mode | none | ✅ работает |
| `/seo-gsc` | Тянет GSC данные (impressions/clicks/queries) | gg-search-console | ⏳ ждёт OAuth |
| `/seo-semrush` | Тянет SEMrush (traffic/keywords/position) | gg-semrush | ⏳ ждёт покупки API |

Run order: `crawl → audit → report` (без MCP) или `crawl → gsc → semrush → audit → report` (полный цикл).

## Что лежит здесь

| Файл | Назначение |
|---|---|
| `mapping.md` | **Главный источник правды** — страница, keywords, meta, GSC/SEMrush данные, диагнозы, issues, H1 fix path |
| `STATUS.md` | Снапшот: что работает, что заблокировано, что в очереди |
| `AUDIT.md` | Технический baseline (стек, on-page, verification keys, robots, sitemap) |
| `DECISIONS.md` | Лог ключевых решений с обоснованием |
| `trends.md` | История метрик по месяцам (заполняется `/seo-gsc`, `/seo-semrush`) |
| `references/MCP-SETUP.md` | Как поднять MCP `gg-search-console` и `gg-semrush` |
| `references/audit-checklist.md` | Что проверять в `/seo-crawl`, severity scale |
| `references/report-template.md` | Шаблон месячного отчёта |
| `references/run-cookbook.md` | Типовые сценарии (cold start, after-deploy, troubleshooting) |
| `reports/` | Куда команды пишут dated отчёты (`seo-{команда}-YYYY-MM-DD.md`) |

## Перед первым запуском

1. **Без MCP можно сразу:** `/seo-crawl /` — проверит реальные meta-теги, найдёт sitemap 404s, оценит JSON-LD.
2. **GSC** — пройти OAuth flow в `references/MCP-SETUP.md` → раздел 1. После этого `/seo-gsc` работает.
3. **SEMrush** — Lev купит API позже. До этого `/seo-semrush` не работает.

## Принципы

- Все команды пишут в `mapping.md` — один источник правды.
- Dated reports в `reports/` — append-only история.
- Если данных нет — пишем `—`, не сочиняем.
- На русском, без англицизмов кроме SEO-терминов (impressions, CTR, etc.).
