# UAT Report — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-24  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Tester:** E2E UAT  
**Environment:** https://workflow-ui-gamma.vercel.app  
**Browser:** Desktop Chrome 1280px + Mobile Chrome 390×844

---

## UAT Execution Summary

**Total Test Cases:** 6  
**Passed:** 6  
**Failed:** 0  
**Pass Rate:** 100%

---

## UAT Verdict

✅ **PASS** — `DeployModal` rewritten to call online API. Single-field form with two-block request body (Block A: applicationSettings, Block B: workflow). Success/error states correct. State resets on reopen. No regressions in application list.

---

## Test Results

### TC-DEPLOY-ONLINE-01 — Deploy triggers online API

**Status:** ✅ PASS  

Verified via browser network tab: single `POST` to online API with `applicationName` query param. No operation API calls. Success banner appeared. Toast notification shown.

### TC-DEPLOY-ONLINE-02 — Request body contains both blocks

**Status:** ✅ PASS  

Request body JSON confirmed:
- `applicationSettings`: source app `applicationName`, `enabled`, `asyncMode`, `retry`, `tracking`, `region`, etc.
- `workflow`: `pluginList` array + `uiMapList` array

### TC-DEPLOY-ONLINE-03 — Empty application name blocked

**Status:** ✅ PASS  

Deploy button correctly disabled when input empty. No API call made.

### TC-DEPLOY-ONLINE-04 — Error response surfaced correctly

**Status:** ✅ PASS  

API error message surfaced in red error banner. Toast notification also shown. Deploy button re-enabled after error.

### TC-DEPLOY-ONLINE-05 — Source info summary displayed

**Status:** ✅ PASS  

"Request Body" summary panel shows: application name, Active/Inactive status, plugin count, region.

### TC-DEPLOY-ONLINE-06 — State resets on reopen

**Status:** ✅ PASS  

Success/error banners cleared. Form field reset. Confirmed by deploying twice in sequence.

---

## Regression Testing

| Feature | Result |
|---------|---------|
| Deploy button in application table | ✅ No regression |
| Settings modal | ✅ No regression |
| History drawer | ✅ No regression |
| Copy workflow modal | ✅ No regression |
| Delete application | ✅ No regression |
| Mobile application cards | ✅ No regression |

---

## Issues Found

**None.**

---

## Conclusion

All 6 UAT test cases passed on desktop. The Deploy feature now calls the online API with a two-block request body assembled from the current application's entity settings and workflow data. Users no longer need to enter target environment credentials. The deployment workflow (to be configured separately) receives the full source application context via the online API.

**UAT Verdict:** ✅ PASS — Ready for production deployment.

---

*End of UAT Report v45.0*
