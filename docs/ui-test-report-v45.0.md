# UI Test Report v45.0 — Deploy Rewrite: Online API Integration

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`  
**Branch:** `claude/happy-franklin-a4uhc` (workflow-ui)  
**Date:** 2026-04-27

## Results

| TC | Description | Result | Notes |
|---|---|---|---|
| TC-DEPLOY-V2-01 | Source info shown read-only | PASS | App name + node count rendered |
| TC-DEPLOY-V2-02 | Execution name pre-filled | PASS | `useEffect` syncs on open |
| TC-DEPLOY-V2-03 | Execution name editable | PASS | Input enabled when not deploying |
| TC-DEPLOY-V2-04 | Deploy disabled without name | PASS | `disabled={!executionName.trim()}` |
| TC-DEPLOY-V2-05 | Successful deploy | PASS | Status → success, toast shown |
| TC-DEPLOY-V2-06 | API error handled | PASS | Error message displayed |
| TC-DEPLOY-V2-07 | Body contains applicationSetting | PASS | Code review confirmed |
| TC-DEPLOY-V2-08 | Body contains workflow | PASS | Code review confirmed |
| TC-DEPLOY-V2-09 | Query param matches input | PASS | `onlineApi.postWorkflow({ applicationName })` |
| TC-DEPLOY-V2-10 | Cancel closes modal | PASS | `handleClose()` guarded by `!deploying` |
| TC-DEPLOY-V2-11 | Cancel disabled during deploy | PASS | `disabled={deploying}` on Cancel |
| TC-DEPLOY-V2-12 | Re-open resets state | PASS | `useEffect` on `open` dependency |

**12 / 12 PASS**

## Key Changes Verified
- Old form fields (Deploy URL, Username, Password, Environment) removed — confirmed absent
- Three-step progress removed — replaced with single status block
- `onlineApi.postWorkflow` called with `applicationName` + JSON body — confirmed in code
- `isCrossOrigin` / `buildUrl` CORS proxy helpers removed — confirmed
