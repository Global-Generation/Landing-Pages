# SEO Mapping — global-generations.us

Source-of-truth file для всего SEO. Команды `/seo-*` читают и пишут сюда.

**Site:** `https://global-generations.us/`
**Language:** RU
**Audience:** абитуриенты СНГ → университеты США
**Type:** Single-page Tilda landing (B2C remarketing для Google Ads)
**Tracking:** GA4 `G-PWTD9C4LZ1`, GTM `GTM-PP5B2S79`, GSC verified, Yandex Webmaster verified

---

## SEO Health

**Last full audit:** —
**Crawl:** —
**GSC sync:** —
**SEMrush sync:** —
**Keywords:** —
**Audit:** —
**Report:** —

### Domain Metrics

| Metric | Current | Δ MoM | Source |
|---|---|---|---|
| Organic traffic | — | — | SEMrush |
| Organic keywords | — | — | SEMrush |
| Domain rank | — | — | SEMrush |
| Impressions (28d) | — | — | GSC |
| Clicks (28d) | — | — | GSC |
| Avg position | — | — | GSC |
| CTR | — | — | GSC |

### Top Priorities

_Заполняется командой `/seo-audit`. Изначально:_

1. 🔥 **Sitemap 404s** — 6 из 7 URL в `sitemap.xml` дают 404 (`/usa`, `/services`, `/premium`, `/faq`, `/reviews`, `/company`). Решение: либо почистить sitemap до `/`, либо вернуть страницы. Quick win — почистить.
2. 🔥 **Title is test placeholder** — `<title>` сейчас «РЕМАРКЕТИНГ» (тест пайплайна). Вернуть на боевой текст до пропагации NS.
3. 🔴 **Single-page indexation risk** — landing-page единственная страница, любая ошибка noindex/canonical = вылет из индекса.
4. 🟡 **No keyword tracking yet** — primary keyword не подтверждён данными SEMrush/GSC.

### Open Issues

_Заполняется командой `/seo-audit`._

- [ ] Sitemap содержит 6 несуществующих URL (404 на CloudFront)
- [ ] Title временно заменён на `РЕМАРКЕТИНГ`
- [ ] Primary keyword не валидирован (vol/KD/intent ещё не проверены)
- [ ] Не настроены MCP `gg-search-console` и `gg-semrush` (см. `references/MCP-SETUP.md`)

### Status Board

| Pages tracked | Indexed | noindex | Errors | Last crawl |
|---|---|---|---|---|
| 1 | ? | ? | 0 | — |

### Key Findings

_Заполняется командой `/seo-diagnose`._

### Competitive Landscape

_Заполняется командой `/seo-gaps`._

Кандидаты в конкуренты (RU-сегмент образовательного консалтинга для США):
- globaldialog.ru
- mba-strategy.com
- educationusa.ru
- studyusa.com (RU-страницы)
- linguatrip.com
- usa.studies.ru
- hardway.school

### Keyword Opportunities

_Заполняется командой `/seo-gaps`._

---

## Pages

### `/` — Landing (главная)

**URL:** `https://global-generations.us/`
**Priority:** 🔴 (единственная страница)
**Status:** ✅ Live (4.7 MB Tilda export, ~5s LCP risk)
**Last audited:** —

#### Keywords

| Slot | Keyword | Volume | KD | SERP intent | Status |
|---|---|---|---|---|---|
| Primary | поступление в вузы США | — | — | — | 🔍 Pending validation |
| Secondary 1 | помощь с поступлением в США | — | — | — | 🔍 Pending |
| Secondary 2 | стипендии в университетах США | — | — | — | 🔍 Pending |
| Secondary 3 | сопровождение поступления в США | — | — | — | 🔍 Pending |

#### On-Page

**Title (12 chars ⚠️ временный):** РЕМАРКЕТИНГ
**Title (target):** «Поступление в вузы США с финансированием — Global Generation» (60 chars ✓)
**H1:** _Tilda рендерит заголовки картинками — текстовый H1 отсутствует в DOM. Visible hero copy: «поступаем в США, Канаду, Британию, Европу, Австралию»_
**H1 has primary keyword?** ✗ (нет текстового H1)
**Description (154 chars ✓):** «Помощь в поступлении в топовые университеты США. Сопровождение от учеников и выпускников вузов США. Подготовка к экзаменам. Гарантия поступления. Запишитесь на бесплатную консультацию.»
**canonical:** `https://global-generations.us` ✓
**hreflang:** _не задан_ (single language — OK)
**noindex:** No
**JSON-LD:** EducationalOrganization, Course, FAQPage (по словам предыдущего аудита — проверить `/seo-crawl`)
**Content quality:** —

#### Live Metrics

**GSC:** —
**SEMrush:** —

#### Diagnosis

_Заполняется командой `/seo-diagnose`._

#### Issues & Actions

- [ ] Текстовый H1 отсутствует — Tilda рендерит heading картинкой. Impact: высокий (нет keyword signal). Effort: высокий (требует Tilda-перекомпиляции либо overlay-текста).
- [ ] Word count неизвестен — провести `/seo-crawl`.
- [ ] FAQ в JSON-LD есть? — провести `/seo-crawl`.

---

## Notes

- `sitemap.xml` сейчас лжёт про 6 несуществующих URL — это первая приоритетная задача.
- Tilda-экспорт массивный (4.7 MB) — лимиты на Core Web Vitals; провести Lighthouse-чек отдельно.
- В этом репо single-page mapping. Если в будущем добавятся страницы — каждая получает свой `### /path` блок по тому же шаблону.
