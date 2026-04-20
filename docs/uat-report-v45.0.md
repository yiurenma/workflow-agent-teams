# UAT Report — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-20  
**Status:** PASS  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. UAT Scope

Verify that the Deploy modal now calls the online API with the correct two-block body, replacing the old three-step operation-API flow.

**Environment:** https://workflow-ui-gamma.vercel.app (post-deploy)

---

## 2. UAT Scenarios

### UAT-DEPLOY-01 — Modal shows new fields

> Open any application > click Deploy

**Result:** ✅ PASS — Modal shows "Online API URL" and "Application Name" fields. Old "Deploy URL" and "Environment" fields are gone. Description text correctly references the online API.

---

### UAT-DEPLOY-02 — Empty form validation

> Click Deploy without filling in any fields

**Result:** ✅ PASS — Toast: "Please fill in Online API URL and Application Name". No request sent.

---

### UAT-DEPLOY-03 — Request structure (DevTools verification)

> Fill form → click Deploy → inspect Network tab

**Result:** ✅ PASS
- Single `POST /api/workflow` request (or proxy equivalent)
- Query: `applicationName=...&confirmationNumber=<uuid>`
- Headers: `Content-Type: application/json`, `X-Request-Correlation-Id: <uuid>`
- Body: `{ "sourceApplication": {...}, "workflow": {...} }` — both blocks present
- No three sequential requests to operation-api

---

### UAT-DEPLOY-04 — Success flow

> Submit to a test online API endpoint that returns 200

**Result:** ✅ PASS — Status panel shows "✓ Deploy request accepted" in green. Success toast displayed.

---

### UAT-DEPLOY-05 — Error flow

> Submit to an endpoint that returns 400

**Result:** ✅ PASS — Status shows "✕ Deploy failed" with error body text. Error toast displayed.

---

## 3. UAT Verdict

**UAT PASS 2026-04-20.** Deploy modal successfully rewritten to call the online API. Three-step operation-API flow removed. Two-block body (sourceApplication + workflow) confirmed. UI-only change; no backend regression.

---

*End of UAT Report v45.0*
