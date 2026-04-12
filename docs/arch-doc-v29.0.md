# Arch Doc v29.0 — Node Editor Drawer Close Button Fix

**Version:** 29.0  
**Date:** 2026-04-12  
**Status:** Draft — awaiting human approval  
**Label:** `TODO-uat-e2e-node-drawer-close-button-broken`  
**Refs:** `pm-doc-v29.0.md` (CV-US-37, CV-AC-37-1~5)

---

## 1. Root Cause Analysis

### Code path

```
WorkflowEditor (worflow-canvas/index.tsx)
  └─ useWorkflowForm → { drawerOpen, onDrawerClose }
  └─ <WorkflowDrawer open={drawerOpen} onClose={onDrawerClose} />
       └─ <Drawer onClose={onClose} mask={false} ...>
```

`onDrawerClose` in `useWorkflowForm.ts:69` is:
```ts
const onDrawerClose = useCallback(() => {
    setDrawerOpen(false);
}, []);
```

This is correct. The `<Drawer>` has `onClose={onClose}` wired. The state update path is sound.

### Actual root cause — pointer event capture on the resize drag handle

The desktop drawer renders a 6px-wide invisible `<div>` at `position: absolute; left: 0` with `onPointerDown` that calls `e.preventDefault()`. This div sits at `zIndex: 10` and spans the full height of the drawer body. When Ant Design renders the close button (×) in the drawer header, the close button's click event fires correctly — **but** the `e.preventDefault()` in the resize handle's `onPointerDown` has a side effect: it suppresses the default pointer-down behavior on the entire drawer panel in some browser/Ant Design versions, causing the close button's synthetic React click to not propagate to Ant Design's internal `onClose` dispatcher.

Additionally, `mask={false}` on the mobile drawer means there is no backdrop to absorb stray pointer events, and ReactFlow's canvas has `panOnDrag={true}` which registers global `pointermove`/`pointerup` listeners that can interfere with the close button's click sequence.

### Secondary cause — missing `maskClosable` / canvas click-through

The desktop drawer has no `mask` at all (Ant Design default is `mask={true}` but the drawer is rendered inside ReactFlow's DOM subtree). ReactFlow's `onPaneClick` is not wired to close the drawer, so clicking the canvas does not close it either.

---

## 2. Fix

### Fix A — Remove `e.preventDefault()` from resize handle `onPointerDown`

In `workflow-drawer/index.tsx`, the resize handle calls `e.preventDefault()` to prevent text selection during drag. Replace with CSS `user-select: none` on the handle div instead. This eliminates the pointer event suppression.

```tsx
// Before
const onResizePointerDown = useCallback(
  (e: React.PointerEvent<HTMLDivElement>) => {
    e.preventDefault();   // ← remove this
    ...
  }, [drawerWidth]
);

// After — use CSS user-select instead
<div
  onPointerDown={onResizePointerDown}
  style={{ ..., userSelect: "none" }}
/>
```

### Fix B — Add `onPaneClick` to close drawer when canvas is clicked

In `worflow-canvas/index.tsx`, wire `onPaneClick` on `<ReactFlow>` to call `onDrawerClose`:

```tsx
<ReactFlow
  ...
  onPaneClick={onDrawerClose}
/>
```

This also satisfies CV-AC-37-5 (clicking outside closes drawer).

### Fix C — Ensure mobile drawer `mask` behavior is correct

The mobile drawer has `mask={false}`. This is intentional (canvas stays visible behind the bottom sheet). No change needed here — Fix A resolves the close button issue on mobile too.

---

## 3. Files Changed

| File | Change |
|------|--------|
| `src/routes/workflows/-components/workflow-drawer/index.tsx` | Remove `e.preventDefault()` from resize handle; add `userSelect: "none"` to handle style |
| `src/routes/workflows/-components/worflow-canvas/index.tsx` | Add `onPaneClick={onDrawerClose}` to `<ReactFlow>` |

---

## 4. Security & Regression

- No API changes
- No new dependencies
- `userSelect: none` is a standard CSS property — no accessibility impact
- `onPaneClick` fires only when clicking the empty canvas background, not on nodes or edges — no conflict with `onNodeClick`

---

## 5. Trade-offs

| Option | Decision |
|--------|----------|
| Remove `e.preventDefault()` entirely | Chosen — CSS `user-select: none` achieves the same drag UX without suppressing pointer events |
| Add explicit `closeIcon` prop override | Rejected — unnecessary complexity; root cause is the preventDefault |
| Wrap close button in a portal | Rejected — over-engineering |

---
