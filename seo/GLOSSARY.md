# Glossary — SEO термины + project-specific

> Ссылка для быстрого ответа на «что это за метрика?». Используется в репо везде где встречается термин.

---

## Метрики

### GSC (Google Search Console)

| Термин | Что значит |
|---|---|
| **Impressions** | Сколько раз страница появилась в результатах Google для поисковых запросов (даже если не кликнули). |
| **Clicks** | Количество кликов из SERP на нашу страницу. |
| **CTR** | Click-Through Rate = Clicks / Impressions × 100%. |
| **Avg position** | Среднее значение позиции в SERP по всем запросам. **Считается асимметрично** (топ-1 кликабельнее топ-10 в разы), поэтому не самый честный показатель. |
| **Coverage state** | Статус индексации страницы. Возможные значения: «Submitted and indexed» (✅ норма), «Discovered – currently not indexed» (Google знает но не проиндексировал), «Crawled – currently not indexed» (просканировал но не включил), etc. |
| **URL Inspection** | API GSC — проверка статуса конкретного URL (last crawl, indexed, robots verdict). |
| **Sitemap submitted** | Sitemap отправлен в GSC, но это **не гарантия** что Google его прочитал. Проверять `lastDownloaded` поле. |

### SEMrush

| Термин | Что значит |
|---|---|
| **Vol** | Volume — среднемесячное количество поисковых запросов по keyword. |
| **KD** | Keyword Difficulty (0-100) — насколько сложно ранжироваться. < 30 — easy, 30–55 — moderate, 55+ — hard. Для нового домена цельтесь в KD ≤ 50. |
| **AS** / **Authority Score** | SEMrush proprietary метрика DR (0-100). Аналог Ahrefs DR. |
| **Domain rank** | Ранжирование SEMrush среди всех доменов в их БД. |
| **Backlinks** | Количество входящих ссылок. Качество > количество. |
| **Database** | Регион SEMrush (`us`, `ru`, `kz`, etc.) — определяет SERP какой страны парсит. |

### Search engines

| Термин | Что значит |
|---|---|
| **SERP** | Search Engine Results Page. |
| **SERP intent** | Тип результатов: commercial / informational / navigational / transactional. Если top-10 — все commercial, то и наш landing должен быть commercial под этот запрос. |
| **Featured snippet** | "Position 0" — Google показывает выдержку из страницы наверху SERP. |
| **People Also Ask** | Раздел SERP с дополнительными вопросами. Источник идей для FAQ. |
| **Sitelinks** | Подссылки внутри результата на бренд. Появляются автоматически. |
| **Knowledge Panel** | Боковая карточка о бренде/организации. Помогает EducationalOrganization schema. |

### On-page

| Термин | Что значит |
|---|---|
| **Title tag** | `<title>` — заголовок страницы в SERP и в браузерной вкладке. |
| **Meta description** | `<meta name="description">` — описание под title в SERP. |
| **H1** | Заголовок первого уровня в HTML. Должен быть один. |
| **Canonical** | `<link rel="canonical">` — указывает Google основную версию страницы (защита от дубликатов). |
| **hreflang** | `<link rel="alternate" hreflang>` — указывает альтернативные языковые версии. |
| **JSON-LD** | Structured data в формате JSON. Помогает Google понимать контент (Course, FAQPage, EducationalOrganization, etc.). |
| **noindex** | `<meta name="robots" content="noindex">` — запрет индексации страницы. |

### Pipeline

| Термин | Что значит |
|---|---|
| **Crawl** | Поисковик скачивает HTML страницы. |
| **Indexation** | Поисковик добавляет страницу в индекс (в SERP можно найти). |
| **Ranking** | Поисковик решает на какой позиции показать страницу для запроса. |

---

## Project-specific

| Термин | Что значит |
|---|---|
| **Primary keyword** | Главный запрос на который оптимизирована страница. У `/` сейчас (pending validation) — «поступление в вузы США». |
| **Secondary keyword** | Дополнительные близкие запросы. У нас 3-5 на страницу. |
| **Long-tail** | Длинные конкретные запросы (≥ 4 слова). Меньше vol, легче ранжироваться. |
| **Keyword gate** | Логика в `/seo-audit` — блокирует keyword-aligned fixes пока primary keyword не валидирован SEMrush данными. |
| **Run order** | Последовательность запуска команд `/seo-*`. См. `README.md → Run order`. |
| **Dated reports** | Файлы вида `seo-{команда}-YYYY-MM-DD.md` в `seo/reports/` — append-only история прогонов. |
| **State files** | `seo/mapping.md`, `seo/STATUS.md`, etc. — текущее состояние, перезаписывается. |
| **Mapping** | `seo/mapping.md` — главный source-of-truth. |
| **Cold start** | Запуск toolkit на «пустом» сайте — много данных нет, нужно прогнать crawl + gsc + semrush + audit чтобы получить baseline. |

---

## Brand & Domain

| Термин | Что значит |
|---|---|
| **Branded query** | Запрос содержащий бренд («global generation», «global generations»). Считается отдельно от non-branded. Высокий branded-share = слабый organic. |
| **Non-branded** | Все остальные запросы — настоящий organic traffic. |
| **EPP code** | Код для трансфера домена. У нас уже использован (трансфер 2026-04-22 завершён 2026-04-27). |
| **NS** | Nameservers — DNS-серверы которые отвечают за домен. |
| **TTL** | Time-to-live — сколько кеши держат DNS-ответ. |

---

## SEO акронимы

| Акроним | Расшифровка |
|---|---|
| **SEO** | Search Engine Optimization |
| **SEM** | Search Engine Marketing (включает paid + SEO) |
| **SERP** | Search Engine Results Page |
| **GSC** | Google Search Console |
| **GSE** | Google Search Engine |
| **CTR** | Click-Through Rate |
| **DR** | Domain Rating (Ahrefs term) |
| **AS** | Authority Score (SEMrush) |
| **KD** | Keyword Difficulty |
| **ICP** | Ideal Customer Profile |
| **B2C** | Business to Consumer |
| **B2B** | Business to Business |
| **HF/MF/LF** | High/Mid/Low Frequency (по запросам в RU SEO жаргоне) |

---

## Где обновляется

- При появлении нового project-specific термина — добавлять.
- Терминология SEO стандартная — обновляется только если индустрия принципиально что-то поменяет (например, AS заменит DR как стандарт).
