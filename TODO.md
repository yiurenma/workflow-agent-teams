# Agent teams to-do

Backlog for agent teams to pick up in order. Check items off as they ship.

## Open

### Completed Items

- [x] **UI Design — IBM Design Language refactoring (requires /ibm-design skill)** — 按照 IBM Design Language 规范对 Workflow Studio 整体 UI 进行重新设计。使用 `/ibm-design` skill 驱动实现，覆盖排版、色彩、间距、组件风格（按钮、表格、表单、抽屉、导航）等全局视觉系统。目标：从当前 Quiet Luxury 风格迁移至 IBM Carbon Design System 风格，保持所有功能不变。**Status:** ✅ DONE — UAT PASS 2026-04-12. IBM Plex Sans/Mono fonts. Blue 60 #0f62fe primary. Gray 100 #161616 nav. 0px border-radius. Carbon token system (--cds-*). 82/84 E2E pass (1 pre-existing drawer close defect). Docs: `pm-doc-v28.0.md`, `arch-doc-v28.0.md`, `test-doc-v28.0.md`, `ui-test-report-v28.0.md`, `e2e-test-report-v28.0.md`, `uat-report-v28.0.md`. **Label:** `TODO-ui-design-ibm-design-language-refactoring`.

- [x] **UI Design — Quiet Luxury visual refactoring (HIGH priority, requires /frontend-design skill)** — Refactor Workflow Studio UI from generic tech aesthetic to sophisticated "Quiet Luxury" design system. **Status:** ✅ DONE — UAT PASS 2026-04-11. Fonts: Lora + DM Sans. Primary: terracotta #7C4A3A. Header: warm charcoal #2A2520 with gold #C9A87C. Background: cream #F9F7F4. Active tags: mint #EAF3EE. Table header: warm sand #F3F0EB. All functional regressions clean. Docs: `pm-doc-v27.0.md`, `arch-doc-v27.0.md`, `test-doc-v27.0.md`, `ui-test-report-v27.0.md`, `e2e-test-report-v27.0.md`, `uat-report-v27.0.md`. **Label:** `TODO-ui-design-quiet-luxury-refactoring-frontend-skill`.

- [x] **E2E — node editor drawer close (HIGH)** *(label: `TODO-uat-e2e-node-drawer-close-button-broken`)* ✅ DONE 2026-04-12
  - Status: ✅ COMPLETE — UAT PASS on gamma environment
  - Docs: `pm-doc-v29.0.md`, `arch-doc-v29.0.md`, `test-doc-v29.0.md`, `ui-test-report-v29.0.md`, `e2e-test-report-v29.0.md`, `uat-report-v29.0.md`
  - Commit: `workflow-ui@cb5465f` - "fix: node editor drawer close button not working"

- [x] **E2E — five-layer validation retrofit** *(label: `TODO-uat-e2e-retrofit-5layer-validation`)* ✅ DONE 2026-04-12
  - Status: ✅ COMPLETE — 11 test cases upgraded with Layer 2/3/5 validation
  - Docs: `pm-doc-v30.0.md`, `arch-doc-v30.0.md`, `test-doc-v30.0.md`, `e2e-test-report-v30.0.md`
  - Commit: `workflow-ui@a4f3a70` - "test: retrofit E2E tests with five-layer validation framework"

- [x] **Node editor — width, read-first mode, edit gate, long content** *(label: `TODO-node-editor-draggable-width-readonly-default`)* ✅ DONE 2026-04-12
  - Status: ✅ COMPLETE — NodeView component created, Edit/Done/Cancel buttons added, resizable drawer already implemented
  - Docs: `pm-doc-v31.0.md`, `arch-doc-v31.0.md`, `test-doc-v31.0.md`
  - Commit: `workflow-ui@1d4fe74` - "feat: add read-first mode and edit gate to node editor"

- [x] **E2E — Fix test failures from full suite run (44 failures)** *(label: `TODO-e2e-fix-test-failures-post-v30`)* ✅ DONE 2026-04-13
  - Status: ✅ COMPLETE — UAT PASS 105/106 (1 skipped) on https://workflow-ui-gamma.vercel.app
  - Docs: `pm-doc-v33.0.md`, `arch-doc-v33.0.md`, `test-doc-v33.0.md`, `e2e-test-report-v34.0.md`
  - Final pass rate: 105/106 (99.1%, 1 viewport-mismatch skip)

- [x] **Branding — snail favicon (browser tab)** *(label: `TODO-branding-favicon-snail-tab-icon`)* ✅ DONE 2026-04-12
  - Status: ✅ COMPLETE — Snail icon created in SVG format with Carbon Blue 60
  - Docs: `pm-doc-v32.0.md`, `arch-doc-v32.0.md`, `test-doc-v32.0.md`
  - Commit: `workflow-ui@8b3fe9a` - "feat: add snail favicon for browser tab branding"

### Pending Items

- [ ] **UI parity — IBM Carbon residual styling + Playwright blind spots** *(label: `TODO-ui-ibm-carbon-audit-residual-styling`)*
  - Context: v28.0 IBM refactor shipped but some surfaces still look like legacy Ant/Quiet Luxury
  - Action: Audit and produce numbered gap list for Applications, Records, canvas, toasts, empty states, Modal.confirm paths
  - Fix: Apply --cds-* tokens, Carbon modal/danger button patterns
  - Done when: Gap list cleared and spot-check UAT passed

- [ ] **Node editor — rule key must be one JSONPath** *(label: `TODO-node-editor-rule-key-json-path-validation`)*
  - Requirement: Each rule key field must contain exactly one valid JSONPath expression
  - UX: Validate on blur and/or before save; blocking error prevents save if invalid
  - Done when: Happy + invalid paths covered in test-doc/Playwright

- [ ] **Node editor — palette type descriptions** *(label: `TODO-node-editor-palette-type-descriptions-tooltips`)*
  - Interaction: Every node type in left palette can be dragged onto canvas
  - Education: Each type shows one-line description and optional tooltip
  - Done when: All registered palette/plugin types have descriptions

- [ ] **Canvas — copy & delete selected node** *(label: `TODO-canvas-node-copy-full-config-delete`)*
  - Copy: Include description, ruleList, action/plugin config; modes: JSON to clipboard or duplicate on canvas
  - Delete: Remove node and all incident edges
  - Placement: Drawer header and/or node context menu
  - Done when: Documented + tested desktop and mobile paths

- [ ] **AI — workflow generator semantic quality** *(label: `TODO-canvas-ai-workflow-generator-quality-prompt-semantics`)*
  - Gap: Model output often parses but semantics are wrong (plugin ids, action kinds, rule keys, types, broken edges)
  - Measurement: Use E2E_TEST_CANVAS for comparison loop
  - Fix: Stronger system prompt, post-parse validation, visual baselines
  - Done when: Failure patterns documented and prompt iterated

- [ ] **Home — Workflow Studio introduction** *(label: `TODO-home-workflow-studio-intro-copy`)*
  - Placement: First/Home screen on applications list
  - Content: Product description, who it's for, benefits, feature bullets
  - Deliverable: Final strings + accessible contrast
  - Done when: Content finalized and implemented

- [ ] **Canvas — import workflow from JSON (toolbar; validate then apply)** *(label: `TODO-canvas-import-workflow-json-validate-apply`)*
  - Separate from AI Generate: Add canvas header action for paste/upload JSON workflow
  - Flow: Parse → Validate schema → Show errors → Preview → Apply with confirm
  - Scope: workflow-ui only, client-side validation
  - Done when: Modal implemented and tested

- [ ] **UI/UX — Search box usability issues** *(label: `TODO-ui-ux-search-box-usability`)*
  - Severity: Medium
  - Status: Identified during UAT testing
  - Done when: Search box UI/UX improved and tested

- [ ] **UI/UX — Red buttons lack text labels** *(label: `TODO-ui-ux-red-buttons-no-labels`)*
  - Severity: Medium
  - Recommendation: Add explicit text labels or icon+tooltip
  - Done when: All red buttons have clear, user-friendly labels

- [ ] **UI/UX — Settings button unstable** *(label: `TODO-ui-ux-settings-button-unstable`)*
  - Severity: Low
  - Status: Intermittent issue during automated testing
  - Done when: Settings button interaction stable and reliable

- [ ] **UI/UX — Mobile drawer scroll bounds overflow** *(label: `TODO-ui-ux-mobile-drawer-scroll-bounds-overflow`)*
  - Severity: Medium
  - Test Evidence: TC-DRAWER-CLOSE-03 failed
  - Done when: Drawer content stays within viewport bounds on all mobile devices

## Done

- [x] **Vercel Build — Missing dayjs dependency causes deployment failure** ✅ COMPLETE
  - Label: `TODO-vercel-build-missing-dayjs-dependency`

- [x] **Node Editor — Form field descriptions color contrast** ✅ COMPLETE
  - Label: `TODO-node-editor-form-descriptions-color-contrast`

- [x] **UAT E2E — Accessibility violations in node editor drawer** ✅ COMPLETE
  - Label: `TODO-uat-e2e-node-drawer-accessibility-violations`

- [x] **Canvas AI / workflow explainer (Funlane-style)** ✅ COMPLETE

- [x] **Explain: no GitHub token → send user to GitHub to authorize** ✅ COMPLETE

- [x] **Post-mortem & process: Explain — device-flow token `gho_` rejected** ✅ COMPLETE
  - Label: `TODO-postmortem-explain-github-gho-token-validation`

- [x] **Post-mortem & process: Explain + GitHub OAuth device flow (CORS / same-origin proxy PR)** ✅ COMPLETE
  - Label: `TODO-postmortem-explain-github-oauth-cors-uat`

- [x] **Artboard node editor — three-panel layout + smart text formatting** ✅ COMPLETE
  - Label: `TODO-artboard-node-editor-3panel-json-format`

- [x] **Node editor — cross-audience clarity (dev / QA / business)** ✅ COMPLETE
  - Label: `TODO-node-editor-cross-audience-clarity`

- [x] **Application + Record tables — no vertical scroll, missing totals & translation** ✅ COMPLETE
  - Label: `TODO-application-record-tables-scroll-count-translation`

- [x] **Explain (GitHub Copilot path) — richer prompt hints + Markdown-friendly response UI** ✅ COMPLETE
  - Label: `TODO-explain-github-copilot-prompt-markdown-ui`

- [x] **Q2 — Mobile (three items)** ✅ COMPLETE
  - Label: `TODO-q2-mobile-apps-actions-desktop-entry-constraints`

- [x] **Hosting — point operation + online API to DigitalOcean URLs** ✅ COMPLETE
  - Label: `TODO-hosting-digitalocean-urls`

- [x] **CI/CD — remove Render deploy GitHub Actions from both API repos** ✅ COMPLETE
  - Label: `TODO-cic
d-remove-render-github-actions`

- [x] **Mobile — add-application FAB (+) should be draggable** ✅ COMPLETE
  - Label: `TODO-mobile-add-application-fab-draggable`

- [x] **Canvas — straight-line workflow layout (recovery)** ✅ COMPLETE
  - Label: `TODO-canvas-straight-line-workflow-recovery`

- [x] **Applications — allow editing application name in the application table** ✅ COMPLETE
  - Label: `TODO-application-table-edit-name`

- [x] **Application name — persist and show `description` (long text)** ✅ COMPLETE
  - Label: `TODO-application-name-description-db-text-ui-api`

- [x] **Mobile — node editor (drawing book) shows large blank area below content** ✅ COMPLETE
  - Label: `TODO-mobile-node-editor-blank-space-below`

- [x] **Canvas AI — workflow generator (beside Explain) — Copilot JSON replaces canvas** ✅ COMPLETE
  - Label: `TODO-canvas-ai-workflow-json-copilot-replace`

- [x] **Mobile — application overflow menu (History / Delete / Copy) navigates to Workflow instead of opening flows** ✅ COMPLETE
  - Label: `TODO-mobile-app-overflow-history-copy-modal-not-navigation`

- [x] **Canvas — workflow data undefined causing crash** ✅ COMPLETE
  - Label: `TODO-canvas-workflow-undefined-crash`

- [x] **Mobile — Records pagination not visible on narrow viewport** ✅ COMPLETE
  - Label: `TODO-mobile-records-pagination-invisible`

- [x] **Mobile / phone viewport — workflow canvas "Add node" (+) FAB should be draggable** ✅ COMPLETE
  - Label: `TODO-mobile-canvas-add-node-fab-draggable`

- [x] **Canvas — JsonPath playground (validate path + preview value)** ✅ COMPLETE
  - Label: `TODO-canvas-json-path-verification-modal`

- [x] **E2E (Playwright) — pass 2: full regression after other Open items** ✅ COMPLETE
  - Label: `TODO-e2e-playwright-pass2-full-regression-after-backlog`
