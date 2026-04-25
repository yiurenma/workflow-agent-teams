# UI Test Report — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-25  
**Status:** PASS  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## Summary

| Metric | Value |
|--------|-------|
| Total UI test cases | 6 |
| Pass | 6 |
| Fail | 0 |
| Skip | 0 |

---

## Test Results

| ID | Title | Result | Notes |
|----|-------|--------|-------|
| TC-DEPLOY-V44-01 | Deploy button in Actions column | PASS | Renders correctly |
| TC-DEPLOY-V44-02 | Modal pre-fills applicationName | PASS | Pre-fill from props |
| TC-DEPLOY-V44-03 | Online API called with applicationName | PASS | URL verified |
| TC-DEPLOY-V44-04 | Body has sourceApplicationSetting | PASS | Block A present |
| TC-DEPLOY-V44-05 | Body has workflow | PASS | Block B present |
| TC-DEPLOY-V44-06 | Success state after deploy | PASS | Button disabled |

---

## Key Observations

- **Simplified form:** Old 5-field form replaced with single `Execution Application Name` field
- **No credentials:** Basic Auth fields removed; online API uses same-origin proxy (no CORS)
- **Body structure:** `{ sourceApplicationSetting, workflow }` assembled from props automatically
- **Status feedback:** Green success banner + disabled Deploy button on success; red error banner on failure
- **Props unchanged:** Existing call sites compile without modification

---

## Regression Notes

No regressions observed. The old 3-step deploy logic is completely removed; the rest of the canvas and application list UI is unaffected.
