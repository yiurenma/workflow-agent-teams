# Test Doc v45.0 — Deploy Rewrite: Online API Integration

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

## Test Cases

| ID | Description | Steps | Expected |
|---|---|---|---|
| TC-DEPLOY-V2-01 | Deploy modal opens with source info | Click Deploy on an application | Modal shows source app name + node count read-only |
| TC-DEPLOY-V2-02 | Execution name pre-filled | Open Deploy modal | Execution name field shows source app name |
| TC-DEPLOY-V2-03 | Execution name editable | Clear and retype execution name | Field accepts new value |
| TC-DEPLOY-V2-04 | Deploy button disabled without name | Clear execution name field | Deploy button is disabled |
| TC-DEPLOY-V2-05 | Successful deploy (mocked) | Enter valid name, click Deploy | Status shows "Deployment triggered successfully", toast shown |
| TC-DEPLOY-V2-06 | API error handled | Mock online API to return 500 | Error status shown with error message text |
| TC-DEPLOY-V2-07 | Request body contains applicationSetting | Intercept online API call | Body JSON has `applicationSetting` matching current entity settings |
| TC-DEPLOY-V2-08 | Request body contains workflow | Intercept online API call | Body JSON has `workflow` matching current workflow |
| TC-DEPLOY-V2-09 | applicationName query param | Intercept online API call | `applicationName` query param matches user-entered execution name |
| TC-DEPLOY-V2-10 | Cancel closes modal | Click Cancel | Modal closes, status resets |
| TC-DEPLOY-V2-11 | Cancel disabled during deploy | Start deploy, check Cancel | Cancel button disabled while deploying |
| TC-DEPLOY-V2-12 | Re-open resets state | Open modal, deploy, close, reopen | Status is idle, execution name reset to source app name |

## Regression Check

- Applications list Deploy button still present and opens modal
- Other modal actions (Settings, History, Copy, Delete) unaffected
- No CORS proxy or Basic Auth logic in new flow
