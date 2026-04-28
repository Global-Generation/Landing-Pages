# Целевые ключевые запросы — global-generations.us

> Последнее обновление: 2026-04-27 (initial draft, требует валидации SEMrush + GSC)
> Язык: RU
> Геопривязка: RU + соседние СНГ (KZ, UA-RU аудитория)
> Обновлять еженедельно по данным GSC (`/seo-gsc`) и при пересмотре стратегии (`/seo-keywords`)

---

## Primary keyword

| Запрос | Vol | KD | SERP intent | Position (Google) | Position (Yandex) | Status |
|---|---|---|---|---|---|---|
| поступление в вузы США | — | — | — | — | — | 🔍 Pending validation |

> Vol/KD/intent заполнятся первым прогоном `/seo-semrush` после покупки API.

---

## Secondary keywords (кандидаты)

| Запрос | Vol | KD | Position | Status |
|---|---|---|---|---|
| помощь с поступлением в США | — | — | — | 🔍 Pending |
| стипендии в университетах США | — | — | — | 🔍 Pending |
| сопровождение поступления в США | — | — | — | 🔍 Pending |
| как поступить в университет США | — | — | — | 🔍 Pending |
| гарантия поступления в вуз США | — | — | — | 🔍 Pending |

---

## Long-tail (быстрый результат — для phase 2)

Сюда `/seo-keywords` будет добавлять кандидатов после research.

| Запрос | Vol | KD | Position | Source | Notes |
|---|---|---|---|---|---|
| _будут добавлены_ | — | — | — | — | — |

---

## Brand keywords

| Запрос | Position | Notes |
|---|---|---|
| global generation | — | Брендовый, должен быть #1 после индексации |
| global generations | — | Без `s` тоже отрабатывает |
| global generation поступление | — | — |

---

## Negatives (скоринг — НЕ целиться)

Запросы которые **не** наша аудитория, чтобы не оптимизироваться под них:

- "global generation" в значении generation gap / поколения
- "поступление" в значении РФ-вузы (не наша целевая)
- "Hogwarts" / "Гарвард" в значении общий interest (без commercial intent)

---

## Status legend

- 🔍 Pending validation — записан, ждёт SEMrush data
- 📋 Pending approval — `/seo-keywords` нашла, ждёт apply
- ✅ Applied — primary keyword страницы
- 🚀 Tracking — мониторим в `/seo-gsc` и `trends.md`
- ❌ Dropped — отказались (с причиной)

---

## Editorial rules

- **Один primary** на лендинг (страница ранжируется по 1 чёткому запросу).
- Secondary 3-5 — близкие по семантике, поддерживают primary.
- Long-tail 10-30 — closes informational long-tail queries (могут быть в FAQ-секции).
- Negatives — пишем явно, чтобы будущие изменения hero-copy не дрейфовали в чужой intent.

---

## Где обновляется автоматически

- `/seo-gsc` — обновляет колонки **Position (Google)** на основе GSC data
- `/seo-semrush` — обновляет **Vol**, **KD**, **Position** (SEMrush version)
- `/seo-keywords` — добавляет новые кандидаты со статусом `📋 Pending approval`
- `/seo-keywords apply` — переключает status `📋` → `✅` и обновляет primary в `mapping.md`
