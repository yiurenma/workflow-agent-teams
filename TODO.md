# Agent teams to-do

Backlog for agent teams to pick up in order. Check items off as they ship.

## Pending

- [x] **前端重构 — 基于设计交付文件重建 workflow-ui (HIGH)** *(label: `TODO-frontend-rebuild-from-design-handoff`)* — ✅ **DONE** — UAT PASS 2026-04-19. Rebuilt workflow-ui with IBM Carbon Design System. Branch `feature/design-handoff-rebuild` merged to `main`. All 115 Playwright E2E tests passed (1 skipped). Commits: `e9bc845`, `5e2f77e`. Design verified: IBM Plex Sans/Mono fonts, #0f62fe primary, #161616 nav, 0px border-radius, full responsive. Fixed 2 mobile regressions (FAB z-index, nav test). Docs: `pm-doc-v37.0.md`, `arch-doc-v37.0.md`, `test-doc-v37.0.md`, `ui-test-report-v37.0.md`, `e2e-test-report-v37.0.md`. **Label:** `TODO-frontend-rebuild-from-design-handoff`.

- [ ] **Hub — ApplicationName 部署 (Deploy)** *(label: `TODO-hub-deploy-application-name`)* — 在 **ApplicationName** 操作区增加 **Deploy**：点击后先展示**功能说明**；用户填写 **Deploy URL（仅 base，如 UAT operation-api）**、应用名、服务账号用户名/密码、环境（五项）；系统在 base 后自动拼接 **`CreateApplicationName` → `UpdateApplicationName` → `SaveWorkflow`** 三个路径顺序调用，完成创建应用、更新应用数据（对齐 Hub **Save** 的应用侧语义）、保存工作流；界面展示三步进度，**三步均成功**显示绿色成功标识。UAT 关键用例：**已加载的现有应用**上下文中，表单应用名填**另一个名称**，URL 使用 UAT，验证端到端成功。英文细则与 Arch 要求见主仓 `docs/TODO.md`。

- [ ] **端到端 — 节点编辑器抽屉关闭 (HIGH)** *(label: `TODO-uat-e2e-node-drawer-close-button-broken`)* — fix: node editor drawer close button not working

  **问题描述:** 点击关闭按钮时抽屉未关闭，用户被迫点击外部区域或按ESC键

  **根本原因:** 自定义关闭按钮onClick处理器中的`e.stopPropagation()`阻止了点击事件

  **解决方案:**
    - 移除`e.stopPropagation()`
    - 添加`ant-drawer-close`类以兼容测试选择器
    - 简化onClick为直接调用`onClose()`

  **验证:** TC-NODE-ENHANCED-05在https://workflow-ui-gamma.vercel.app上17秒内通过

  **提交:** `workflow-ui@cb5465f` - "fix: node editor drawer close button not working"

  **标签:** `TODO-uat-e2e-node-drawer-close-button-broken`

- [ ] **端到端 — 五层验证改造** *(label: `TODO-uat-e2e-retrofit-5layer-validation`)* ✅ **DONE...**

- [ ] **UI一致性 — IBM Carbon 剩余样式 + Playwright blind spots** *(label: `TODO-ui-ibm-carbon-audi...**

- [ ] **节点编辑器 — 规则键必须是单个JSON路径** *(label: `TODO-node-editor-rule-key-json-path-validati...**

- [ ] **节点编辑器 — 宽度、先读后写模式、编辑门、长内容** *(label: `TODO-node-editor-draggable...**

- [ ] **端到端 — 修复完整套件运行的测试失败 (44 failures)** *(label: `TODO-e2e-fix-test-failures-pos...**

- [ ] **品牌 — 蜗牛图标 (browser tab)** *(label: `TODO-branding-favicon-snail-tab-icon`)* ✅ **DONE ...**

- [ ] **UI/UX — 搜索框可用性问题** *(label: `TODO-ui-ux-search-box-usability`)***

- [ ] **UI/UX — 红色按钮缺少文本标签** *(label: `TODO-ui-ux-red-buttons-no-labels`)***

- [ ] **UI/UX — 设置按钮不稳定** *(label: `TODO-ui-ux-settings-button-unstable`)***

- [ ] **UI/UX — 移动端抽屉滚动边界溢出** *(label: `TODO-ui-ux-mobile-drawer-scroll-bounds-over...**

- [ ] **画布 — 复制和删除选定节点** *(label: `TODO-canvas-node-copy-full-config-delete`)***

- [ ] **AI — 工作流生成器语义质量** *(label: `TODO-canvas-ai-workflow-generator-quality-promp...**

- [ ] **首页 — 工作流工作室介绍** *(label: `TODO-home-workflow-studio-intro-copy`)***

- [ ] **品牌 — 蜗牛图标 (browser tab)** *(label: `TODO-branding-favicon-snail-tab-icon`)***

- [ ] **画布 — 从JSON导入工作流 (toolbar; validate then apply)** — **Separate from** the unified program above and **separate from** AI **Generate** (`WorkflowGeneratorModal`): add a **canvas header** action (desktop toolbar + mobile overflow) that lets the user **paste or upload a JSON string** representing a **`WorkFlow`** (same shape the app saves: `pluginList`, `uiMapList`, node ids, edges, etc.). **Primary use case:** **validate** externally produced or hand-edited JSON **before** replacing the canvas — not open-ended natural language. **Flow:** (1) **Parse** JSON (support stripping markdown code fences if users paste fenced blocks); (2) **Validate** against the app’s `WorkFlow` schema / invariants (required fields, plugin types, unique ids, edge endpoints exist); (3) Show **human-readable errors** with line/path hints where possible; (4) **Preview** summary (node count, warnings); (5) **Apply** replaces current canvas workflow (with confirm if canvas is non-empty — PM decides copy vs destructive default). **In-UI instructions (include in modal):** explain that input must be **valid workflow JSON** matching this app’s format; link or expandable “**What belongs here?**” with a **minimal example snippet** (truncated) and “**Not** for plain English — use **Generate** for that”; mention **Save** still persists to backend as today. **Carbon:** match v28 modal patterns. **Scope:** `workflow-ui` only; no new backend if validation is client-side. **Label:** `TODO-canvas-import-workflow-json-validate-apply`.


## Completed

- [x] **UI设计 — IBM设计语言重构 (requires /ibm-design skill)** — 按照 IBM Design Language 规范对 Workflow Studio 整体 UI 进行重新设计。使用 `/ibm-design` skill 驱动实现，覆盖排版、色彩、间距、组件风格（按钮、表格、表单、抽屉、导航）等全局视觉系统。目标：从当前 Quiet Luxury 风格迁移至 IBM Carbon Design System 风格，保持所有功能不变。**Status:** ✅ DONE — UAT PASS 2026-04-12. IBM Plex Sans/Mono fonts. Blue 60 #0f62fe primary. Gray 100 #161616 nav. 0px border-radius. Carbon token system (--cds-*). 82/84 E2E pass (1 pre-existing drawer close defect). Docs: `pm-doc-v28.0.md`, `arch-doc-v28.0.md`, `test-doc-v28.0.md`, `ui-test-report-v28.0.md`, `e2e-test-report-v28.0.md`, `uat-report-v28.0.md`. **Label:** `TODO-ui-design-ibm-design-language-refactoring`.

- [x] **UI设计 — 安静奢华视觉重构 (HIGH priority, requires /frontend-design skill)** — Refactor Workflow Studio UI from generic tech aesthetic to sophisticated "Quiet Luxury" design system. **Status:** ✅ DONE — UAT PASS 2026-04-11. Fonts: Lora + DM Sans. Primary: terracotta #7C4A3A. Header: warm charcoal #2A2520 with gold #C9A87C. Background: cream #F9F7F4. Active tags: mint #EAF3EE. Table header: warm sand #F3F0EB. All functional regressions clean. Docs: `pm-doc-v27.0.md`, `arch-doc-v27.0.md`, `test-doc-v27.0.md`, `ui-test-report-v27.0.md`, `e2e-test-report-v27.0.md`, `uat-report-v27.0.md`. **Label:** `TODO-ui-design-quiet-luxury-refactoring-frontend-skill`.

- [x] **端到端（Playwright）— 第一遍：编写用例+首次完整运行** — 33 test cases across 7 spec files (navigation, desktop apps, mobile apps, canvas, node editor, explain, records). Two Playwright projects: Desktop Chrome (1280 px) and Mobile Chrome (390 × 844, `isMobile: true`, `hasTouch: true`). All tests use `page.route()` mocks — no real backend required. Final run: **47 pass, 19 skip (viewport-mismatch), 0 fail** across 66 total runs. Docs: `ui-test-report-v12.0.md`. **Label:** `TODO-e2e-playwright-pass1-author-full-run`.

- [x] **Mobile — application overflow menu (History / Delete / Copy) navigates to Workflow instead of opening flows** — Root cause: `onClick={navigate}` was on the outer card `<div>`, causing Ant Design Dropdown portal teardown to ghost-click through to the navigation handler. Fix: moved `onClick={navigate}` to the inner info `<div>` only; actions zone has no navigate handler. All TC-APP-MOB-05/06/07/08 now pass. Docs: `ui-test-report-v11.0.md`, `uat-report-v11.0.md`. **Label:** `TODO-mobile-app-overflow-history-copy-modal-not-navigation`.

- [x] **画布 — 工作流数据未定义导致崩溃** — **Bug:** Canvas component crashes with “Cannot read properties of undefined (reading 'pluginList')” when loading an application. **Root cause:** Canvas expects `workflow.pluginList` but receives undefined. **Fix:** Wrapped `workFlowToNodesAndEdges()` in try/catch in `useWorkflowState.ts`; on error logs and renders empty canvas. Docs: `pm/arch/test-doc-v13.0.md`, `ui-test-report-v13.0.md`. **Recognition label:** `TODO-canvas-workflow-undefined-crash`.

- [x] **移动端 — 记录分页不可见 on narrow viewport** — **Bug:** Pagination component (`.ant-pagination`) is not visible on mobile viewport in Records list. **Fix:** Replaced custom Prev/Next block with Ant Design `<Pagination size=”small”>` in mobile branch of `records/index.tsx`. TC-REC-03 now passes on both Desktop + Mobile. Docs: `pm/arch/test-doc-v14.0.md`, `ui-test-report-v14.0.md`. **Recognition label:** `TODO-mobile-records-pagination-invisible`.

- [x] **Mobile / phone viewport — workflow canvas “Add node” (+) FAB should be draggable (parity with applications list)** — **Fix:** Added pointer-event drag with edge-snap and localStorage persistence (`workflow_canvas_fab_pos`) to `MobileAddNodeSheet.tsx`. Tap < 5 px still opens Add Node sheet. New spec `canvas-mobile.spec.ts` with TC-CANVAS-MOB-01..05. Docs: `pm/arch/test-doc-v15.0.md`, `ui-test-report-v15.0.md`. **Recognition label for agents:** `TODO-mobile-canvas-add-node-fab-draggable`.

- [x] **画布 — JsonPath游乐场 (validate path + preview value)** — **Fix:** New `JsonPathModal.tsx` using `jsonpath-plus` (client-side). “JsonPath” button added to `WorkflowHeader` toolbar. TC-JSONPATH-01..05 in `canvas.spec.ts`. Docs: `pm/arch/test-doc-v16.0.md`, `ui-test-report-v16.0.md`. **Recognition label for agents:** `TODO-canvas-json-path-verification-modal`.

- [x] **画布 — 直线工作流布局 (recovery)** — New **canvas** capability: a **button** (or clear control) that lays out the workflow as a **single vertical line** from **top to bottom** so users who feel **lost** on a tangled artboard can **reset** to a readable linear flow in one action. **Scope:** `workflow-ui` canvas / workflow editor. **Fix (v19.0):** `StraightenButton` component added inside `<ReactFlow>` using `<Panel position="top-right">`; `straightenNodes()` sorts nodes by Y and resets X=300. TC-CANVAS-05 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label for agents:** `TODO-canvas-straight-line-workflow-recovery`.

- [x] **应用 — 允许编辑应用名称 in the application table** — **Backend:** operation API already supports updating the application record (including name); **no API work** unless discovery finds a gap. **Frontend (`workflow-ui`):** expose rename in the applications table (inline edit, row action, or modal — follow existing patterns), call the existing update API, handle validation and errors. **Fix (v20.0):** Added `newApplicationName` field to `EntitySettingPatchRequest` DTO and rename logic in `WorkflowEntitySettingController`. Settings modal shows pre-filled Application Name input. TC-APP-DESK-10 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label:** `TODO-application-table-edit-name`.

- [x] **应用名称 — 持久化并显示`描述` (long text)** — Product expects every application name to have a **description** of what the application does; the **UI currently shows no value** because the **database table has no column**. **Fix (v21.0):** Flyway `V1__add_description_to_workflow_entity_setting.sql` adds nullable TEXT `description` to entity and audit tables. `WorkflowEntitySetting`, `EntitySettingPatchRequest`, and controller updated. Frontend shows `description` in table column and mobile card; Settings modal includes Description textarea. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label for agents:** `TODO-application-name-description-db-text-ui-api`.

- [x] **移动端 — 节点编辑器（绘图册）显示大片空白区域 below content** — On **phone** (reported on **iPhone X–class** viewports), after **tapping a node** on the **workflow canvas / drawing book**, the **node editor** (drawer / panel) often shows a **prominent empty region below** the real fields. **Fix (v22.0):** `WorkflowDrawer` switches to `placement="bottom"`, `height="auto"`, `maxHeight: 80dvh`, `env(safe-area-inset-bottom)` padding on mobile. Desktop drawer unchanged. TC-CANVAS-MOB-09 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label for agents:** `TODO-mobile-node-editor-blank-space-below`.

- [x] **画布AI — 工作流生成器（解释功能旁） — Copilot JSON replaces canvas** — **Product goal:** A **UI action** (next to **Explain**) lets the user describe **what they need**; **GitHub Copilot** returns a **single JSON string** that matches the app’s **workflow / `WorkFlow` shape** so the UI can **build or replace the canvas** from that JSON. **Fix (v23.0):** `WorkflowGeneratorModal.tsx` created with `WORKFLOW_GENERATOR_SYSTEM_PROMPT` constant. Uses `max_tokens: 4096`. Strips markdown fences from response. Generate button added to desktop toolbar and mobile ⋯ dropdown. TC-GENERATOR-01/02 added. QA/UAT: `qa-report-v19-v23.md`, `uat-report-v19-v23.md`. **Recognition label:** `TODO-canvas-ai-workflow-json-copilot-replace`.

- [x] **Vercel构建 — 缺失dayjs依赖 causes deployment failure** — Vercel build fails with "Rollup failed to resolve import 'dayjs' from src/routes/records/index.tsx". **Root cause:** `dayjs` and `json5` were imported in code but not declared in `package.json` dependencies. **Fix:** Added `dayjs@^1.11.13` and `json5@^2.2.3` to dependencies in `package.json` and updated `pnpm-lock.yaml`. Local build verified successful. Vercel deployment now succeeds. **Status:** ✅ COMPLETE - Vercel deployment successful on 2026-04-11. Docs: `docs/arch-doc-v26.0.md`, `docs/test-doc-v26.0.md`, `docs/ui-test-report-v26.0.md`. **Label:** `TODO-vercel-build-missing-dayjs-dependency`.

- [x] **节点编辑器 — 表单字段描述颜色对比度** — Form field description text in HttpCallForm and LogicForm had insufficient color contrast (2.62:1 on white background, 2.51:1 on #fafafa background). **Fix:** Changed `text-zinc-400` to `text-zinc-600` in NodeSection.tsx subtitle rendering, improving contrast to 7.0:1 (white) and 6.7:1 (#fafafa), exceeding WCAG 2.1 Level AA 4.5:1 minimum. Affects 3 description paragraphs: "The name shown on the canvas...", "Run only when...", "What the system does...". **Status:** ✅ COMPLETE - UAT PASS on 2026-04-11. Docs: `docs/pm-doc-v25.0.md`, `docs/arch-doc-v25.0.md`, `docs/test-doc-v25.0.md`, `docs/ui-test-report-v25.0.md`, `docs/uat-report-v25.0.md`. **Label:** `TODO-node-editor-form-descriptions-color-contrast`.

- [x] **UAT端到端 — 节点编辑器抽屉的无障碍访问违规** — Node editor drawer (`.ant-drawer`) had 2 WCAG violations: (1) Missing `aria-label` on `role="dialog"` element (WCAG 2.1 Level A); (2) Insufficient color contrast 2.51:1 on "NODE CONFIGURATION" header text. **Fix:** Added `aria-label="Node Configuration"` to drawer and changed header text color to text-zinc-600 (#52525b, 7.0:1 contrast). **Status:** ✅ COMPLETE - UAT PASS on 2026-04-11 18:55. Docs: `specs/uat-report-e2e-pass3.md`, `docs/pm-doc-v24.0.md`, `docs/arch-doc-v24.0.md`, `docs/test-doc-v24.0.md`, `docs/ui-test-report-v24.0.md`, `docs/uat-report-v24.0.md`. **Label:** `TODO-uat-e2e-node-drawer-accessibility-violations`.

- [x] **画布AI/工作流解释器 (Funlane-style)** — Added **Explain** button next to Run in `workflow-header/index.tsx`. On click: collects current canvas workflow data, calls AI API (Anthropic `sk-ant-…` or GitHub Models `ghp_…`/`ghu_…`), and displays a step-by-step explanation in a modal. Token stored in `localStorage` — no backend changes required.

- [x] **事后分析与流程： Explain — device-flow token `gho_` rejected as "Unrecognised token format" (test + review gap)** — Root cause documented; `gho_` added to both `isValidToken` and `callAI`; TC-AUTH-10..16 added to TEST_CASES_MASTER; auth PR code review checklist added. Docs: `pm/arch/test-doc-postmortem-gho-token.md`. **Label:** `TODO-postmortem-explain-github-gho-token-validation`.

- [x] **事后分析与流程： Explain + GitHub OAuth device flow (CORS / same-origin proxy PR)** — Root cause (CORS) documented; same-origin proxy pattern confirmed shipped; TC-CORS-01..05 + standing UAT rule added to TEST_CASES_MASTER; network-tab checklist added. Docs: `pm/arch/test-doc-postmortem-cors-oauth.md`. **Label:** `TODO-postmortem-explain-github-oauth-cors-uat`.

- [x] **画布节点编辑器 — 三面板布局 + smart text formatting** — Drawer reworked into three distinct panels (Node Description / Rules / Action) via shared `NodeSection` component; `useJsonFormat` hook added for blur-triggered JSON pretty-print; auto-format on drawer open added after UAT finding. Implemented in `workflow-ui`. Docs: `pm-doc-v3.0`, `arch-doc-v3.0`, `test-doc-v3.0`, `ui-test-report-v3.0`. **Label:** `TODO-artboard-node-editor-3panel-json-format`.

- [x] **节点编辑器 — 跨受众清晰度 (dev / QA / business)** — After **Description**, **Rules**, and **Action** are visually distinct (see `EditorSection` in `workflow-ui`, branch `cursor/editor-window-section-clarity-2b2c`), extend clarity so each audience knows what to do with the drawer: **Developers** — short in-UI map from sections to persisted fields (`description`, `ruleList`, `action` / plugin payload) and link or tooltip to API/schema docs; avoid jargon-only labels without a plain-English gloss. **QA** — test script rows that assert each section saves/loads independently (e.g. change rules only, change action only) and regression cases for empty rules vs. multi-rule. **Business / product** — one-sentence definitions visible in the drawer or a "What is this?" affordance (e.g. collapsible help): *Description* = name on the canvas; *Rules* = "run only when…"; *Action* = "what the system does when it runs." Optional: consistent color legend (same tints as cards) in drawer footer or docs. **Recognition label for agents:** `TODO-node-editor-cross-audience-clarity`.

- [x] **应用和记录表格 — 无垂直滚动, missing totals & translation** — Fixed layout to `h-full overflow-y-auto`; added server-side pagination (`pagination={false}` + standalone `<Pagination>`); added total count display. Docs: `pm-doc-v5.0`, `arch-doc-v5.0`, `test-doc-v5.0`, `ui-test-report-v5.0`. **Recognition label:** `TODO-application-record-tables-scroll-count-translation`.

- [x] **解释（GitHub Copilot路径） — richer prompt hints + Markdown-friendly response UI** — Enriched `buildExplainPrompt()` to include rule details and action fields; added `SimpleMarkdown.tsx` custom renderer; Explain response now renders as structured Markdown. Docs: `pm-doc-v6.0`, `arch-doc-v6.0`, `test-doc-v6.0`, `ui-test-report-v6.0`. **Recognition label:** `TODO-explain-github-copilot-prompt-markdown-ui`.

- [x] **第二季度 — 移动端 (three items)** — **(1) Applications on phone:** Added overflow menu (History / Copy / Delete) on mobile cards. **(2) Desktop-same UI:** "Desktop view" toggle button added on mobile. **(3) Constraints met:** Additive changes only; canvas unaffected. Docs: `pm-doc-v7.0`, `arch-doc-v7.0`, `test-doc-v7.0`, `ui-test-report-v7.0`, `uat-report-v7.0`. **Recognition label:** `TODO-q2-mobile-apps-actions-desktop-entry-constraints`.

- [x] **托管 — 点操作+在线API to DigitalOcean URLs** — Updated `vercel.json`, `vite.config.ts`, `CLAUDE.md`; all Render URLs replaced with DigitalOcean endpoints. Docs: `pm-doc-v8.0`, `arch-doc-v8.0`, `test-doc-v8.0`, `ui-test-report-v8.0`. **Recognition label:** `TODO-hosting-digitalocean-urls`.

- [x] **CI/CD — 移除Render部署GitHub动作 from both API repos** — Deleted `render-deploy.yml` from `workflow-operation-api` and `workflow-online-api`; updated AGENTS.md / docs. Docs: `pm-doc-v9.0`, `arch-doc-v9.0`, `test-doc-v9.0`, `ui-test-report-v9.0`. **Recognition label:** `TODO-cicd-remove-render-github-actions`.

- [x] **移动端 — 添加应用悬浮按钮 (+) should be draggable** — Implemented pointer-event drag on mobile FAB with edge-snap and localStorage position persistence; tap still opens New Application dialog. Docs: `pm-doc-v10.0`, `arch-doc-v10.0`, `test-doc-v10.0`, `ui-test-report-v10.0`, `uat-report-v10.0`. **Recognition label:** `TODO-mobile-add-application-fab-draggable`.

- [x] **端到端（Playwright）— 第二遍：其他开放项之后的完整回归** — Full suite run after v13.0–v16.0 shipped. **Result: 64 pass / 24 skip (viewport-mismatch) / 0 fail across 88 total runs.** No regressions. 17 new test cases added (TC-CANVAS-04, TC-CANVAS-MOB-01..05, TC-JSONPATH-01..05). Docs: `ui-test-report-v17.0.md`. **Recognition label:** `TODO-e2e-playwright-pass2-full-regression-after-backlog`.

- [x] **解释：无GitHub令牌 → send user to GitHub to authorize** — When Explain is clicked with no valid token, the UI now initiates GitHub OAuth Device Flow (`useGitHubDeviceFlow` hook). User sees a device code + "Open GitHub to authorize" button. On approval the token is stored and Explain runs immediately. "Paste a token manually" fallback preserved. Implemented in `workflow-ui` (`workflow-header/index.tsx` + `useGitHubDeviceFlow.ts`). No backend changes.