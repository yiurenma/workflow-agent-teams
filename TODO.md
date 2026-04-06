# Agent teams to-do

Backlog for agent teams to pick up in order. Check items off as they ship.

## Open

- [ ] **Explain: no GitHub token → send user to GitHub to authorize**

  **Purpose**  
  When the user clicks **Explain** and the app has **no valid GitHub token** (or none stored), the flow should **open GitHub’s OAuth authorization** so the user can grant access, then return and run Explain with a stored token—instead of failing silently or showing a dead end.

  **Details (implementation outline)**  
  - **Trigger**: Explain (or “use GitHub Models”) path detects missing/invalid token in whatever store the UI uses today (currently `localStorage` per shipped Explain work).  
  - **OAuth**: Register a **GitHub OAuth App** or **GitHub App** with correct **callback / redirect URIs** for each environment (local, staging, prod).  
  - **Flow**: Build authorize URL with `client_id`, requested **scopes**, `redirect_uri`, and **`state`** (CSRF). On callback, exchange code for token per your architecture (prefer **PKCE** for public clients; avoid putting **client secret** in the browser—use a small backend if you need secret exchange).  
  - **After auth**: Persist token securely, resume **Explain** (same intent as before login—e.g. stash “pending explain” or deep link).  
  - **Codebase**: Likely `workflow-ui` (e.g. `workflow-header` / Explain modal); exact files to align with current Explain implementation.

  **Problems / risks (track here)**  
  - **Security**: `localStorage` for tokens is weak to XSS; stronger options (httpOnly cookie via backend, or platform secret storage) should be decided for production.  
  - **UX**: Pop-up blockers if authorization opens in a new window without a direct user gesture; prefer same-tab navigation or explicit “Connect GitHub” if needed.  
  - **Enterprise**: GitHub Enterprise Server uses different host URLs and app registration.  
  - **Org policies**: OAuth app may require **org admin approval** (SSO / restricted OAuth apps).  
  - **Submodule pin**: Parent `workflow` repo may pin `workflow-agent-teams` to an older commit without `TODO.md`; bump submodule after this lands so others see the file.

  **Coordination**  
  Product / sequencing: **Shuang Xia Wen** (confirm scope, OAuth app ownership, and env redirect URLs before implementation).

- [ ] **Artboard node editor — three-panel layout + smart text formatting** — Rework the per-node editor on the workflow artboard so it is unmistakably split into **three modules**: (1) **Node description** — what this node is for; (2) **Rules** — configuration for when/how the node applies; (3) **Action type** — what the node does (behavior / next steps). Each section should be visually and structurally distinct (clear headings, spacing, and scannable layout). In text fields inside the editor frame, add **basic format helpers**: if pasted or entered content is valid **JSON**, pretty-print / normalize it automatically; if it is not JSON, apply light structure improvements (line breaks, indentation, or similar) so unstructured input is easier to read — without corrupting user intent when parsing fails. **Recognition label for agents:** `TODO-artboard-node-editor-3panel-json-format`.

## Done

- [x] **Canvas AI / workflow explainer (Funlane-style)** — Added **Explain** button next to Run in `workflow-header/index.tsx`. On click: collects current canvas workflow data, calls AI API (Anthropic `sk-ant-…` or GitHub Models `ghp_…`/`ghu_…`), and displays a step-by-step explanation in a modal. Token stored in `localStorage` — no backend changes required.
