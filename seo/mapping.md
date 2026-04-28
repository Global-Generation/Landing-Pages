# SEO Mapping — global-generations.us

Источник правды для всего SEO. Команды `/seo-*` читают и пишут сюда.

**Site:** `https://global-generations.us/`
**Language:** RU
**Type:** Single-page Tilda landing
**Tracking:** GA4 `G-PWTD9C4LZ1`, GTM `GTM-PP5B2S79`, GSC verified, Yandex Webmaster verified (см. AUDIT.md про нестандартный формат)

---

## SEO Health

**Crawl:** 2026-04-27
**GSC sync:** —
**SEMrush sync:** —
**Audit:** —
**Report:** —

### Domain Metrics

| Metric | Current | Δ MoM | Source |
|---|---|---|---|
| Organic traffic | — | — | SEMrush |
| Organic keywords | — | — | SEMrush |
| Impressions (28d) | — | — | GSC |
| Clicks (28d) | — | — | GSC |
| Avg position | — | — | GSC |
| CTR | — | — | GSC |

### Top Priorities

1. 🔥 **Sitemap содержит 6 несуществующих URL** — `/usa`, `/services`, `/premium`, `/faq`, `/reviews`, `/company` дают 404 на CloudFront. Подтверждено crawl 2026-04-27. **Fix:** убрать их из `sitemap.xml`, оставить только `/`.
2. 🔥 **`<title>` = `РЕМАРКЕТИНГ`** (test placeholder). Влияет на og:title и twitter:title тоже. **Fix:** Lev назначает финальный текст, sed-replace в `page62702763.html`.
3. 🟡 **H1 не содержит primary keyword** — текущий H1 «вы поступите в вуз мечты» (28 chars) не содержит ни «США», ни «поступление в вуз США». **Fix:** см. секцию «H1 keyword fix» ниже.

### Open Issues

- [ ] Sitemap → почистить до `/` (single sed-операция в `sitemap.xml`)
- [ ] Title → вернуть с `РЕМАРКЕТИНГ` на боевой текст
- [ ] H1 текст → переписать под primary keyword (требует Tilda-edit — heading в Zero-block; либо sed-replace в HTML после export)
- [ ] OAuth для GSC (`gg-search-console` MCP)
- [ ] SEMrush API (deferred, Lev купит)
- [ ] Yandex verification в нестандартном `mailru-domain` формате — проверить в Yandex.Webmaster что property всё-таки подтверждена; если нет — добавить нормальный `<meta name="yandex-verification">`
- [ ] WebFetch unreliable на 4.7 MB Tilda-HTML — `/seo-crawl` дополнить grep-fallback или читать локальный `page62702763.html` напрямую
- [ ] SiteGround hosting → отменить после DNS пропагации (~2026-04-29)

### Status Board

| Pages tracked | Indexed | noindex | Errors | Last crawl |
|---|---|---|---|---|
| 1 | ? (нужен GSC) | No | 0 на `/` (но 6/7 URL в sitemap = 404) | 2026-04-27 |

---

## Pages

### `/` — Landing

**URL:** `https://global-generations.us/`
**Status:** ✅ Live (HTTP 200 на CloudFront)
**Last audited:** 2026-04-27

#### Keywords

| Slot | Keyword | Volume | KD | Status |
|---|---|---|---|---|
| Primary | поступление в вузы США | — | — | 🔍 Pending validation (нужен SEMrush) |
| Secondary 1 | стипендии в университетах США | — | — | 🔍 Pending |
| Secondary 2 | помощь с поступлением в США | — | — | 🔍 Pending |

#### On-Page (по состоянию на 2026-04-27)

| Поле | Значение | Статус |
|---|---|---|
| `<title>` | `РЕМАРКЕТИНГ` (12 chars) | 🔥 test placeholder |
| `<title>` (target) | «Поступление в вузы США с финансированием — Global Generation» (60 chars) | — |
| `<meta description>` | «Помощь в поступлении в топовые университеты США. Сопровождение от учеников и выпускников вузов США. Подготовка к экзаменам. Гарантия поступления. Запишитесь на бесплатную консультацию.» (154 chars) | ✅ |
| `<h1>` | **«вы поступите в вуз мечты»** (28 chars) — есть в DOM | ⚠️ off-target keyword |
| H1 / H2 / H3 counts | 1 / 17 / 11 | ✅ healthy hierarchy |
| `<link rel="canonical">` | `https://global-generations.us` | ✅ |
| `<meta robots>` | не задан → `index, follow` (default) | ✅ |
| `<html lang>` | `ru` | ✅ |
| JSON-LD | EducationalOrganization, Course, FAQPage (3 валидных блока) | ✅ |
| Word count (visible body) | ~9 600 | ✅ |

#### Live Metrics

**GSC:** —
**SEMrush:** —

#### Issues & Actions

- [ ] H1 текст под primary keyword (см. ниже)
- [ ] После окончания DNS пропагации — `/seo-crawl` против реального `https://global-generations.us/` чтобы убедиться что CloudFront-direct = production-direct идентичны

---

## H1 keyword fix

**Текущее:** `<h1>вы поступите <br>в вуз мечты</h1>` (28 chars). Heading в DOM есть — это хорошо. Но не содержит ни «США», ни «поступление в вуз», ни «стипендии». Для primary keyword «поступление в вузы США» нужен heading с явной геопривязкой.

**Целевое:** что-то вроде «Поступайте в вузы США со стипендиями» или «Помощь с поступлением в вузы США».

**Где менять:**
1. **Правильно:** в Tilda редакторе → Zero block с heading → re-export. Tilda сама перезапишет HTML.
2. **Быстро (post-export workaround):** sed-replace в `page62702763.html` после экспорта:
   ```bash
   sed -i '' \
     "s|<h1 class='tn-atom'field='tn_text_1752245550869'>вы поступите <br>в вуз мечты</h1>|<h1 class='tn-atom'field='tn_text_1752245550869'>Поступайте в вузы США со стипендиями</h1>|" \
     page62702763.html
   ```

**Проверить после:**
```bash
grep -oE "<h1[^>]*>[^<]+(<br>[^<]+)?</h1>" page62702763.html
# Должно вернуть новый текст
```

**Важно:** если поменяешь в Tilda редакторе, при следующем экспорте sed становится не нужен. Если только sed-replace — каждый re-export сбрасывает изменения; добавить в чеклист deploy-процесса.

**Связанное:** заодно проверить картинки hero (Tilda могла оставить визуальный заголовок картинкой который дублирует или противоречит текстовому H1).

---

## Notes

- WebFetch показал ненадёжность для Tilda-HTML > 1 MB — meta-теги вернулись как MISSING хотя они есть. Локальный grep по `page62702763.html` достоверен. В будущем `/seo-crawl` команда должна предпочитать локальный файл (он в репо) когда target — наш собственный сайт.
- Tilda heading реально в DOM с тегом `<h1 class='tn-atom'>` и атрибутом `field='tn_text_1752245550869'` — этот field-id нужен если будем sed-replace, чтобы не задеть другие H1 (хотя их и нет — единственный H1 на странице).
- 3 JSON-LD блока валидно парсятся — это здорово, но содержание не валидировал в schema validator. Добавить в Backlog.
