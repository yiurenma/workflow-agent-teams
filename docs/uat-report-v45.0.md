# UAT Report v45.0 — Deploy Rewrite: Online API

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`
**Date:** 2026-04-24
**Verdict:** ✅ UAT PASS

## Acceptance Criteria Verification

| AC | Description | Result |
|----|-------------|--------|
| AC-45.1 | Deploy modal shows "Source Application" info block (read-only) | ✅ PASS |
| AC-45.2 | "Executing Application Name" field pre-filled with source application name | ✅ PASS |
| AC-45.3 | User can override executing application name | ✅ PASS |
| AC-45.4 | Deploy button disabled when executing application name is empty | ✅ PASS |
| AC-45.5 | Click Deploy sends POST to online API with dual-block body | ✅ PASS |
| AC-45.6 | Success response shown in green result block | ✅ PASS |
| AC-45.7 | Error response shown in red result block with message | ✅ PASS |
| AC-45.8 | No Deploy URL / credentials / environment fields in form | ✅ PASS |

## Regression
- Applications table (TC-APP-DESK-01..10): no regression
- Canvas save / run / explain / import: no regression
- Import workflow modal: no regression

## Summary

`DeployModal` rewritten to call the online API directly with a single POST. The old 5-field form (Deploy URL, Application Name, Username, Password, Environment) is replaced by a single **Executing Application Name** field. The dual-block body (`entitySetting` + `workFlow`) is auto-assembled from Hub data already fetched before the modal opens. 7/7 E2E tests pass. 8/8 ACs met.
