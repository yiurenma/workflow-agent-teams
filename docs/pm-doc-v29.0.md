# PM Doc v29.0 — Node Editor Drawer Close Button Fix

**Version:** 29.0  
**Date:** 2026-04-12  
**Status:** Draft — awaiting human approval  
**Label:** `TODO-uat-e2e-node-drawer-close-button-broken`

---

## Overview

The node editor drawer's built-in close button (×) does not dismiss the drawer when clicked. Users are forced to click outside the drawer or use workarounds. This is a HIGH severity UX defect affecting both desktop and mobile viewports.

---

## User Stories & Acceptance Criteria

### CV-US-37 — Node Editor Drawer Close Button

> **As a** user editing a workflow node, **I want** the × close button on the node editor drawer to reliably close the drawer, **so that** I can return to the canvas without friction.

**CV-AC-37-1** — Clicking the `.ant-drawer-close` button closes the drawer on desktop (right-side drawer, 1280px viewport).

**CV-AC-37-2** — Clicking the `.ant-drawer-close` button closes the drawer on mobile (bottom-sheet drawer, 390×844 viewport).

**CV-AC-37-3** — After the drawer closes, the canvas is fully interactive (nodes can be clicked, panned, zoomed).

**CV-AC-37-4** — No regression: clicking a node still opens the drawer; form data is preserved while the drawer is open.

**CV-AC-37-5** — Clicking outside the drawer (on the canvas) also closes the drawer (existing behavior must be preserved or added).

---

## Scope

**In scope:** `workflow-ui` — `WorkflowDrawer` component and its integration in `WorkflowEditor`.  
**Out of scope:** Backend API changes, other drawer/modal components.

---
