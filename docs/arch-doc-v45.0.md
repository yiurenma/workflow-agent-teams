# Arch Doc v45.0 — Deploy Rewrite: Online API Integration

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

## Changed Files

### `src/components/DeployModal.tsx` (workflow-ui)

**Removed:**
- Direct 3-step calls to target operation API (CreateApplicationName → UpdateApplicationName → SaveWorkflow)
- Form fields: Deploy URL, Username, Password, Environment
- `isCrossOrigin` / `buildUrl` proxy helpers
- `DeployProgress` type and 3-step progress state

**Added:**
- `useEffect` to reset state and sync execution name when modal opens
- Single `Execution Application Name` input (query param, pre-filled from `currentSettings.applicationName`)
- Read-only Source panel (shows `applicationName` + node count)
- `onlineApi.postWorkflow()` call with assembled two-block body
- Single-step status indicator (`idle` → `in-progress` → `success` | `error`)

**Imports changed:**
- Removed: none new
- Added: `import { onlineApi } from "@/api/services/online"`
- Added: `import { useEffect } from "react"`

## Request Structure

```
POST {ONLINE_API_BASE}/workflow?applicationName={executionName}
Content-Type: application/json
X-Request-Correlation-Id: <uuid>

{
  "applicationSetting": { ...WorkflowEntitySettingRow },
  "workflow": { ...WorkFlow }
}
```

## Data Flow

1. User clicks **Deploy** in Applications list → `handleDeploy()` fetches current workflow from operation API
2. `DeployModal` receives `currentSettings: WorkflowEntitySettingRow` + `currentWorkflow: WorkFlow` as props
3. User optionally edits execution application name (defaults to source app name)
4. On Deploy click: assembles body `{ applicationSetting, workflow }` → calls `onlineApi.postWorkflow()`
5. Shows `in-progress` → `success` or `error` status with message

## No Backend Changes Required (this iteration)

The Online API already exposes `POST /workflow?applicationName=`. The two-block body format is new and will be consumed by the deploy workflow implemented in the online-api task.

## No API Service Changes

Existing `onlineApi.postWorkflow(req: OnlineWorkflowRequest)` is used as-is. The `body` field accepts the JSON-stringified two-block payload.
