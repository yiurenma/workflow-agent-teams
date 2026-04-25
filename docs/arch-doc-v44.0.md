# Architecture — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-25  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Overview

This document describes the architecture change for the Deploy modal rewrite. The change is **UI-only** (no backend modifications). The operation-API proxy and online-API proxy remain unchanged.

---

## 2. Component Changes

### 2.1 `src/components/DeployModal.tsx` (rewritten)

**Before:**
- Form fields: `baseUrl`, `applicationName`, `username`, `password`, `environment`
- Logic: 3-step fetch sequence to a remote URL via `DeployProxyController`
- Used `btoa` for Basic Auth header construction

**After:**
- Form fields: `executionName` only (single field)
- Logic: single `onlineApi.postWorkflow()` call
- No credentials required

#### Request Flow

```
User clicks Deploy
  → validate executionName (non-empty)
  → validate currentSettings + currentWorkflow non-null
  → build body = JSON.stringify({
        sourceApplicationSetting: currentSettings,   // Block A
        workflow: currentWorkflow,                    // Block B
    })
  → onlineApi.postWorkflow({
        applicationName: executionName,
        body,
        contentType: 'application/json',
    })
  → POST /api/proxy/online/workflow?applicationName=<executionName>
    Content-Type: application/json
    X-Request-Correlation-Id: <uuid>
    Body: { sourceApplicationSetting: {...}, workflow: {...} }
  → success → show success banner, disable Deploy button
  → error   → show error banner with message
```

---

## 3. API Contract

### Request

```
POST /api/proxy/online/workflow?applicationName={executionName}
Content-Type: application/json
X-Request-Correlation-Id: {uuid}

{
  "sourceApplicationSetting": {
    "id": ...,
    "applicationName": "source-app",
    "enabled": true,
    "asyncMode": false,
    "retry": false,
    "tracking": true,
    "ignoreDuplicateRecordError": false,
    "region": "SG",
    "eimId": "...",
    "defaultServiceAccount": "...",
    "description": "..."
  },
  "workflow": {
    "pluginList": [...],
    "uiMapList": [...]
  }
}
```

### Response

The online API response is passed through unchanged. The UI only checks `response.ok`.

---

## 4. Data Sources

| Block | Source | Type |
|-------|--------|------|
| A — sourceApplicationSetting | `currentSettings` prop (from parent state) | `WorkflowEntitySettingRow` |
| B — workflow | `currentWorkflow` prop (from parent state) | `WorkFlow` |

Both props are already loaded by the parent (workflow canvas page) before the Deploy modal is opened. No additional API calls are needed to assemble the body.

---

## 5. Props Interface (unchanged)

```typescript
interface DeployModalProps {
  open: boolean;
  onClose: () => void;
  currentWorkflow: WorkFlow | null;
  currentSettings: WorkflowEntitySettingRow | null;
}
```

All existing call sites remain compatible.

---

## 6. Dependencies

- `src/api/services/online.ts` — `onlineApi.postWorkflow()` (already exists, no change)
- `src/contexts/ToastContext` — `useToast()` (already exists, no change)
- `src/api/types` — `WorkFlow`, `WorkflowEntitySettingRow` (already exists, no change)

---

## 7. Removed Code

- `isCrossOrigin()` helper — no longer needed
- `buildUrl()` helper with proxy routing — no longer needed
- `deployToRemote()` 3-step function — replaced by single `onlineApi.postWorkflow()` call
- `DeployProgress` interface and step tracker — replaced by simple status state
- Basic Auth (`btoa`) — removed entirely
- Form fields: `baseUrl`, `username`, `password`, `environment` — removed
