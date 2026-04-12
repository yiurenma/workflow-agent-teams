# UAT Report v29.0 — Node Editor Drawer Close Button Fix

**Version:** 29.0  
**Date:** 2026-04-12  
**Status:** PASS  
**Label:** `TODO-uat-e2e-node-drawer-close-button-broken`  
**UAT Environment:** https://workflow-ui-gamma.vercel.app  
**Build:** workflow-ui `a7dc488` (main)  
**E2E Tester:** Playwright (headed Desktop Chrome 1280px + Mobile Chrome 390×844)

---

## Overall Verdict: PASS

All 22 drawer-close-v29 test cases pass. Full suite: 105 passed, 0 failed, 1 skipped (pre-existing unrelated).

---

## Fixes Verified

| Fix | Description | Result |
|-----|-------------|--------|
| `closable={false}` | Ant Design default close button hidden | ✅ |
| Custom `onClick={onClose}` button in title | Bypasses rc-drawer dispatcher; explicit handler | ✅ |
| `minHeight: "40dvh"` on mobile wrapper + content | Drawer body expands to meaningful height | ✅ |
| `e.preventDefault()` removed from resize handle | `userSelect:"none"` prevents text selection | ✅ |
| `onPaneClick={onDrawerClose}` on ReactFlow | Canvas click closes drawer | ✅ |

---

## 5-Layer Mobile Bounding Box Results (TC-DRAWER-CLOSE-02, 390×844)

| Layer | Check | Actual | Spec | Result |
|-------|-------|--------|------|--------|
| L1 | `.ant-drawer-content-wrapper` visible | present | present | PASS |
| L1 | `[aria-label="Close"]` visible | present | present | PASS |
| L2 | `wrapperBox.height` | 337.59px | > 295.4px (35% of 844) | PASS |
| L2 | `wrapperBox.height` | 337.59px | ≤ 591px (70dvh) | PASS |
| L2 | `wrapperBox.width` | 390px | = 390px | PASS |
| L2 | `wrapperBox.bottom` | ≤ 844px | ≤ 844px | PASS |
| L2 | `closeBox.width` | 44px | ≥ 44px | PASS |
| L2 | `closeBox.height` | 44px | ≥ 44px | PASS |
| L2 | `closeBox.x + width` | 374px | ≤ 390px | PASS |
| L2 | `closeBox.y` | 518px | ≥ 0 | PASS |
| L3 | Drawer header `toBeInViewport()` | in viewport | in viewport | PASS |
| L3 | Close button `toBeInViewport()` | in viewport | in viewport | PASS |
| L3 | Drawer body `toBeInViewport()` | in viewport | in viewport | PASS |
| L3 | `scrollWidth` | 390px | ≤ 390px | PASS |
| L4 | Tap close button → drawer closes | closes | closes | PASS |
| L5 | `.ant-drawer-content-wrapper` not visible after close | not visible | not visible | PASS |
| L5 | Canvas `pointer-events` restored | not `none` | not `none` | PASS |

---

## Full Suite Summary

| Project | Passed | Failed | Skipped |
|---------|--------|--------|---------|
| Desktop Chrome (1280px) | 53 | 0 | 1 (pre-existing) |
| Mobile Chrome (390×844) | 52 | 0 | 0 |
| **Total** | **105** | **0** | **1** |

---

## Acceptance Criteria

| AC | Description | Result |
|----|-------------|--------|
| CV-AC-37-1 | Desktop × button closes drawer | PASS |
| CV-AC-37-2 | Mobile × button closes bottom sheet | PASS |
| CV-AC-37-3 | Canvas interactive after close | PASS |
| CV-AC-37-4 | No regression: node click opens drawer; form data preserved | PASS |
| CV-AC-37-5 | Canvas pane click closes drawer | PASS |

---
