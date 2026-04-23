# Product Requirements — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-23  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Background

The existing Deploy feature uses a 3-step sequential API sequence against the operation-api:
1. `POST /workflow/entity-setting?applicationName=...` — CreateApplicationName  
2. `PATCH /workflow/entity-setting?applicationName=...` — UpdateApplicationName  
3. `POST /workflow?applicationName=...` — SaveWorkflow  

This requires the user to enter 5 fields (Deploy URL, Application Name, Username, Password, Environment) and manages Basic Auth manually from the browser.

The new design delegates the deploy logic to a **pre-configured deploy workflow** on the online API. The UI assembles a two-block payload from existing app data and calls `POST /api/workflow?applicationName=<executionName>` once.

---

## 2. User Story

**APP-US-52 (revised)** — Deploy application to online environment

> **As** an integration engineer,  
> **I want** to submit the current application's settings and workflow to the online API with a single click,  
> **So that** the pre-configured deploy workflow handles remote deployment without me calling multiple APIs manually.

---

## 3. Acceptance Criteria

**APP-AC-52-D1** — Deploy button remains in the ApplicationName Actions column (desktop table) and overflow menu (mobile).

**APP-AC-52-D2** — Deploy modal shows a description explaining the online API deployment.

**APP-AC-52-D3** — Form has one field: **Execution Application Name** (pre-filled from current app, user-editable). No Deploy URL, Username, Password, or Environment fields.

**APP-AC-52-D4** — On Deploy click, the system:
- Reads `currentSettings` (Block A — entity settings) and `currentWorkflow` (Block B — workflow data) from the current application context
- Auto-generates a `confirmationNumber` (UUID v4)
- Calls `POST /api/workflow?applicationName=<executionName>&confirmationNumber=<uuid>` on the online API with `Content-Type: application/json`
- Request body: `{ "entitySetting": <currentSettings>, "workflow": <currentWorkflow> }`

**APP-AC-52-D5** — Payload summary panel shows: Block A (source app name), Block B (node count).

**APP-AC-52-D6** — Success: green success banner displayed.

**APP-AC-52-D7** — Failure: error banner with API error message.

**APP-AC-52-M1** — Deploy action available in mobile overflow menu (unchanged).

**APP-AC-52-G1** — Deploy uses existing `currentSettings` and `currentWorkflow` props — no additional API calls from UI.

**APP-AC-52-G2** — Execution Application Name is pre-filled but editable; source application data is always the currently-loaded Hub application.

**APP-AC-52-G3** — `confirmationNumber` is a UUID auto-generated per deploy attempt.

**APP-AC-52-G4** — `X-Request-Correlation-Id` header auto-generated (existing `onlineApi.postWorkflow` behavior).

---

## 4. Out of Scope

- The deploy workflow configuration on the online API (covered by a separate task)
- Changes to the online API backend
- CORS or proxy configuration changes (already handled by existing `/api/proxy/online` proxy)

---

## 5. Success Metrics

- Single API call replaces the 3-step operation-API sequence
- Form reduced from 5 fields to 1 field
- UAT: Deploy button → online API 200 OK → success banner displayed
