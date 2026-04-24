# UI Test Report v45.0 — Deploy Rewrite: Online API

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`
**Date:** 2026-04-24
**Tester:** Claude Agent
**Environment:** Playwright + page.route mocks (Desktop Chrome 1280px)

## Test Results

| ID | Description | Status | Notes |
|----|-------------|--------|-------|
| TC-DEPLOY-01 | Deploy button visible in Actions column | ✅ PASS | |
| TC-DEPLOY-02 | Modal opens with Source Application block | ✅ PASS | |
| TC-DEPLOY-03 | Executing Application Name pre-filled | ✅ PASS | Value = "test-app-01" |
| TC-DEPLOY-04 | User can change executing application name | ✅ PASS | |
| TC-DEPLOY-05 | Empty name disables Deploy button | ✅ PASS | button[disabled] confirmed |
| TC-DEPLOY-06 | Successful deploy shows success result | ✅ PASS | Green block, "Deploy Request Sent" text |
| TC-DEPLOY-07 | Cancel closes modal | ✅ PASS | |

**Summary: 7/7 PASS, 0 FAIL, 0 SKIP**

## Visual Verification
- Source Application block: IBM Plex Mono font, #f4f4f4 background, gray uppercase label
- `applicationName` code tag: monospace with light background
- Success result: #defbe6 background, #24a148 border, green uppercase label
- Error result: #fff1f1 background, #da1e28 border, red uppercase label
- Response body in scrollable pre block (max-height 200px)
- No password/username/URL inputs present in form
