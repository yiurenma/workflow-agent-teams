# Test Doc v29.0 — Node Editor Drawer Close Button Fix

**Version:** 29.0  
**Date:** 2026-04-12  
**Status:** Draft — awaiting human approval  
**Label:** `TODO-uat-e2e-node-drawer-close-button-broken`  
**Refs:** `pm-doc-v29.0.md` (CV-US-37), `arch-doc-v29.0.md`

---

## 5-Layer UX Validation Framework

Every test case is tagged with the layers it exercises:

| Layer | Name | What to check |
|-------|------|---------------|
| L1 | **Exist** | Element is present in DOM (`toBeVisible`, `locator.count() > 0`) |
| L2 | **Size** | `getBoundingClientRect()` — height, width, position meet spec |
| L3 | **Viewport** | Content not clipped: `toBeInViewport()`, no overflow outside screen |
| L4 | **Interact** | Tap/click/input events fire and produce the expected state change |
| L5 | **Effect** | Computed styles (`getComputedStyle`) or DOM state match spec after interaction |

**Mobile viewport for all mobile tests:** 390 × 844 px, `isMobile: true`, `hasTouch: true`

---

## Test Cases

---

### TC-DRAWER-CLOSE-01 — Desktop: close button dismisses drawer

**Layers:** L1 · L4 · L5  
**Viewport:** Desktop Chrome 1280px

**Steps:**
1. Navigate to `/workflows/$applicationName` (any application with nodes)
2. Click any node on the canvas
3. L1: Assert `.ant-drawer` is visible in DOM
4. Click `.ant-drawer-close` button
5. L4: Assert click event fires without error
6. L5: Assert `.ant-drawer` is no longer visible (`display: none` or removed from DOM)

**Expected:** Drawer dismissed; canvas interactive

---

### TC-DRAWER-CLOSE-02 — Mobile: close button dismisses drawer (L1–L5 full)

**Layers:** L1 · L2 · L3 · L4 · L5  
**Viewport:** Mobile Chrome 390×844

This is the **primary regression test** for the bug. All 5 layers must pass.

**L1 — Exist**
1. Navigate to `/workflows/$applicationName`
2. Tap any node on the canvas
3. Assert `.ant-drawer` is present in DOM
4. Assert `.ant-drawer-close` button is present in DOM
5. Assert drawer header title text is visible

**L2 — Size (bounding box)**
6. Call `getBoundingClientRect()` on the `.ant-drawer-content-wrapper` element
7. Assert `height > 0.35 * 844` (drawer occupies at least 35% of viewport height = 295px)
8. Assert `height <= 0.70 * 844` (drawer does not exceed 70% of viewport = 591px — matches `maxHeight: 70dvh`)
9. Assert `width === 390` (drawer spans full viewport width)
10. Assert `bottom <= 844` (drawer bottom edge is within viewport — not clipped below screen)
11. Assert `top >= 0` (drawer top edge is within viewport — not clipped above screen)
12. Call `getBoundingClientRect()` on `.ant-drawer-close` button
13. Assert close button `width >= 44` and `height >= 44` (minimum touch target size per WCAG 2.5.5)
14. Assert close button `right <= 390` (close button not clipped outside right edge)
15. Assert close button `top >= 0` (close button not clipped above viewport)

**L3 — Viewport (not clipped)**
16. Assert `.ant-drawer-header` `toBeInViewport()` — header fully visible
17. Assert `.ant-drawer-close` `toBeInViewport()` — close button fully visible and tappable
18. Assert `.ant-drawer-body` `toBeInViewport()` — body content not scrolled out of view on open
19. Assert no horizontal overflow: `document.documentElement.scrollWidth === 390`

**L4 — Interact**
20. Tap `.ant-drawer-close` button
21. Assert tap event fires (no JS errors in console)
22. Assert drawer is no longer visible after tap

**L5 — Effect (computed styles)**
23. Before close: assert `getComputedStyle(.ant-drawer-content-wrapper).transform` is not `translateY(100%)` (drawer is open, not hidden)
24. After close: assert `.ant-drawer` is not in DOM or has `display: none`
25. Assert canvas `.react-flow__pane` is interactive after close (pointer-events not blocked)

---

### TC-DRAWER-CLOSE-03 — Mobile: drawer body scroll within bounds (L2 · L3)

**Layers:** L2 · L3  
**Viewport:** Mobile Chrome 390×844

**Steps:**
1. Tap a node to open drawer
2. If drawer body content overflows, scroll within the drawer body
3. L2: Assert drawer body `getBoundingClientRect().height <= 591` (≤ 70dvh) at all times
4. L2: Assert drawer body `getBoundingClientRect().bottom <= 844` — never extends below screen
5. L3: Assert `toBeInViewport()` on the first form field in the drawer body
6. L3: Scroll to bottom of drawer body; assert last form field `toBeInViewport()` after scroll
7. Assert no content is clipped by `overflow: hidden` on the wrapper

**Expected:** Drawer body scrolls internally; no content escapes the 70dvh cap; all form fields reachable

---

### TC-DRAWER-CLOSE-04 — Mobile: safe-area inset padding applied (L2 · L5)

**Layers:** L2 · L5  
**Viewport:** Mobile Chrome 390×844

**Steps:**
1. Tap a node to open drawer
2. L2: Assert drawer body `getBoundingClientRect().bottom <= 844`
3. L5: Assert `getComputedStyle(.ant-drawer-body).paddingBottom` is not `0px` (safe-area-inset-bottom applied)
4. Assert the last visible element in the drawer body is not obscured by the device home indicator area

**Expected:** Bottom padding accounts for safe-area; content not hidden behind home bar

---

### TC-DRAWER-CLOSE-05 — Mobile: canvas partially visible behind bottom sheet (L1 · L2 · L3)

**Layers:** L1 · L2 · L3  
**Viewport:** Mobile Chrome 390×844

**Steps:**
1. Tap a node to open drawer
2. L1: Assert `.react-flow__renderer` is present in DOM
3. L2: Assert drawer `getBoundingClientRect().top > 0` — canvas is visible above the drawer
4. L2: Assert visible canvas area height = drawer `top` value > 100px (meaningful canvas area exposed)
5. L3: Assert at least one canvas node is `toBeInViewport()` above the drawer
6. Assert `mask={false}` — no dark overlay covering the canvas

**Expected:** Canvas nodes visible above the bottom sheet; no mask overlay

---

### TC-DRAWER-CLOSE-06 — Desktop: close button dismisses drawer (L1–L5 full)

**Layers:** L1 · L2 · L3 · L4 · L5  
**Viewport:** Desktop Chrome 1280px

**L1 — Exist**
1. Click a node; assert `.ant-drawer` present; assert `.ant-drawer-close` present

**L2 — Size (bounding box)**
2. Assert drawer `getBoundingClientRect().width >= 320` (MIN_WIDTH)
3. Assert drawer `getBoundingClientRect().width <= 900` (MAX_WIDTH)
4. Assert drawer `getBoundingClientRect().height === 100% of canvas area` (full height)
5. Assert `.ant-drawer-close` button `width >= 32` and `height >= 32`

**L3 — Viewport**
6. Assert `.ant-drawer-header` `toBeInViewport()`
7. Assert `.ant-drawer-close` `toBeInViewport()`

**L4 — Interact**
8. Click `.ant-drawer-close`; assert drawer closes

**L5 — Effect**
9. Assert `.ant-drawer` not visible after close
10. Assert canvas `pointer-events` not blocked after close

---

### TC-DRAWER-CLOSE-07 — Desktop: canvas click closes drawer (L4 · L5)

**Layers:** L4 · L5  
**Viewport:** Desktop Chrome 1280px

**Steps:**
1. Click a node to open drawer
2. Click `.react-flow__pane` (empty canvas area)
3. L4: Assert click fires on pane
4. L5: Assert drawer is no longer visible

**Expected:** `onPaneClick` handler closes drawer

---

### TC-DRAWER-CLOSE-08 — Desktop: resize handle drag works after fix (L4 · L5)

**Layers:** L4 · L5  
**Viewport:** Desktop Chrome 1280px

**Steps:**
1. Open drawer; record initial `getBoundingClientRect().width`
2. Drag the left edge of the drawer 80px to the left
3. L4: Assert drag completes without JS error
4. L5: Assert new `getBoundingClientRect().width > initial width`
5. Assert no text is selected on page (`window.getSelection().toString() === ""`)

**Expected:** Drawer resizes; no text selection; `userSelect: none` effective

---

### TC-DRAWER-CLOSE-09 — Regression: node tap opens drawer with correct data (L1 · L4 · L5)

**Layers:** L1 · L4 · L5  
**Viewport:** Desktop Chrome 1280px + Mobile Chrome 390×844

**Steps:**
1. Tap/click node A
2. L1: Assert drawer visible
3. L5: Assert drawer title contains node A's label
4. Close drawer
5. Tap/click node B
6. L5: Assert drawer title contains node B's label (not stale node A data)

---

### TC-DRAWER-CLOSE-10 — Regression: form data preserved while drawer open (L4 · L5)

**Layers:** L4 · L5  
**Viewport:** Desktop Chrome 1280px

**Steps:**
1. Open drawer; type a value into the description field
2. L4: Assert input accepts the value
3. L5: Assert `input.value` matches typed text (not cleared by re-render)

---

