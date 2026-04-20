# PM Doc v45.0 — Deploy Rewrite: Online API with Two-Block Body

## Feature
**TODO-deploy-rewrite-online-api-workflow-json**

## Summary
Rewrite the Deploy feature so that clicking **Deploy** automatically calls the **online API** instead of the previous 3-step operation-API sequence (CreateApplicationName → UpdateApplicationName → SaveWorkflow).

## User Story
As a workflow studio user, when I click **Deploy** on an application, I want the system to automatically send the source application settings and workflow to a designated online-API deploy-workflow application so that the target environment receives all necessary data in a single request.

## Acceptance Criteria
1. The Deploy modal no longer shows Deploy URL, username, password, or environment fields.
2. The user provides:
   - **Execution Application Name** — the online-API application that handles the deploy request.
   - **Confirmation Number** — pre-filled with a timestamp-based default; editable.
   - **Channel Kind** — optional.
3. The body sent to the online API contains exactly two blocks:
   - **A** (`applicationInfo`): full source application settings from Hub.
   - **B** (`workflow`): full workflow JSON from Hub.
4. The request goes through the existing `ONLINE_API_BASE` proxy; no CORS proxy required.
5. Success and error status are shown inline inside the modal.

## Out of Scope
- The deploy-workflow definition running inside the online API (handled by a separate task).
- Authentication fields — the online API proxy handles auth.

## Version
- **workflow-ui**: `claude/happy-franklin-S3CT9`, commit `4bb7b1af`
- **Date**: 2026-04-20
