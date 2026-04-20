# Architecture Document — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-20  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Overview

Rewrite `DeployModal.tsx` in `workflow-ui` to replace the three-step operation-API flow with a single `POST /api/workflow` call to the online API. All request body data is assembled from props already passed to the modal — no new API fetches required.

---

## 2. Current Behaviour

**File:** `workflow-ui/src/components/DeployModal.tsx`

Three sequential calls to the **target operation-api**:

```
POST  {baseUrl}/workflow/entity-setting?applicationName={name}   ← Create
PATCH {baseUrl}/workflow/entity-setting?applicationName={name}   ← Update settings
POST  {baseUrl}/workflow?applicationName={name}                  ← Save workflow
```

Problems:
- Hub must know the target operation-api's internal API structure
- Three network round-trips, each a failure point
- Environment coupling: a different target API version can break all three steps

---

## 3. New Behaviour

**Single call to online API:**

```
POST {onlineApiUrl}/api/workflow
  ?applicationName={executionName}
  &confirmationNumber={uuid}
Headers:
  Content-Type: application/json
  X-Request-Correlation-Id: {uuid}
  Authorization: Basic {base64}   ← optional
Body:
  {
    "sourceApplication": { ...WorkflowEntitySettingRow },   ← Block A
    "workflow":           { ...WorkFlow }                   ← Block B
  }
```

- **`applicationName`** query param = user-input execution name (the deploy workflow registered in the online API)
- **`confirmationNumber`** = client-generated UUID (idempotency key)
- **`X-Request-Correlation-Id`** = client-generated UUID (duplicate-run detection per `WorkflowOnlineController`)
- **Block A** (`sourceApplication`) = `currentSettings` prop (full `WorkflowEntitySettingRow`)
- **Block B** (`workflow`) = `currentWorkflow` prop (full `WorkFlow`)

---

## 4. Component Changes

### 4.1 `workflow-ui/src/components/DeployModal.tsx`

**Form state interface:**

```typescript
interface DeployFormData {
  onlineApiUrl: string;     // replaces baseUrl
  applicationName: string;  // execution name in online API
  username: string;         // optional Basic Auth
  password: string;         // optional Basic Auth
}
// removed: environment field
```

**Progress state:**

```typescript
type DeployStatus = "idle" | "in-progress" | "success" | "error";
// replaces DeployProgress[] with 3 steps
```

**`handleDeploy` logic:**

```typescript
const correlationId = generateUUID();
const confirmationNumber = generateUUID();
const params = new URLSearchParams({ applicationName, confirmationNumber });
const headers = {
  "Content-Type": "application/json",
  "X-Request-Correlation-Id": correlationId,
  ...(username && password ? { Authorization: `Basic ${btoa(username+":"+password)}` } : {}),
};
const body = JSON.stringify({ sourceApplication: currentSettings, workflow: currentWorkflow });
await fetch(buildUrl(`/api/workflow?${params}`), { method: "POST", headers, body });
```

**Cross-origin proxy** — `buildUrl()` unchanged: cross-origin URLs are forwarded through `https://workflow-operation-api-n9sbp.ondigitalocean.app/workflow/deploy/proxy?targetUrl=...`.

---

## 5. Online API Contract (Reference)

**Endpoint:** `WorkflowOnlineController.java` — `POST /api/workflow`

| Parameter | Location | Required | Notes |
|-----------|----------|----------|-------|
| `applicationName` | query | ✅ | Must match an existing `WorkflowEntitySetting` in online API |
| `confirmationNumber` | query | ✅ | Stored with run for idempotency |
| `X-Request-Correlation-Id` | header | ✅ | Duplicate-run detection |
| `Content-Type` | header | ✅ | `application/json` |
| Request body | body | — | Passed as `ingressBody` to deploy workflow |

The online API looks up the workflow entity setting by `applicationName`, creates a `WorkflowRuntimePayload` with `ingressBody = { sourceApplication, workflow }`, and dispatches the deploy workflow. The deploy workflow itself is configured by a separate task.

---

## 6. Impact Analysis

### 6.1 Frontend (`workflow-ui`)

| File | Change |
|------|--------|
| `src/components/DeployModal.tsx` | Full rewrite of form fields and deploy logic |

No other files changed. The modal's props interface is unchanged:
```typescript
interface DeployModalProps {
  open: boolean;
  onClose: () => void;
  currentWorkflow: WorkFlow | null;
  currentSettings: WorkflowEntitySettingRow | null;
}
```

### 6.2 Backend

**No changes.** `WorkflowOnlineController` already accepts the required parameters. The deploy workflow to be registered in the online API is out of scope for this ticket.

---

## 7. UUID Generation

Client-side UUID generation uses `crypto.randomUUID()` (Web Crypto API, available in all modern browsers and Vercel edge) with a legacy fallback:

```typescript
const generateUUID = (): string => {
  if (typeof crypto !== "undefined" && typeof crypto.randomUUID === "function") {
    return crypto.randomUUID();
  }
  return "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g, (c) => {
    const r = (Math.random() * 16) | 0;
    return (c === "x" ? r : (r & 0x3) | 0x8).toString(16);
  });
};
```

---

## 8. Security Considerations

- Credentials sent only when both username and password are non-empty
- HTTP warning prompt retained for non-HTTPS URLs
- No credentials stored client-side between sessions
- Cross-origin proxy already in production; no new attack surface

---

## 9. Deployment

1. Merge PR in `workflow-ui` to `main`
2. Vercel auto-deploys to https://workflow-ui-gamma.vercel.app
3. No backend deployment needed

---

*End of Architecture Document v45.0*
