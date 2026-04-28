# MCP setup — gg-search-console + gg-semrush

Инструкция как поднять MCP-серверы для команд `/seo-gsc`, `/seo-semrush`, `/seo-daily`, `/seo-keywords`, `/seo-diagnose`, `/seo-gaps`.

Серверы живут **вне репо**, в `~/.claude/mcp-servers/`. В `~/.claude/mcp.json` нужно зарегистрировать их так чтобы Claude Code их подхватывал.

## Текущий статус (2026-04-27)

| Сервер | Статус | Блокер |
|---|---|---|
| `gg-search-console` | ⏳ Pending OAuth setup | Нужен refresh token (см. ниже) |
| `gg-semrush` | ⏳ **Deferred** | Lev купит SEMrush API позже. Не настраивать до этого. |

**Что работает прямо сейчас (без MCP):**
- `/seo-crawl` — WebFetch, no API
- `/seo-audit` — read-only по mapping
- `/seo-weekly`, `/seo-report` — синтез из существующих файлов

**Что заблокировано до OAuth:**
- `/seo-gsc`, `/seo-daily`

**Что заблокировано до покупки SEMrush:**
- `/seo-semrush`, `/seo-keywords`, `/seo-diagnose`, `/seo-gaps`

---

## TL;DR

```bash
# 1. Скопировать рабочие MCP (есть в ~/.claude/mcp-servers/sat-search-console и sat-semrush)
cp -r ~/.claude/mcp-servers/sat-search-console ~/.claude/mcp-servers/gg-search-console
cp -r ~/.claude/mcp-servers/sat-semrush ~/.claude/mcp-servers/gg-semrush

# 2. Отредактировать env vars / сборка (см. ниже)
cd ~/.claude/mcp-servers/gg-search-console && npm install && npm run build
cd ~/.claude/mcp-servers/gg-semrush && npm install && npm run build

# 3. Добавить блоки в ~/.claude/mcp.json (см. ниже)

# 4. Reload Claude Code (перезапуск MCP)
```

---

## 1. gg-search-console

### Зависимости
- Node.js + npm
- Google API project с включённым Search Console API
- OAuth client (Desktop app type)
- Refresh token для аккаунта который подтверждён в GSC для `https://global-generations.us/`

### env vars

```bash
GSC_CLIENT_ID=<из Google API console — OAuth client ID>
GSC_CLIENT_SECRET=<из Google API console>
GSC_REFRESH_TOKEN=<полученный через oauth-setup.ts флоу>
SITE_URL=https://global-generations.us/
SEO_DIR=/Users/levavdosin/global-generations-us-tilda/global-generations/docs
```

`SEO_DIR` указывает куда писать апдейты (например, для `update_keywords_md` инструмента).

### Получить refresh token (один раз)

```bash
cd ~/.claude/mcp-servers/gg-search-console
node dist/oauth-setup.js  # либо src/oauth-setup.ts через ts-node
```

Откроет браузер → залогиниться аккаунтом который подтверждён в GSC для global-generations.us → получишь refresh token → положить в env.

### Регистрация в `~/.claude/mcp.json`

```json
{
  "mcpServers": {
    "gg-search-console": {
      "command": "node",
      "args": ["/Users/levavdosin/.claude/mcp-servers/gg-search-console/dist/index.js"],
      "env": {
        "GSC_CLIENT_ID": "...",
        "GSC_CLIENT_SECRET": "...",
        "GSC_REFRESH_TOKEN": "...",
        "SITE_URL": "https://global-generations.us/",
        "SEO_DIR": "/Users/levavdosin/global-generations-us-tilda/global-generations/docs"
      }
    }
  }
}
```

### Проверка

```
mcp__gg-search-console__site_snapshot()
```

Должен вернуть totals по сайту за последние 28 дней.

---

## 2. gg-semrush

> ⏳ **Deferred** — Lev купит SEMrush API позже. До тех пор этот раздел — справочный, не настраивать.

### Зависимости
- Node.js + npm
- SEMrush API key (оплачивается отдельно — Lev купит, см. SEMrush.com → Subscription → API access)

### env vars

```bash
SEMRUSH_API_KEY=<из SEMrush Subscription info → API access>
SEMRUSH_DATABASE=ru
```

### Регистрация в `~/.claude/mcp.json`

```json
{
  "mcpServers": {
    "gg-semrush": {
      "command": "node",
      "args": ["/Users/levavdosin/.claude/mcp-servers/gg-semrush/dist/index.js"],
      "env": {
        "SEMRUSH_API_KEY": "...",
        "SEMRUSH_DATABASE": "ru"
      }
    }
  }
}
```

### Проверка

```
mcp__gg-semrush__domain_rank(domain="global-generations.us", database="ru")
```

Должен вернуть traffic / keywords / authority для домена (если данные у SEMrush ещё накоплены — для нового домена может быть пусто, это норма).

---

## 3. Полный mcp.json fragment

После добавления оба блока должны выглядеть так:

```json
{
  "mcpServers": {
    "...другие сервера...": {},
    "gg-search-console": {
      "command": "node",
      "args": ["/Users/levavdosin/.claude/mcp-servers/gg-search-console/dist/index.js"],
      "env": { ... }
    },
    "gg-semrush": {
      "command": "node",
      "args": ["/Users/levavdosin/.claude/mcp-servers/gg-semrush/dist/index.js"],
      "env": { ... }
    }
  }
}
```

---

## Проверка end-to-end

После настройки:

1. Перезапусти Claude Code (новые MCP подгружаются).
2. В чате скажи `/seo-daily` — команда должна выполнить index_status + sitemap + 7d performance и записать в `docs/seo-daily-log.md`.
3. Если ошибка `MCP server not found` → `mcp.json` синтаксически неверен или путь к dist/index.js не найден.
4. Если ошибка `Authentication failed` → проблема с GSC refresh token (истёк / OAuth client revoked) либо неверный SEMrush key.

---

## Troubleshooting

| Ошибка | Что проверить |
|---|---|
| `Cannot find module '/.../dist/index.js'` | `npm run build` в директории MCP |
| GSC `invalid_grant` | refresh token истёк / OAuth client revoked → перегенерировать |
| GSC `403 not verified` | аккаунт не имеет доступа к global-generations.us в GSC. Lev должен пригласить себя владельцем GSC property. |
| SEMrush `LIMIT EXCEEDED` | API quota исчерпан на месяц |
| SEMrush `NOTHING FOUND` | домен слишком новый, в их БД ещё нет данных. Норма для new domains, повторить через 1-2 недели |
