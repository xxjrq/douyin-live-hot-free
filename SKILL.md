---
name: douyin-live-hot-free
description: Search current Douyin Selected web results for any keyword through an existing Easy WebBridge browser session, then export structured JSON, Markdown, and a searchable HTML report. Use when an agent needs free Douyin keyword search, current topic discovery, visible result metadata, creator research, AI-tool content discovery, or a point-in-time search snapshot without a paid data API. Requires Easy WebBridge and an authorized Chromium browser session.
license: MIT
metadata:
  author: xxjrq
  version: "0.2.0"
  runtime: "easy-webbridge >=0.3.2"
  runtime-repository: "https://github.com/xxjrq/easy-webbridge"
---

# Douyin Live Hot Free

Search Douyin Selected by keyword through Easy WebBridge. Treat output as a point-in-time search-page snapshot, not an official API response, popularity ranking, or historical trend.

## Preflight

1. Require Easy WebBridge on `127.0.0.1` and one authorized browser online.
2. List browsers before every run because `browserId` and online state can change.
3. Select the exact display name or `browserId`; never broadcast commands.
4. Stop on login, CAPTCHA, access-control, or risk-control pages.

```bash
node scripts/douyin-live-hot-free.mjs list
```

## Search

```bash
node scripts/douyin-live-hot-free.mjs search \
  --keyword "AI工具" \
  --browser "自媒体" \
  --limit 20 \
  --output /absolute/path/to/output
```

The command creates:

- `search-results.json`: normalized search snapshot.
- `search-results.md`: compact result table.
- `index.html`: responsive report with local text and type filters.

The script creates one task-scoped Easy WebBridge group, performs all steps in that group, reuses its tab, and closes the group after export. Pass `--keep-tab` only when the user explicitly wants the search page left open.

## Operation sequence

1. Select one online browser.
2. Create one task-scoped browser group.
3. Open `https://www.douyin.com/jingxuan/search/<keyword>?type=general` in that group.
4. Wait for the official search input and semantic result cards.
5. Scroll only as needed for the requested result limit.
6. Read visible fields and export the snapshot.
7. Close only the task-scoped group.

Read [references/search-method.md](references/search-method.md) before changing browser behavior. Read [references/data-schema.md](references/data-schema.md) before changing output fields or adding consumers.

## Render saved data

```bash
node scripts/douyin-live-hot-free.mjs render \
  --input /absolute/path/search-results.json \
  --output /absolute/path/to/output
```

## Guardrails

- Depend on Easy WebBridge through its local HTTP API; do not copy extension code or use repository-relative imports.
- Never store bridge tokens, cookies, passwords, account identifiers, browser IDs, or session state in outputs.
- Call the action “搜索”, not “查询接口” or “热榜排名”.
- Use only fields visible on the current search page. Do not invent views, comments, shares, growth rates, or historical movement.
- Preserve work URLs as source links without implying Douyin endorsement.
- Do not publish, send messages, modify accounts, or perform unrelated browser actions.
