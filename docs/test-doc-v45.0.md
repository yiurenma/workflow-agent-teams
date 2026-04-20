# Test Doc v45.0 — Deploy Rewrite: Online API with Two-Block Body

## Test Cases

### TC-DEPLOY-NEW-01 — Happy path: deploy request accepted
- **Pre-condition**: Application with workflow exists in Hub.
- **Steps**: Click Deploy → Modal opens → Enter executionAppName = `deploy-workflow`, confirmationNumber pre-filled → Click Deploy.
- **Expected**: Online API called with `applicationName=deploy-workflow`; body contains `applicationInfo` and `workflow` blocks; modal shows green success banner.

### TC-DEPLOY-NEW-02 — Missing execution application name
- **Steps**: Leave executionAppName blank → Click Deploy.
- **Expected**: Toast error "Execution application name is required"; no API call made.

### TC-DEPLOY-NEW-03 — Missing confirmation number
- **Steps**: Clear confirmationNumber field → Click Deploy.
- **Expected**: Toast error "Confirmation number is required"; no API call made.

### TC-DEPLOY-NEW-04 — Online API error response
- **Steps**: Enter invalid executionAppName that does not exist in online API → Click Deploy.
- **Expected**: Modal shows red error banner with API error message.

### TC-DEPLOY-NEW-05 — Optional channelKind included
- **Steps**: Enter executionAppName, set channelKind = `EMAIL` → Click Deploy.
- **Expected**: `channelKind=EMAIL` included in query string to online API.

### TC-DEPLOY-NEW-06 — Empty channelKind omitted from request
- **Steps**: Leave channelKind blank → Click Deploy.
- **Expected**: `channelKind` parameter absent from online API request.

### TC-DEPLOY-NEW-07 — Source application info displayed read-only
- **Steps**: Open Deploy modal for an application.
- **Expected**: Source Application section shows correct `applicationName` and node count; these fields are not editable.

### TC-DEPLOY-NEW-08 — Modal cannot be closed while deploying
- **Steps**: Click Deploy → while deploying, click overlay or Cancel.
- **Expected**: Modal remains open; buttons disabled.

### TC-DEPLOY-NEW-09 — Confirmation number pre-filled with timestamp
- **Steps**: Open Deploy modal.
- **Expected**: Confirmation number field contains `deploy-{timestamp}` by default.

### TC-DEPLOY-NEW-10 — Body contains both blocks
- **Pre-condition**: Mock online API to capture request.
- **Steps**: Open modal, click Deploy.
- **Expected**: Request body is valid JSON with keys `applicationInfo` (object) and `workflow` (object with `pluginList`).
