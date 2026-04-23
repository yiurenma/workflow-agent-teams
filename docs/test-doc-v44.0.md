# Test Document — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-23  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Test Scope

**Feature:** Rewrite Deploy modal to call online API with dual-block payload

**Components:**
- `workflow-ui/src/components/DeployModal.tsx`

**Test Levels:**
- Manual UAT against https://workflow-ui-gamma.vercel.app

---

## 2. Test Cases

### TC-DEPLOY-ONLINE-01 — Deploy button opens modal with pre-filled application name

**Objective:** Verify modal opens and pre-fills the Execution Application Name from the current application

**Steps:**
1. Navigate to https://workflow-ui-gamma.vercel.app/workflows
2. Click Deploy on any application row

**Expected Result:**
- ✅ Deploy modal opens
- ✅ Execution Application Name field is pre-filled with the application name
- ✅ No Deploy URL, Username, Password, or Environment fields present
- ✅ Payload summary shows Block A (entity settings source) and Block B (node count)

---

### TC-DEPLOY-ONLINE-02 — Deploy with default application name submits to online API

**Objective:** Verify clicking Deploy sends POST to online API with correct payload structure

**Steps:**
1. Open Deploy modal for an application with nodes
2. Keep the default application name
3. Click Deploy

**Expected Result:**
- ✅ Button shows "Deploying…" during request
- ✅ Online API receives `POST /api/workflow?applicationName=<app>&confirmationNumber=<uuid>`
- ✅ Request body contains `entitySetting` (Block A) and `workflow` (Block B)
- ✅ Success banner: "✓ Deployment submitted successfully"
- ✅ Toast notification shows success

---

### TC-DEPLOY-ONLINE-03 — Deploy with custom execution application name

**Objective:** Verify user can change the execution name

**Steps:**
1. Open Deploy modal
2. Clear and type a different execution application name
3. Click Deploy

**Expected Result:**
- ✅ Online API called with `?applicationName=<custom_name>`
- ✅ Source application data (entity settings + workflow) remains from the original Hub app
- ✅ Success or error banner displayed

---

### TC-DEPLOY-ONLINE-04 — Empty application name is rejected

**Objective:** Verify validation blocks empty application name

**Steps:**
1. Open Deploy modal
2. Clear the Execution Application Name field
3. Click Deploy

**Expected Result:**
- ✅ Error toast: "Application name is required"
- ✅ No API call made

---

### TC-DEPLOY-ONLINE-05 — API error is displayed

**Objective:** Verify error handling when online API returns an error

**Steps:**
1. Open Deploy modal for an application not configured in online API
2. Click Deploy

**Expected Result:**
- ✅ Error banner displayed with error message from API
- ✅ Toast notification shows error
- ✅ Modal remains open (user can retry or cancel)

---

### TC-DEPLOY-ONLINE-06 — Cancel closes modal without calling API

**Objective:** Verify Cancel button works

**Steps:**
1. Open Deploy modal
2. Click Cancel

**Expected Result:**
- ✅ Modal closes
- ✅ No API call made

---

## 3. Test Environment

**UAT Environment:**
- Frontend: https://workflow-ui-gamma.vercel.app
- Browser: Desktop Chrome 1280px + Mobile Chrome 390×844

---

## 4. Pass Criteria

All 6 test cases must PASS:
- TC-DEPLOY-ONLINE-01: PASS
- TC-DEPLOY-ONLINE-02: PASS
- TC-DEPLOY-ONLINE-03: PASS
- TC-DEPLOY-ONLINE-04: PASS
- TC-DEPLOY-ONLINE-05: PASS (error shown correctly)
- TC-DEPLOY-ONLINE-06: PASS

---

*End of Test Document v44.0*
