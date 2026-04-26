# UAT Report — Deploy Rewrite: Online API Integration v44.0

**Document Version:** 44.0  
**Date:** 2026-04-26  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Tester:** E2E Tester (Playwright UAT)  
**Environment:** https://workflow-ui-gamma.vercel.app  
**Browser:** Desktop Chrome 1280px + Mobile Chrome 390×844  
**Test Mode:** Headed (visual verification)

---

## UAT Execution Summary

**Total Test Cases:** 6  
**Passed:** 6  
**Failed:** 0  
**Pass Rate:** 100%  
**Execution Time:** 47 seconds

---

## UAT Verdict

✅ **PASS** — All 6 test cases passed. Deploy modal successfully rewritten to call the online API with dual-block request body. Old form fields (Deploy URL, Username, Password, Environment) removed. Ready for production.

---

## Test Results

### TC-DEPLOY-REWRITE-01 — Deploy modal opens with pre-filled execution name

**Status:** ✅ PASS  
**Duration:** 6.2s

**UAT Steps:**
1. Navigated to https://workflow-ui-gamma.vercel.app
2. Clicked Deploy for the first application in the list
3. Modal opened

**UAT Result:**
- ✅ Title: "Deploy Application"
- ✅ Field: "Execution Application Name" pre-filled with application name
- ✅ No Deploy URL field visible
- ✅ No Username/Password fields visible
- ✅ No Environment field visible
- ✅ Request Body preview shows Block A (source app name) and Block B (node count)

---

### TC-DEPLOY-REWRITE-02 — Deploy button disabled when application name is empty

**Status:** ✅ PASS  
**Duration:** 5.8s

**UAT Result:**
- ✅ Clear the Execution Application Name field → Deploy button disabled
- ✅ Re-enter a name → Deploy button enabled

---

### TC-DEPLOY-REWRITE-03 — Successful deploy calls online API

**Status:** ✅ PASS  
**Duration:** 9.4s

**UAT Result:**
- ✅ POST request made to `/api/proxy/online/workflow?applicationName=test-app`
- ✅ Request body JSON: `{ "entitySetting": {...}, "workflow": {...} }`
- ✅ Green "Deployment Response" panel displayed
- ✅ Toast: "Deployment initiated"

---

### TC-DEPLOY-REWRITE-04 — Failed deploy shows error panel

**Status:** ✅ PASS  
**Duration:** 8.1s

**UAT Result:**
- ✅ Red "Deployment Failed" panel with error message
- ✅ Toast: error message
- ✅ Deploy button re-enabled

---

### TC-DEPLOY-REWRITE-05 — Modal closes on Cancel

**Status:** ✅ PASS  
**Duration:** 5.6s

**UAT Result:**
- ✅ Modal closes immediately
- ✅ State reset on re-open (no stale response/error)

---

### TC-DEPLOY-REWRITE-06 — Modal cannot be closed during deploy

**Status:** ✅ PASS  
**Duration:** 7.3s

**UAT Result:**
- ✅ Cancel button disabled while deploying
- ✅ X button disabled while deploying
- ✅ Click outside modal does not close while deploying

---

## Mobile UAT

**Device:** Mobile Chrome 390×844  
**Test Cases:** TC-DEPLOY-REWRITE-01 through TC-DEPLOY-REWRITE-06 re-run on mobile  
**Result:** ✅ All PASS  
**Notes:** Modal responsive; Deploy button accessible via overflow menu in mobile card view

---

## Regression Testing

**All existing application list actions:** PASS (Open, Settings, History, Copy, Delete unaffected)

---

## Issues Found

**None**

---

## Conclusion

All 6 UAT test cases passed on both desktop and mobile. The Deploy modal has been successfully rewritten to:

1. Remove direct remote-server calls (no Deploy URL, no credentials)
2. Call the online API with `applicationName` as query parameter
3. Send dual-block request body: Block A (entitySetting) + Block B (workflow)
4. Display the online API's response

**UAT Verdict:** ✅ PASS — Ready for production deployment

---

*End of UAT Report v44.0*
