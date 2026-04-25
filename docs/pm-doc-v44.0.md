# Product Requirements — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-25  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Background

The existing Deploy modal performs a 3-step sequence against a **remote** operation-API URL (CreateApplicationName → UpdateApplicationName → SaveWorkflow) using Basic Auth credentials supplied by the user. This approach has several limitations:

- Users must know and enter the remote API URL and credentials
- CORS issues require a proxy workaround
- The flow is not integrated with the Hub's own online API
- There is no way to trigger an execution-level deploy workflow

The new design replaces the remote 3-step flow with a single POST to the **Hub's own online API**, passing the source application settings (block A) and full workflow JSON (block B) as the request body.

---

## 2. User Story

**US-DEPLOY-44-01** — Deploy via online API

> **As** an integration engineer,  
> **I want** to deploy my application by clicking Deploy and entering only the execution name,  
> **So that** the Hub automatically gathers my app settings and workflow and submits them to the online API without me needing to provide remote API credentials.

---

## 3. Acceptance Criteria

**AC-DEPLOY-44-01** — Simplified form

- Deploy modal shows only one user-editable field: **Execution Application Name**
- Field is pre-filled with the current application's `applicationName`
- No baseUrl, username, password, or environment fields

**AC-DEPLOY-44-02** — Automatic data assembly

- On click Deploy, the UI reads `currentSettings` (block A: `WorkflowEntitySettingRow`) from props
- On click Deploy, the UI reads `currentWorkflow` (block B: `WorkFlow`) from props
- No manual user input is required for the request body

**AC-DEPLOY-44-03** — Online API call

- The UI POSTs to the Hub's online API (`/api/proxy/online/workflow`)
- Query parameter: `applicationName` = the user-entered execution name
- Request body: `{ "sourceApplicationSetting": A, "workflow": B }`
- Content-Type: `application/json`

**AC-DEPLOY-44-04** — Status feedback

- While deploying: button shows "Deploying…" and is disabled
- On success: green success banner shown; Deploy button disabled
- On error: red error banner shows the error message; user can retry

**AC-DEPLOY-44-05** — Backward compatibility

- The `DeployModal` component retains the same props interface (`open`, `onClose`, `currentWorkflow`, `currentSettings`)
- All existing call sites continue to work without modification

---

## 4. Out of Scope

- The deploy workflow itself (what the online API executes) — handled by a separate task
- Backend changes to the online API — this task is UI-only
- Credential management (no longer needed for this flow)

---

## 5. Success Metrics

- Deploy modal renders with a single input field
- Clicking Deploy sends a POST to `/api/proxy/online/workflow` with the correct body structure
- E2E tests TC-DEPLOY-V44-01 through TC-DEPLOY-V44-06 all pass
