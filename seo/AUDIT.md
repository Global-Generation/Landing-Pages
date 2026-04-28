# Technical SEO Audit — global-generations.us

> Только то что **знаем точно**. Плейсхолдеры (`—`) для метрик которые требуют замера — туда `/seo-crawl` и `/seo-gsc` запишут реальные значения.
> Последнее обновление: 2026-04-27

---

## Стек

| Слой | Что |
|---|---|
| HTML source | Tilda export — `page62702763.html` (4.7 MB) |
| Hosting | S3 `global-generations-us-site` |
| CDN | CloudFront `EXJ3W80LP8A65` (`dixmq96bmzcjx.cloudfront.net`) |
| SSL | ACM `arn:aws:acm:us-east-1:818034793268:certificate/7cf28642-110a-40a5-ba52-c23c24b3f45d` |
| Domain | `global-generations.us` (Route 53 Domains, expires 2027-05-15) |
| DNS | Route 53 zone `Z054617150JR83K8E03X`, 4 awsdns NS |

## On-page (`/`, на 2026-04-27)

| Элемент | Значение |
|---|---|
| `<html lang>` | `ru` |
| `<title>` | `РЕМАРКЕТИНГ` 🔥 (тестовый) |
| `<meta description>` | присутствует, 154 chars |
| `<meta robots>` | _не задан_ → `index, follow` (default) |
| `<link rel="canonical">` | `https://global-generations.us` |
| `<link rel="alternate" hreflang>` | _не задан_ (single locale OK) |
| `og:*` | присутствуют (title=РЕМАРКЕТИНГ — тоже тестовый) |
| `twitter:*` | присутствуют |
| Текстовый H1 | **отсутствует** (Tilda рендерит картинкой) |
| JSON-LD | EducationalOrganization, Course, FAQPage (валидировать через `/seo-crawl`) |

## Verification & analytics

| Сервис | ID / token | Где |
|---|---|---|
| Google Search Console | `emB0zgRKtastF9dTZvO_Oh3Nny9XizERshJLx7mIPEk` | `<meta name="google-site-verification">` в HTML |
| Yandex Webmaster | `mailru-domain: Ny0LUK0o7rTUxyPh` | `<meta name="yandex-verification">` в HTML (нестандартный mailru-формат — проверить что Yandex принял) |
| Google Analytics 4 | `G-PWTD9C4LZ1` | через GTM |
| Google Tag Manager | `GTM-PP5B2S79` | inline в HTML |

## Robots.txt

```
User-Agent: *
Allow: /
Disallow: /files/
Disallow: /htaccess
Disallow: /tilda/
Disallow: /members/

User-agent: GPTBot         Disallow: /
User-agent: CCBot          Disallow: /
User-agent: anthropic-ai   Disallow: /
User-agent: Google-Extended Disallow: /
User-agent: Bytespider     Disallow: /
User-agent: Amazonbot      Disallow: /

Sitemap: https://global-generations.us/sitemap.xml
Host: https://global-generations.us
```

- Googlebot и YandexBot пропущены ✅
- AI-краулеры заблокированы (осознанное решение)
- `Host:` директива устарела (Yandex её больше не учитывает с 2018) — можно удалить
- `Disallow: /tilda/`, `/members/`, `/htaccess` — защита служебных путей

## Sitemap.xml

7 URL — но только `/` отдаёт 200. Остальные 6 (`/usa`, `/services`, `/premium`, `/faq`, `/reviews`, `/company`) — 404 на CloudFront.

🔥 **Action:** почистить sitemap до `/`. Это первый пункт Top Priorities в `mapping.md`.

## Что требует измерения

Запустить отдельно (не через Claude — отдельные тулы):

- **Lighthouse / PageSpeed Insights** — LCP, CLS, INP, TTFB. 4.7 MB Tilda-export потенциально провален по Core Web Vitals.
- **GSC URL Inspection** — coverage state страницы `/` (после OAuth setup `/seo-daily` будет это делать).
- **Schema validator** (`https://validator.schema.org/`) — проверить что JSON-LD blocks валидны и читаются Google. Что внутри `Course` и `FAQPage` — нужно проверить (Tilda иногда генерит пустые/невалидные блоки).

## Проблемы по приоритету

| Severity | Проблема | Эффект |
|---|---|---|
| 🔥 | Title = `РЕМАРКЕТИНГ` | Google индексирует тестовый текст в snippet |
| 🔥 | Sitemap 404s | GSC флагает coverage errors, Google теряет crawl budget |
| 🔴 | Нет текстового H1 | Нет heading-keyword signal в DOM (см. `mapping.md → H1 fix`) |
| 🟡 | Page weight 4.7 MB | LCP под угрозой — нужен Lighthouse-замер |
| 🟢 | `Host:` в robots устарела | Косметика |

---

## Когда обновлять

- После изменения hosting / CDN / DNS provider
- После Lighthouse-аудита — заполнить performance-метрики
- После полного `/seo-crawl` — обновить On-page если что-то изменилось
