# Test Doc v29.0 — Node Editor Drawer Close Button Fix

**Version:** 29.0  
**Date:** 2026-04-12  
**Status:** Draft — awaiting human approval  
**Label:** `TODO-uat-e2e-node-drawer-close-button-broken`  
**Refs:** `pm-doc-v29.0.md` (CV-US-37), `arch-doc-v29.0.md`

---

## Test Scope

Verify the drawer close button works on desktop and mobile, and that no regressions are introduced. All tests run against https://workflow-ui-gamma.vercel.app

---

## Test Cases

### TC-DRAWER-CLOSE-01 — Desktop: close button dismisses drawer

**Layer:** 4 + 5  
**Viewport:** Desktop Chrome 1280px  
**Steps:**
1. Navigate to `/workflows/$applicationName` (any application with nodes)
2. Click any node on the canvas — drawer opens
3. Click the `.ant-drawer-close` button (×)

**Expected:** Drawer is no longer visible; `open` state is `false`; canvas is interactive

---

### TC-DRAWER-CLOSE-02 — Mobile: close button dismisses drawer

**Layer:** 4 + 5  
**Viewport:** Mobile Chrome 390×844  
**Steps:**
1. Navigate to `/workflows/$applicationName`
2. Tap any node — bottom-sheet drawer opens
3. Tap the `.ant-drawer-close` button (×)

**Expected:** Drawer closes; canvas is visible and interactive

---

### TC-DRAWER-CLOSE-03 — Desktop: clicking canvas closes drawer

**Layer:** 4  
**Viewport:** Desktop Chrome 1280px  
**Steps:**
1. Open drawer by clicking a node
2. Click on the empty canvas background (not on a node or edge)

**Expected:** Drawer closes

---

### TC-DRAWER-CLOSE-04 — Resize handle drag still works after fix

**Layer:** 4  
**Viewport:** Desktop Chrome 1280px  
**Steps:**
1. Open drawer by clicking a node
2. Drag the left edge of the drawer to resize it
3. Verify no text is selected during drag

**Expected:** Drawer resizes correctly; no text selection occurs; `userSelect: none` applied

---

### TC-DRAWER-CLOSE-05 — Regression: clicking node opens drawer

**Layer:** 4  
**Viewport:** Desktop Chrome 1280px + Mobile Chrome 390×844  
**Steps:**
1. Click/tap any node on the canvas

**Expected:** Drawer opens with correct node data

---

### TC-DRAWER-CLOSE-06 — Regression: form data preserved while drawer open

**Layer:** 4 + 5  
**Viewport:** Desktop Chrome 1280px  
**Steps:**
1. Open drawer, edit a field (e.g., node description)
2. Do NOT close drawer
3. Verify field value is retained

**Expected:** Form data is not lost while drawer is open

---

### TC-DRAWER-CLOSE-07 — Regression: close then reopen shows fresh node data

**Layer:** 4 + 5  
**Viewport:** Desktop Chrome 1280px  
**Steps:**
1. Click node A — drawer opens with node A data
2. Close drawer via × button
3. Click node B — drawer opens with node B data

**Expected:** Drawer shows node B data, not stale node A data

---

### TC-DRAWER-CLOSE-08 — Mobile: canvas remains visible behind bottom sheet

**Layer:** 1 + 3  
**Viewport:** Mobile Chrome 390×844  
**Steps:**
1. Open drawer on mobile
2. Verify canvas nodes are visible behind the bottom sheet

**Expected:** Canvas is partially visible; `mask={false}` behavior preserved

---
