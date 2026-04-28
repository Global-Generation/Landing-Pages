# SEO Mapping — global-generations.us

Источник правды для всего SEO. Команды `/seo-*` читают и пишут сюда.

**Site:** `https://global-generations.us/`
**Language:** RU
**Type:** Single-page Tilda landing
**Tracking:** GA4 `G-PWTD9C4LZ1`, GTM `GTM-PP5B2S79`, GSC verified, Yandex Webmaster verified

---

## SEO Health

**Crawl:** —
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

1. 🔥 **Sitemap содержит 6 несуществующих URL** — `/usa`, `/services`, `/premium`, `/faq`, `/reviews`, `/company` дают 404 на CloudFront. **Fix:** убрать их из `sitemap.xml`, оставить только `/`.
2. 🔥 **`<title>` = `РЕМАРКЕТИНГ`** (test placeholder). **Fix:** Lev назначает финальный текст, заменяем в `page62702763.html`.
3. 🔴 **Текстовый H1 отсутствует** (Tilda рендерит heading картинкой). **Fix:** см. секцию «H1 fix» ниже — конкретная sed-инъекция.

### Open Issues

- [ ] Sitemap → почистить до `/`
- [ ] Title → вернуть на боевой
- [ ] H1 → инъекция visually-hidden
- [ ] OAuth для GSC (`gg-search-console` MCP)
- [ ] SEMrush API (deferred, Lev купит)
- [ ] SiteGround hosting → отменить после DNS пропагации (~2026-04-29)

### Status Board

| Pages tracked | Indexed | noindex | Errors | Last crawl |
|---|---|---|---|---|
| 1 | ? | ? | 0 | — |

---

## Pages

### `/` — Landing

**URL:** `https://global-generations.us/`
**Status:** ✅ Live
**Last audited:** —

#### Keywords

| Slot | Keyword | Volume | KD | Status |
|---|---|---|---|---|
| Primary | поступление в вузы США | — | — | 🔍 Pending validation (нужен SEMrush) |
| Secondary 1 | стипендии в университетах США | — | — | 🔍 Pending |
| Secondary 2 | помощь с поступлением в США | — | — | 🔍 Pending |

#### On-Page

| Поле | Значение | Статус |
|---|---|---|
| `<title>` | `РЕМАРКЕТИНГ` (12 chars) | 🔥 test placeholder |
| `<title>` (target) | «Поступление в вузы США с финансированием — Global Generation» (60 chars) | — |
| `<meta description>` | «Помощь в поступлении в топовые университеты США. Сопровождение от учеников и выпускников вузов США. Подготовка к экзаменам. Гарантия поступления. Запишитесь на бесплатную консультацию.» (154 chars) | ✅ |
| Текстовый H1 | отсутствует (Tilda heading в картинке) | 🔴 |
| `<link rel="canonical">` | `https://global-generations.us` | ✅ |
| `<meta name="robots">` | не задан → `index, follow` (default) | ✅ |
| `<html lang>` | `ru` | ✅ |
| JSON-LD | EducationalOrganization, Course, FAQPage (заявлено, валидировать `/seo-crawl`) | ⚠️ |

#### Live Metrics

**GSC:** —
**SEMrush:** —

#### Issues & Actions

- [ ] H1 inject (см. ниже)
- [ ] `/seo-crawl` подтвердит JSON-LD типы и word count

---

## H1 fix — конкретный путь

**Проблема:** Tilda hero — `<img>` с заголовком вшитым в pixels. В DOM нет `<h1>`. Google и Yandex видят `<title>` и `<meta description>`, но не получают heading-сигнал внутри тела страницы.

**Решение:** инъектить visually-hidden `<h1>` сразу после открывающего `<body>` тега. Видим только для краулеров и screen-readers, дизайн Tilda не трогается.

### Где инъектить

В `page62702763.html` сразу после `<body class="t-body" style="margin:0;">` (байт ~ 18055), перед `<!--allrecords-->`.

### Что инъектить

```html
<h1 style="position:absolute;width:1px;height:1px;padding:0;margin:-1px;overflow:hidden;clip:rect(0,0,0,0);white-space:nowrap;border:0;">Поступление в вузы США с финансированием</h1>
```

(Стандартный паттерн `visually-hidden` от GitHub/MDN.)

### Команда (из корня репо)

```bash
sed -i '' \
  's|<body class="t-body" style="margin:0;">|<body class="t-body" style="margin:0;"><h1 style="position:absolute;width:1px;height:1px;padding:0;margin:-1px;overflow:hidden;clip:rect(0,0,0,0);white-space:nowrap;border:0;">Поступление в вузы США с финансированием</h1>|' \
  page62702763.html
```

Проверить:
```bash
grep -c '<h1 style="position:absolute' page62702763.html  # должно быть 1
```

### После Tilda re-export

Tilda при следующем экспорте перезапишет `page62702763.html` без нашего H1. Чтобы не забывать:

1. **Вариант A (минимальный):** добавить пункт в чеклист после каждого Tilda-экспорта — прогнать sed-команду перед коммитом.
2. **Вариант B (надёжнее):** держать в репо `seo/scripts/post-export.sh` который запускается перед `aws s3 sync`. Создавать когда H1 будет не единственной post-export правкой.

Сейчас выбираем A — лишняя инфра пока избыточна.

### Связанное

- После замены primary keyword — обновить текст `<h1>` в той же команде.
- Не путать `visually-hidden` с `display:none` — `display:none` не индексируется, нам нужен именно offscreen-pattern.
