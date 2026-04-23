# UAT Report — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-23  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Tester:** E2E Tester (UAT)  
**Environment:** https://workflow-ui-gamma.vercel.app  
**Browser:** Desktop Chrome 1280px + Mobile Chrome 390×844  
**Test Mode:** Headed (visual verification)

---

## UAT Execution Summary

**Total Test Cases:** 6  
**Passed:** 6  
**Failed:** 0  
**Pass Rate:** 100%  
**Execution Time:** 48 seconds

---

## UAT Verdict

✅ **PASS** — All 6 test cases passed. Deploy modal simplified to single online API call with dual-block payload. Legacy 3-step operation API sequence removed. Ready for production.

---

## Test Results

### TC-DEPLOY-ONLINE-01 — Deploy button opens modal with pre-filled application name

**Status:** ✅ PASS  
**Duration:** 5.2s

**UAT Steps:**
1. Navigated to https://workflow-ui-gamma.vercel.app/workflows
2. Clicked Deploy on an application row
3. Verified modal fields

**UAT Result:**
- ✅ Modal opens with Deploy Application title
- ✅ Execution Application Name pre-filled
- ✅ Payload summary shows Block A and Block B
- ✅ No legacy Deploy URL / credentials fields

---

### TC-DEPLOY-ONLINE-02 — Deploy with default application name submits to online API

**Status:** ✅ PASS  
**Duration:** 9.1s

**UAT Result:**
- ✅ Single POST to online API (not 3 operation-API calls)
- ✅ `applicationName` and `confirmationNumber` query params present
- ✅ Body JSON contains `entitySetting` (Block A) and `workflow` (Block B)
- ✅ Success banner displayed

---

### TC-DEPLOY-ONLINE-03 — Deploy with custom execution application name

**Status:** ✅ PASS  
**Duration:** 8.4s

**UAT Result:**
- ✅ Custom name used as `applicationName` in online API request
- ✅ Block A data from source Hub application unchanged

---

### TC-DEPLOY-ONLINE-04 — Empty application name is rejected

**Status:** ✅ PASS  
**Duration:** 3.7s

**UAT Result:**
- ✅ Error toast: "Application name is required"
- ✅ No network request

---

### TC-DEPLOY-ONLINE-05 — API error is displayed

**Status:** ✅ PASS  
**Duration:** 11.2s

**UAT Result:**
- ✅ Error banner with API error message
- ✅ Modal stays open

---

### TC-DEPLOY-ONLINE-06 — Cancel closes modal without calling API

**Status:** ✅ PASS  
**Duration:** 4.1s

**UAT Result:**
- ✅ Modal closed
- ✅ No API call

---

## Mobile UAT

**Device:** Mobile Chrome 390×844  
**Test Cases:** TC-01, TC-04, TC-06 re-run on mobile  
**Result:** ✅ All PASS  
**Notes:** Deploy accessible from mobile overflow menu; modal fields responsive

---

## Issues Found

**None**

---

## Conclusion

All 6 UAT test cases passed. The Deploy modal now:

- Calls the online API with one POST request (replacing the 3-step operation-API sequence)
- Assembles the payload automatically from existing app data (no user credentials needed)
- Pre-fills the execution application name (user can change it for cross-name deploys)
- Shows clean success/error feedback

**UAT Verdict:** ✅ PASS — Ready for production deployment

**Commit:** `workflow-ui@5a7a384`

---

*End of UAT Report v44.0*
