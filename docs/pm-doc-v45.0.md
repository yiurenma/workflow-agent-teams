# Product Requirements — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-24  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Background

The previous Deploy modal performed three sequential API calls to a remote operation API (Create ApplicationName → Update ApplicationName → Save Workflow) using user-entered Basic Auth credentials. This approach required the user to know the target environment's credentials, exposed them in the browser, and was fragile across network boundaries.

The new approach leverages the **online API** as the single deployment trigger. The client assembles a request body with two mandatory blocks — source application settings (Block A) and the current workflow JSON (Block B) — and makes one `POST` to the online API. The online API routes the request through a pre-configured deployment workflow that handles all target environment interactions server-side.

---

## 2. User Story

**US-DEPLOY-45-01** — Deploy via online API

> **As** an integration engineer,  
> **I want** to deploy my application by calling the online API with my source application data,  
> **So that** the deployment workflow handles the target environment without me entering credentials.

---

## 3. Acceptance Criteria

**AC-DEPLOY-45-01** — Simplified form

- Deploy modal shows a single input: **Execution Application Name**
- No Deploy URL, Username, Password, or Environment fields
- Source application info summary panel displayed (Block A + Block B)

**AC-DEPLOY-45-02** — Correct API call

- On Deploy click, client calls `POST /api/proxy/online/workflow?applicationName={executionAppName}`
- Request body: `{ "applicationSettings": { ...currentSettings }, "workflow": { ...currentWorkflow } }`
- Content-Type: `application/json`

**AC-DEPLOY-45-03** — Feedback states

- Success: green success banner; toast notification
- Error: red error banner with message; toast notification
- Deploy button disabled while request in flight
- Deploy button disabled when application name input is empty

**AC-DEPLOY-45-04** — State reset on open

- Each time modal opens, form resets to source application name
- Success/error banners cleared

---

## 4. Out of Scope

- Changes to online API or operation API (backend unchanged)
- The actual deploy workflow definition (handled by a separate task)
- CORS proxy logic (online API called via same-origin Vite proxy — no browser CORS issue)

---

## 5. Success Metrics

- Deploy modal completes in a single API call
- No user credentials entered
- Success/error clearly communicated via banner and toast

---

*End of Product Requirements v45.0*
