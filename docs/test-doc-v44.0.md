# Test Document — Deploy Rewrite: Online API Integration v44.0

**Document Version:** 44.0  
**Date:** 2026-04-26  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Test Scope

**Feature:** Rewrite DeployModal to call online API with dual-block request body

**Components:**
- `workflow-ui/src/components/DeployModal.tsx`

**Test Levels:**
- Manual UI tests (browser)
- E2E tests (Playwright against UAT environment)

---

## 2. Test Cases

### TC-DEPLOY-REWRITE-01 — Deploy modal opens with pre-filled execution name

**Objective:** Verify modal pre-fills the execution name from current application

**Preconditions:**
- Navigate to https://workflow-ui-gamma.vercel.app
- At least one application exists in the list

**Steps:**
1. Click Deploy for any application in the table
2. Observe modal contents

**Expected Result:**
- ✅ Modal opens with title "Deploy Application"
- ✅ "Execution Application Name" field pre-filled with current application name
- ✅ No Deploy URL, Username, Password, Environment fields present
- ✅ Request Body preview shows Block A and Block B info

---

### TC-DEPLOY-REWRITE-02 — Deploy button disabled when application name is empty

**Objective:** Verify form validation prevents empty execution name

**Steps:**
1. Open Deploy modal
2. Clear the Execution Application Name field
3. Observe Deploy button state

**Expected Result:**
- ✅ Deploy button is disabled when name field is empty

---

### TC-DEPLOY-REWRITE-03 — Successful deploy calls online API

**Preconditions:**
- Mock: online API POST `/workflow?applicationName=test-deploy` returns 200 with `{"status":"ok"}`

**Steps:**
1. Open Deploy modal
2. Enter "test-deploy" in Execution Application Name
3. Click Deploy
4. Observe network request and UI response

**Expected Result:**
- ✅ POST to `/api/proxy/online/workflow?applicationName=test-deploy` is made
- ✅ Request body contains `entitySetting` (Block A) and `workflow` (Block B)
- ✅ "Deployment initiated" toast appears
- ✅ Green response panel shows API response
- ✅ Deploy button re-enables after completion

---

### TC-DEPLOY-REWRITE-04 — Failed deploy shows error panel

**Preconditions:**
- Mock: online API POST returns 500

**Steps:**
1. Open Deploy modal
2. Enter execution name
3. Click Deploy
4. Observe error handling

**Expected Result:**
- ✅ Error toast appears
- ✅ Red error panel shown with error message
- ✅ Deploy button re-enables

---

### TC-DEPLOY-REWRITE-05 — Modal closes on Cancel

**Steps:**
1. Open Deploy modal
2. Click Cancel

**Expected Result:**
- ✅ Modal closes
- ✅ State reset (no leftover response/error from previous deploy)

---

### TC-DEPLOY-REWRITE-06 — Modal cannot be closed during deploy

**Steps:**
1. Open Deploy modal
2. Click Deploy (with slow/hanging API)
3. Attempt to click Cancel or X button

**Expected Result:**
- ✅ Cancel and X buttons are disabled while deploying
- ✅ Click outside modal does nothing while deploying

---

## 3. Test Environment

**UAT Environment:**
- Frontend: https://workflow-ui-gamma.vercel.app
- Browser: Desktop Chrome 1280px + Mobile Chrome 390×844

---

## 4. Pass Criteria

All 6 test cases must PASS.

---

*End of Test Document v44.0*
