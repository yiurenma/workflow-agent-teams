# PM Doc v45.0 — Deploy Rewrite: Online API Integration

**Label:** `TODO-deploy-rewrite-online-api-workflow-json`

## Summary

Rewrote the Deploy modal to call the Online API instead of making direct 3-step calls to the operation API. Users enter an execution application name; the client assembles a two-block request body (application settings + workflow) from Hub data and posts it to the Online API runtime.

## User Story

As a workflow engineer, when I click Deploy on an application, the system should automatically send both the application configuration and the workflow definition to the Online API runtime, so the application can be executed under the specified name without manual API calls.

## Requirements

1. User clicks **Deploy** → client automatically calls Online API (`POST /workflow?applicationName=<executionName>`)
2. User enters the **execution application name** (query param) — defaults to source app name, editable
3. Request body contains two mandatory blocks:
   - **A — Application Setting**: Full `WorkflowEntitySettingRow` for the Hub's current application
   - **B — Workflow**: Full `WorkFlow` JSON for the current application
4. No credentials or target URL required from the user — uses configured `ONLINE_API_BASE`
5. UI shows single-step progress: in-progress → success / error
6. Source application info shown read-only (name, node count)

## Out of Scope (this iteration)

- The deploy workflow executed inside the Online API (CreateApplicationName → UpdateApplicationName → SaveWorkflow) is handled by a separate task (`TODO-online-api-post-optional-sse-runtime-per-step`)
- SSE streaming of deploy progress
- Custom online API URL override per deploy
