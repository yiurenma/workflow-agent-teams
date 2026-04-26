# Architecture Document — Deploy Rewrite: Online API Integration v44.0

**Document Version:** 44.0  
**Date:** 2026-04-26  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Overview

Rewrite `DeployModal.tsx` to replace the three-step direct remote-server calls with a single POST to the online API. The modal assembles a dual-block request body and delegates all deployment orchestration to the online API's deploy workflow.

---

## 2. Current Architecture

**File:** `workflow-ui/src/components/DeployModal.tsx`

**Form fields:** Deploy URL, Application Name, Username, Password, Environment

**Deployment flow:**
1. Browser → `POST {deployUrl}/workflow/entity-setting?applicationName={name}` (CORS proxy if cross-origin)
2. Browser → `PATCH {deployUrl}/workflow/entity-setting?applicationName={name}` (CORS proxy if cross-origin)
3. Browser → `POST {deployUrl}/workflow?applicationName={name}` (CORS proxy if cross-origin)

**Problems:**
- 3 direct HTTP calls from the browser to an external server
- CORS proxy required for cross-origin targets
- Basic Auth credentials stored in browser memory
- No workflow orchestration, retry, or audit

---

## 3. New Architecture

**File:** `workflow-ui/src/components/DeployModal.tsx`

**Form fields:** Execution Application Name (only)

**Deployment flow:**
```
Browser → POST /api/proxy/online/workflow?applicationName={executionName}
  Headers: X-Request-Correlation-Id: {uuid}
  Body: {
    "entitySetting": { ...WorkflowEntitySettingRow },
    "workflow": { ...WorkFlow }
  }
→ Online API executes named deploy workflow with request body as input
→ Deploy workflow reads entitySetting (Block A) and workflow (Block B)
→ Deploy workflow orchestrates CreateApplicationName → UpdateApplicationName → SaveWorkflow on target
```

---

## 4. Component Changes

### 4.1 DeployModal.tsx — Full Rewrite

**Removed state:**
- `form.baseUrl`, `form.username`, `form.password`, `form.environment`
- `progress[]` (three-step progress tracker)

**New state:**
- `applicationName: string` — execution name (pre-filled from `currentSettings.applicationName`)
- `deploying: boolean` — loading state
- `responseText: string | null` — success response display
- `errorMsg: string | null` — error display

**New deployment logic (`handleDeploy`):**
```typescript
const response = await onlineApi.postWorkflow({
  applicationName: applicationName.trim(),
  body: JSON.stringify({
    entitySetting: currentSettings,
    workflow: currentWorkflow,
  }),
  contentType: "application/json",
});
const text = await response.text();
setResponseText(text || "Deployment initiated successfully");
```

**Removed logic:**
- `buildUrl()` — CORS proxy detection no longer needed
- `deployToRemote()` — three-step sequence replaced
- `isCrossOrigin()` — no longer needed
- `STEP_LABELS`, `DeployProgress`, `resetProgress()`, `updateProgress()`, `stepIcon()`

**New import:**
- `onlineApi` from `@/api/services/online`

---

## 5. Request Body Schema

```typescript
interface DeployRequestBody {
  entitySetting: WorkflowEntitySettingRow;  // Block A: source application settings
  workflow: WorkFlow;                        // Block B: workflow definition
}
```

**Block A (`entitySetting`):** Full `WorkflowEntitySettingRow` including applicationName, enabled, asyncMode, retry, tracking, ignoreDuplicateRecordError, region, eimId, defaultServiceAccount, retryProperties, description.

**Block B (`workflow`):** Full `WorkFlow` object including `pluginList` and `uiMapList`.

---

## 6. API Endpoint

**Service:** `onlineApi.postWorkflow()` in `src/api/services/online.ts` — no changes needed

**URL:** `POST /api/proxy/online/workflow?applicationName={executionName}`  
**Headers:** `Content-Type: application/json`, `X-Request-Correlation-Id: {uuid}`  
**Response:** Any JSON response from the online API's deploy workflow execution

---

## 7. Impact Analysis

### 7.1 Frontend (`workflow-ui`)

**Changed:**
- `src/components/DeployModal.tsx` — full rewrite

**Unchanged:**
- `src/routes/workflows/index.tsx` — `handleDeploy` and `DeployModal` usage unchanged (same props interface)
- `src/api/services/online.ts` — no changes
- All other components

### 7.2 Backend

**workflow-online-api:** Receives the new POST request shape. The deploy workflow execution is a separate task.  
**workflow-operation-api:** No changes. CORS proxy endpoint retained for potential future use.

---

## 8. Security Considerations

- Credentials (username/password) are removed from the browser entirely
- Authentication to remote deploy target is handled by the online API's deploy workflow
- No CORS proxy dependency in the happy path

---

## 9. Deployment

1. Merge PR to `workflow-ui` `main` branch
2. Vercel auto-deploys to production
3. No backend deployment needed for this UI change

---

*End of Architecture Document v44.0*
