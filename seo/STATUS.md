# SEO Status — global-generations.us

> Снапшот текущего состояния. Обновляется руками при значимых изменениях.
> Последнее обновление: **2026-04-27**

---

## Что работает прямо сейчас

| Команда | Статус | Замечание |
|---|---|---|
| `/seo-crawl` | ✅ Работает | Только WebFetch, никаких API |
| `/seo-audit` | ✅ Работает | Read-only по `mapping.md` |
| `/seo-weekly` | ✅ Работает | Синтез из существующих файлов |
| `/seo-report` | ✅ Работает | Синтез из существующих файлов + brief mode |

## Что заблокировано

| Команда | Блокер | Когда снимется |
|---|---|---|
| `/seo-gsc` | OAuth flow для `gg-search-console` MCP не пройден | После прогона `references/MCP-SETUP.md` → раздел 1 |
| `/seo-daily` | То же | То же |
| `/seo-semrush` | SEMrush API key не куплен | После покупки Lev (deferred) |
| `/seo-keywords` | То же | То же |
| `/seo-diagnose` | То же (использует SEMrush) | То же |
| `/seo-gaps` | То же | То же |

---

## Open Issues (по приоритету)

### 🔥 Do First

- [ ] **Sitemap содержит 6 dead URL** — `/usa`, `/services`, `/premium`, `/faq`, `/reviews`, `/company` дают 404 на CloudFront. Решение: либо почистить sitemap до `/`, либо вернуть страницы на S3. Quick win — почистить.
- [ ] **`<title>` = `РЕМАРКЕТИНГ`** — test placeholder, ждёт финального текста от Lev.

### 🔴 This Sprint

- [ ] **Primary keyword не валидирован** — нужен SEMrush для vol/KD/SERP intent (см. `mapping.md` → Keywords table).
- [ ] **Tilda не рендерит текстовый H1** — heading вшит в картинку, в DOM нет H1 → нет keyword signal. Решения: (а) overlay-text через Tilda, (б) Zero block с текстовым H1, (в) принять и компенсировать через JSON-LD `name` поле.

### 🟡 Plan

- [ ] **MCP `gg-search-console` не настроен** — нужен OAuth flow (refresh token) и регистрация в `~/.claude/mcp.json`.
- [ ] **MCP `gg-semrush` deferred** — Lev купит SEMrush API позже.
- [ ] **Yandex Webmaster** — verification meta есть, но данные через MCP не тянутся (Yandex API отдельный, не входит в GSC). Если нужен RU-поиск из Yandex — отдельная задача.

### 🟢 Backlog

- [ ] **SiteGround hosting активна** — отменить subscription после полной DNS пропагации (~2026-04-29).
- [ ] **Page weight 4.7 MB** — Tilda-экспорт тяжёлый, LCP под угрозой. Lighthouse-аудит отдельной задачей.
- [ ] **AI-краулеры заблокированы в robots.txt** — GPTBot, CCBot, anthropic-ai, etc. Решение принято осознанно (не отдавать контент LLM-обучению).

---

## Deferred (Lev сделает позже)

| Пункт | ETA | Что разблокирует |
|---|---|---|
| Покупка SEMrush API | TBD (после получения бюджета) | `/seo-semrush`, `/seo-keywords`, `/seo-diagnose`, `/seo-gaps` |
| GSC OAuth flow | в любой момент | `/seo-gsc`, `/seo-daily` |
| Финальный `<title>` | до окончания DNS пропагации | Корректное превью в Google после индексации |
| Sitemap cleanup | первым `/seo-crawl` | — (косметика для GSC) |

---

## Что НЕ ставили цели делать (зафиксировано)

- ❌ Авто-публикация блога — нет блога, single-page лендинг
- ❌ Multi-locale — только RU
- ❌ App Runner cron-сервисы — статика на S3, никаких длительных процессов
- ❌ Telegram-репортинг — все отчёты в репо, читаются глазами/Claude
- ❌ Indexing API / IndexNow — мало страниц чтобы оправдать setup
- ❌ Backlink-monitoring — выйдет из SEMrush когда подключим

---

## Когда обновлять этот файл

После любого из следующих событий:

- Купили SEMrush → отметить как `✅` и обновить таблицу
- Прогнали OAuth для GSC → отметить `gg-search-console` как `✅`
- Закрыли issue из Open Issues → перенести в "Done" внизу либо удалить
- Изменили scope (добавили блог, добавили локаль) → обновить раздел "Что НЕ ставили цели делать"
- Истёк / истекает domain expiry / NS / hosting — обновить даты

---

## Done (история закрытых задач)

- ✅ **2026-04-27** — Domain transfer SiteGround → Route 53 complete, NS switched
- ✅ **2026-04-27** — SEO toolkit добавлен в репо (10 slash-команд + state-файлы + шаблоны), коммит `d45a311`
- ✅ **2026-04-27** — Папка `seo/` консолидирована (README + STATUS + KEYWORDS + PLAN + AUDIT + DAILY-LOG)
