# UI Test Report — Deploy Rewrite: Online API Integration v44.0

**Document Version:** 44.0  
**Date:** 2026-04-26  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Tester:** Manual QA  
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

### TC-DEPLOY-REWRITE-01 — Deploy modal opens with pre-filled execution name

**Status:** ✅ PASS  
**Result:**
- Modal opens correctly with title "Deploy Application"
- Execution Application Name pre-filled with current application name
- Deploy URL, Username, Password, Environment fields removed
- Block A / Block B preview panel visible

---

### TC-DEPLOY-REWRITE-02 — Deploy button disabled when application name is empty

**Status:** ✅ PASS  
**Result:** Deploy button correctly disabled when field is empty; re-enables when text is entered

---

### TC-DEPLOY-REWRITE-03 — Successful deploy calls online API

**Status:** ✅ PASS  
**Result:**
- POST to online API made with correct `applicationName` query param
- Request body JSON contains both `entitySetting` and `workflow` keys
- Green response panel displayed
- "Deployment initiated" toast shown

---

### TC-DEPLOY-REWRITE-04 — Failed deploy shows error panel

**Status:** ✅ PASS  
**Result:** Red error panel shown with error message; toast displayed; button re-enabled

---

### TC-DEPLOY-REWRITE-05 — Modal closes on Cancel

**Status:** ✅ PASS  
**Result:** Modal closes; state reset on re-open

---

### TC-DEPLOY-REWRITE-06 — Modal cannot be closed during deploy

**Status:** ✅ PASS  
**Result:** Cancel and X buttons disabled during deploying state

---

## Regression Testing

- Applications list loads normally
- Deploy button in table still opens modal
- Deploy button in mobile overflow menu still opens modal
- Settings, History, Copy, Delete actions unaffected

---

## Issues Found

**None**

---

*End of UI Test Report v44.0*
