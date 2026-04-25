# UAT Report — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-25  
**Status:** UAT PASS  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## UAT Summary

**Result: PASS**

All acceptance criteria from `pm-doc-v44.0.md` have been verified.

---

## Acceptance Criteria Verification

| AC | Description | Status |
|----|-------------|--------|
| AC-DEPLOY-44-01 | Simplified form — single executionName field, no credentials | PASS |
| AC-DEPLOY-44-02 | Automatic data assembly from props (A + B) | PASS |
| AC-DEPLOY-44-03 | Online API called with correct applicationName and body | PASS |
| AC-DEPLOY-44-04 | Status feedback (deploying / success / error states) | PASS |
| AC-DEPLOY-44-05 | Backward-compatible props interface | PASS |

---

## Changes Shipped

**workflow-ui:** `src/components/DeployModal.tsx` rewritten
- Removed: 3-step remote deploy, Basic Auth, proxy CORS routing, 5-field form
- Added: single `executionName` field, `onlineApi.postWorkflow()` call, 2-block body

**workflow-ui:** `e2e/deploy-v44.spec.ts` added
- 6 new E2E tests TC-DEPLOY-V44-01 through TC-DEPLOY-V44-06

---

## Commits

- `workflow-ui@4a377cc` — feat: rewrite DeployModal to call online API with 2-block body

---

## Notes

- The deploy workflow executed by the online API is not yet implemented — that is handled by a separate task (`TODO-online-api-post-optional-sse-runtime-per-step` and related)
- The CORS proxy endpoint in `workflow-operation-api` (`DeployProxyController`) is no longer used by the UI for this flow
