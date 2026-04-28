# SEO — global-generations.us

> Последнее обновление: 2026-04-27
> Сайт: `https://global-generations.us/`
> Тип: B2C remarketing landing (Tilda export, single page)
> Язык: RU
> Аудитория: абитуриенты СНГ → университеты США

Всё по SEO для лендинга `global-generations.us`. Эта папка — single source of truth.

---

## Текущий статус (2026-04-27)

| Что | Статус |
|---|---|
| **Сайт** | ✅ Работает на CloudFront, NS пропагация в процессе (24-48ч от 2026-04-27) |
| **`<title>`** | 🔥 `РЕМАРКЕТИНГ` — тестовый, ждёт финального текста от Lev |
| **`sitemap.xml`** | 🔥 6 из 7 URL дают 404 — почистить первым `/seo-crawl` |
| **GSC** | ✅ Verified, MCP сервер ⏳ pending OAuth |
| **SEMrush** | ⏳ **Deferred** — Lev купит API позже |
| **Yandex Webmaster** | ✅ Verified |
| **GA4** | ✅ `G-PWTD9C4LZ1` |
| **GTM** | ✅ `GTM-PP5B2S79` |

Подробности и open issues — `STATUS.md`.

---

## Быстрые команды

| Задача | Команда | Cadence |
|---|---|---|
| Кравлер сайта (sync meta в mapping) | `/seo-crawl` | После каждого деплоя |
| Daily routine (5 мин) | `/seo-daily` | Каждый рабочий день |
| GSC данные (impressions/clicks/queries) | `/seo-gsc` | Еженедельно |
| SEMrush (traffic/keywords/position) | `/seo-semrush` | Ежемесячно |
| Подбор keywords | `/seo-keywords` | По требованию |
| Root cause: почему не ранжируется | `/seo-diagnose` | После gsc + semrush |
| Конкуренты + keyword gap | `/seo-gaps` | Раз в квартал |
| Приоритизация фиксов | `/seo-audit` | Ежемесячно |
| Пятничный digest | `/seo-weekly` | Каждую пятницу |
| Месячный отчёт | `/seo-report` | 1-е число месяца |

Slash-команды живут в `.claude/commands/seo-*.md` (репо-уровень).

### Run order для нового сайта (как сейчас)

```
/seo-crawl  →  /seo-gsc  →  /seo-semrush  →  /seo-keywords  →  /seo-diagnose  →  /seo-audit
```

---

## Файлы в этой папке

### State (источники правды)

| Файл | Что |
|---|---|
| `mapping.md` | **Главный** source-of-truth: страницы, keywords, GSC/SEMrush, диагнозы, issues, SEO Health. Команды читают и пишут сюда. |
| `STATUS.md` | Снапшот текущего состояния — что работает, что заблокировано, что в очереди |
| `KEYWORDS.md` | Target keywords (RU + позиции) — обновляется через GSC |
| `AUDIT.md` | Техническое состояние SEO (meta, schema, robots, sitemap, perf, accessibility) |
| `PLAN.md` | 6-месячный SEO-план — milestones, цели, definition of done |
| `DAILY-LOG.md` | Append-log от `/seo-daily` |
| `trends.md` | История метрик по месяцам |

### Reports (dated, append-only)

`reports/` — куда пишут команды с датами:
- `seo-crawl-YYYY-MM-DD.md`
- `seo-gsc-YYYY-MM-DD.md`
- `seo-semrush-YYYY-MM-DD.md`
- `seo-keywords-YYYY-MM-DD.md`
- `seo-diagnose-YYYY-MM-DD.md`
- `seo-gaps-YYYY-MM-DD.md`
- `seo-audit-YYYY-MM-DD.md`
- `seo-weekly-YYYY-MM-DD.md`
- `seo-report-YYYY-MM-DD.md`

### References (шаблоны и инструкции)

| Файл | Что |
|---|---|
| `references/MCP-SETUP.md` | Как поднять `gg-search-console` и `gg-semrush` MCP-серверы |
| `references/audit-checklist.md` | Что проверять при кравле, severity scale |
| `references/report-template.md` | Шаблон месячного отчёта |
| `references/weekly-template.md` | Шаблон пятничного digest |

---

## Сравнение с SAT SEO системой

В отличие от SAT (`Global-Generation/Product-SAT/seo/`), здесь:

| Параметр | SAT | global-generations.us |
|---|---|---|
| Тип сайта | Многостраничный продукт + блог 90+ статей | Single-page landing (Tilda) |
| Auto-publish блога | ✅ `BlogAutopublishService` (cron 06:00 UTC) | ❌ нет блога |
| GSC monitor cron | ✅ `BlogSeoMonitorService` 08:00 UTC + Telegram | ❌ ручной `/seo-daily` |
| Locales | RU + KK + EN | только RU |
| MCP-серверы | `sat-search-console` + `sat-semrush` | `gg-search-console` (pending), `gg-semrush` (deferred) |
| KEYWORDS.md | RU + KK + EN таблицы | RU только |
| Контент-календарь | 15 статей в очереди | — |
| Indexing API + IndexNow | ✅ авто после публикации | ❌ ручной submit |

**Что взято из SAT:**
- Структура папки (`README.md`, `STATUS.md`/`AUDIT.md`/`KEYWORDS.md`/`PLAN.md`/`DAILY-LOG.md`)
- Slash-команды pattern + dated reports
- MCP-сервера паттерн (sat-* → gg-*)
- DAILY-LOG.md formate
- Run order для cold start

**Что НЕ берём (пока):**
- Auto-publish блога (нет блога)
- App Runner cron-сервисы (лендинг — статика на S3)
- Telegram-репортинг (можно добавить позже)
- IndexNow (полезен только если есть много страниц для индексации)
- Multi-locale (single language)

---

## Перед первым запуском

1. **Прочитать `STATUS.md`** — текущие blockers и что уже работает.
2. **Если запускаешь `/seo-gsc` или `/seo-daily`** — сначала прогнать OAuth flow (`references/MCP-SETUP.md` → раздел 1).
3. **Если запускаешь `/seo-semrush` / `/seo-keywords` / `/seo-diagnose` / `/seo-gaps`** — после покупки SEMrush API key, см. `references/MCP-SETUP.md` → раздел 2.
4. **Можно сразу без MCP:** `/seo-crawl` (через WebFetch), `/seo-audit` (read-only), `/seo-weekly`, `/seo-report` (синтез).

---

## Принципы

- **Все команды пишут в `mapping.md`** — это единый источник правды. Не пишем разрозненно.
- **Dated reports никогда не перезаписываются** — каждый прогон → новый файл в `reports/`.
- **`STATUS.md` обновляется руками** при изменении глобального статуса (купили SEMrush, сменили title, etc.).
- **`KEYWORDS.md` обновляется автоматически** через `/seo-gsc` и `/seo-keywords apply`.
- **Editorial: всё на русском.** Англицизмы только для SEO-терминов (impressions, CTR, etc.).
- **Никаких "может быть"** — либо знаем, либо пишем `data gap`.
- **`/seo-daily` — кофе-команда.** 5 минут, читает GSC, пишет рекомендации.

---

## Где что лежит вне этой папки

- Slash-команды: `.claude/commands/seo-*.md` (10 файлов)
- HTML страницы: `page62702763.html`, `404.html`
- Статика: `css/`, `js/`, `images/`, `files/`
- SEO-инфра: `robots.txt`, `sitemap.xml` (в корне репо)
- MCP-серверы: `~/.claude/mcp-servers/gg-search-console/` и `~/.claude/mcp-servers/gg-semrush/` (вне репо)
