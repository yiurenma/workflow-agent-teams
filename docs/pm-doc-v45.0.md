# Product Requirements — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-20  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Background

The existing Deploy feature performs three sequential API calls directly against the target `operation-api` (CreateApplicationName → UpdateApplicationName → SaveWorkflow). This tight coupling means the Hub UI must know and maintain the target environment's API structure.

The new approach delegates deployment to a **deploy workflow** registered in the **online API**. The Hub UI submits a single request containing the source application's full settings and workflow; the online API's deploy workflow handles all provisioning steps on the target environment.

---

## 2. User Story

**APP-US-58** — Deploy via online API workflow

> **As** an integration engineer,  
> **I want** to click Deploy and have the Hub automatically submit the current application and workflow to the online API,  
> **So that** the online API's deploy workflow provisions the target environment without the Hub needing to know target API internals.

---

## 3. Acceptance Criteria

**APP-AC-58-D1** — Deploy button opens DeployModal with updated form:  
- Field 1: **Online API URL** (base URL of the target online API)  
- Field 2: **Application Name** — the execution name registered in the online API for the deploy workflow  
- Fields 3–4: **Username / Password** (optional, for Basic Auth if target requires it)  
- Old fields removed: Deploy URL (operation-api), Environment

**APP-AC-58-D2** — On Deploy click the UI:  
1. Auto-generates `X-Request-Correlation-Id` (UUID) and `confirmationNumber` (UUID)  
2. Sends `POST {onlineApiUrl}/api/workflow?applicationName={name}&confirmationNumber={uuid}` with headers `Content-Type: application/json`, `X-Request-Correlation-Id: {uuid}`, optional `Authorization: Basic {base64}`  
3. Request body has two mandatory blocks: **sourceApplication** (full entity setting of Hub's current app) + **workflow** (full workflow definition)  

**APP-AC-58-D3** — Progress indicator shows single-step status (Submitting → Accepted / Failed) replacing the old 3-step indicator.

**APP-AC-58-D4** — Cross-origin URLs are automatically routed through the operation-api proxy (existing behaviour retained).

**APP-AC-58-D5** — HTTP (non-HTTPS) URL triggers a confirmation warning before sending credentials.

**APP-AC-58-M1** — Deploy action in mobile overflow menu invokes the same updated modal.

**APP-AC-58-G1** — No backend changes required for this ticket; the online API's deploy workflow is set up by a separate task.

---

## 4. Out of Scope

- Setting up the deploy workflow in the online API (separate task)
- Adding `channelKind` / `isSelfRequest` / retry parameters to the form
- Saving the Online API URL or credentials between sessions

---

## 5. Success Metrics

- Single HTTP request replaces three sequential requests for deployment
- UI component change only — no backend changes in this ticket
- Form validates required fields (URL + Application Name) before submission
