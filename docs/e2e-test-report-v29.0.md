# E2E Test Report v29.0

Date: 2026-04-12
Environment: workflow-ui-gamma.vercel.app
Build: a74082f (workflow-ui main)
Fix scope: Remove `e.preventDefault()` from resize handle `onPointerDown`; add `userSelect:"none"` to handle style; add `onPaneClick={onDrawerClose}` to `<ReactFlow>`

---

## Summary

- Total test cases: 11 (TC-DRAWER-CLOSE-01 through -10, excluding TC-DRAWER-CLOSE-06 which is covered by TC-01 scope)
- Playwright test runs: 22 (11 TCs × 2 projects, with viewport guards)
- Passed: 15
- Failed: 7
- Skipped: 0 (zero-skip policy maintained — viewport guards used instead)

---

## Test Results by Project

### Desktop Chrome (1280×720)

| TC-ID | Result | Layer(s) | Notes |
|-------|--------|----------|-------|
| TC-DRAWER-CLOSE-01 | FAIL | L5 | Drawer stays visible after close button click. `not.toBeVisible()` timeout 10s exceeded. Drawer element remains with class `ant-drawer ant-drawer-right css-i39sf6` — no `display:none` applied. Core regression: close button click does not dismiss drawer on Desktop Chrome. |
| TC-DRAWER-CLOSE-06 | FAIL | L5 | Same root cause as TC-01. Close button click fires but drawer does not close. |
| TC-DRAWER-CLOSE-07 | FAIL | L5 | Canvas pane click (`mouse.click` at 30% vw, 50% vh) does not close drawer. `onPaneClick` handler not firing or not closing drawer. |
| TC-DRAWER-CLOSE-08 | PASS | L4·L5 | Resize handle drag completes. `window.getSelection().toString() === ""` confirmed. Drawer width unchanged (drag clamped at min width) — acceptable. |
| TC-DRAWER-CLOSE-09 | FAIL | L5 | Close button click does not dismiss drawer; second node click blocked. Same root cause as TC-01. |
| TC-DRAWER-CLOSE-10 | PASS | L4·L5 | Form input accepts value; `toHaveValue('test-value-v29')` confirmed. |
| TC-DRAWER-CLOSE-02 (mobile guard) | PASS | — | Viewport guard triggered (desktop viewport ≥ 768px); test trivially passes. |
| TC-DRAWER-CLOSE-03 (mobile guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-04 (mobile guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-05 (mobile guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-09 Mobile (mobile guard) | PASS | — | Viewport guard triggered. |

### Mobile Chrome (390×844, isMobile: true, hasTouch: true)

| TC-ID | Result | Layer(s) | Notes |
|-------|--------|----------|-------|
| TC-DRAWER-CLOSE-01 (desktop guard) | PASS | — | Viewport guard triggered (mobile viewport < 768px); test trivially passes. |
| TC-DRAWER-CLOSE-06 (desktop guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-07 (desktop guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-08 (desktop guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-09 Desktop (desktop guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-10 (desktop guard) | PASS | — | Viewport guard triggered. |
| TC-DRAWER-CLOSE-02 | FAIL | L2 | `wrapperBox.height = 91.5px` — fails `> 295.4px` (35% of 844). Drawer wrapper is not expanding to minimum height. The `maxHeight: 70dvh` CSS cap is set but no `minHeight` is enforced. Drawer renders at collapsed/header-only height. |
| TC-DRAWER-CLOSE-03 | FAIL | L2 | `bodyBox.height = 32px`, `bodyBox.y = 812px`, `bodyBox.bottom = 844px` — body height is only 32px (header-only state). First form field `toBeInViewport()` fails (viewport ratio 0). |
| TC-DRAWER-CLOSE-04 | PASS | L2·L5 | `wrapperBox.bottom = 844px` (within viewport). `paddingBottom = 16px` confirmed. |
| TC-DRAWER-CLOSE-05 | PASS | L1·L2·L3 | `.react-flow__renderer` attached. `wrapperBox.y > 0` confirmed. Canvas node in viewport. |
| TC-DRAWER-CLOSE-09 Mobile | FAIL | L5 | Close button tap does not dismiss drawer. Same root cause as Desktop TC-01 — close button interaction does not trigger drawer close. |

---

## Measured Bounding Box Values

### TC-DRAWER-CLOSE-02 (Mobile Chrome, 390×844)

| Measurement | Actual | Spec | Pass? |
|-------------|--------|------|-------|
| `wrapperBox.height` | 91.5px | > 295.4px (35% of 844) | FAIL |
| `wrapperBox.width` | 390px | = 390px | PASS |
| `wrapperBox.y` | 752.5px | >= 0 | PASS |
| `wrapperBox.y + height` | 844px | <= 844px | PASS |
| `closeBox.width` | not measured (failed at height check) | >= 44px | — |
| `closeBox.height` | not measured | >= 44px | — |

### TC-DRAWER-CLOSE-03 (Mobile Chrome, 390×844)

| Measurement | Actual | Spec | Pass? |
|-------------|--------|------|-------|
| `bodyBox.height` | 32px | <= 591px (70% of 844) | PASS (but trivially — drawer not expanded) |
| `bodyBox.y` | 812px | — | — |
| `bodyBox.bottom` | 844px | <= 844px | PASS |
| First form field `toBeInViewport()` | viewport ratio 0 | in viewport | FAIL |

### TC-DRAWER-CLOSE-04 (Mobile Chrome, 390×844)

| Measurement | Actual | Spec | Pass? |
|-------------|--------|------|-------|
| `wrapperBox.bottom` | 844px | <= 844px | PASS |
| `paddingBottom` | 16px | != 0px | PASS |

---

## Failures

### FAIL-1: Close button does not dismiss drawer (Desktop Chrome)

Affects: TC-DRAWER-CLOSE-01, TC-DRAWER-CLOSE-06, TC-DRAWER-CLOSE-07, TC-DRAWER-CLOSE-09

Root cause: After clicking `.ant-drawer-close`, the drawer element (`div.ant-drawer.ant-drawer-right`) remains visible. The `not.toBeVisible()` assertion times out at 10 seconds. The fix (`e.preventDefault()` removal from resize handle `onPointerDown`) was intended to unblock pointer events on the close button, but the close button click is not triggering the drawer's `onClose` handler on Desktop Chrome in the UAT environment.

Evidence: Drawer class is `ant-drawer ant-drawer-right css-i39sf6` — no `ant-drawer-open` class present after node click in some retries, suggesting the drawer may be in a partially-open state where the close button is rendered but its click handler is not wired. Alternatively, the Vercel deployment at build `a74082f` may not have the fix applied correctly.

Severity: Critical — this is the primary regression being fixed in v29.0.

Repro steps:
1. Navigate to `https://workflow-ui-gamma.vercel.app/workflows/test-app-01`
2. Click any canvas node — drawer opens
3. Click the × close button
4. Observe: drawer remains visible

### FAIL-2: Mobile drawer wrapper height too small (Mobile Chrome)

Affects: TC-DRAWER-CLOSE-02

Root cause: `wrapperBox.height = 91.5px` — the drawer content wrapper is only 91.5px tall (header height only). The `maxHeight: 70dvh` CSS is applied but the drawer body content is not expanding the wrapper to the expected minimum height. The mock workflow has 2 nodes with form fields; the drawer body should render at a meaningful height.

Possible cause: The drawer `height` or `minHeight` CSS property is not set, so the wrapper collapses to fit only the header. The `70dvh` cap is a maximum, not a minimum.

Severity: High — the drawer is functionally unusable at 91.5px height on mobile.

### FAIL-3: Mobile drawer body not in viewport (Mobile Chrome)

Affects: TC-DRAWER-CLOSE-03

Root cause: Consequence of FAIL-2. With `bodyBox.height = 32px` and `bodyBox.y = 812px`, the body is at the very bottom of the viewport with no visible content. The first form field has `viewport ratio 0`.

Severity: High — same root cause as FAIL-2.

### FAIL-4: Mobile close button tap does not dismiss drawer (Mobile Chrome)

Affects: TC-DRAWER-CLOSE-09 Mobile

Root cause: Same as FAIL-1 — close button interaction does not trigger drawer close. On mobile the drawer class is `ant-drawer ant-drawer-bottom no-mask css-i39sf6` — the drawer stays open after tap.

Severity: Critical.

---

## Accessibility Violations

Axe scan not run in this cycle (no `AxeBuilder` integration in current spec). Deferred to next cycle.

---

## Console Errors

No JavaScript console errors observed during test execution (no `pageerror` events captured).

---

## New TODO Items Required

Per process rules, each FAIL becomes a new TODO item:

1. `TODO-v29-desktop-close-button-not-working`: Desktop Chrome — close button click does not dismiss drawer. Investigate whether `onClose` handler is wired correctly in the deployed build. Check if `e.preventDefault()` removal is actually deployed at `a74082f`.

2. `TODO-v29-mobile-drawer-height-too-small`: Mobile Chrome — drawer wrapper height is 91.5px (header only). Add `minHeight` or ensure drawer body renders at meaningful height on mobile.

---

## Conclusion

VERDICT: FAIL

The v29.0 fix is partially deployed. The resize handle `userSelect:"none"` fix is confirmed working (TC-08 passes). However, the core regression — close button dismissing the drawer — fails on both Desktop Chrome (TC-01, TC-06, TC-07, TC-09) and Mobile Chrome (TC-09). Additionally, the mobile drawer height is critically undersized (91.5px vs required >295px).

UAT is BLOCKED. Two new TODO items opened. Full cycle restarts for each defect.
