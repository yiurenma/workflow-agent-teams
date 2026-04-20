# UI Test Report — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-20  
**Status:** PASS  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Summary

| Test Case | Result |
|-----------|--------|
| TC-DEPLOY-ONLINE-01 — Form fields | ✅ PASS |
| TC-DEPLOY-ONLINE-02 — Validation | ✅ PASS |
| TC-DEPLOY-ONLINE-03 — HTTP warning | ✅ PASS |
| TC-DEPLOY-ONLINE-04 — Success status | ✅ PASS |
| TC-DEPLOY-ONLINE-05 — Error status | ✅ PASS |
| TC-DEPLOY-ONLINE-06 — Request body shape | ✅ PASS |
| TC-DEPLOY-ONLINE-07 — Basic Auth header | ✅ PASS |
| TC-DEPLOY-ONLINE-08 — No auth header | ✅ PASS |

**Total: 8/8 PASS**

---

## 2. Code Review Notes

- `generateUUID()` uses `crypto.randomUUID()` with legacy fallback — correct
- `buildUrl()` proxy logic unchanged from previous implementation — no regression
- `DeployFormData.environment` removed; no other component references this field
- Props interface (`DeployModalProps`) unchanged — callers in `workflows/index.tsx` require no update
- `deploying` guard on Cancel button and modal backdrop click — prevents premature close
- Status resets to `idle` on modal close — no stale state on reopen

---

## 3. Regression Check

- Modal open/close from desktop table Actions column: ✅ unaffected
- Modal open/close from mobile overflow menu: ✅ unaffected (mobile overflow menu not yet wired for Deploy — pre-existing gap, out of scope)
- `currentWorkflow` / `currentSettings` null guard: ✅ present, shows error toast

---

*End of UI Test Report v45.0*
