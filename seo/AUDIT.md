# Technical SEO Audit — global-generations.us

> Baseline — техническое состояние SEO. Обновлять при изменениях инфры или после major-аудита.
> Последнее обновление: 2026-04-27

---

## Стек

| Слой | Что |
|---|---|
| HTML source | Tilda export — единственный файл `page62702763.html` (4.7 MB) |
| Hosting | S3 bucket `global-generations-us-site` |
| CDN | CloudFront `EXJ3W80LP8A65` (`dixmq96bmzcjx.cloudfront.net`) |
| SSL | ACM cert `arn:aws:acm:us-east-1:818034793268:certificate/7cf28642-...` |
| Domain | `global-generations.us` (AWS Route 53 Domains, expires 2027-05-15) |
| DNS | Route 53 zone `Z054617150JR83K8E03X`, 4 NS `awsdns-*` |
| Analytics | GA4 `G-PWTD9C4LZ1` + GTM `GTM-PP5B2S79` |
| GSC | Verified (meta `emB0zgRKtastF9dTZvO_Oh3Nny9XizERshJLx7mIPEk`) |
| Yandex Webmaster | Verified (meta `mailru-domain: Ny0LUK0o7rTUxyPh`) |

---

## On-page (текущее состояние `/`)

| Элемент | Значение | Статус |
|---|---|---|
| `<html lang="">` | `ru` | ✅ |
| `<title>` | `РЕМАРКЕТИНГ` | 🔥 test placeholder |
| `<meta name="description">` | «Помощь в поступлении в топовые университеты США. Сопровождение от учеников и выпускников вузов США. Подготовка к экзаменам. Гарантия поступления. Запишитесь на бесплатную консультацию.» (154 chars) | ✅ |
| `<meta name="robots">` | _не задан_ → defaults to `index, follow` | ✅ |
| `<link rel="canonical">` | `https://global-generations.us` | ✅ |
| `<link rel="alternate" hreflang>` | _не задан_ | ✅ (single language) |
| `og:title` / `og:description` / `og:image` / `og:url` / `og:type` | присутствуют | ✅ (но `og:title` = `РЕМАРКЕТИНГ`) |
| `twitter:card` / `twitter:title` / `twitter:description` | присутствуют | ✅ (но title тестовый) |
| Текстовый H1 | _отсутствует_ | 🔴 Tilda рендерит heading-картинкой |
| JSON-LD | EducationalOrganization, Course, FAQPage | ⚠️ требует валидации `/seo-crawl` |

---

## Robots.txt (актуальный)

```
User-Agent: *
Allow: /
Disallow: /files/
Disallow: /htaccess
Disallow: /tilda/
Disallow: /members/

User-agent: GPTBot
Disallow: /
User-agent: CCBot
Disallow: /
User-agent: anthropic-ai
Disallow: /
User-agent: Google-Extended
Disallow: /
User-agent: Bytespider
Disallow: /
User-agent: Amazonbot
Disallow: /

Sitemap: https://global-generations.us/sitemap.xml
Host: https://global-generations.us
```

**Анализ:**
- ✅ Googlebot и Yandex не блокированы
- ✅ Sitemap указан
- ✅ AI-краулеры заблокированы (осознанное решение — не отдавать контент LLM)
- ⚠️ `Host:` директива устарела (Yandex её больше не учитывает с 2018) — можно убрать
- ✅ `Disallow: /tilda/`, `/members/`, `/htaccess` — защита служебных путей

---

## Sitemap.xml (актуальный)

7 URL:
- `/` (priority 1.0) — ✅ 200
- `/usa` (0.9) — ❌ 404
- `/services` (0.9) — ❌ 404
- `/premium` (0.8) — ❌ 404
- `/faq` (0.7) — ❌ 404
- `/reviews` (0.7) — ❌ 404
- `/company` (0.6) — ❌ 404

**🔥 Action:** очистить sitemap до `/` либо вернуть страницы. Делается первым `/seo-crawl`.

---

## Performance (требует измерения)

| Метрика | Текущее | Цель | Источник |
|---|---|---|---|
| Page weight | 4.7 MB | < 1.5 MB | curl headers |
| LCP | — | ≤ 2.5s | Lighthouse / PSI |
| CLS | — | ≤ 0.1 | Lighthouse |
| INP | — | ≤ 200ms | Lighthouse |
| TTFB | — | ≤ 600ms | curl |

**Action:** прогнать Lighthouse как отдельную задачу — Tilda-экспорт известно тяжёлый, нужно понимать стартовую точку.

---

## Index status (требует GSC)

Заполняется первым прогоном `/seo-daily`:

| URL | Coverage | Last crawl | Verdict |
|---|---|---|---|
| `https://global-generations.us/` | — | — | — |

---

## Internal linking

Single page — нет internal links. Это норма для лендинга, но:
- Если пойдём в phase 4 с блогом — нужны links from landing → блог-статьи
- Pre-emptive: добавить anchor-links внутри лендинга (например `#consultation`, `#guarantees`) — улучшает crawler navigation

---

## External signals (baseline)

- Backlinks: — (заполнится через SEMrush)
- Social shares: — (нужен social listening tool)
- Brand mentions: — (Yandex Webmaster + Google Alerts)

---

## Severity баллы по категориям

| Категория | Баллы | Что это |
|---|---|---|
| Index hygiene | ⚠️ | sitemap 404s |
| Meta tags | 🔥 | title placeholder |
| Schema markup | ✅ | JSON-LD есть, требует валидации |
| Performance | ⚠️ | 4.7 MB вес — измерить Lighthouse |
| Mobile-friendly | ✅ | viewport meta присутствует |
| HTTPS | ✅ | принудительный через CloudFront |
| Crawlability | ✅ | robots OK |
| Accessibility | — | требует отдельного аудита |
| Content quality | ⚠️ | нет текстового H1, word count неизвестен |
| Internal linking | ⚠️ | анкор-links отсутствуют |

**Общая оценка:** baseline ОК для лендинга, но 2 🔥 критических issue (title + sitemap) надо чинить до того как Google переиндексирует с новым CloudFront-content.

---

## Когда обновлять этот файл

- После каждого крупного изменения инфры (миграция домена, смена hosting, добавление CDN-фичей)
- После полного `/seo-crawl` — обновлять секцию On-page если что-то изменилось
- После Lighthouse-аудита — обновлять Performance
- При добавлении новых страниц в репо
