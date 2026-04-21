---
name: wechat-miniprogram-production
description: End-to-end production workflow for WeChat Mini Programs. Use when building a mini program from a reference app, screenshots, product idea, or existing prototype; covers competitor/function analysis, prototype/docs, WeChat project scaffolding, real local-storage features, personal-account compliance cleanup, legal text, verification in WeChat Developer Tools, and submission-review preparation.
---

# WeChat Mini Program Production

## Goal

Turn a reference mini program or product idea into a WeChat Mini Program that is usable enough for first submission review, not just a clickable demo.

Prefer a conservative first version: local utility value, clear flows, no risky social/financial/third-party features unless explicitly required and suitable for the registered subject type.

## Workflow

### 1. Establish Scope

Confirm or infer:

- Subject type: personal, company, or other.
- First-version target: prototype, demo, or submission-ready.
- Reference source: screenshots, desktop mini program, web prototype, PRD, or verbal description.
- Data approach: local cache first unless the user explicitly needs sync/cloud.

For personal-subject mini programs, default to hiding:

- multi-user shared accounts
- invite/member management
- AA splitting or settlement
- payment, lending, wealth management, rebate, investment features
- official-account follow prompts
- third-party mini program jumps
- forced ads or reward-gated core functions

### 2. Inspect Reference Product

When a reference mini program or screenshots are available:

- Browse every major tab and menu entry.
- Click each available button in critical flows.
- Test scrollable regions, tabs, sheets, modals, pickers, and disabled states.
- Record functions as: keep, simplify, hide, or skip.
- If using desktop control, verify actual rendered screenshots instead of relying only on source assumptions.

For each important page, capture:

- page purpose
- top navigation/title
- primary actions
- list/card structure
- modal/sheet structure
- data fields
- expected empty/error states

### 3. Produce Planning Artifacts

Create concise project docs when starting from scratch:

- `docs/PRD.md`: product goal, users, core flows, first-version scope.
- `docs/FUNCTION_MAP.md`: reference features mapped to keep/simplify/hide/skip.
- `docs/DEVELOPMENT.md`: project structure, data model, run/test notes.
- `docs/SUBMISSION_CHECKLIST.md`: review-readiness checklist.

Keep these docs practical. Do not let documentation replace implementation.

### 4. Scaffold Project

Create a standard WeChat Mini Program structure:

```text
project.config.json
project.private.config.json
miniprogram/
  app.js
  app.json
  app.wxss
  sitemap.json
  utils/
    store.js
    finance.js
  pages/
    home/
    detail/
    mine/
```

Add pages based on scope: stats, calendar, assets, category management, account management, import, export, about, legal, edit pages.

Use `app.json` as the source of truth for routes. After deleting pages, verify every route has `.js`, `.json`, `.wxml`, and `.wxss`.

### 5. Build Usable Data First

Avoid demo-only state once moving toward review.

For local-first projects, implement `utils/store.js` with:

- `ensureSeed()`
- `getState()`
- `setState()`
- domain methods such as add/update/remove bills, categories, accounts
- migration/normalization for old local cache

Use `wx.getStorageSync` / `wx.setStorageSync` for first versions unless cloud sync is explicitly required.

Normalize data on launch:

- ensure required arrays/objects exist
- coerce amounts to numbers
- remove hidden/risky demo entries
- preserve user-created data during migration

### 6. Implement Real CRUD

For each core domain, include actual create/update/delete behavior:

- Bills: add, edit, delete, list, filter, group by date.
- Categories: add, switch type, delete/hide.
- Accounts: add, edit balance/name where needed, delete.
- Import/export: use a minimal robust format such as CSV copy/paste for first version.

Prefer simple WeChat-native interactions first:

- `wx.showActionSheet` for item actions
- `wx.showModal({ editable: true })` for simple text input
- `picker mode="date"` for dates
- `wx.setClipboardData` for CSV export

### 7. Match UI Against Real Developer Tools

Do not trust WXML/WXSS intent alone. Verify in WeChat Developer Tools.

Common mini-program layout pitfalls:

- Native `button` has default layout behavior. Use `view bindtap` for non-form controls such as keypads, chips, category icons, and menu rows.
- Avoid component WXSS tag selectors such as `button`, `view`, `text`, `input`, `textarea` inside page WXSS.
- If `calc()` or percentage widths render incorrectly, use fixed `rpx` widths for stable grids.
- For horizontally scrollable chip rows, use `scroll-view scroll-x` and fixed chip widths.
- For vertically scrollable category grids, use `scroll-view scroll-y` and fixed row/column sizes.
- Always compare screenshots after compiling, not just after editing.

### 8. Prepare Compliance for Submission

For personal-subject first versions:

- Remove unused risky pages from `app.json`, not just hide buttons.
- Delete orphan risky page files if they contain sensitive visible text.
- Search source for risky strings before finalizing.
- Provide in-app legal pages:
  - user agreement
  - privacy policy
  - version notes
- State clearly if data is local-only and not uploaded.

Read `references/audit-checklist.md` before final submission preparation.

### 9. Validate

Run these checks before final response:

```powershell
Get-ChildItem -Path 'miniprogram' -Recurse -Filter '*.js' | ForEach-Object { node --check $_.FullName }
Get-ChildItem -Path 'miniprogram' -Recurse -Filter '*.json' | ForEach-Object { $null = Get-Content -Encoding utf8 -Raw $_.FullName | ConvertFrom-Json }
```

Check routes:

```powershell
$app = Get-Content -Encoding utf8 -Raw 'miniprogram\app.json' | ConvertFrom-Json
$missing = @()
foreach ($p in $app.pages) {
  foreach ($ext in 'js','json','wxml','wxss') {
    $path = Join-Path 'miniprogram' ($p + '.' + $ext)
    if (-not (Test-Path $path)) { $missing += $path }
  }
}
$missing
```

Check page WXSS:

```powershell
Get-ChildItem -Path 'miniprogram\pages' -Recurse -Filter '*.wxss' |
  Select-String -Pattern '\s(button|view|text|input|textarea)(:|\s|\{|\.|#)'
```

Search for risky visible strings relevant to the project before upload.

## Final Handoff

Summarize:

- what moved from demo to usable
- storage approach
- key files changed
- verification commands run
- remaining platform/manual steps: upload, service category, privacy protection guide, review note

Do not claim submission readiness until the project has been compiled in WeChat Developer Tools and the critical flows have been clicked through.
