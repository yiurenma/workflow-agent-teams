# Product Requirements — Deploy Rewrite: Online API Integration v44.0

**Document Version:** 44.0  
**Date:** 2026-04-26  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Background

The current Deploy feature makes three direct API calls from the browser to a remote operation-api instance (CreateApplicationName → UpdateApplicationName → SaveWorkflow) using Basic Authentication and a CORS proxy. This approach has several limitations:

- Requires users to enter and manage remote server credentials in the browser
- CORS proxy adds latency and a dependency on the proxy server
- No audit trail of deployments
- Cannot leverage the workflow engine's built-in orchestration, retry, or tracking capabilities

The new design routes all deployments through the online API, which executes a pre-configured deploy workflow. This decouples the UI from direct remote-server knowledge and enables the deploy workflow to evolve independently.

---

## 2. User Story

**US-DEPLOY-44-01** — Deploy via online API

> **As** a workflow developer,  
> **I want** to click Deploy and have the system automatically call the online API,  
> **So that** my application and workflow are deployed without me having to manage remote credentials or CORS issues.

---

## 3. Acceptance Criteria

**AC-DEPLOY-44-01** — Online API call on deploy

- Clicking Deploy triggers a POST to the online API at `/workflow?applicationName={executionName}`
- No direct calls to remote operation-api from the browser

**AC-DEPLOY-44-02** — Dual-block request body

- Request body contains **Block A**: source application entity settings (`entitySetting` field)
- Request body contains **Block B**: current workflow definition (`workflow` field)
- Both blocks are required; deploy is disabled if either is null

**AC-DEPLOY-44-03** — Execution application name

- User enters the execution application name (the deploy workflow name on the online API)
- This name is passed as `applicationName` query parameter to the online API
- Field is pre-filled with the current application's name as a default

**AC-DEPLOY-44-04** — Form simplification

- Deploy URL, Username, Password, and Environment fields are removed
- Only Execution Application Name is required

**AC-DEPLOY-44-05** — Response display

- On success: response body is displayed in a green panel
- On error: error message is displayed in a red panel

---

## 4. Out of Scope

- The actual deploy workflow (CreateApplicationName → UpdateApplicationName → SaveWorkflow) that runs on the online API — handled by a separate task
- SSE/streaming response from the online API — handled by `TODO-online-api-post-optional-sse-runtime-per-step`
- Authentication changes to the online API

---

## 5. Success Metrics

- Deploy button triggers POST to `/workflow?applicationName={name}` on the online API
- Request body contains both `entitySetting` and `workflow` fields
- No regressions in other Deploy modal interactions (open, close, cancel)

---

*End of Product Requirements v44.0*
