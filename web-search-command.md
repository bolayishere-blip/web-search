---
name: web-search-command
description: >
  IMMEDIATELY activate this skill when the user's message contains `/x`
  as a standalone token — with or without a following query, at any
  position in the message. Do NOT wait for further explanation.
  Trigger patterns: `/x`, `/x <query>`, `/x <URL>`,
  or any message containing `/x` as a token.
  When triggered: extract the query (or infer from context),
  then perform a web search or URL fetch as described below.
examples:
  - input: "/x bitcoin price"
    action: "Search: 'bitcoin price'"
  - input: "/x"
    action: "Infer topic from last user message, then search"
  - input: "tell me about /x climate change"
    action: "Search: 'climate change'"
  - input: "/x https://example.com"
    action: "Fetch URL directly: https://example.com"
  - input: "/x ရေနံဈေးနှုန်း"
    action: "Search: 'ရေနံဈေးနှုန်း'"
---

# Web Search Command Skill (`/x`)

## Purpose

When the user types `/x [query]`, this skill:
**Performs a live web search** on the query (or infers topic from conversation context if no query given)

---

## Trigger Detection

| Input pattern | Action |
|---|---|
| `/x bitcoin price` | Search: "bitcoin price" |
| `/x` (no query) | Infer topic from last user message or recent conversation |
| `tell me about /x climate change` | Search: "climate change" |
| `/x https://example.com` | Fetch that URL directly as context |

---

## Step-by-Step Execution

### Step 1 — Detect and Extract Query

- Strip the `/x` token
- If remaining text is a **URL** → go to **URL Fetch Mode**
- If remaining text is a **search query** → go to **Search Mode**
- If no text → **infer** from last user message topic

### Step 2a — Search Mode

Use `web_search` tool with the extracted query. Fetch top results.
Then use `web_fetch` on the most relevant 1–2 URLs for full content.

### Step 2b — URL Fetch Mode

Use `web_fetch` directly on the provided URL.

---

## Error Handling

| Situation | Response |
|---|---|
| Search returns no results | Tell user in Burmese, suggest alternative query |
| URL fetch fails (403/timeout) | Note failure, use search snippets only |
| Query too vague | Ask one clarifying question in Burmese |
| `/x` with no context at all | Ask "ဘာအကြောင်း ရှာမလဲ?" |

---
