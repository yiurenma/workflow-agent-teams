# Test Document — Deploy Rewrite: Online API v45.0

**Document Version:** 45.0  
**Date:** 2026-04-20  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Test Scope

**Feature:** Rewrite DeployModal to call online API with two-block request body

**Component:** `workflow-ui/src/components/DeployModal.tsx`

**Test Levels:** Manual QA + Playwright E2E

---

## 2. Test Cases

### TC-DEPLOY-ONLINE-01 — Form renders with correct fields

**Preconditions:** Open any application's Deploy modal

**Steps:**
1. Click Deploy button on any application row

**Expected:**
- Modal title: "Deploy Application"
- Fields visible: Online API URL, Application Name, Username (optional), Password (optional)
- **Not present:** Deploy URL (operation-api), Environment field
- Description text mentions "online API" and "deploy workflow"

---

### TC-DEPLOY-ONLINE-02 — Validation requires Online API URL and Application Name

**Steps:**
1. Open Deploy modal
2. Leave all fields empty, click Deploy

**Expected:**
- Toast error: "Please fill in Online API URL and Application Name"
- No HTTP request sent

---

### TC-DEPLOY-ONLINE-03 — HTTP URL triggers confirmation warning

**Steps:**
1. Enter `http://example.com` as Online API URL
2. Enter any application name
3. Click Deploy

**Expected:**
- Browser confirm dialog: warns about HTTP / unencrypted credentials
- Cancelling stops deployment
- Confirming proceeds with request

---

### TC-DEPLOY-ONLINE-04 — Successful deploy shows single-step success status

**Preconditions:** Mock online API responds 200

**Steps:**
1. Fill in valid Online API URL and Application Name
2. Click Deploy

**Expected:**
- Status panel shows "Submitting to online API…" (in-progress)
- On 200 response: "✓ Deploy request accepted" in green
- Success toast shown
- No 3-step progress — single status indicator only

---

### TC-DEPLOY-ONLINE-05 — Failed deploy shows error status with message

**Preconditions:** Mock online API responds 400 or 500

**Steps:**
1. Fill in valid form fields
2. Click Deploy
3. Mock returns error response

**Expected:**
- Status panel shows "✕ Deploy failed" in red
- Error message text displayed below
- Error toast shown

---

### TC-DEPLOY-ONLINE-06 — Request body contains sourceApplication and workflow blocks

**Steps:** Intercept the outbound request (browser DevTools Network tab or Playwright route intercept)

**Expected request:**
- Method: POST
- URL: `{onlineApiUrl}/api/workflow?applicationName={input}&confirmationNumber={uuid}`
- Headers: `Content-Type: application/json`, `X-Request-Correlation-Id: {uuid}`
- Body: `{ "sourceApplication": { ...entitySetting }, "workflow": { ...workflowData } }`
- Both blocks present and non-empty

---

### TC-DEPLOY-ONLINE-07 — Optional credentials sent as Basic Auth header

**Steps:**
1. Fill in Username and Password
2. Intercept outbound request

**Expected:**
- Header `Authorization: Basic {base64(user:pass)}` present

---

### TC-DEPLOY-ONLINE-08 — No Authorization header when credentials omitted

**Steps:**
1. Leave Username and Password empty
2. Intercept outbound request

**Expected:**
- No `Authorization` header in request

---

## 3. Test Environment

- **UAT:** https://workflow-ui-gamma.vercel.app
- **Browser:** Desktop Chrome 1280px + Mobile Chrome 390×844

---

## 4. Pass Criteria

All 8 test cases PASS. Zero regression on existing modal open/close behaviour.

---

*End of Test Document v45.0*
