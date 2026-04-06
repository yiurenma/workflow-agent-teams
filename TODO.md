# Agent teams to-do

Backlog for agent teams to pick up in order. Check items off as they ship.

## Open

*(no open items)*

- [ ] **Artboard node editor — three-panel layout + smart text formatting** — Rework the per-node editor on the workflow artboard so it is unmistakably split into **three modules**: (1) **Node description** — what this node is for; (2) **Rules** — configuration for when/how the node applies; (3) **Action type** — what the node does (behavior / next steps). Each section should be visually and structurally distinct (clear headings, spacing, and scannable layout). In text fields inside the editor frame, add **basic format helpers**: if pasted or entered content is valid **JSON**, pretty-print / normalize it automatically; if it is not JSON, apply light structure improvements (line breaks, indentation, or similar) so unstructured input is easier to read — without corrupting user intent when parsing fails. **Recognition label for agents:** `TODO-artboard-node-editor-3panel-json-format`.

## Done

- [x] **Canvas AI / workflow explainer (Funlane-style)** — Added **Explain** button next to Run in `workflow-header/index.tsx`. On click: collects current canvas workflow data, calls AI API (Anthropic `sk-ant-…` or GitHub Models `ghp_…`/`ghu_…`), and displays a step-by-step explanation in a modal. Token stored in `localStorage` — no backend changes required.

- [x] **Explain: no GitHub token → send user to GitHub to authorize** — When Explain is clicked with no valid token, the UI now initiates GitHub OAuth Device Flow (`useGitHubDeviceFlow` hook). User sees a device code + "Open GitHub to authorize" button. On approval the token is stored and Explain runs immediately. "Paste a token manually" fallback preserved. Implemented in `workflow-ui` (`workflow-header/index.tsx` + `useGitHubDeviceFlow.ts`). No backend changes.
