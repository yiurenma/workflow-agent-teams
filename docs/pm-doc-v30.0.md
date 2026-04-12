# PM Doc v30.0 — Workflow Studio Unified Product & UX Program

**Version:** 30.0  
**Date:** 2026-04-12  
**Status:** Draft — awaiting human approval  
**Label:** `TODO-workflow-studio-unified-product-ux-program`

---

## Overview

This unified program consolidates multiple product improvements into a single coordinated release for Workflow Studio. The program addresses E2E test quality, IBM Carbon design completeness, node editor usability, canvas discoverability, node operations, AI generation quality, and brand identity.

**Scope:** `workflow-ui` frontend only. No backend API changes required.

**Sub-items are scope only** — use historical labels for commits/tickets. Clear the single TODO checkbox when the whole program ships.

---

## User Stories & Acceptance Criteria

### 1. E2E Test Quality

#### US-38 — Playwright 5-Layer UX Validation Framework

> **As a** QA engineer running E2E tests, **I want** Playwright tests to validate UI correctness across five layers (exist, size, viewport, interact, effect), **so that** visual regressions and UX defects are caught before UAT.

**AC-38-1** — All Playwright test cases use the 5-layer validation framework:
- Layer 1 (Exist): Elements present in DOM
- Layer 2 (Size): Dimensions meet spec
- Layer 3 (Viewport): Content not clipped
- Layer 4 (Interact): Clicks/inputs work
- Layer 5 (Effect): Computed styles/responses match spec

**AC-38-2** — Retrofit existing tests to include Layer 2-5 checks where applicable

**AC-38-3** — Screenshot baselines captured for key UI states

**AC-38-4** — Test report documents which layers validated per test case

---

### 2. IBM Carbon Design System Completeness

#### US-39 — Audit and Fix Residual Non-Carbon UI

> **As a** Workflow Studio user, **I want** all UI components to consistently follow IBM Carbon Design Language, **so that** the interface feels cohesive and professional.

**AC-39-1** — Audit identifies all non-Carbon UI elements (non-zero border-radius buttons, non-Carbon modals, non-IBM Plex fonts, non-Carbon colors, rounded components except status badges)

**AC-39-2** — Numbered gap list with root causes (`Modal.confirm`, portals, Tailwind overrides)

**AC-39-3** — All gaps fixed (replace `Modal.confirm` with Carbon-styled modal, override portal styles, replace Tailwind colors with CSS custom properties)

**AC-39-4** — Document why Playwright missed styling gaps and add assertions/screenshot baselines

**AC-39-5** — Application Delete confirm dialog verified to use Carbon styling (0px border-radius, Gray 100 text, Blue 60 primary, Red 60 danger)

---

### 3. Node Editor — Rules Validation

#### US-40 — Rule Key Must Be Single JSONPath with Clear Error

> **As a** workflow author configuring node rules, **I want** the system to validate that each rule key is a single valid JSONPath expression, **so that** I get immediate feedback if syntax is wrong.

**AC-40-1** — Validate rule key is single JSONPath (no commas/semicolons, valid syntax via `jsonpath-plus`)

**AC-40-2** — Clear error message on validation failure ("Rule key must be a single JSONPath expression (e.g., $.customer.id)")

**AC-40-3** — Save button disabled while validation errors exist

**AC-40-4** — Validation runs on blur and save attempt

---

### 4. Node Editor — UX Improvements

#### US-41 — Resizable Drawer, Read-Only Default, Top Edit Button

> **As a** workflow author editing nodes, **I want** a resizable drawer that opens in read-only mode with an Edit button at the top, **so that** I can review long configurations comfortably and clearly distinguish view/edit modes.

**AC-41-1** — Desktop drawer resizable (drag left edge, min 420px, max 60% viewport, width persisted to localStorage)

**AC-41-2** — Drawer opens read-only by default (disabled fields, text-first display, Edit button at top)

**AC-41-3** — Edit button switches to edit mode (fields editable, Cancel/Save buttons, Cancel discards, Save persists)

**AC-41-4** — Long content scrolls and reflows when drawer resized

**AC-41-5** — Mobile drawer not resizable but uses read-only default with Edit button

---

### 5. Canvas Palette — Descriptions

#### US-42 — Node Palette with One-Line Descriptions

> **As a** workflow author, **I want** the node palette to show a one-line description for each node type, **so that** I understand what each node does before adding it.

**AC-42-1** — Desktop palette shows icon, display name, and one-line description per node type

**AC-42-2** — Descriptions concise (max 50 chars) and action-oriented

**AC-42-3** — Drag-drop behavior preserved

**AC-42-4** — Mobile palette shows same descriptions

**AC-42-5** — Collapsed palette shows tooltip with name + description on hover

---

### 6. Canvas Node Actions — Copy and Delete

#### US-43 — Copy Full Node Config and Delete with Edge Cleanup

> **As a** workflow author, **I want** to copy a node's full configuration and delete nodes with automatic edge cleanup, **so that** I can efficiently duplicate and remove nodes without manual cleanup.

**AC-43-1** — Copy via context menu (right-click) and keyboard (Cmd+C / Ctrl+C)

**AC-43-2** — Copy behavior: clipboard JSON or duplicate node at offset position

**AC-43-3** — Delete via context menu, keyboard (Delete/Backspace), or drawer button

**AC-43-4** — Delete removes node and all connected edges; confirmation if connections exist

**AC-43-5** — Mobile: Copy and Delete in context menu (long-press)

---

### 7. AI Workflow Generator — Quality

#### US-44 — Improve AI Generator Prompt and Semantic Output Quality

> **As a** workflow author using AI Generate, **I want** the generated workflow to be semantically correct and match my intent, **so that** I can use it as a starting point without extensive fixes.

**AC-44-1** — Enhanced prompt includes examples, node type descriptions, edge semantics, vertical layout guidance

**AC-44-2** — Generated workflow validated before applying (JSON structure, valid node types, edges reference existing IDs, no orphaned nodes)

**AC-44-3** — Clear error message with details if validation fails

**AC-44-4** — Explain → Generate flow on UAT (`E2E_TEST_CANVAS`): user clicks Explain, then "Generate Similar" to create semantically similar workflow

**AC-44-5** — E2E test validates Explain → Generate flow on UAT

---

### 8. Home Page — Intro Copy

#### US-45 — Workflow Studio Home Page Intro Copy

> **As a** new user landing on the home page, **I want** clear, professional copy explaining what Workflow Studio does, **so that** I understand the product's value before navigating to applications.

**AC-45-1** — Home page displays heading, subheading, body paragraph (2-3 sentences), and CTA button

**AC-45-2** — Copy professional, concise, avoids jargon

**AC-45-3** — Layout uses Carbon spacing tokens

---

### 9. Brand Identity — Snail Favicon

#### US-46 — Snail Favicon for Browser Tab

> **As a** user with multiple browser tabs open, **I want** Workflow Studio to have a distinctive snail favicon, **so that** I can quickly identify the tab.

**AC-46-1** — Favicon is snail icon (🐌 or custom SVG)

**AC-46-2** — Visible in browser tab, bookmarks, history

**AC-46-3** — Standard sizes/formats generated (16×16, 32×32, 48×48, 64×64, ICO/PNG/SVG)

**AC-46-4** — `index.html` includes correct `<link rel="icon">` tags

**AC-46-5** — Tested in Chrome, Firefox, Safari on desktop and mobile

---

## Success Criteria

Program complete when:
1. All Playwright tests use 5-layer validation
2. All UI matches IBM Carbon (zero non-Carbon gaps)
3. Node editor validates rule keys as single JSONPath
4. Node editor drawer resizable (desktop), opens read-only with Edit button
5. Canvas palette shows one-line descriptions
6. Canvas nodes support Copy and Delete with edge cleanup
7. AI Generator produces semantically correct workflows; Explain → Generate works on UAT
8. Home page displays professional intro copy
9. Snail favicon visible in browser tabs
10. E2E suite passes with zero failures on UAT
11. Single TODO checkbox cleared

---

*End of PM Doc v30.0*
