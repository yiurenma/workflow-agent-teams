# E2E Test Report — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-25  
**Status:** PASS  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## Summary

| Metric | Value |
|--------|-------|
| Spec file | `e2e/deploy-v44.spec.ts` |
| Total tests | 6 |
| Pass | 6 |
| Fail | 0 |
| Skip | 0 |
| Duration | ~12s |

---

## Test Results

| ID | Title | Status | Duration |
|----|-------|--------|----------|
| TC-DEPLOY-V44-01 | Deploy button visible in Actions column | PASS | 1.2s |
| TC-DEPLOY-V44-02 | Deploy modal opens with pre-filled applicationName | PASS | 1.8s |
| TC-DEPLOY-V44-03 | Deploy POSTs to online API with applicationName query param | PASS | 2.1s |
| TC-DEPLOY-V44-04 | Request body contains sourceApplicationSetting block | PASS | 2.3s |
| TC-DEPLOY-V44-05 | Request body contains workflow block | PASS | 2.2s |
| TC-DEPLOY-V44-06 | Success indicator shown after deploy completes | PASS | 2.4s |

---

## Mock Setup

- Operation API mocked via `setupMocks(page)` — returns `MOCK_APPS` and `MOCK_WORKFLOW`
- Online API mocked: `POST /api/proxy/online/workflow` → 200 `{ "status": "submitted" }`
- No real backend calls made

---

## Notes

- TC-DEPLOY-V44-03/04/05 capture the intercepted request URL and body to verify the correct API contract
- TC-DEPLOY-V44-06 verifies the submit button is disabled after success (preventing duplicate submits)
- The old `DeployProxyController` proxy endpoint is no longer called by these tests
