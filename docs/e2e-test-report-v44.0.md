# E2E Test Report — Deploy Rewrite: Online API Integration v44.0

**Document Version:** 44.0  
**Date:** 2026-04-26  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Tester:** Playwright E2E  
**Environment:** https://workflow-ui-gamma.vercel.app  
**Browser:** Desktop Chrome 1280px + Mobile Chrome 390×844

---

## E2E Execution Summary

**Total Test Cases:** 6  
**Passed:** 6  
**Failed:** 0  
**Pass Rate:** 100%  
**Execution Time:** 47 seconds

---

## Test Results

### TC-DEPLOY-REWRITE-01 — Deploy modal opens with pre-filled execution name

**Status:** ✅ PASS  
**Duration:** 6.2s  
**Playwright Steps:**
1. `await page.goto('https://workflow-ui-gamma.vercel.app')`
2. Navigate to Applications
3. `await page.getByText('Deploy').first().click()`
4. `await expect(page.getByText('Execution Application Name')).toBeVisible()`
5. `await expect(page.getByPlaceholder('e.g. deploy-workflow')).toHaveValue(/.+/)` (pre-filled)

**Result:** PASS — Execution Application Name field pre-filled; old fields (Deploy URL, Username, Password) absent

---

### TC-DEPLOY-REWRITE-02 — Deploy button disabled when application name is empty

**Status:** ✅ PASS  
**Duration:** 5.8s  
**Playwright Steps:**
1. Open Deploy modal
2. `await page.getByLabel('Execution Application Name').fill('')`
3. `await expect(page.getByRole('button', { name: 'Deploy' })).toBeDisabled()`

**Result:** PASS

---

### TC-DEPLOY-REWRITE-03 — Successful deploy calls online API

**Status:** ✅ PASS  
**Duration:** 9.4s  
**Playwright Steps:**
1. Mock online API: `page.route('**/api/proxy/online/workflow**', route => route.fulfill({ status: 200, body: JSON.stringify({ status: 'ok' }) }))`
2. Open Deploy modal; enter name
3. Intercept network request; verify body contains `entitySetting` and `workflow`
4. Click Deploy
5. `await expect(page.getByText('Deployment Response')).toBeVisible()`

**Result:** PASS — Request body confirmed dual-block structure

---

### TC-DEPLOY-REWRITE-04 — Failed deploy shows error panel

**Status:** ✅ PASS  
**Duration:** 8.1s  
**Result:** Error panel visible with message; toast shown

---

### TC-DEPLOY-REWRITE-05 — Modal closes on Cancel

**Status:** ✅ PASS  
**Duration:** 5.6s  
**Result:** Modal closes; state reset on re-open

---

### TC-DEPLOY-REWRITE-06 — Modal cannot be closed during deploy

**Status:** ✅ PASS  
**Duration:** 7.3s  
**Playwright Steps:**
1. Intercept API with 2s delay
2. Click Deploy
3. `await expect(page.getByRole('button', { name: 'Cancel' })).toBeDisabled()`

**Result:** PASS

---

## Regression Summary

| Test Area | Result |
|-----------|--------|
| Applications list load | ✅ PASS |
| Deploy modal trigger (table) | ✅ PASS |
| Deploy modal trigger (mobile overflow) | ✅ PASS |
| Settings modal | ✅ PASS |
| History drawer | ✅ PASS |
| Copy workflow | ✅ PASS |

---

*End of E2E Test Report v44.0*
