# Test Doc v30.0 — Workflow Studio Unified Product & UX Program

**Version:** 30.0  
**Date:** 2026-04-12  
**Status:** Draft — awaiting human approval  
**Label:** `TODO-workflow-studio-unified-product-ux-program`

---

## Overview

Test cases for the unified product & UX program. All test cases use the 5-layer UX validation framework.

**Test Environment:**
- **UAT:** https://workflow-ui-gamma.vercel.app
- **Desktop:** Chrome 1280px viewport
- **Mobile:** Chrome 390×844 viewport, `isMobile: true`, `hasTouch: true`
- **E2E Framework:** Playwright with headed browser mode

---

## Test Cases

### 1. E2E Test Quality (3 cases)

**TC-5LAYER-01: Validation Helper Functions Exist**
- Check `workflow-ui/e2e/helpers/five-layer.ts` exists
- Verify exports: `validateLayer1`, `validateLayer2`, `validateLayer3`, `validateLayer4`, `validateLayer5`
- **Expected:** All helper functions exported with correct TypeScript signatures

**TC-5LAYER-02: Retrofit Existing Test Cases**
- Run full E2E suite before retrofit
- Retrofit all test cases to use 5-layer validation
- Run full E2E suite after retrofit
- **Expected:** All tests pass; report shows layers validated per test case

**TC-5LAYER-03: Screenshot Baselines Captured**
- Run Playwright with `--update-snapshots`
- Verify screenshots in `e2e/__screenshots__/`
- Check baselines exist for: app-list-desktop, app-list-mobile, canvas-desktop, canvas-mobile, node-editor-desktop, node-editor-mobile
- **Expected:** Screenshot baselines captured and committed

---

### 2. IBM Carbon Completeness (5 cases)

**TC-CARBON-01: Audit Script Identifies Gaps**
- Run `npm run audit:carbon`
- Review output for identified gaps
- **Expected:** Script outputs numbered list of non-Carbon UI elements with file paths

**TC-CARBON-02: Application Delete Modal Uses Carbon Styling**
- Navigate to `/workflows`
- Click Delete on an application
- Inspect delete confirmation modal
- **Expected:** Modal `border-radius: 0px`, OK button `background-color: rgb(218, 30, 40)`, buttons `border-radius: 0px`

**TC-CARBON-03: All Buttons Have Zero Border Radius**
- Navigate to `/workflows`
- Inspect all buttons (primary, secondary, danger)
- Navigate to canvas and inspect toolbar buttons
- Open node editor and inspect Save/Cancel buttons
- **Expected:** All buttons have `border-radius: 0px` (except status badges at `24px`)

**TC-CARBON-04: Portal Components Use Carbon Colors**
- Hover over tooltip, open dropdown, open modal, open drawer
- **Expected:** All portals use Carbon palette (Gray 100 background, white text, Gray 30 borders)

**TC-CARBON-05: IBM Plex Fonts Applied Globally**
- Navigate to `/workflows`, canvas, node editor
- Inspect text elements (headings, body, code)
- **Expected:** Headings/body use `IBM Plex Sans`, code uses `IBM Plex Mono`

---

### 3. Node Editor Rules (5 cases)

**TC-RULE-01: Valid Single JSONPath Accepted**
- Click node, click Edit, enter `$.customer.id`, blur
- **Expected:** No error, Save button enabled

**TC-RULE-02: Multiple Paths Rejected (Comma)**
- Click node, click Edit, enter `$.customer.id, $.order.total`, blur
- **Expected:** Error "Rule key must be a single JSONPath expression", Save disabled

**TC-RULE-03: Multiple Paths Rejected (Semicolon)**
- Click node, click Edit, enter `$.customer.id; $.order.total`, blur
- **Expected:** Error message shown, Save disabled

**TC-RULE-04: Invalid JSONPath Syntax Rejected**
- Click node, click Edit, enter `$[invalid syntax`, blur
- **Expected:** Error "Invalid JSONPath syntax:", Save disabled

**TC-RULE-05: Empty Rule Key Rejected**
- Click node, click Edit, clear rule key, blur
- **Expected:** Error "Rule key cannot be empty", Save disabled

---

### 4. Node Editor UX (9 cases)

**TC-DRAWER-01: Desktop Drawer is Resizable**
- Desktop 1280px, click node
- Hover left edge, verify resize cursor
- Drag left/right
- **Expected:** Drawer width changes, clamped to 420px–768px

**TC-DRAWER-02: Drawer Width Persisted**
- Resize drawer to 600px, close, reload, open
- **Expected:** Drawer opens at 600px

**TC-DRAWER-03: Drawer Opens in Read-Only Mode**
- Click node
- **Expected:** Edit button visible, fields read-only, no Save/Cancel buttons

**TC-DRAWER-04: Edit Button Switches to Edit Mode**
- Click Edit
- **Expected:** Edit button disappears, Cancel/Save appear, fields editable

**TC-DRAWER-05: Cancel Returns to Read-Only**
- Modify field, click Cancel
- **Expected:** Edit button reappears, changes discarded

**TC-DRAWER-06: Save Persists Changes**
- Modify field, click Save
- **Expected:** Edit button reappears, changes persisted

**TC-DRAWER-07: Long Content Scrolls**
- Node with long JSON, scroll within drawer
- **Expected:** Content scrolls, header fixed

**TC-DRAWER-08: Mobile Drawer Not Resizable**
- Mobile 390×844, click node
- **Expected:** No resize handle, full width

**TC-DRAWER-09: Mobile Uses Read-Only Default**
- Mobile, click node
- **Expected:** Edit button visible, fields read-only

---

### 5. Canvas Palette (5 cases)

**TC-PALETTE-01: Desktop Shows Descriptions**
- Navigate to canvas, expand sidebar
- **Expected:** Each node shows icon, name, description (e.g., "HTTP Fetch: Fetch data from external APIs")

**TC-PALETTE-02: Collapsed Shows Tooltip**
- Collapse sidebar, hover node icon
- **Expected:** Tooltip shows name + description

**TC-PALETTE-03: Mobile Shows Descriptions**
- Mobile, open Add Node sheet
- **Expected:** Each node shows icon, name, description

**TC-PALETTE-04: Drag-Drop Preserved**
- Drag node from palette to canvas
- **Expected:** Node created at drop location

**TC-PALETTE-05: Descriptions Match Spec**
- Verify descriptions:
  - HTTP Fetch: "Fetch data from external APIs"
  - Dispatch: "Send messages to channels"
  - Condition: "Branch based on conditions"
  - Transform: "Transform data with logic"
  - Safe Fetch: "Safe fetch without errors"
  - Transform+: "Advanced data transform"
- **Expected:** All descriptions match

---

### 6. Canvas Node Actions (6 cases)

**TC-NODE-COPY-01: Copy to Clipboard via Context Menu**
- Right-click node, click "Copy to Clipboard"
- **Expected:** Node config copied to clipboard as JSON

**TC-NODE-COPY-02: Copy via Keyboard**
- Select node, press Cmd+C / Ctrl+C
- **Expected:** Node config copied to clipboard

**TC-NODE-COPY-03: Duplicate Node**
- Right-click node, click "Duplicate Node"
- **Expected:** New node at +50px offset with same config

**TC-NODE-DELETE-01: Delete Node with No Connections**
- Right-click isolated node, click Delete
- **Expected:** Node removed immediately, no confirmation

**TC-NODE-DELETE-02: Delete with Connections Shows Confirmation**
- Right-click connected node, click Delete
- **Expected:** Confirmation "This node has X connection(s). Delete anyway?"

**TC-NODE-DELETE-03: Confirm Delete Removes Node and Edges**
- Click Delete in confirmation
- **Expected:** Node and all edges removed

---

### 7. AI Generator (6 cases)

**TC-GEN-01: Enhanced Prompt Includes Examples**
- Inspect `WORKFLOW_GENERATOR_SYSTEM_PROMPT`
- **Expected:** Includes JSON schema, node descriptions, edge semantics, layout guidance

**TC-GEN-02: Generated Workflow Validated**
- Click Generate, enter prompt, wait
- **Expected:** Validation runs before applying; clear error if fails

**TC-GEN-03: Invalid Node Type Rejected**
- Mock response with `"plugin": "InvalidType"`
- **Expected:** Error "Node 0: invalid plugin type 'InvalidType'"

**TC-GEN-04: Orphaned Edge Rejected**
- Mock response with edge to non-existent node
- **Expected:** Error "Edge 0: target 'non-existent' not found"

**TC-GEN-05: Valid Workflow Applied**
- Generate simple workflow
- **Expected:** Workflow appears on canvas, nodes laid out vertically

**TC-GEN-06: Explain → Generate on UAT**
- Navigate to https://workflow-ui-gamma.vercel.app/workflows/E2E_TEST_CANVAS
- Click Explain, wait, click "Generate Similar"
- **Expected:** Generated workflow semantically similar to original

---

### 8. Home Page (3 cases)

**TC-HOME-01: Displays Intro Copy**
- Navigate to `/`
- **Expected:** Heading "Workflow Studio", subheading, body paragraph, CTA button with correct fonts/colors

**TC-HOME-02: CTA Navigates**
- Click "Go to Applications"
- **Expected:** Navigate to `/workflows`

**TC-HOME-03: Copy is Professional**
- Read intro copy
- **Expected:** Professional, concise, avoids jargon

---

### 9. Favicon (7 cases)

**TC-FAVICON-01: Files Exist**
- Check `public/` for favicon files
- **Expected:** `favicon.ico`, `favicon.svg`, `favicon-16x16.png`, `favicon-32x32.png`, `apple-touch-icon.png` exist

**TC-FAVICON-02: HTML Head Tags**
- Inspect `index.html` `<head>`
- **Expected:** All favicon link tags present

**TC-FAVICON-03: Visible in Chrome Desktop**
- Navigate to `/workflows` in Chrome
- **Expected:** Snail icon visible in tab

**TC-FAVICON-04: Visible in Firefox Desktop**
- Navigate to `/workflows` in Firefox
- **Expected:** Snail icon visible in tab

**TC-FAVICON-05: Visible in Safari Desktop**
- Navigate to `/workflows` in Safari
- **Expected:** Snail icon visible in tab

**TC-FAVICON-06: Visible in Chrome Mobile**
- Navigate to `/workflows` in Chrome mobile
- **Expected:** Snail icon visible in tab

**TC-FAVICON-07: Visible in Bookmarks**
- Bookmark page, open bookmarks
- **Expected:** Snail icon visible next to bookmark

---

## Test Summary

| Category | Test Cases | Layers |
|----------|------------|--------|
| 5-Layer Validation | 3 | N/A |
| IBM Carbon | 5 | 1,2,3,4,5 |
| Node Editor Rules | 5 | 1,4,5 |
| Node Editor UX | 9 | 1,2,3,4 |
| Canvas Palette | 5 | 1,2,4,5 |
| Canvas Node Actions | 6 | 1,2,4,5 |
| AI Generator | 6 | 1,4 |
| Home Page | 3 | 1,4,5 |
| Favicon | 7 | 1 |
| **Total** | **49** | **All** |

---

## Pass Criteria

Program passes UAT when:
1. All 49 test cases pass on UAT environment
2. Zero WCAG 2.1 AA violations
3. Zero performance regressions
4. All existing E2E tests continue to pass
5. Manual visual inspection confirms Carbon compliance

---

*End of Test Doc v30.0*
