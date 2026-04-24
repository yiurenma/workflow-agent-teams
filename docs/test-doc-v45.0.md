# Test Document — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-24  
**Status:** Complete  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Test Scope

Validate the rewritten `DeployModal` component:
- Single-field form (Execution Application Name)
- Single online API call with two-block request body
- Success and error feedback states
- State reset on modal open
- Deploy button disabled guard (empty name)

---

## 2. Test Cases

### TC-DEPLOY-ONLINE-01 — Deploy triggers online API

**Preconditions:** Application loaded with workflow data  
**Steps:**
1. Click Deploy on an application row
2. Verify modal opens with Execution Application Name pre-filled
3. Click Deploy button

**Expected:**
- Online API called: `POST /api/proxy/online/workflow?applicationName={appName}`
- Body contains `applicationSettings` and `workflow` keys
- Success banner: "Deployment triggered successfully"
- Toast notification shown
- No operation API calls (CreateApplicationName / UpdateApplicationName / SaveWorkflow)

---

### TC-DEPLOY-ONLINE-02 — Request body contains both blocks

**Preconditions:** Application with settings and workflow loaded  
**Steps:**
1. Open Deploy modal
2. Click Deploy
3. Inspect network request body

**Expected:**
- `applicationSettings`: contains `applicationName`, `enabled`, `region`, and other entity setting fields
- `workflow`: contains `pluginList` (array of nodes) and `uiMapList` (edges)

---

### TC-DEPLOY-ONLINE-03 — Empty application name blocked

**Steps:**
1. Open Deploy modal
2. Clear the application name input
3. Attempt to click Deploy

**Expected:**
- Deploy button is disabled when input is empty
- If triggered programmatically: toast error "Please enter the execution application name"
- No API call made

---

### TC-DEPLOY-ONLINE-04 — Error response surfaced correctly

**Steps:**
1. Open Deploy modal
2. Enter application name
3. Click Deploy (simulate API error response)

**Expected:**
- Red error banner appears with the error message text
- Deploy button re-enabled after request completes
- Toast error notification shown

---

### TC-DEPLOY-ONLINE-05 — Source info summary displayed

**Steps:**
1. Open Deploy modal

**Expected:**
- "Request Body" summary panel visible
- Block A — Application: shows source application name
- Status: shows Active/Inactive
- Block B — Nodes: shows plugin count
- Region: shows region value

---

### TC-DEPLOY-ONLINE-06 — State resets on modal reopen

**Steps:**
1. Deploy successfully (success banner shown)
2. Close modal
3. Open modal again for same application

**Expected:**
- Success banner cleared
- Application name field reset to source application name
- No residual error state

---

## 3. Regression Tests

| Test | Expected |
|------|---------|
| Deploy button visible in desktop application table Actions column | ✅ |
| Deploy modal opens when Deploy clicked | ✅ |
| Modal closes via ✕ button | ✅ |
| Modal closes via backdrop click | ✅ |
| Modal cannot be closed during in-flight request | ✅ |
| Toast notifications display for success and error | ✅ |

---

*End of Test Document v45.0*
