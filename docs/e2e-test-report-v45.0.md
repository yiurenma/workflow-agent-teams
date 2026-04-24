# E2E Test Report v45.0 — Deploy Rewrite: Online API

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`
**Date:** 2026-04-24
**Spec:** `e2e/deploy-v45.spec.ts`
**Playwright project:** Desktop Chrome (1280px)

## Results

| ID | Test Name | Result |
|----|-----------|--------|
| TC-DEPLOY-01 | Deploy button visible in table Actions column | ✅ PASS |
| TC-DEPLOY-02 | Clicking Deploy opens modal with source application info block | ✅ PASS |
| TC-DEPLOY-03 | Executing Application Name field is pre-filled | ✅ PASS |
| TC-DEPLOY-04 | User can change executing application name | ✅ PASS |
| TC-DEPLOY-05 | Deploy button disabled when executing name is empty | ✅ PASS |
| TC-DEPLOY-06 | Successful deploy shows success result block | ✅ PASS |
| TC-DEPLOY-07 | Cancel button closes modal without deploying | ✅ PASS |

**Total: 7 pass / 0 fail / 0 skip**

## Mock Verification
- Operation API (`/api/proxy/operation/workflow?applicationName=test-app-01`): mocked via `setupMocks`, returns MOCK_WORKFLOW — confirms dual-block body can be assembled
- Online API (`/api/proxy/online/workflow`): mocked to return `{ status: 'accepted', correlationId: 'deploy-test-001' }` — confirms success path rendering
- No real network calls made during test run

## Confirmed Absence of Old Fields
No `[placeholder="https://workflow-operation-api*"]`, `[type="password"]`, or Environment inputs detected in the new modal.
