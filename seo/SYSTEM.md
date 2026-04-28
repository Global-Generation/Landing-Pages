# SEO System — global-generations.us

> Техническая документация: как устроена SEO-инфраструктура для лендинга.
> Последнее обновление: 2026-04-27

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  Source                                                     │
│  ──────                                                     │
│  Tilda (внешний редактор) → export HTML → коммит в репо    │
│  page62702763.html (4.7 MB), css/, js/, images/, files/    │
└──────────────────────┬──────────────────────────────────────┘
                       │ git push origin main
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  Deploy (manual)                                            │
│  ───────────────                                            │
│  aws s3 sync . s3://global-generations-us-site/             │
│  aws cloudfront create-invalidation --paths "/*"            │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  Hosting                                                    │
│  ───────                                                    │
│  S3 bucket: global-generations-us-site                      │
│  CloudFront: dixmq96bmzcjx.cloudfront.net (EXJ3W80LP8A65)   │
│  ACM cert: us-east-1, both alias variants                   │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  DNS                                                        │
│  ───                                                        │
│  Route 53 zone: Z054617150JR83K8E03X                        │
│  4 awsdns NS (transferred from SiteGround 2026-04-27)       │
│  Domain: global-generations.us → Gandi (Route 53 backend)   │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  User & Search engines                                      │
│  ──────────────────────                                     │
│  Browsers: GA4 + GTM telemetry                              │
│  Googlebot: indexes via /sitemap.xml + /robots.txt          │
│  YandexBot: indexes (verified)                              │
│  AI bots: blocked in robots.txt                             │
└─────────────────────────────────────────────────────────────┘

  ▲                                                       │
  │                                                       │
  │ index data feeds back ───────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────┐
│  SEO Toolkit (this folder)                                  │
│  ─────────────────────────                                  │
│  Slash commands (.claude/commands/seo-*.md)                 │
│    └─→ MCP servers (~/.claude/mcp-servers/gg-*)             │
│         ├─ gg-search-console → GSC API                      │
│         └─ gg-semrush → SEMrush API (deferred)              │
│    └─→ State files (seo/mapping.md, KEYWORDS.md, ...)       │
│    └─→ Dated reports (seo/reports/*.md)                     │
└─────────────────────────────────────────────────────────────┘
```

---

## Cron / automation

**Текущее состояние:** автоматизации нет — лендинг статичный, никаких длительных процессов в инфре.

В отличие от SAT (App Runner с `BlogAutopublishService` и `BlogSeoMonitorService` через `@Cron`), здесь все операции — **manual** через slash-команды.

| Cadence | Команда | Trigger |
|---|---|---|
| Каждый рабочий день | `/seo-daily` | Lev запускает в Claude Code |
| Еженедельно (пятница) | `/seo-weekly` | Lev запускает |
| Еженедельно | `/seo-gsc` | Lev запускает |
| Ежемесячно | `/seo-semrush` | Lev запускает |
| Ежемесячно | `/seo-audit` | Lev запускает |
| Ежемесячно (1-е число) | `/seo-report` | Lev запускает |
| Ежеквартально | `/seo-gaps` | Lev запускает |
| После каждого деплоя | `/seo-crawl` | Lev запускает |

**Future**: можно поднять локальный cron на Mac через `launchd` или серверный воркер на EC2 если захочется автоматики (Lev уже арендует `18.211.149.64` для других сервисов).

---

## Components

### 1. Slash commands (`.claude/commands/seo-*.md`)

10 команд, каждая — одна задача. Pattern взят у SAT (system-prompt + step-by-step + MCP calls + output format).

| Команда | Зависимости | Output |
|---|---|---|
| `/seo-crawl` | WebFetch | `seo/mapping.md` + `seo/reports/seo-crawl-*.md` |
| `/seo-gsc` | `mcp__gg-search-console__*` | `seo/mapping.md` + `seo/trends.md` + `seo/reports/seo-gsc-*.md` |
| `/seo-semrush` | `mcp__gg-semrush__*` | `seo/mapping.md` + `seo/trends.md` + `seo/reports/seo-semrush-*.md` |
| `/seo-keywords` | `mcp__gg-semrush__*` | `seo/KEYWORDS.md` + `seo/mapping.md` |
| `/seo-diagnose` | `mcp__gg-semrush__*` + WebFetch | `seo/mapping.md` + `seo/reports/seo-diagnose-*.md` |
| `/seo-gaps` | `mcp__gg-semrush__*` | `seo/mapping.md` + `seo/reports/seo-gaps-*.md` + `seo/COMPETITORS.md` |
| `/seo-audit` | none (читает mapping) | `seo/mapping.md` + `seo/reports/seo-audit-*.md` |
| `/seo-daily` | `mcp__gg-search-console__*` | `seo/DAILY-LOG.md` (append) |
| `/seo-weekly` | none | `seo/reports/seo-weekly-*.md` |
| `/seo-report` | none | `seo/mapping.md` + `seo/reports/seo-report-*.md` |

### 2. MCP-серверы (вне репо, в `~/.claude/mcp-servers/`)

| Сервер | Источник pattern | Status | Endpoints |
|---|---|---|---|
| `gg-search-console` | копия `sat-search-console` | ⏳ pending OAuth | `site_snapshot`, `advanced_search_analytics`, `get_index_status`, `get_sitemaps`, `get_pages`, `update_keywords_md` |
| `gg-semrush` | копия `sat-semrush` | ⏳ deferred (Lev купит API) | `domain_rank`, `domain_rank_history`, `domain_organic`, `execute_report` (phrase_organic / phrase_related / domain_organic_organic / domain_domains) |

Setup: `seo/references/MCP-SETUP.md`.

### 3. State files (источник правды)

| Файл | Кто пишет | Что внутри |
|---|---|---|
| `seo/mapping.md` | все команды | Главный source-of-truth — страницы, on-page, GSC/SEMrush metrics, диагнозы, issues, SEO health |
| `seo/STATUS.md` | Lev / агент при значимых изменениях | Снапшот: что работает, что заблокировано, что в очереди |
| `seo/KEYWORDS.md` | `/seo-gsc`, `/seo-keywords` | Target keywords с позициями (Google + Yandex) |
| `seo/AUDIT.md` | Lev / агент после technical аудитов | Стек, on-page, robots, sitemap, perf, verification keys |
| `seo/PLAN.md` | Lev / агент при пересмотре стратегии | 6-месячный план, milestones, decision points |
| `seo/COMPETITORS.md` | `/seo-gaps` + руками | Профили конкурентов, их keyword strategy |
| `seo/DECISIONS.md` | Lev | Лог ключевых решений с обоснованием |
| `seo/CONTENT-PLAN.md` | Lev | Что планируем менять/добавлять на лендинге (FAQ, секции, в будущем блог) |
| `seo/EDITORIAL-GUIDE.md` | once | RU style guide для копирайта |
| `seo/GLOSSARY.md` | once | Термины |
| `seo/DAILY-LOG.md` | `/seo-daily` (append) | Daily snapshots |
| `seo/trends.md` | `/seo-gsc`, `/seo-semrush` | История метрик по месяцам |

### 4. Dated reports (`seo/reports/`)

Append-only — каждый прогон создаёт новый файл с датой. Историю не трогаем.

```
seo/reports/
  seo-crawl-2026-04-28.md
  seo-gsc-2026-04-28.md
  seo-semrush-2026-04-30.md
  seo-audit-2026-05-01.md
  seo-weekly-2026-05-02.md
  ...
```

### 5. References (шаблоны)

| Файл | Что |
|---|---|
| `seo/references/MCP-SETUP.md` | Как поднять `gg-search-console` и `gg-semrush` MCP |
| `seo/references/audit-checklist.md` | Что проверять в кравле, severity scale |
| `seo/references/report-template.md` | Шаблон месячного отчёта |
| `seo/references/weekly-template.md` | Шаблон пятничного digest |
| `seo/references/run-cookbook.md` | Типовые сценарии (cold start, weekly cycle, etc.) |

---

## Verification keys & credentials

### Google Search Console
- **Verification meta**: `emB0zgRKtastF9dTZvO_Oh3Nny9XizERshJLx7mIPEk`
- **Property URL**: `https://global-generations.us/`
- **Где хранится**: `page62702763.html` (Tilda-export, прямо в `<head>`)

### Yandex Webmaster
- **Verification meta**: `mailru-domain: Ny0LUK0o7rTUxyPh` (нестандартный формат — Tilda использует mailru-domain как proxy)
- **Где хранится**: `page62702763.html`
- **Note**: проверить что Yandex действительно принял этот формат (стандартный — `<meta name="yandex-verification" content="...">`)

### Google Analytics 4
- **Measurement ID**: `G-PWTD9C4LZ1`
- **Где хранится**: `page62702763.html` через GTM

### Google Tag Manager
- **Container ID**: `GTM-PP5B2S79`
- **Где хранится**: `page62702763.html` (Tilda-injected GTM snippet)

### MCP credentials
- **GSC OAuth client** — нужно создать в Google Cloud Console (Desktop app type)
- **GSC refresh token** — генерится через `references/MCP-SETUP.md` → раздел 1
- **SEMrush API key** — после покупки, из SEMrush Subscription → API access

---

## SEO-related files in repo

| Файл | Что делает |
|---|---|
| `page62702763.html` | Главная (и единственная) HTML страница, содержит все meta-теги, JSON-LD, GTM/GA4 snippets |
| `404.html` | Error page (не индексируется по умолчанию) |
| `robots.txt` | Allow/Disallow правила, AI-bots блокировка, sitemap reference |
| `sitemap.xml` | 7 URL — `/` + 6 dead `/usa /services ...` (TODO: почистить) |
| `.claude/commands/seo-*.md` | 10 slash-команд |
| `seo/*.md` | State файлы, документация, decision log, шаблоны |

---

## Что нет (но было у SAT и можно скопировать)

| Компонент SAT | Зачем у SAT | Стоит ли копировать |
|---|---|---|
| `BlogAutopublishService` (cron 06:00 UTC) | Авто-генерация статей через Claude API + commit на GitHub | ❌ не сейчас. Если будет блог — копировать pattern |
| `BlogSeoMonitorService` (cron 08:00 UTC) | Daily GSC мониторинг + Telegram-репорт | 🟡 имеет смысл если хочется без ручного `/seo-daily` |
| Indexing API + IndexNow | После каждой публикации статьи push в Google/Bing/Yandex | ❌ для single-page бесполезно |
| Multi-locale articles + hreflang | RU/KK/EN | ❌ только RU |
| `frontend/src/app/sitemap.ts` (динамический) | Next.js auto-generate sitemap из articles registry | ❌ статика — sitemap руками |
| `frontend/src/lib/i18n/metadata.ts` (centralized) | Все meta в одном модуле | ⚠️ Tilda-export — meta в HTML, нельзя централизовать без миграции платформы |

---

## Когда обновлять SYSTEM.md

- При смене hosting / CDN / DNS provider
- При добавлении нового компонента (cron-сервис, новый MCP, новый state-файл)
- При миграции на другую SEO-стратегию (single-page → blog → multi-page)
- При изменении component dependencies (например, MCP подключили к другому ID property)
