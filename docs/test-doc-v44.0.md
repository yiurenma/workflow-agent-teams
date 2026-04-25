# Test Specification — Deploy Rewrite: Online API v44.0

**Document Version:** 44.0  
**Date:** 2026-04-25  
**Status:** Draft  
**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

---

## 1. Test Scope

This document covers test cases for the Deploy modal rewrite. Tests verify:

1. Modal renders with simplified form (single input)
2. Modal pre-fills applicationName correctly
3. Deploy POSTs to the online API (not operation API)
4. Request body contains both block A (sourceApplicationSetting) and block B (workflow)
5. Success and error states are handled correctly
6. Deploy button is disabled during/after deploy

---

## 2. Test Cases

| ID | Title | Type | Expected Result |
|----|-------|------|-----------------|
| TC-DEPLOY-V44-01 | Deploy button visible in Actions column | E2E | Deploy button rendered in table row |
| TC-DEPLOY-V44-02 | Modal opens with pre-filled applicationName | E2E | Input shows current app name |
| TC-DEPLOY-V44-03 | Deploy POSTs to online API with applicationName | E2E | URL contains `/api/proxy/online/workflow?applicationName=...` |
| TC-DEPLOY-V44-04 | Request body contains sourceApplicationSetting | E2E | Parsed body has `sourceApplicationSetting` key |
| TC-DEPLOY-V44-05 | Request body contains workflow | E2E | Parsed body has `workflow` key |
| TC-DEPLOY-V44-06 | Success state shown after deploy | E2E | Deploy button disabled after success |

---

## 3. Test Case Details

### TC-DEPLOY-V44-01
**Preconditions:** Applications list page loaded with mock data  
**Steps:** Open applications page  
**Expected:** Deploy button visible in first table row's Actions column  

### TC-DEPLOY-V44-02
**Preconditions:** Applications list page loaded  
**Steps:** Click Deploy button on first row  
**Expected:** Modal opens; `Execution Application Name` input pre-filled with `test-app-01`  

### TC-DEPLOY-V44-03
**Preconditions:** Online API mock intercepts `/api/proxy/online/workflow`  
**Steps:** Click Deploy → click Deploy button in modal  
**Expected:** Intercepted URL contains `applicationName=test-app-01`  

### TC-DEPLOY-V44-04
**Preconditions:** Online API mock captures request body  
**Steps:** Click Deploy → submit  
**Expected:** Parsed JSON body has `sourceApplicationSetting` property  

### TC-DEPLOY-V44-05
**Preconditions:** Online API mock captures request body  
**Steps:** Click Deploy → submit  
**Expected:** Parsed JSON body has `workflow` property  

### TC-DEPLOY-V44-06
**Preconditions:** Online API mock returns 200  
**Steps:** Click Deploy → submit → wait 800ms  
**Expected:** Deploy button is disabled (status=success)  

---

## 4. Regression Checklist

- [ ] DeployModal props interface unchanged (open, onClose, currentWorkflow, currentSettings)
- [ ] No credentials field in UI
- [ ] No 3-step progress tracker in UI
- [ ] Existing call sites compile without changes
- [ ] Cancel button closes modal
- [ ] Clicking overlay closes modal
