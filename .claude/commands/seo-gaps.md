---
description: Конкурентный ландшафт + keyword gap analysis. Авто-обнаружение конкурентов через SEMrush + curated list (RU education-консалтинг для США). Находит keywords где конкуренты ранжируются а мы нет.
argument-hint: [optional "--gaps-only" чтобы пропустить competitor overview]
---

# /seo-gaps — Competitive landscape + keyword gaps

**Target file:** `docs/seo_mapping.md` (sections: `### Competitive Landscape`, `### Keyword Opportunities`)
**Aux output:** `docs/seo-gaps-YYYY-MM-DD.md`
**Argument:** `$ARGUMENTS` — `--gaps-only` пропускает competitor overview, сразу к gap-кейвордам.
**Requires:** MCP `gg-semrush`.

**Curated competitors (RU education-консалтинг US):**
- globaldialog.ru
- mba-strategy.com
- educationusa.ru
- studyusa.com
- linguatrip.com
- usa.studies.ru
- hardway.school

---

## Step 1: Auto-discover competitors

```
mcp__gg-semrush__execute_report(report="domain_organic_organic", domain="global-generations.us", database="ru", display_limit=20)
```

Слить с curated list. Дубликаты убрать.

## Step 2: Per competitor — overview (skip если --gaps-only)

Для каждого из топ 5–7 финального списка:

```
mcp__gg-semrush__domain_rank(domain="<comp>", database="ru")
mcp__gg-semrush__execute_report(report="domain_organic", domain="<comp>", database="ru", display_limit=20)
```

Собрать:
- Traffic (visits/mo)
- Keywords count
- Authority Score / Rank
- Top-20 organic keywords (имеющие отношение к нашему ICP)

Записать в `### Competitive Landscape`:

```
| Competitor | Traffic | Keywords | AS | Top keywords (в нашем space) |
|---|---|---|---|---|
| globaldialog.ru | N | N | X | "поступление в вузы США", ... |
```

## Step 3: Gap analysis

Для каждого конкурента (top 5):

```
mcp__gg-semrush__execute_report(report="domain_domains", domains=["global-generations.us|*", "<comp>|*"], database="ru", display_limit=200)
```

Это даст keywords где конкурент ранжируется (top-50), а мы нет (>80 / not ranking).

Filter:
- Vol ≥ 100
- ICP-relevant (для нас: education / US universities / стипендии / экзамены / поступление). Skip B2B/HR/employment / consumer staples.
- KD ≤ 55

Score:
- Opportunity = vol × (1 / competitor_position)
- Высокий opportunity = высокий vol + конкурент тоже не в топ-3 → достижимо

## Step 4: Topic clustering

Сгруппировать gap-keywords по кластерам:
- Поступление общее
- Финансирование/стипендии
- Экзамены (SAT, TOEFL, IELTS)
- Документы (motivational letter, recommendations, transcripts)
- Конкретные университеты / Лига плюща
- По специальностям
- Visa / F-1
- Подготовка к учёбе / адаптация

## Step 5: Update mapping

В `### Keyword Opportunities`:

```
| Keyword | Vol | KD | Best comp pos | Cluster | Opportunity score |
|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... |
```

Top-N (N = 30) высоких opportunity — добавить как `**Secondary** candidate` в страницу `/` (с пометкой `*(gap — {YYYY-MM-DD})*`).

## Step 6: Dated report

`docs/seo-gaps-YYYY-MM-DD.md`:

```
# Competitive & Gap Analysis — {YYYY-MM-DD}

## Competitive matrix
{table}

## Top opportunities (high value)
{table}

## By cluster
### Поступление общее
- ...

### Финансирование
- ...

## Recommendations
1. Что добавить в landing
2. Что вынести в отдельные страницы (если делать blog/листинг)
3. Что игнорировать
```

---

## Notes

- Gap analysis имеет смысл раз в квартал, не чаще.
- Для single-page лендинга многие gaps решаются не на самом лендинге, а добавлением блог-/listing-страниц. Gap report в таком случае работает как input для product roadmap.
- KD-фильтр 55 — потому что домен новый и backlink-профиль слабый.
