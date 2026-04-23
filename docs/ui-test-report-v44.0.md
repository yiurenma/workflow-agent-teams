# UI Test Report — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-23  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Tester:** Manual UAT  
**Environment:** https://workflow-ui-gamma.vercel.app  
**Browser:** Desktop Chrome 1280px

---

## Test Execution Summary

**Total Test Cases:** 6  
**Passed:** 6  
**Failed:** 0  
**Pass Rate:** 100%

---

## Test Results

### TC-DEPLOY-ONLINE-01 — Deploy button opens modal with pre-filled application name

**Status:** ✅ PASS

- Modal opens correctly
- Execution Application Name pre-filled from current app
- No legacy fields (Deploy URL, Username, Password, Environment) present
- Payload summary panel visible with Block A / Block B labels

---

### TC-DEPLOY-ONLINE-02 — Deploy with default application name submits to online API

**Status:** ✅ PASS

- "Deploying…" button state shown during request
- Online API POST confirmed via Network tab
- Request body contained `entitySetting` and `workflow` blocks
- Success banner displayed: "✓ Deployment submitted successfully"

---

### TC-DEPLOY-ONLINE-03 — Deploy with custom execution application name

**Status:** ✅ PASS

- Custom application name sent correctly in `?applicationName=` query param
- Source data (Block A / Block B) unchanged — still from Hub current app

---

### TC-DEPLOY-ONLINE-04 — Empty application name is rejected

**Status:** ✅ PASS

- Error toast "Application name is required" shown
- No network request made

---

### TC-DEPLOY-ONLINE-05 — API error is displayed

**Status:** ✅ PASS

- Error banner displayed with message from online API response
- Modal remains open for retry

---

### TC-DEPLOY-ONLINE-06 — Cancel closes modal without calling API

**Status:** ✅ PASS

- Modal closed cleanly
- No network request observed

---

## Regression Check

- Applications list: ✅ No regression
- Deploy button placement (table Actions + mobile overflow): ✅ Unchanged
- All other modal interactions: ✅ No regression

---

*End of UI Test Report v44.0*
