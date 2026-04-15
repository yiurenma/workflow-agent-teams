# Agent teams to-do

Backlog for agent teams to pick up in order. Check items off as they ship.


## Pending

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


## Completed

- [x] **UI Design — IBM Design Language refactoring** *(label: `TODO-ui-design-ibm-design-language-refactoring`)*

- [x] **UI Design — Quiet Luxury visual refactoring** *(label: `TODO-ui-design-quiet-luxury-refactoring-frontend-skill`)*

- [x] **E2E — node editor drawer close (HIGH)** *(label: `TODO-uat-e2e-node-drawer-close-button-broken`)*

- [x] **E2E — five-layer validation retrofit** *(label: `TODO-uat-e2e-retrofit-5layer-validation`)*

- [x] **Node editor — width, read-first mode, edit gate, long content** *(label: `TODO-node-editor-draggable-width-readonly-default`)*

- [x] **E2E — Fix test failures from full suite run (44 failures)** *(label: `TODO-e2e-fix-test-failures-post-v30`)*

- [x] **Branding — snail favicon (browser tab)** *(label: `TODO-branding-favicon-snail-tab-icon`)*

- [x] **Vercel Build — Missing dayjs dependency causes deployment failure** *(label: `TODO-vercel-build-missing-dayjs-dependency`)*

- [x] **Node Editor — Form field descriptions color contrast** *(label: `TODO-node-editor-form-descriptions-color-contrast`)*

- [x] **UAT E2E — Accessibility violations in node editor drawer** *(label: `TODO-uat-e2e-node-drawer-accessibility-violations`)*

- [x] **Canvas AI / workflow explainer (Funlane-style)**

- [x] **Explain: no GitHub token → send user to GitHub to authorize**

- [x] **Post-mortem & process: Explain — device-flow token `gho_` rejected** *(label: `TODO-postmortem-explain-github-gho-token-validation`)*

- [x] **Post-mortem & process: Explain + GitHub OAuth device flow (CORS / same-origin proxy PR)** *(label: `TODO-postmortem-explain-github-oauth-cors-uat`)*

- [x] **Artboard node editor — three-panel layout + smart text formatting** *(label: `TODO-artboard-node-editor-3panel-json-format`)*

- [x] **Node editor — cross-audience clarity (dev / QA / business)** *(label: `TODO-node-editor-cross-audience-clarity`)*

- [x] **Application + Record tables — no vertical scroll, missing totals & translation** *(label: `TODO-application-record-tables-scroll-count-translation`)*

- [x] **Explain (GitHub Copilot path) — richer prompt hints + Markdown-friendly response UI** *(label: `TODO-explain-github-copilot-prompt-markdown-ui`)*

- [x] **Q2 — Mobile (three items)** *(label: `TODO-q2-mobile-apps-actions-desktop-entry-constraints`)*

- [x] **Hosting — point operation + online API to DigitalOcean URLs** *(label: `TODO-hosting-digitalocean-urls`)*

- [x] **CI/CD — remove Render deploy GitHub Actions from both API repos** *(label: `TODO-cicd-remove-render-github-actions`)*

- [x] **Mobile — add-application FAB (+) should be draggable** *(label: `TODO-mobile-add-application-fab-draggable`)*

- [x] **Canvas — straight-line workflow layout (recovery)** *(label: `TODO-canvas-straight-line-workflow-recovery`)*

- [x] **Applications — allow editing application name in the application table** *(label: `TODO-application-table-edit-name`)*

- [x] **Application name — persist and show `description` (long text)** *(label: `TODO-application-name-description-db-text-ui-api`)*

- [x] **Mobile — node editor (drawing book) shows large blank area below content** *(label: `TODO-mobile-node-editor-blank-space-below`)*

- [x] **Canvas AI — workflow generator (beside Explain) — Copilot JSON replaces canvas** *(label: `TODO-canvas-ai-workflow-json-copilot-replace`)*

- [x] **Mobile — application overflow menu (History / Delete / Copy) navigates to Workflow instead of opening flows** *(label: `TODO-mobile-app-overflow-history-copy-modal-not-navigation`)*

- [x] **Canvas — workflow data undefined causing crash** *(label: `TODO-canvas-workflow-undefined-crash`)*

- [x] **Mobile — Records pagination not visible on narrow viewport** *(label: `TODO-mobile-records-pagination-invisible`)*

- [x] **Mobile / phone viewport — workflow canvas "Add node" (+) FAB should be draggable** *(label: `TODO-mobile-canvas-add-node-fab-draggable`)*

- [x] **Canvas — JsonPath playground (validate path + preview value)** *(label: `TODO-canvas-json-path-verification-modal`)*

- [x] **E2E (Playwright) — pass 2: full regression after other Open items** *(label: `TODO-e2e-playwright-pass2-full-regression-after-backlog`)*
