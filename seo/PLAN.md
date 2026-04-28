# SEO Plan — global-generations.us

> 6-месячный план с milestones. Обновлять при пересмотре приоритетов.
> Последнее обновление: 2026-04-27
> Цель сайта: B2C remarketing landing для Google Ads + органический трафик из RU-поиска по запросам про поступление в США.

---

## North Star

К концу 6 месяцев (2026-10):
- ≥ 500 organic clicks/месяц из GSC по non-branded запросам
- ≥ 30 ranked keywords в top-50 (SEMrush, db=ru)
- Лендинг ранжируется в top-10 хотя бы по 3 long-tail запросам
- 100% URL в `sitemap.xml` отдают 200

---

## Phase 1: Foundation (2026-04 → 2026-05)

**Цель:** базовая чистота — индексация, нет тех долгов, есть данные.

- [x] SEO-toolkit в репо (slash-команды + state-файлы + шаблоны) — _2026-04-27_
- [ ] Title revert с `РЕМАРКЕТИНГ` на финальный
- [ ] Sitemap cleanup — оставить только `/`
- [ ] OAuth flow для `gg-search-console`, прогон `/seo-daily` 7 дней подряд
- [ ] Покупка SEMrush API + настройка `gg-semrush` MCP
- [ ] Первые `/seo-gsc`, `/seo-semrush` для baseline в `trends.md`
- [ ] Lighthouse audit — измерить LCP/CLS/FID, понять степень тех долга от 4.7MB Tilda-экспорта
- [ ] JSON-LD audit — убедиться что EducationalOrganization + Course + FAQPage схемы валидны и видны Google

**Definition of done:** в `mapping.md` все поля заполнены реальными данными, не плейсхолдерами.

---

## Phase 2: Validate keyword strategy (2026-05 → 2026-06)

**Цель:** понять — работает ли single-page стратегия для primary keyword. Принять решение либо сменить keyword, либо запустить блог.

- [ ] `/seo-keywords` — research для primary kw кандидатов (vol ≥ 200, KD ≤ 50, B2C educational SERP)
- [ ] `/seo-diagnose` — root cause анализ против top-3 SERP-конкурентов (`globaldialog.ru`, `mba-strategy.com`, etc.)
- [ ] Решение по primary keyword: оставить «поступление в вузы США» или сменить на более достижимый long-tail
- [ ] Если решено: обновить `<title>`, og-теги, контент лендинга под новый primary
- [ ] `/seo-gaps` — identify keyword opportunities. Если их > 30 высокого качества — решение: запускать ли блог-секцию на отдельном поддомене или в Notion-сайте

**Decision point:** к концу phase 2 ясно — single-page достаточно или нужен content-hub.

---

## Phase 3: Backlinks (2026-06 → 2026-08)

**Цель:** domain authority через 5-10 качественных ссылок.

- [ ] Гостевые посты на образовательных RU-площадках (theoryandpractice, vc.ru/education, специализированные)
- [ ] Партнёрства: cross-linking с complementary services (репетиторы, переводчики, юристы по визам)
- [ ] PR через success stories выпускников
- [ ] Цель: AS / DR ≥ 20 к концу phase 3 (стартовая точка измерится в phase 1)

---

## Phase 4: Content (2026-08 → 2026-10)

**Цель:** ответить на keyword gaps, увеличить топ-10 ранжирующихся keywords.

Если в phase 2 решено НЕ делать блог:
- [ ] Расширить лендинг секцией FAQ (с реальным FAQ JSON-LD) — closes long-tail informational queries
- [ ] Hero copy переписать под top GSC query (после 2 месяцев данных)
- [ ] Добавить case-studies / success stories в виде секции лендинга

Если в phase 2 решено делать блог:
- [ ] Копировать pattern SAT (`Product-SAT/seo/SYSTEM.md`) — auto-publish через Claude API
- [ ] 2 статьи/неделю под gap-keywords из phase 2
- [ ] Internal linking из лендинга в блог-статьи
- [ ] `/seo-daily` мониторит индексацию новых статей

---

## Что мониторим непрерывно (после phase 1)

- **Daily:** `/seo-daily` — индексация, top queries, top pages, sitemap coverage
- **Weekly:** `/seo-gsc` (синк в `mapping.md` + `trends.md`) + `/seo-weekly` (digest на пятницу)
- **Monthly:** `/seo-semrush` + `/seo-audit` + `/seo-report`
- **Quarterly:** `/seo-gaps` (re-evaluate competitors)

---

## Risks & assumptions

| Риск | Митигация |
|---|---|
| Tilda-экспорт не позволяет менять H1/семантическую структуру без перерендера | Если критично — миграция на собственный layer (Astro / Next.js single-page) |
| Single-page не ранжируется по broad keywords | Phase 2 решает: pivot на long-tail или launch контент-хаба |
| SEMrush data для нового домена пустые | Использовать GSC как primary signal первые 2-3 месяца |
| AI-краулеры заблокированы в robots — могут ли влиять на обычное Google ранжирование? | По текущим данным Google использует Googlebot независимо от GPTBot. Не должно влиять. Мониторить. |
| DNS пропагация не завершилась → перерывы в трафике | Уже в процессе с 2026-04-27, ETA 2026-04-29 |

---

## Изменения плана

| Дата | Что изменено | Кем |
|---|---|---|
| 2026-04-27 | План создан | initial draft |
