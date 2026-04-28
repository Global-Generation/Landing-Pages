---
description: Пятничный digest для команды. Чистый синтез из существующих файлов (seo_mapping, seo-trends, последние dated отчёты). Никаких API-вызовов. Output на русском, готов копировать в Slack/Notion.
argument-hint: [нет]
---

# /seo-weekly — Weekly digest (Friday)

**Inputs:** `seo/mapping.md`, `seo/trends.md`, последние `seo/reports/seo-{gsc|semrush|crawl|audit|diagnose}-*.md` за прошедшую неделю.
**Output:** `seo/reports/seo-weekly-YYYY-MM-DD.md` + текст для копи-пейста.
**Template:** `seo/references/weekly-template.md`.
**Requires:** ничего внешнего.

---

## Step 1: Freshness check

Проверить даты последних обновлений:
- GSC sync — если `> 7 дней` назад → warning «GSC данные устарели»
- SEMrush sync — если `> 35 дней` → warning
- Crawl — если `> 14 дней` → warning

Warnings отдельным блоком `## Data freshness` в начале digest.

## Step 2: Прочитать current state

Из `seo/mapping.md`:
- Domain Metrics секция
- Top Priorities (top 5)
- Open Issues (top 5)
- Per-page GSC line для всех страниц (сейчас одна `/`)

Из `seo/trends.md`:
- Последние 2 строки domain trends
- Последние 2 строки GSC trends
- Последние 2 строки page position

## Step 3: Identify what moved

Diff неделя-к-неделе:
- Position Δ (положительные = wins, отрицательные = regressions)
- Impressions/clicks Δ (%)
- Closed issues с прошлого weekly (✅ что сделано)
- Новые open issues
- Технические события (deploys, sitemap fixes, redirects, schema added)

## Step 4: Root causes (если что-то упало)

Для каждой regression > 5% — попытаться найти причину в:
- Recent commits в репо (`git log --since="last week" --oneline`)
- Crawl-отчётах (изменения title/description/noindex)
- Diagnose-отчётах

Если причина не очевидна — пишем `Root cause: проверить (data gap)`.

## Step 5: Write digest

Использовать `seo/references/weekly-template.md`:

```
# SEO Weekly — global-generations.us — {YYYY-MM-DD}

## TL;DR
2-3 строки.

## Scorecard
| Metric | На неделе | Прошлая | Δ |
| ... | ... | ... | ... |

## Что изменилось
- Контент: ...
- Технические: ...
- Внешние сигналы: ...

## Корневые причины
- ...

## Следующая неделя
- [ ] ...
- [ ] ...

## Watch list
- ...
```

## Step 6: Save + print

Сохранить в `seo/reports/seo-weekly-YYYY-MM-DD.md`. Вывести содержимое в чат целиком — Lev копирует в Slack/Notion.

`**Last weekly:** {YYYY-MM-DD}` в `## SEO Health` (если поле уже есть, иначе добавить).

---

## Editorial rules (важно!)

- Только русский. Без англицизмов кроме SEO-терминов (impressions, CTR, position и т.п.).
- Без воды и без жаргона. Цель: маркетолог без SEO-бэкграунда понял.
- Никаких "может быть" / "вероятно". Либо знаем, либо пишем «проверить».
- Цифры конкретные. Если данных нет — `data gap`.
- Заголовок digest — `# SEO Weekly` без преамбулы.
- Cadence: каждую пятницу.
