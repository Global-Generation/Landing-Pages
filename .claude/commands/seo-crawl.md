---
description: Кравлит реальный сайт global-generations.us, синкает title / description / canonical / noindex / JSON-LD / word count в docs/seo_mapping.md. Запускать после любого деплоя который трогал meta-теги.
argument-hint: [optional URL path, default = все страницы из mapping]
---

# /seo-crawl — Sync live page data into seo_mapping.md

**Target file:** `docs/seo_mapping.md`
**Aux output:** `docs/seo-crawl-YYYY-MM-DD.md` (dated отчёт по результатам)
**Argument:** `$ARGUMENTS` — optional URL path (`/`, `/usa`, etc.). Пусто = все страницы из mapping.
**Requires:** WebFetch (built-in).
**Reference:** `references/audit-checklist.md` — что проверять и какой severity.

---

## Step 1: Прочитать mapping

Открыть `docs/seo_mapping.md`, собрать список страниц секции `## Pages` (каждый `### /path` или `### URL: ...` блок).

Если `$ARGUMENTS` задан — кравлим только этот path. Иначе — все.

## Step 2: WebFetch + extract

Для каждой страницы — `https://global-generations.us{path}` через WebFetch с таким промптом:

```
Извлеки из HTML страницы:
1. <title> — полный текст
2. <meta name="description"> content
3. <meta name="robots"> content (искать "noindex")
4. <link rel="canonical"> href
5. <html lang="">
6. Все <meta property="og:*"> и <meta name="twitter:*">
7. Все <script type="application/ld+json"> — для каждого попытайся извлечь @type
8. Первый <h1> — текст (может отсутствовать в Tilda-экспортах)
9. Word count visible body (без nav, footer, скриптов)
10. Кол-во H2, H3 тегов
11. Есть ли FAQ schema (FAQPage в JSON-LD)
12. HTTP status

Формат ответа (строго):
TITLE: ...
TITLE_CHARS: N
DESC: ...
DESC_CHARS: N
NOINDEX: yes|no
CANONICAL: ...
LANG: ...
OG_TITLE: ...
OG_DESC: ...
OG_IMAGE: ...
TWITTER_CARD: ...
JSONLD_TYPES: [Type1, Type2, ...]
H1: ...
WORD_COUNT: N
H2_COUNT: N
H3_COUNT: N
FAQ_SCHEMA: yes|no
HTTP_STATUS: N
```

Если WebFetch упал/4xx/5xx — записать `HTTP_STATUS` и в отчёте флаг `⚠️ Could not fetch`.

## Step 3: Сравнить с mapping

Для каждой страницы — diff между live и mapping. Флагать:

- Title text differs (даже на 1 символ — flag, пусть Lev решает)
- Description differs
- noindex changed
- canonical differs
- lang differs
- JSON-LD types changed (set diff)
- H1 differs
- HTTP status != 200

Применить severity по `references/audit-checklist.md`.

## Step 4: Обновить mapping

Для каждой расхождения — обновить нужную строку в `docs/seo_mapping.md`. Пересчитать char counts (Title ≤ 70, Desc 120–160). Обновить `**Last audited:**` на сегодня.

`**Content quality:**` всегда переписать актуальными данными:

```
**Content quality:** ~Nw · H2: N · H3: N · FAQ: Yes/No
```

Если страница вернула не-200 — добавить в `### Open Issues` пункт `[ ] {path}: HTTP {code}` если ещё нет.

## Step 5: Output

Создать `docs/seo-crawl-YYYY-MM-DD.md` со структурой:

```
# SEO Crawl — {YYYY-MM-DD}

Pages crawled: N
Pages updated: N
Pages unchanged: N
Failed: N

## Per-page changes

### /
- Title: "OLD" → "NEW" (chars N → M)
- noindex: No (unchanged)
- JSON-LD: [EducationalOrganization, Course, FAQPage] (unchanged)
- Content: ~Nw · H2: N · H3: N · FAQ: Yes
- Issues found: ⚠️ Title is test placeholder

### /usa
- HTTP 404 ⚠️

## CRITICAL flags

- ...

## WARNINGs

- ...

## Notes

- Lev: посмотри title — он сейчас "РЕМАРКЕТИНГ" (test).
```

Поверх обновить раздел `## Pages` в `docs/seo_mapping.md` и поле `**Crawl:** {YYYY-MM-DD}` в `## SEO Health`.

## Step 6: Sitemap sanity

В конце прогнать ещё одну проверку: каждый `<loc>` из `sitemap.xml` → HEAD-запрос → если не 200, добавить пункт в `### Open Issues`.

---

## Notes

- В этом репо лендинг в основном single-page. Если sitemap содержит фантомные URL (404) — флагать.
- Tilda-экспорт рендерит heading-картинками — если `H1` пустой, это норма для Tilda; флагать как WARNING (нет keyword signal в DOM).
- Запускать после каждого деплоя меняющего meta или после ручного редактирования `page62702763.html`.
