# Arch Doc v45.0 — Deploy Rewrite: Online API

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`
**Date:** 2026-04-24

## Component Change: `src/components/DeployModal.tsx`

### Props (unchanged)
```typescript
interface DeployModalProps {
  open: boolean;
  onClose: () => void;
  currentWorkflow: WorkFlow | null;
  currentSettings: WorkflowEntitySettingRow | null;
}
```

### State (simplified)
```typescript
const [executingAppName, setExecutingAppName] = useState<string>("");
const [deploying, setDeploying] = useState(false);
const [result, setResult] = useState<{ ok: boolean; text: string } | null>(null);
```

Removed: `form` (5-field object), `progress` (3-step array), `STEP_LABELS`.

### API Call

```typescript
const body = JSON.stringify({
  entitySetting: currentSettings,  // Block A
  workFlow: currentWorkflow,        // Block B
});
const res = await onlineApi.postWorkflow({
  applicationName: executingAppName.trim(),
  body,
  contentType: "application/json",
});
```

Uses existing `onlineApi.postWorkflow` from `src/api/services/online.ts`.  
Endpoint: `POST /api/proxy/online/workflow?applicationName=<name>`

### Removed Code
- `buildUrl()` — CORS proxy routing for remote hub URLs
- `deployToRemote()` — 3-step sequential API call sequence
- `STEP_LABELS`, `DeployProgress`, progress state and rendering
- `isCrossOrigin()` helper
- `DeployFormData` interface (5 fields)

### Caller: `src/routes/workflows/index.tsx` (no change)
The `handleDeploy` function already fetches the workflow before opening the modal:
```typescript
const handleDeploy = async (record: WorkflowEntitySettingRow) => {
  const workflow = await operationApi.getWorkflow(record.applicationName);
  setDeployTarget({ settings: record, workflow });
};
```
The fetched `workflow` and `record` become `currentWorkflow` and `currentSettings` props — Block B and Block A of the request body.

## Data Flow

```
User clicks Deploy (table row)
  → handleDeploy: GET /api/proxy/operation/workflow?applicationName=<source>
  → DeployModal opens (currentSettings = entity settings, currentWorkflow = workflow)
  → User reviews/edits executingAppName (defaults to source app name)
  → User clicks "Deploy to Online API"
  → POST /api/proxy/online/workflow?applicationName=<executingName>
      Content-Type: application/json
      body: { entitySetting: <WorkflowEntitySettingRow>, workFlow: <WorkFlow> }
  → Response text shown inline (green on 2xx, red on error)
```

## No Backend Changes Required
The `onlineApi.postWorkflow` service function already handles:
- URL construction via `ONLINE_API_BASE`
- `X-Request-Correlation-Id` header
- Custom `Content-Type` override
- Error handling via `handleApiError`
