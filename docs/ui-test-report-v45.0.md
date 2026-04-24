# UI Test Report — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-24  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## Summary

| Test Case | Description | Result |
|-----------|-------------|--------|
| TC-DEPLOY-ONLINE-01 | Deploy triggers online API | ✅ PASS |
| TC-DEPLOY-ONLINE-02 | Request body contains both blocks | ✅ PASS |
| TC-DEPLOY-ONLINE-03 | Empty name blocked | ✅ PASS |
| TC-DEPLOY-ONLINE-04 | Error response surfaced | ✅ PASS |
| TC-DEPLOY-ONLINE-05 | Source info summary displayed | ✅ PASS |
| TC-DEPLOY-ONLINE-06 | State resets on reopen | ✅ PASS |

**Pass Rate:** 6/6 (100%)

---

## Findings

### Form Simplification Verified

Old form: 5 fields (Deploy URL, Application Name, Username, Password, Environment).  
New form: 1 field (Execution Application Name). Verified that no credential fields are rendered.

### API Call Verified

Network tab inspection confirmed:
- Request: `POST /api/proxy/online/workflow?applicationName=<value>`
- Content-Type: `application/json`
- Body: `{ "applicationSettings": { ... }, "workflow": { ... } }`
- Zero calls to `/workflow/entity-setting` (operation API) — old 3-step sequence removed

### Source Info Summary

"Request Body" panel correctly shows:
- Block A — Application name + Active/Inactive status
- Block B — Node (plugin) count
- Region field

### State Reset

`useEffect` on `[open, currentSettings?.applicationName]` correctly resets:
- `executionAppName` to source application name
- `deployStatus` to `"idle"`
- `deployError` to `null`

### Regression

- Deploy button in application table row still visible and functional
- Modal opens/closes correctly (✕, backdrop)
- Toast notifications fire on success and error
- No regressions in other application list actions (Settings, History, Copy, Delete)

---

*End of UI Test Report v45.0*
