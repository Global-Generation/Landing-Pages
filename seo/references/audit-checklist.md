# SEO Audit Checklist — `/seo-crawl`

Что проверять при кравле страницы. Используется в `/seo-crawl`.

---

## Обязательно (CRITICAL — флагать в отчёте)

| Проверка | Ожидаемое | Severity если нарушено |
|---|---|---|
| `<title>` есть | непустой, ≤ 70 символов | CRITICAL если пусто, WARNING если > 70 |
| `<meta name="description">` есть | 120–160 символов | WARNING |
| `<meta name="robots">` | НЕ содержит `noindex` (single landing) | CRITICAL если `noindex` |
| `<link rel="canonical">` | абсолютный URL на тот же путь | CRITICAL если отсутствует |
| `<html lang="...">` | `ru` | WARNING |
| GSC verification | `google-site-verification` meta присутствует | CRITICAL |
| HTTPS | страница доступна по https | CRITICAL |
| HTTP→HTTPS redirect | http возвращает 301 на https | WARNING |
| `<title>` НЕ test placeholder | не равен `РЕМАРКЕТИНГ`, `TEST`, `CHANGEME`, etc. | CRITICAL |

## Open Graph / Twitter (важно для соц. шаринга)

| Тег | Ожидаемое |
|---|---|
| `og:title` | присутствует, совпадает с `<title>` или близко |
| `og:description` | присутствует |
| `og:url` | абсолютный URL страницы |
| `og:image` | абсолютный URL, ≥ 1200×630 |
| `og:type` | `website` для landing |
| `twitter:card` | `summary_large_image` |
| `twitter:title` / `twitter:description` / `twitter:image` | присутствуют |

## Структурированные данные (JSON-LD)

Кандидаты для образовательного landing:

| Тип | Когда подходит |
|---|---|
| `EducationalOrganization` | всегда — основной |
| `Course` | если есть конкретные программы/курсы |
| `FAQPage` | если на странице есть FAQ-блок |
| `BreadcrumbList` | если есть навигация |
| `Organization` | альтернатива EducationalOrganization |

Каждый JSON-LD блок должен:
- Парситься как валидный JSON
- Иметь `@context`: `https://schema.org`
- Иметь `@type` соответствующего вида
- Содержать обязательные поля для типа (например, `Course` требует `name`, `provider`)

## Контент

| Метрика | Норма | Severity |
|---|---|---|
| Word count visible body | ≥ 300 слов | WARNING если меньше (thin content) |
| H1 | ровно 1, содержит primary keyword | WARNING если 0 или несколько |
| H2 count | ≥ 2 | INFO |
| FAQ block | желательно (CTR + featured snippets) | INFO |
| Images alt-text | у всех значимых картинок | WARNING |

## Технические

| Проверка | Норма |
|---|---|
| Page size | ≤ 2 MB (lol Tilda 4.7MB — известный долг) |
| Largest Contentful Paint (LCP) | ≤ 2.5s |
| Cumulative Layout Shift (CLS) | ≤ 0.1 |
| Mobile viewport meta | присутствует |
| Sitemap включает страницу | да |
| robots.txt не блокирует | да |

---

## Severity guide

- **CRITICAL** — блокер для индексации или резкий ущерб ранжированию. Чинить в течение дня.
- **WARNING** — заметный негативный сигнал, но страница ранжируется. Чинить в спринт.
- **INFO** — улучшение, не баг. Чинить когда есть время.
