# UAT Report v45.0 — Deploy Rewrite: Online API with Two-Block Body

## Feature
TODO-deploy-rewrite-online-api-workflow-json

## UAT Status: PASS ✅

**Date:** 2026-04-20  
**Branch:** `workflow-ui@claude/happy-franklin-S3CT9` (commit `4bb7b1af`)

## Test Execution Summary
| TC | Description | Result |
|---|---|---|
| TC-DEPLOY-NEW-01 | Happy path: deploy request accepted | PASS |
| TC-DEPLOY-NEW-02 | Missing execution application name | PASS |
| TC-DEPLOY-NEW-03 | Missing confirmation number | PASS |
| TC-DEPLOY-NEW-04 | Online API error response | PASS |
| TC-DEPLOY-NEW-05 | Optional channelKind included | PASS |
| TC-DEPLOY-NEW-06 | Empty channelKind omitted | PASS |
| TC-DEPLOY-NEW-07 | Source application info displayed read-only | PASS |
| TC-DEPLOY-NEW-08 | Modal cannot be closed while deploying | PASS |
| TC-DEPLOY-NEW-09 | Confirmation number pre-filled with timestamp | PASS |
| TC-DEPLOY-NEW-10 | Body contains both blocks | PASS |

**10/10 PASS**

## Key Observations
- Old 3-step progress UI (CreateApplicationName / UpdateApplicationName / SaveWorkflow) completely removed.
- Old fields (Deploy URL, username, password, environment) removed — no regression risk for existing users.
- New modal is simpler: 3 fields total (1 required, 1 pre-filled, 1 optional).
- Source application info section confirms user is deploying the correct app before sending.
- Body structure `{ applicationInfo, workflow }` confirmed via network inspection.
- No CORS proxy required; request goes through `ONLINE_API_BASE` proxy.

## Regressions Checked
- Applications list Deploy button still opens modal correctly.
- Modal close / cancel / overlay-click still work when not deploying.
- Toast notifications fire correctly on success and error.
