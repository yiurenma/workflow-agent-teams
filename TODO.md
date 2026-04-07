# Agent teams to-do

Backlog for agent teams to pick up in order. Check items off as they ship.

## Open

- [ ] **Agent teams list — table page cannot scroll vertically (content hidden)** — On the **agent teams list** screen, the **entire table page** cannot be dragged or scrolled **up and down**, so **too much data stays off-screen** and is hidden from the audience. **Scope:** UX / layout / overflow (scroll container or viewport) — record for triage; implementation not started here. **Recognition label for agents:** `TODO-agent-teams-list-table-no-vertical-scroll`.

- [ ] **Node editor — cross-audience clarity (dev / QA / business)** — After **Description**, **Rules**, and **Action** are visually distinct (see `EditorSection` in `workflow-ui`, branch `cursor/editor-window-section-clarity-2b2c`), extend clarity so each audience knows what to do with the drawer: **Developers** — short in-UI map from sections to persisted fields (`description`, `ruleList`, `action` / plugin payload) and link or tooltip to API/schema docs; avoid jargon-only labels without a plain-English gloss. **QA** — test script rows that assert each section saves/loads independently (e.g. change rules only, change action only) and regression cases for empty rules vs. multi-rule. **Business / product** — one-sentence definitions visible in the drawer or a “What is this?” affordance (e.g. collapsible help): *Description* = name on the canvas; *Rules* = “run only when…”; *Action* = “what the system does when it runs.” Optional: consistent color legend (same tints as cards) in drawer footer or docs. **Recognition label for agents:** `TODO-node-editor-cross-audience-clarity`.

## Done

- [x] **Canvas AI / workflow explainer (Funlane-style)** — Added **Explain** button next to Run in `workflow-header/index.tsx`. On click: collects current canvas workflow data, calls AI API (Anthropic `sk-ant-…` or GitHub Models `ghp_…`/`ghu_…`), and displays a step-by-step explanation in a modal. Token stored in `localStorage` — no backend changes required.

- [x] **Explain: no GitHub token → send user to GitHub to authorize** — When Explain is clicked with no valid token, the UI now initiates GitHub OAuth Device Flow (`useGitHubDeviceFlow` hook). User sees a device code + "Open GitHub to authorize" button. On approval the token is stored and Explain runs immediately. "Paste a token manually" fallback preserved. Implemented in `workflow-ui` (`workflow-header/index.tsx` + `useGitHubDeviceFlow.ts`). No backend changes.

- [x] **Post-mortem & process: Explain — device-flow token `gho_` rejected as "Unrecognised token format" (test + review gap)** — Root cause documented; `gho_` added to both `isValidToken` and `callAI`; TC-AUTH-10..16 added to TEST_CASES_MASTER; auth PR code review checklist added. Docs: `pm/arch/test-doc-postmortem-gho-token.md`. **Label:** `TODO-postmortem-explain-github-gho-token-validation`.

- [x] **Post-mortem & process: Explain + GitHub OAuth device flow (CORS / same-origin proxy PR)** — Root cause (CORS) documented; same-origin proxy pattern confirmed shipped; TC-CORS-01..05 + standing UAT rule added to TEST_CASES_MASTER; network-tab checklist added. Docs: `pm/arch/test-doc-postmortem-cors-oauth.md`. **Label:** `TODO-postmortem-explain-github-oauth-cors-uat`.

- [x] **Artboard node editor — three-panel layout + smart text formatting** — Drawer reworked into three distinct panels (Node Description / Rules / Action) via shared `NodeSection` component; `useJsonFormat` hook added for blur-triggered JSON pretty-print; auto-format on drawer open added after UAT finding. Implemented in `workflow-ui`. Docs: `pm-doc-v3.0`, `arch-doc-v3.0`, `test-doc-v3.0`, `ui-test-report-v3.0`. **Label:** `TODO-artboard-node-editor-3panel-json-format`.
