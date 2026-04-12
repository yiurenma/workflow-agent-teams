# Agent teams to-do

Backlog for agent teams to pick up in order. Check items off as they ship.

## Open

- [x] **UI Design — IBM Design Language refactoring (requires /ibm-design skill)** — 按照 IBM Design Language 规范对 Workflow Studio 整体 UI 进行重新设计。使用 `/ibm-design` skill 驱动实现，覆盖排版、色彩、间距、组件风格（按钮、表格、表单、抽屉、导航）等全局视觉系统。目标：从当前 Quiet Luxury 风格迁移至 IBM Carbon Design System 风格，保持所有功能不变。**Status:** ✅ DONE — UAT PASS 2026-04-12. IBM Plex Sans/Mono fonts. Blue 60 #0f62fe primary. Gray 100 #161616 nav. 0px border-radius. Carbon token system (--cds-*). 82/84 E2E pass (1 pre-existing drawer close defect). Docs: `pm-doc-v28.0.md`, `arch-doc-v28.0.md`, `test-doc-v28.0.md`, `ui-test-report-v28.0.md`, `e2e-test-report-v28.0.md`, `uat-report-v28.0.md`. **Label:** `TODO-ui-design-ibm-design-language-refactoring`.

- [x] **UI Design — Quiet Luxury visual refactoring (HIGH priority, requires /frontend-design skill)** — Refactor Workflow Studio UI from generic tech aesthetic to sophisticated "Quiet Luxury" design system. **Status:** ✅ DONE — UAT PASS 2026-04-11. Fonts: Lora + DM Sans. Primary: terracotta #7C4A3A. Header: warm charcoal #2A2520 with gold #C9A87C. Background: cream #F9F7F4. Active tags: mint #EAF3EE. Table header: warm sand #F3F0EB. All functional regressions clean. Docs: `pm-doc-v27.0.md`, `arch-doc-v27.0.md`, `test-doc-v27.0.md`, `ui-test-report-v27.0.md`, `e2e-test-report-v27.0.md`, `uat-report-v27.0.md`. **Label:** `TODO-ui-design-quiet-luxury-refactoring-frontend-skill`.

- [ ] **Workflow Studio — unified product & UX program (single open item)**

  **Master label (use for epic / tracking):** `TODO-workflow-studio-unified-product-ux-program`

  **Intent:** Ship everything below as **one coordinated program** in **`workflow-ui`** (plus Playwright, QA/UAT docs, and `pm-doc-*` / `arch-doc-*` / `test-doc-*` per `CLAUDE.md`). **There is still only this one Open checkbox** — do not split the backlog into multiple top-level `[ ]` lines; use the **historical labels** in parentheses only for commits, PR titles, or sub-issues. **Mark this item Done** only when **every subsection** below is implemented, tested on UAT (`https://workflow-ui-gamma.vercel.app`), and documented.

  **Cross-cutting process:** PM / Arch / Test docs and human approval gates still apply where the org requires them. **Regression bar:** Existing E2E cases (e.g. in `test-doc-v28.0.md`, `e2e-test-report-v29.0.md`) must stay green unless explicitly superseded.

  ---

  **1) E2E — node editor drawer close (HIGH)** *(label: `TODO-uat-e2e-node-drawer-close-button-broken`)*

  - **Symptom:** Clicking **`.ant-drawer-close`** does **not** close the node editor drawer; drawer stays open; user must click outside or ESC (ESC not available on mobile).
  - **Evidence:** TC-NODE-ENHANCED-05 (Layer 5), `specs/uat-report-e2e-pass3.md`.
  - **Expected:** One click on the drawer close control **always** closes the drawer and returns focus to the canvas; works on **Desktop Chrome 1280** and **Mobile Chrome 390×844**.
  - **Investigation hints:** Event handler not bound, stopped propagation, Ant Design `destroyOnClose` / `open` state mismatch, z-index or portal overlay blocking hit target, ReactFlow capturing pointer.
  - **Done when:** Fix merged; new or updated Playwright case proves close **effect** (drawer absent from DOM or not visible + canvas interactive); UAT note updated.

  ---

  **2) E2E — five-layer validation retrofit** *(label: `TODO-uat-e2e-retrofit-5layer-validation`)*

  - **Problem:** Suite can be 100% pass while **missing** viewport, size, interaction, and **visual/style** guarantees.
  - **Layers (enforce in specs):** (1) **Exist** — element in DOM; (2) **Size** — e.g. `boundingBox()`, drawer height vs viewport (e.g. >35% where spec requires); (3) **Viewport** — `toBeInViewport()` / not clipped; (4) **Interact** — real click/type, not only `toBeVisible()`; (5) **Effect** — post-condition + **computed styles**, tokens, or **`toHaveScreenshot()`** where stable.
  - **Scope:** Prioritize mobile drawers/modals, forms, node editor, app overflow menus, critical modals (Delete confirm, Explain, Generate, JsonPath).
  - **References:** `specs/uat-report-e2e-pass3.md`, Appendix A template if present.
  - **Done when:** Documented targets hit; representative specs upgraded; report in `e2e-test-report-v*.md`.

  ---

  **3) UI parity — IBM Carbon residual styling + Playwright blind spots** *(label: `TODO-ui-ibm-carbon-audit-residual-styling`)*

  - **Context:** v28.0 IBM refactor shipped (`TODO-ui-design-ibm-design-language-refactoring`, `arch-doc-v28.0.md`); some surfaces still look like **legacy Ant / Quiet Luxury** (wrong radius, colors, button variant).
  - **Reported example:** **Delete** flow for an **application** (table row or overflow) — confirm dialog or actions **not** Carbon-aligned.
  - **Audit:** Produce a **numbered gap list**: route, screen, component file, screenshot, severity. Cover **Applications** (table, mobile cards, Settings, History, Copy, Delete), **Records**, **canvas** (toolbar, `WorkflowDrawer`, JsonPath / Explain / Generate modals), **toasts**, **empty states**, any **`Modal.confirm` / `Popconfirm`** paths.
  - **Typical root causes:** Imperative **`Modal.confirm`** bypassing themed wrapper; **Dropdown / Tooltip / Popconfirm** in portal without Carbon `className`; **residual Tailwind** or inline styles; path **missed** in v28 sweep; feature added after v28 without Carbon review.
  - **Fix direction:** Apply **`--cds-*` tokens**, Carbon **modal** / **danger** button patterns; prefer **declarative** `<Modal>` where needed; align typography and spacing to Carbon.
  - **Playwright / “why didn’t we catch it?”:** E2E does **not** infer design systems — it fails only on **asserted** behavior or visuals. If tests never **open** Delete confirm, or only assert `toBeVisible()` with no **Layer 5** (computed color, border-radius, Carbon class substring, or screenshot), **pass ≠ Carbon compliance**. **Deliverable:** list which v28-era tests lacked assertions; add **per-modal** checks or **visual baselines** for critical dialogs.
  - **Done when:** Gap list cleared; spot-check UAT; optional new UI test report version.

  ---

  **4) Node editor — rule key must be one JSONPath** *(label: `TODO-node-editor-rule-key-json-path-validation`)*

  - **Requirement:** Each **rule key** field must contain **exactly one valid JSONPath** expression (syntax and product rules per Arch — e.g. no comma-separated paths, no free text).
  - **UX:** Validate on **blur** and/or **before save**; blocking error prevents save if invalid.
  - **Copy:** User-facing message must **explicitly** say the field accepts **only a single JSONPath** (not generic “invalid”).
  - **Done when:** Happy + invalid paths covered in `test-doc` / Playwright if applicable.

  ---

  **5) Node editor — width, read-first mode, edit gate, long content** *(label: `TODO-node-editor-draggable-width-readonly-default`)*

  - **Resizable drawer:** User can **drag the inner vertical edge** toward the canvas to **widen** the panel; respect **min/max** width; persist optional (session or `localStorage`). **Mobile:** touch drag handle or documented deferral.
  - **Default = view:** On first open after selecting a node, show **read-only** rendering: formatted text / pretty JSON **without** input boxes so users can **skim full content** quickly.
  - **Edit:** Prominent control at **top** (e.g. **Edit**) switches to existing **form** mode; **Done / Cancel** behavior for drafts per Arch (discard vs keep local state).
  - **Long content:** Inner body **scrolls**; content **reflows** when panel widens; avoid silent truncation; virtualization / “Show more” only if performance requires.
  - **Coordination:** If drawer chrome changes overlap with **§1**, implement in one pass to avoid regressions.

  ---

  **6) Canvas — left palette: drag-and-drop + descriptions** *(label: `TODO-canvas-node-palette-descriptions-drag-drop`)*

  - **Interaction:** Every node type in the **left palette** can be **dragged** onto the canvas (parity with expectations; align copy with **mobile “Add node”** sheet).
  - **Education:** Each type shows a **one-line description** (subtitle under title) and optional **tooltip** / “?” for a second sentence.
  - **Example copy direction (PM + eng to validate vs runtime):** **HTTP fetch** — performs an HTTP request and brings response data into the workflow; **Safe HTTP fetch** — same, but **HTTP failure does not fail the entire workflow** (exact semantics in Arch).
  - **Done when:** All registered palette / plugin types have descriptions; mobile lists show the **same** text.

  ---

  **7) Canvas — copy & delete selected node** *(label: `TODO-canvas-node-copy-full-config-delete`)*

  - **Copy — full payload:** Include **description**, **ruleList**, **action** / plugin config, and any other persisted fields for that node in `WorkFlow`.
  - **Modes:** (A) **Copy JSON to clipboard** for external use; (B) **Duplicate on canvas** — new **node id**, **offset** position, same config; **edge policy** explicit (e.g. no edges by default, or PM-specified).
  - **Delete:** Remove node from graph; **remove all incident edges** (no dangling handles).
  - **Placement:** Primary actions in **drawer header** and/or **node context menu** (right-click / long-press); optional keyboard shortcuts; **Delete** confirm when config or connectivity is non-trivial (PM sets threshold).
  - **Done when:** Documented + tested desktop and mobile overflow/drawer paths.

  ---

  **8) AI — workflow generator semantic quality** *(label: `TODO-canvas-ai-workflow-generator-quality-prompt-semantics`)*

  - **Baseline shipped:** v23.0 `WorkflowGeneratorModal.tsx`, `WORKFLOW_GENERATOR_SYSTEM_PROMPT` (`TODO-canvas-ai-workflow-json-copilot-replace`).
  - **Gap:** Model output often **parses** and **renders** but **semantics** are wrong: plugin ids, action kinds, **rule keys** (JSONPath), rule **types**, broken edges.
  - **Measurement loop (UAT):** App **`E2E_TEST_CANVAS`** — open `https://workflow-ui-gamma.vercel.app/workflows/E2E_TEST_CANVAS`; run **Explain** on reference workflow; paste explanation into **Generate**; compare output to golden workflow (open nodes, inspect rules/actions). Document failure **patterns** with screenshots + JSON snippets for prompt iteration.
  - **Fix levers:** Stronger **system prompt** with canonical examples; enumerated allowed shapes; post-parse **validation** + user-visible repair; extend Playwright beyond TC-GENERATOR-01/02 if feasible.
  - **Refs:** `docs/e2e-testing-guide.md`, `docs/e2e-full-test-guide.md`, `pm-doc-v23.0.md`, `arch-doc-v23.0.md`.

  ---

  **9) Home — Workflow Studio introduction** *(label: `TODO-home-workflow-studio-intro-copy`)*

  - **Placement:** First / **Home** screen where users land on **applications** list.
  - **Content:** What the product **is**, **who** it is for, **benefits** (speed vs code-only integration, shared business/engineering view, rules as guardrails, records as observability), and **feature bullets**: applications, canvas + nodes + rules/JSONPath, run, records, Explain, Generate, JsonPath tool, mobile experience.
  - **Deliverable:** Final strings + optional layout note (hero, 2–3 benefits, feature list); accessible contrast (Carbon).

  ---

  **10) Branding — snail favicon (browser tab)** *(label: `TODO-branding-favicon-snail-tab-icon`)*

  - **Deliverable:** Snail icon — **SVG** + **`favicon.ico`** / PNG sizes for legacy browsers; wire via `index.html` `<link rel="icon" …>`; add **`apple-touch-icon`** and **PWA manifest** icons if the app declares them.
  - **Quality:** Readable at **16×16** and **32×32**; align with Carbon neutrals or document intentional brand exception.

  ---

  **Consolidated acceptance (program complete):** All of §1–§10 satisfied; UAT PASS on gamma for touched flows; `TODO.md` this item marked `[x]`; submodule / monorepo pointers and docs updated per org rules.

- [x] **E2E (Playwright) — pass 1: write cases + first full run** — 33 test cases across 7 spec files (navigation, desktop apps, mobile apps, canvas, node editor, explain, records). Two Playwright projects: Desktop Chrome (1280 px) and Mobile Chrome (390 × 844, `isMobile: true`, `hasTouch: true`). All tests use `page.route()` mocks — no real backend required. Final run: **47 pass, 19 skip (viewport-mismatch), 0 fail** across 66 total runs. Docs: `ui-test-report-v12.0.md`. **Label:** `TODO-e2e-playwright-pass1-author-full-run`.

- [x] **Mobile — application overflow menu (History / Delete / Copy) navigates to Workflow instead of opening flows** — Root cause: `onClick={navigate}` was on the outer card `<div>`, causing Ant Design Dropdown portal teardown to ghost-click through to the navigation handler. Fix: moved `onClick={navigate}` to the inner info `<div>` only; actions zone has no navigate handler. All TC-APP-MOB-05/06/07/08 now pass. Docs: `ui-test-report-v11.0.md`, `uat-report-v11.0.md`. **Label:** `TODO-mobile-app-overflow-history-copy-modal-not-navigation`.

- [x] **Canvas — workflow data undefined causing crash** — **Bug:** Canvas component crashes with “Cannot read properties of undefined (reading 'pluginList')” when loading an application. **Root cause:** Canvas expects `workflow.pluginList` but receives undefined. **Fix:** Wrapped `workFlowToNodesAndEdges()` in try/catch in `useWorkflowState.ts`; on error logs and renders empty canvas. Docs: `pm/arch/test-doc-v13.0.md`, `ui-test-report-v13.0.md`. **Recognition label:** `TODO-canvas-workflow-undefined-crash`.

- [x] **Mobile — Records pagination not visible on narrow viewport** — **Bug:** Pagination component (`.ant-pagination`) is not visible on mobile viewport in Records list. **Fix:** Replaced custom Prev/Next block with Ant Design `<Pagination size=”small”>` in mobile branch of `records/index.tsx`. TC-REC-03 now passes on both Desktop + Mobile. Docs: `pm/arch/test-doc-v14.0.md`, `ui-test-report-v14.0.md`. **Recognition label:** `TODO-mobile-records-pagination-invisible`.

- [x] **Mobile / phone viewport — workflow canvas “Add node” (+) FAB should be draggable (parity with applications list)** — **Fix:** Added pointer-event drag with edge-snap and localStorage persistence (`workflow_canvas_fab_pos`) to `MobileAddNodeSheet.tsx`. Tap < 5 px still opens Add Node sheet. New spec `canvas-mobile.spec.ts` with TC-CANVAS-MOB-01..05. Docs: `pm/arch/test-doc-v15.0.md`, `ui-test-report-v15.0.md`. **Recognition label for agents:** `TODO-mobile-canvas-add-node-fab-draggable`.

- [x] **Canvas — JsonPath playground (validate path + preview value)** — **Fix:** New `JsonPathModal.tsx` using `jsonpath-plus` (client-side). “JsonPath” button added to `WorkflowHeader` toolbar. TC-JSONPATH-01..05 in `canvas.spec.ts`. Docs: `pm/arch/test-doc-v16.0.md`, `ui-test-report-v16.0.md`. **Recognition label for agents:** `TODO-canvas-json-path-verification-modal`.

- [x] **E2E (Playwright) — pass 2: full regression after other Open items** — Full suite run after v13.0–v16.0 shipped. **Result: 64 pass / 24 skip (viewport-mismatch) / 0 fail across 88 total runs.** No regressions. 17 new test cases added (TC-CANVAS-04, TC-CANVAS-MOB-01..05, TC-JSONPATH-01..05). Docs: `ui-test-report-v17.0.md`. **Recognition label:** `TODO-e2e-playwright-pass2-full-regression-after-backlog`.

- [x] **Canvas — straight-line workflow layout (recovery)** — New **canvas** capability: a **button** (or clear control) that lays out the workflow as a **single vertical line** from **top to bottom** so users who feel **lost** on a tangled artboard can **reset** to a readable linear flow in one action. **Scope:** `workflow-ui` canvas / workflow editor. **Fix (v19.0):** `StraightenButton` component added inside `<ReactFlow>` using `<Panel position="top-right">`; `straightenNodes()` sorts nodes by Y and resets X=300. TC-CANVAS-05 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label for agents:** `TODO-canvas-straight-line-workflow-recovery`.

- [x] **Applications — allow editing application name in the application table** — **Backend:** operation API already supports updating the application record (including name); **no API work** unless discovery finds a gap. **Frontend (`workflow-ui`):** expose rename in the applications table (inline edit, row action, or modal — follow existing patterns), call the existing update API, handle validation and errors. **Fix (v20.0):** Added `newApplicationName` field to `EntitySettingPatchRequest` DTO and rename logic in `WorkflowEntitySettingController`. Settings modal shows pre-filled Application Name input. TC-APP-DESK-10 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label:** `TODO-application-table-edit-name`.

- [x] **Application name — persist and show `description` (long text)** — Product expects every application name to have a **description** of what the application does; the **UI currently shows no value** because the **database table has no column**. **Fix (v21.0):** Flyway `V1__add_description_to_workflow_entity_setting.sql` adds nullable TEXT `description` to entity and audit tables. `WorkflowEntitySetting`, `EntitySettingPatchRequest`, and controller updated. Frontend shows `description` in table column and mobile card; Settings modal includes Description textarea. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label for agents:** `TODO-application-name-description-db-text-ui-api`.

- [x] **Mobile — node editor (drawing book) shows large blank area below content** — On **phone** (reported on **iPhone X–class** viewports), after **tapping a node** on the **workflow canvas / drawing book**, the **node editor** (drawer / panel) often shows a **prominent empty region below** the real fields. **Fix (v22.0):** `WorkflowDrawer` switches to `placement="bottom"`, `height="auto"`, `maxHeight: 80dvh`, `env(safe-area-inset-bottom)` padding on mobile. Desktop drawer unchanged. TC-CANVAS-MOB-09 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label for agents:** `TODO-mobile-node-editor-blank-space-below`.

- [x] **Canvas AI — workflow generator (beside Explain) — Copilot JSON replaces canvas** — **Product goal:** A **UI action** (next to **Explain**) lets the user describe **what they need**; **GitHub Copilot** returns a **single JSON string** that matches the app’s **workflow / `WorkFlow` shape** so the UI can **build or replace the canvas** from that JSON. **Fix (v23.0):** `WorkflowGeneratorModal.tsx` created with `WORKFLOW_GENERATOR_SYSTEM_PROMPT` constant. Uses `max_tokens: 4096`. Strips markdown fences from response. Generate button added to desktop toolbar and mobile ⋯ dropdown. TC-GENERATOR-01/02 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label:** `TODO-canvas-ai-workflow-json-copilot-replace`.

## Done

- [x] **Vercel Build — Missing dayjs dependency causes deployment failure** — Vercel build fails with "Rollup failed to resolve import 'dayjs' from src/routes/records/index.tsx". **Root cause:** `dayjs` and `json5` were imported in code but not declared in `package.json` dependencies. **Fix:** Added `dayjs@^1.11.13` and `json5@^2.2.3` to dependencies in `package.json` and updated `pnpm-lock.yaml`. Local build verified successful. Vercel deployment now succeeds. **Status:** ✅ COMPLETE - Vercel deployment successful on 2026-04-11. Docs: `docs/arch-doc-v26.0.md`, `docs/test-doc-v26.0.md`, `docs/ui-test-report-v26.0.md`. **Label:** `TODO-vercel-build-missing-dayjs-dependency`.

- [x] **Node Editor — Form field descriptions color contrast** — Form field description text in HttpCallForm and LogicForm had insufficient color contrast (2.62:1 on white background, 2.51:1 on #fafafa background). **Fix:** Changed `text-zinc-400` to `text-zinc-600` in NodeSection.tsx subtitle rendering, improving contrast to 7.0:1 (white) and 6.7:1 (#fafafa), exceeding WCAG 2.1 Level AA 4.5:1 minimum. Affects 3 description paragraphs: "The name shown on the canvas...", "Run only when...", "What the system does...". **Status:** ✅ COMPLETE - UAT PASS on 2026-04-11. Docs: `docs/pm-doc-v25.0.md`, `docs/arch-doc-v25.0.md`, `docs/test-doc-v25.0.md`, `docs/ui-test-report-v25.0.md`, `docs/uat-report-v25.0.md`. **Label:** `TODO-node-editor-form-descriptions-color-contrast`.

- [x] **UAT E2E — Accessibility violations in node editor drawer** — Node editor drawer (`.ant-drawer`) had 2 WCAG violations: (1) Missing `aria-label` on `role="dialog"` element (WCAG 2.1 Level A); (2) Insufficient color contrast 2.51:1 on "NODE CONFIGURATION" header text. **Fix:** Added `aria-label="Node Configuration"` to drawer and changed header text color to text-zinc-600 (#52525b, 7.0:1 contrast). **Status:** ✅ COMPLETE - UAT PASS on 2026-04-11 18:55. Docs: `specs/uat-report-e2e-pass3.md`, `docs/pm-doc-v24.0.md`, `docs/arch-doc-v24.0.md`, `docs/test-doc-v24.0.md`, `docs/ui-test-report-v24.0.md`, `docs/uat-report-v24.0.md`. **Label:** `TODO-uat-e2e-node-drawer-accessibility-violations`.

- [x] **Canvas AI / workflow explainer (Funlane-style)** — Added **Explain** button next to Run in `workflow-header/index.tsx`. On click: collects current canvas workflow data, calls AI API (Anthropic `sk-ant-…` or GitHub Models `ghp_…`/`ghu_…`), and displays a step-by-step explanation in a modal. Token stored in `localStorage` — no backend changes required.

- [x] **Explain: no GitHub token → send user to GitHub to authorize** — When Explain is clicked with no valid token, the UI now initiates GitHub OAuth Device Flow (`useGitHubDeviceFlow` hook). User sees a device code + "Open GitHub to authorize" button. On approval the token is stored and Explain runs immediately. "Paste a token manually" fallback preserved. Implemented in `workflow-ui` (`workflow-header/index.tsx` + `useGitHubDeviceFlow.ts`). No backend changes.

- [x] **Post-mortem & process: Explain — device-flow token `gho_` rejected as "Unrecognised token format" (test + review gap)** — Root cause documented; `gho_` added to both `isValidToken` and `callAI`; TC-AUTH-10..16 added to TEST_CASES_MASTER; auth PR code review checklist added. Docs: `pm/arch/test-doc-postmortem-gho-token.md`. **Label:** `TODO-postmortem-explain-github-gho-token-validation`.

- [x] **Post-mortem & process: Explain + GitHub OAuth device flow (CORS / same-origin proxy PR)** — Root cause (CORS) documented; same-origin proxy pattern confirmed shipped; TC-CORS-01..05 + standing UAT rule added to TEST_CASES_MASTER; network-tab checklist added. Docs: `pm/arch/test-doc-postmortem-cors-oauth.md`. **Label:** `TODO-postmortem-explain-github-oauth-cors-uat`.

- [x] **Artboard node editor — three-panel layout + smart text formatting** — Drawer reworked into three distinct panels (Node Description / Rules / Action) via shared `NodeSection` component; `useJsonFormat` hook added for blur-triggered JSON pretty-print; auto-format on drawer open added after UAT finding. Implemented in `workflow-ui`. Docs: `pm-doc-v3.0`, `arch-doc-v3.0`, `test-doc-v3.0`, `ui-test-report-v3.0`. **Label:** `TODO-artboard-node-editor-3panel-json-format`.

- [x] **Node editor — cross-audience clarity (dev / QA / business)** — After **Description**, **Rules**, and **Action** are visually distinct (see `EditorSection` in `workflow-ui`, branch `cursor/editor-window-section-clarity-2b2c`), extend clarity so each audience knows what to do with the drawer: **Developers** — short in-UI map from sections to persisted fields (`description`, `ruleList`, `action` / plugin payload) and link or tooltip to API/schema docs; avoid jargon-only labels without a plain-English gloss. **QA** — test script rows that assert each section saves/loads independently (e.g. change rules only, change action only) and regression cases for empty rules vs. multi-rule. **Business / product** — one-sentence definitions visible in the drawer or a "What is this?" affordance (e.g. collapsible help): *Description* = name on the canvas; *Rules* = "run only when…"; *Action* = "what the system does when it runs." Optional: consistent color legend (same tints as cards) in drawer footer or docs. **Recognition label for agents:** `TODO-node-editor-cross-audience-clarity`.

- [x] **Application + Record tables — no vertical scroll, missing totals & translation** — Fixed layout to `h-full overflow-y-auto`; added server-side pagination (`pagination={false}` + standalone `<Pagination>`); added total count display. Docs: `pm-doc-v5.0`, `arch-doc-v5.0`, `test-doc-v5.0`, `ui-test-report-v5.0`. **Recognition label:** `TODO-application-record-tables-scroll-count-translation`.

- [x] **Explain (GitHub Copilot path) — richer prompt hints + Markdown-friendly response UI** — Enriched `buildExplainPrompt()` to include rule details and action fields; added `SimpleMarkdown.tsx` custom renderer; Explain response now renders as structured Markdown. Docs: `pm-doc-v6.0`, `arch-doc-v6.0`, `test-doc-v6.0`, `ui-test-report-v6.0`. **Recognition label:** `TODO-explain-github-copilot-prompt-markdown-ui`.

- [x] **Q2 — Mobile (three items)** — **(1) Applications on phone:** Added overflow menu (History / Copy / Delete) on mobile cards. **(2) Desktop-same UI:** "Desktop view" toggle button added on mobile. **(3) Constraints met:** Additive changes only; canvas unaffected. Docs: `pm-doc-v7.0`, `arch-doc-v7.0`, `test-doc-v7.0`, `ui-test-report-v7.0`, `uat-report-v7.0`. **Recognition label:** `TODO-q2-mobile-apps-actions-desktop-entry-constraints`.

- [x] **Hosting — point operation + online API to DigitalOcean URLs** — Updated `vercel.json`, `vite.config.ts`, `CLAUDE.md`; all Render URLs replaced with DigitalOcean endpoints. Docs: `pm-doc-v8.0`, `arch-doc-v8.0`, `test-doc-v8.0`, `ui-test-report-v8.0`. **Recognition label:** `TODO-hosting-digitalocean-urls`.

- [x] **CI/CD — remove Render deploy GitHub Actions from both API repos** — Deleted `render-deploy.yml` from `workflow-operation-api` and `workflow-online-api`; updated AGENTS.md / docs. Docs: `pm-doc-v9.0`, `arch-doc-v9.0`, `test-doc-v9.0`, `ui-test-report-v9.0`. **Recognition label:** `TODO-cicd-remove-render-github-actions`.

- [x] **Mobile — add-application FAB (+) should be draggable** — Implemented pointer-event drag on mobile FAB with edge-snap and localStorage position persistence; tap still opens New Application dialog. Docs: `pm-doc-v10.0`, `arch-doc-v10.0`, `test-doc-v10.0`, `ui-test-report-v10.0`, `uat-report-v10.0`. **Recognition label:** `TODO-mobile-add-application-fab-draggable`.
