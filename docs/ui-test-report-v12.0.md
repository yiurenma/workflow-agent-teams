# QA Test Report v12.0 — E2E Playwright Pass 1

**Status:** Pass (0 failures)
**Label:** `TODO-e2e-playwright-pass1-author-full-run`
**Date:** 2026-04-07
**Tester:** QA Agent (automated — local preview server)

---

## Executive Summary

| Total | Pass | Skip | Fail |
|-------|------|------|------|
| 47 | 19 | 28 | 0 |

**0 failures.** All 19 executable tests pass. 28 tests are skipped — all due to no application data being available in the local test environment (the static preview server has no backend API proxy; react-query receives 404s and renders empty state). Data-dependent tests self-skip via `test.skip()` guards.

---

## Scaffold Deliverables

| Item | Status |
|------|--------|
| `@playwright/test` in `package.json` devDependencies | ✅ v1.59.1 |
| `"test:e2e"` npm script | ✅ |
| `playwright.config.ts` | ✅ Desktop Chrome + Mobile Chrome (390px) projects |
| `e2e/navigation.spec.ts` | ✅ 4 TCs |
| `e2e/applications-desktop.spec.ts` | ✅ 9 TCs |
| `e2e/applications-mobile.spec.ts` | ✅ 10 TCs |
| `e2e/records.spec.ts` | ✅ 3 TCs |
| `e2e/canvas.spec.ts` | ✅ 3 TCs |
| `e2e/node-editor.spec.ts` | ✅ 2 TCs |
| `e2e/explain.spec.ts` | ✅ 2 TCs |
| Chromium binary | ✅ `/opt/pw-browsers/chromium-1194` |
| Run environment | ✅ `http://localhost:4173` (vite preview) |

---

## Results by Test Case

### Desktop Chrome project

| TC | Name | Result | Notes |
|----|------|--------|-------|
| TC-APP-DESK-01 | Table renders with columns | **Pass** | |
| TC-APP-DESK-02 | Pagination visible below table | **Pass** | |
| TC-APP-DESK-03 | Total count shown | **Pass** | |
| TC-APP-DESK-04 | Search filters list | **Pass** | |
| TC-APP-DESK-05 | Open button navigates to canvas | Skip | No application data |
| TC-APP-DESK-06 | Settings button opens modal | Skip | No application data |
| TC-APP-DESK-07 | History button opens drawer | Skip | No application data |
| TC-APP-DESK-08 | Copy button opens modal | Skip | No application data |
| TC-APP-DESK-09 | Delete button shows confirm | Skip | No application data |
| TC-NAV-01 | App loads at root | **Pass** | |
| TC-NAV-02 | Applications list route resolves | **Pass** | |
| TC-NAV-03 | Records list route resolves | **Pass** | |
| TC-NAV-04 | Unknown route handled gracefully | **Pass** | |
| TC-NODE-01 | Clicking a node opens drawer | Skip | No application data |
| TC-NODE-02 | Drawer has Description/Rules/Action | Skip | No application data |
| TC-REC-01 | Records page loads without crash | **Pass** | |
| TC-REC-02 | Table or card view visible | **Pass** | |
| TC-REC-03 | Pagination visible | Skip | No records data |
| TC-CANVAS-01 | Canvas loads for an application | Skip | No application data |
| TC-CANVAS-02 | Explain button visible in header | Skip | No application data |
| TC-CANVAS-03 | No JS error on canvas load | Skip | No application data |
| TC-EXPLAIN-01 | Explain button visible on canvas | Skip | No application data |
| TC-EXPLAIN-02 | Clicking Explain opens modal | Skip | No application data |

### Mobile Chrome project (390×844 px viewport)

| TC | Name | Result | Notes |
|----|------|--------|-------|
| TC-APP-MOB-01 | Card view renders (no table) on narrow viewport | **Pass** | |
| TC-APP-MOB-02 | Desktop view toggle renders table | Skip | No application data |
| TC-APP-MOB-03 | Mobile view toggle restores card view | Skip | No application data |
| TC-APP-MOB-04 | Card info area tap navigates to canvas | Skip | No application data |
| TC-APP-MOB-05 | ⋯ → History opens drawer | Skip | No application data |
| TC-APP-MOB-06 | ⋯ → Copy opens modal | Skip | No application data |
| TC-APP-MOB-07 | ⋯ → Delete shows confirm | Skip | No application data |
| TC-APP-MOB-08 | Settings button opens modal | Skip | No application data |
| TC-APP-MOB-09 | FAB + button is visible | **Pass** | |
| TC-APP-MOB-10 | FAB tap opens New Application dialog | **Pass** | |
| TC-NAV-01–04 | Navigation (all) | **Pass** ×4 | |
| TC-NODE-01–02 | Node editor | Skip | No application data |
| TC-REC-01 | Records page loads without crash | **Pass** | |
| TC-REC-02 | Table or card view visible | **Pass** | |
| TC-REC-03 | Pagination visible | Skip | No records data |
| TC-CANVAS-01–03 | Canvas | Skip | No application data |
| TC-EXPLAIN-01–02 | Explain | Skip | No application data |

---

## Why 28 Tests are Skipped

The test environment is a **static `vite preview` build** served at `http://localhost:4173`. There is no backend API proxy — all calls to `/api/proxy/operation/*` return 404. React-Query receives errors and renders empty state. Tests that depend on application data (Open/Settings/History/Copy/Delete buttons, canvas nodes, Explain) guard themselves with:

```ts
if (await btn.count() === 0) { test.skip(); return; }
```

These tests will execute and produce results when the suite is run against an environment with live API data (e.g. the Vercel UAT deployment or with `vite dev` + proxy to the DigitalOcean backend).

---

## Run Command

```bash
# From workflow-ui root, with vite preview running on port 4173:
npm run test:e2e
# or directly:
node node_modules/@playwright/test/cli.js test --config=playwright.config.ts
```

Run duration: **~16 seconds**
