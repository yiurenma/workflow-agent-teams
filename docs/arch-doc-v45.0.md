# Architecture Document — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-24  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Overview

Rewrite `DeployModal.tsx` to replace the 3-step operation API sequence with a single `POST` to the online API. The UI assembles a two-block request body (application settings + workflow JSON) and delegates all deployment logic to the online API's configured workflow.

---

## 2. Before vs. After

| Aspect | Before | After |
|--------|--------|-------|
| Form fields | 5 (Deploy URL, App Name, Username, Password, Environment) | 1 (Execution Application Name) |
| API calls | 3 sequential (Create → Update → Save) | 1 (online API POST) |
| Auth | Basic Auth (user-entered credentials) | None (uses app proxy) |
| Progress UI | 3-step progress indicator | Success/error banner |
| CORS handling | Proxy detection + `isCrossOrigin()` helper | Removed (not needed) |

---

## 3. Implementation

### 3.1 Component: `src/components/DeployModal.tsx`

Complete rewrite. Key change:

```typescript
await onlineApi.postWorkflow({
  applicationName: executionAppName.trim(),
  body: JSON.stringify({
    applicationSettings: currentSettings,  // Block A
    workflow: currentWorkflow,             // Block B
  }),
  contentType: "application/json",
});
```

### 3.2 Request Body Structure

```json
{
  "applicationSettings": {
    "applicationName": "string",
    "enabled": true,
    "asyncMode": false,
    "retry": false,
    "tracking": false,
    "ignoreDuplicateRecordError": false,
    "region": "string",
    "eimId": "string",
    "defaultServiceAccount": "string",
    "retryProperties": "string",
    "description": "string"
  },
  "workflow": {
    "pluginList": [ /* BackendPlugin[] */ ],
    "uiMapList": [ /* edge topology */ ]
  }
}
```

**Block A** (`applicationSettings`) = `WorkflowEntitySettingRow` from the Hub operation API.  
**Block B** (`workflow`) = `WorkFlow` currently loaded on the canvas.

### 3.3 State Management

```typescript
const [executionAppName, setExecutionAppName] = useState("");
const [deploying, setDeploying] = useState(false);
const [deployStatus, setDeployStatus] = useState<"idle" | "success" | "error">("idle");
const [deployError, setDeployError] = useState<string | null>(null);

useEffect(() => {
  if (open) {
    setExecutionAppName(currentSettings?.applicationName ?? "");
    setDeployStatus("idle");
    setDeployError(null);
  }
}, [open, currentSettings?.applicationName]);
```

### 3.4 Props Interface (Unchanged)

```typescript
interface DeployModalProps {
  open: boolean;
  onClose: () => void;
  currentWorkflow: WorkFlow | null;
  currentSettings: WorkflowEntitySettingRow | null;
}
```

No changes required in `src/routes/workflows/index.tsx`.

---

## 4. Removed Code

- `DeployFormData` interface (5-field form)
- `DeployProgress` interface + `STEP_LABELS` constant
- `resetProgress()`, `updateProgress()` helpers
- `isCrossOrigin()`, `buildUrl()` CORS proxy helpers
- `deployToRemote()` 3-step function
- 3-step progress tracker UI block

---

## 5. API Service Used

Existing `onlineApi.postWorkflow` in `src/api/services/online.ts` — **no changes required**.

---

## 6. Impact Analysis

| Layer | Changed |
|-------|---------|
| `src/components/DeployModal.tsx` | Full rewrite |
| `src/api/services/online.ts` | No change |
| `src/routes/workflows/index.tsx` | No change |
| Backend (operation-api, online-api) | No change |

---

## 7. Deployment

1. Merge PR `workflow-ui` branch `claude/happy-franklin-3xotl` → `main`
2. Vercel auto-deploys frontend
3. No backend deployment required

---

*End of Architecture Document v45.0*
