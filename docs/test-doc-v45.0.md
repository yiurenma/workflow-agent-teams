# Test Doc v45.0 — Deploy Rewrite: Online API

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`
**Date:** 2026-04-24
**Spec file:** `e2e/deploy-v45.spec.ts`

## Test Cases

| ID | Description | Steps | Expected |
|----|-------------|-------|----------|
| TC-DEPLOY-01 | Deploy button visible in table Actions column | Navigate to /workflows/, wait for table | Green "Deploy" button visible in first row |
| TC-DEPLOY-02 | Clicking Deploy opens modal with Source Application block | Click Deploy | Modal visible; "Source Application" label and app name shown |
| TC-DEPLOY-03 | Executing Application Name pre-filled | Open modal | Input not empty; value matches source application name |
| TC-DEPLOY-04 | User can change executing application name | Clear + type new name | Input accepts and retains new value |
| TC-DEPLOY-05 | Empty executing name disables Deploy button | Clear input | "Deploy to Online API" button is disabled |
| TC-DEPLOY-06 | Successful deploy shows success result | Click Deploy to Online API (mocked 200) | Green "Deploy Request Sent" block appears |
| TC-DEPLOY-07 | Cancel button closes modal | Click Cancel | Modal not visible |

## Mock Setup
```typescript
// Operation API (workflow fetch before modal opens) — handled by setupMocks()
// Online API (actual deploy call) — mocked in test beforeEach:
await page.route(
  (url) => url.pathname.startsWith('/api/proxy/online'),
  async (route) => {
    route.fulfill({
      status: 200,
      contentType: 'application/json',
      body: JSON.stringify({ status: 'accepted', correlationId: 'deploy-test-001' }),
    });
  }
);
```

## Regression Checks
- Applications table columns and actions: no regression (TC-APP-DESK-01..10)
- No credentials/URL fields in new modal
- Canvas save, run, explain, import: no regression
