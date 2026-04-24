# PM Doc v45.0 — Deploy Rewrite: Online API, Executing Name, Dual-Block Body

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`
**Status:** ✅ DONE — UAT PASS 2026-04-24
**Repo:** workflow-ui
**Branch:** claude/happy-franklin-uxLAC

## Summary

Rewrote the Deploy feature so the UI calls the **online API** directly instead of the old 3-step flow against a remote operation API.

## User Story

As a workflow engineer, I want to click **Deploy** and have the application automatically sent to the online runtime, so that I don't need to manually configure a Deploy URL, credentials, or environment each time.

## What Changed

### Before (v38 — 3-step flow)
- Form required 5 fields: Deploy URL, Application Name, Username, Password, Environment
- 3 API calls: CreateApplicationName → UpdateApplicationName → SaveWorkflow
- Used Basic Auth against user-specified remote URL
- Required CORS proxy for cross-origin targets

### After (v45 — online API)
- Form requires 1 field: **Executing Application Name** (pre-filled with source application name)
- 1 API call: `POST /api/workflow?applicationName=<executingName>` to online API
- Body auto-assembled from Hub data — no user configuration of credentials or URL needed
- Response shown inline

## Request Body Structure

```json
{
  "entitySetting": { /* Block A: source application entity settings */ },
  "workFlow":      { /* Block B: workflow definition (pluginList, uiMapList) */ }
}
```

- **Block A** — `entitySetting`: full `WorkflowEntitySettingRow` fetched from Hub operation API before opening modal
- **Block B** — `workFlow`: full `WorkFlow` object (pluginList + uiMapList) fetched from Hub operation API

## Acceptance Criteria

- AC-45.1: Deploy modal shows "Source Application" info block (read-only)
- AC-45.2: "Executing Application Name" field pre-filled with source application name
- AC-45.3: User can override executing application name
- AC-45.4: Deploy button disabled when executing application name is empty
- AC-45.5: Click Deploy sends `POST /api/proxy/online/workflow?applicationName=<name>` with dual-block JSON body
- AC-45.6: Success response shown in green result block
- AC-45.7: Error response shown in red result block with message
- AC-45.8: No Deploy URL / credentials / environment fields in the form

## Out of Scope (next task)
The **JSON workflow** that implements the three-step deploy logic inside the online runtime (CreateApplicationName → UpdateApplicationName → SaveWorkflow) is handled by a separate task. This task only changes the UI call pattern.
