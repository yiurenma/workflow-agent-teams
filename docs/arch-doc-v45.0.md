# Architecture Doc v45.0 — Deploy Rewrite: Online API with Two-Block Body

## Change Summary
Replaced the 3-step operation-API deploy sequence in `DeployModal.tsx` with a single online-API call.

## Affected Files
| Repo | File | Change |
|---|---|---|
| `workflow-ui` | `src/components/DeployModal.tsx` | Full rewrite |

## Data Flow (New)
```
User clicks Deploy
  → DeployModal reads currentSettings + currentWorkflow (already fetched by parent via operationApi.getWorkflow)
  → Builds body: { applicationInfo: currentSettings, workflow: currentWorkflow }
  → Calls onlineApi.postWorkflow({ applicationName: executionAppName, confirmationNumber, channelKind?, body })
  → Online API dispatches to the configured deploy-workflow
  → Modal shows success / error inline
```

## Data Flow (Old — removed)
```
Step 1: POST /workflow/entity-setting?applicationName=... (create)
Step 2: PATCH /workflow/entity-setting?applicationName=... (update settings)
Step 3: POST /workflow?applicationName=... (save workflow)
(All calls to user-supplied Deploy URL via CORS proxy)
```

## Request Body Shape
```json
{
  "applicationInfo": { ...WorkflowEntitySettingRow },
  "workflow": { ...WorkFlow }
}
```
Passed as `body: string` to `onlineApi.postWorkflow()`, which sets `Content-Type: application/json`.

## API Integration
- Uses existing `onlineApi.postWorkflow()` from `src/api/services/online.ts`.
- Routes through `ONLINE_API_BASE` (`/api/proxy/online` in dev; `VITE_ONLINE_API_BASE` env var in production).
- No additional CORS proxy required.

## Props Interface (unchanged)
```ts
interface DeployModalProps {
  open: boolean;
  onClose: () => void;
  currentWorkflow: WorkFlow | null;
  currentSettings: WorkflowEntitySettingRow | null;
}
```
The parent (`workflows/index.tsx`) already fetches both via `operationApi.getWorkflow()` before opening the modal — no parent changes needed.

## Removed Dependencies
- Inline Basic Auth construction (`btoa`)
- CORS proxy detection logic (`isCrossOrigin` / `buildUrl`)
- 3-step progress state machine
