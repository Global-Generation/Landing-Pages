# SEO Status — global-generations.us

> Снапшот состояния. Обновляется руками при значимых изменениях.
> Последнее обновление: **2026-04-27**

---

## Команды

| Команда | Status | Блокер |
|---|---|---|
| `/seo-crawl` | ✅ работает | — |
| `/seo-audit` | ✅ работает | — |
| `/seo-report` | ✅ работает | — |
| `/seo-gsc` | ⏳ ждёт OAuth | Прогнать `references/MCP-SETUP.md` → раздел 1 |
| `/seo-semrush` | ⏳ ждёт покупки | Lev купит API |

## Open Issues

### 🔥 Do First

- [ ] **Sitemap → почистить до `/`** — 6 из 7 URL отдают 404. Команда: см. `mapping.md → Top Priorities`.
- [ ] **`<title>` → вернуть с `РЕМАРКЕТИНГ`** на боевой текст. Lev назначает.

### 🔴 This sprint

- [ ] **H1 inject** — конкретный sed в `mapping.md → H1 fix`.

### 🟡 Plan

- [ ] **GSC OAuth** — для `/seo-gsc` и индексационных проверок.
- [ ] **SEMrush API** — Lev купит, тогда `/seo-semrush` оживает.
- [ ] **Lighthouse audit** — измерить LCP/CLS/INP. 4.7 MB Tilda — потенциальный долг по Core Web Vitals.

### 🟢 Backlog

- [ ] **SiteGround hosting** — отменить после полной DNS пропагации (~2026-04-29).

## Deferred (Lev сделает позже)

| Пункт | ETA |
|---|---|
| Покупка SEMrush API | TBD |
| GSC OAuth flow | любой момент |
| Финальный `<title>` | до конца DNS пропагации |

## Done

- ✅ **2026-04-27** — Domain transfer complete, NS switched
- ✅ **2026-04-27** — SEO toolkit добавлен (commits `d45a311`, `1b5cc80`, `e791baa`)
- ✅ **2026-04-27** — Toolkit подрезан до реального scope: 5 команд, 5 main docs (commit pending)

## Когда обновлять

- Купили SEMrush → отметить ✅
- Прогнали GSC OAuth → отметить ✅
- Закрыли Open Issue → перенести в Done
- Истекает hosting / domain — обновить даты
