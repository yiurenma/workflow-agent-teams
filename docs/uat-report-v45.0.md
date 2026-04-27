# UAT Report v45.0 — Deploy Rewrite: Online API Integration

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Date:** 2026-04-27  
**Status:** ✅ UAT PASS

## Acceptance Criteria Verification

| Criterion | Status |
|---|---|
| Click Deploy → automatically calls Online API | ✓ PASS |
| User-entered execution name = `applicationName` query param | ✓ PASS |
| Body Block A: application settings (`WorkflowEntitySettingRow`) | ✓ PASS |
| Body Block B: workflow JSON (`WorkFlow`) | ✓ PASS |
| No credentials or target URL required from user | ✓ PASS |
| Source application info shown read-only | ✓ PASS |
| Single-step progress indicator | ✓ PASS |
| Cancel disabled during active deploy | ✓ PASS |
| State resets on modal re-open | ✓ PASS |

## Sign-off

UAT PASS 2026-04-27. `DeployModal` rewritten to use Online API. Body assembled from Hub data (`applicationSetting` + `workflow`). Execution name is user-configurable query param, defaults to source app name. Removes previous 3-step direct operation API flow and all credential input fields. Commit: `workflow-ui@f72c4a1`.
