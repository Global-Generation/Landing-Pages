# Content Plan — global-generations.us

> Roadmap по контенту: что добавить на лендинг, что планируется в дальнейшем.
> Последнее обновление: 2026-04-27

---

## Текущий контент `/`

Tilda-лендинг с секциями (по hero copy, грубо):
1. Hero: «поступаем в США, Канаду, Британию, Европу, Австралию»
2. Acquisition timer (16 мест, скидка 30%)
3. Услуги: Premium / стандарт / основные пакеты
4. Гарантия результата (тексты с цифрами: $10M+ стипендий, 100+ офферов)
5. Подготовка к экзаменам
6. Айсберг (визуал)
7. Команда / отзывы
8. CTA — «Бесплатная консультация»

**Текстовые блоки малые** — основная коммуникация в визуалах, что плохо для SEO.

---

## Phase 1 — Quick wins (на текущем лендинге)

Что добавить **без перерендера Tilda** (сменой кода / вставкой блоков):

### FAQ блок с FAQPage JSON-LD

10–15 типовых вопросов от абитуриентов. Каждый вопрос-ответ → JSON-LD entry. Дополнительно решает long-tail informational queries.

Кандидаты вопросов (нужно проверить через GSC top queries):

- Что нужно для поступления в вуз США из СНГ?
- Какие экзамены сдавать для US-вузов?
- Сколько стоит обучение в США? Можно ли получить стипендию?
- В каком возрасте начинать готовиться к поступлению?
- Что такое Common App?
- Сколько вузов выбирать?
- Нужен ли SAT для всех университетов?
- Как написать motivational letter?
- Какие документы переводить на английский?
- Виза F-1 — сложно получить?
- Можно ли работать студентом в США?
- В каких штатах обучение дешевле?
- Что такое Need-Based vs Merit-Based стипендии?
- Принимают ли в US-вузы российские аттестаты после 11 класса?
- Можно ли поступить если плохой средний балл?

> **Реализация**: попросить Tilda-дизайнера добавить блок «Вопросы и ответы» (Tilda нативно поддерживает FAQ-блок с auto-генерацией FAQPage schema).

### Текстовый H1 (overlay над hero-картинкой)

Сейчас heading вшит в картинку. Добавить:
```html
<h1 class="visually-hidden-or-styled">Поступление в вузы США с финансированием</h1>
```

Либо через Tilda Zero block, либо через post-export injection в `page62702763.html`.

### Anchor links в hero

Внутренние якоря: `#consultation`, `#guarantees`, `#programs`, `#team`, `#testimonials`. Помогает crawler navigation, улучшает UX, даёт Google «Sitelinks search box» на брендовый запрос.

### Alt-tags на ключевых картинках

Tilda иногда оставляет пустые alt — пройтись по `images/*` и для significant изображений добавить осмысленный alt.

---

## Phase 2 — Decision point (после данных GSC + SEMrush)

К концу 2 месяцев данных: понять направление контентной стратегии.

**Развилка А — Single-page deep:**
- Расширить лендинг — добавить case-studies / success stories секцию (~ 1500 слов на странице)
- Перевести heading-картинки в текст (через rebuild Tilda)
- Сильно углубить FAQ
- Добавить блок «Подходит ли вам наш сервис» с интерактивным тестом

**Развилка Б — Content hub:**
- Запустить блог-секцию на отдельном поддомене (`blog.global-generations.us`) или отдельным path (`/blog`)
- Технически: либо клонировать SAT pattern (Next.js + `BlogAutopublishService`), либо использовать готовое решение (Notion-сайт через Super, Webflow, Ghost)
- 2 статьи/неделю под gap keywords

**Развилка В — Hybrid:**
- Лендинг остаётся «коммерческой» страницей под primary keyword
- Блог отдельно под informational long-tail
- Cross-linking landing ↔ blog

---

## Phase 4 — Content production (если выбрана развилка Б или В)

Идеи статей (заполнятся `/seo-keywords` data):

### Pillar / cornerstone articles

| Тема | Цель | Word count target |
|---|---|---|
| Полный гид: поступление в вуз США из СНГ (от 9 класса до зачисления) | top-3 by «поступление в вуз США» | 5000+ |
| Стипендии в США для иностранных студентов: полный список | top-3 by «стипендии в США» | 4000+ |
| Common App vs Coalition vs UC application | top-3 by application platform queries | 3000+ |

### Cluster articles (поддерживают pillar)

- Как написать motivational letter
- Recommendation letters: советы, primery
- SAT/ACT vs Test-Optional политики 2026
- Visa F-1: процесс, документы, сроки
- TOEFL vs IELTS vs DET
- Top-50 universities: short profiles
- Cost of living в США по штатам
- Internships для F-1 студентов

### Comparison articles

- Поступление в США vs Канаду vs UK
- Public vs Private universities
- Liberal Arts College vs Research University

---

## Editorial pipeline (если запустим блог)

Можно склонировать SAT pattern:
1. Topic queue в `seo/CONTENT-QUEUE.ts` (или `.md`)
2. Cron / manual trigger → Claude API генерит RU article (1500-3000 слов, SEO-оптимизированный)
3. Атомарный коммит через GitHub API
4. После деплоя — submit URL в GSC через Indexing API

Detail: см. `Product-SAT/seo/SYSTEM.md` для reference implementation.

---

## Что **НЕ делаем**

- ❌ Накопирование контента из SAT — там SAT-специфика, нерелевантно для GG
- ❌ Авто-перевод на KK/EN — не нужно, целимся в RU аудиторию
- ❌ Контент-фермы / спин-контент — Google понизит за thin content
- ❌ Партизанский guest blogging на низкосортных сайтах — backlink-quality > quantity

---

## Definition of done для Content Plan

- [ ] Phase 1 FAQ-блок задеплоен на лендинг с FAQPage schema
- [ ] Phase 1 текстовый H1 присутствует в DOM
- [ ] Phase 1 anchor links и alt-tags есть
- [ ] Phase 2 решение по развилке принято (с обоснованием в `DECISIONS.md`)
- [ ] Phase 4 (если применимо) — pillar #1 опубликован, ранжируется в top-50

---

## Где обновляется

- При получении новых GSC top queries (раз в неделю) — добавлять FAQ-кандидатов
- При появлении новых gap keywords (`/seo-gaps`) — добавлять в article ideas
- При закрытии phase — отмечать ✅ и переходить к следующему
