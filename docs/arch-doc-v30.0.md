# Architecture Doc v30.0 — Workflow Studio Unified Product & UX Program

**Version:** 30.0  
**Date:** 2026-04-12  
**Status:** Draft — awaiting human approval  
**Label:** `TODO-workflow-studio-unified-product-ux-program`

---

## Overview

Technical specifications for the unified product & UX program. All changes are frontend-only in `workflow-ui`. No backend API modifications required.

---

## 1. E2E Test Quality — 5-Layer Validation

### Current State
Existing tests in `workflow-ui/e2e/` validate Layer 1 (existence) primarily.

### Implementation
Create reusable validation helpers in `workflow-ui/e2e/helpers/five-layer.ts`:

```typescript
export async function validateLayer1(locator, name) {
  await expect(locator).toBeVisible();
}

export async function validateLayer2(locator, minW?, minH?, maxW?, maxH?) {
  const box = await locator.boundingBox();
  if (minW) expect(box.width).toBeGreaterThanOrEqual(minW);
  if (minH) expect(box.height).toBeGreaterThanOrEqual(minH);
}

export async function validateLayer3(page, locator) {
  const box = await locator.boundingBox();
  const vp = page.viewportSize();
  expect(box.x + box.width).toBeLessThanOrEqual(vp.width);
}

export async function validateLayer4(locator) {
  await expect(locator).toBeEnabled();
  await locator.click();
}

export async function validateLayer5(locator, prop, expected) {
  const computed = await locator.evaluate((el, p) => 
    window.getComputedStyle(el).getPropertyValue(p), prop
  );
  expect(computed).toBe(expected);
}
```

Retrofit existing specs to use these helpers.

---

## 2. IBM Carbon Completeness

### Audit Methodology
1. Automated scan: `scripts/audit-carbon.ts` finds non-Carbon patterns
2. Manual review of portal components (`.ant-dropdown`, `.ant-tooltip`, `.ant-modal`)
3. Playwright visual inspection

### Root Causes & Fixes

| Gap | Root Cause | Fix |
|-----|------------|-----|
| Rounded buttons | Ant Design default | Override with `border-radius: 0` in global CSS |
| Non-Carbon modal | `Modal.confirm()` | Replace with `CarbonModal` component |
| Portal styling | Portals bypass Tailwind | Add global CSS overrides for `.ant-*` |
| Color inconsistency | Tailwind classes | Replace with CSS custom properties `var(--cds-*)` |

### Implementation

**File:** `workflow-ui/src/styles/carbon-overrides.css`

```css
.ant-modal-content { border-radius: 0 !important; }
.ant-btn { border-radius: 0 !important; }
.ant-btn-primary { background: #0f62fe !important; }
.ant-btn-dangerous { background: #da1e28 !important; }
.ant-tag { border-radius: 24px !important; } /* exception */
```

**File:** `workflow-ui/src/components/CarbonModal.tsx`

```typescript
export function carbonConfirm(config) {
  return Modal.confirm({
    ...config,
    className: 'carbon-modal',
    okButtonProps: { style: { borderRadius: 0 } },
  });
}
```

Replace all `Modal.confirm()` calls with `carbonConfirm()`.

---

## 3. Node Editor — Rule Key Validation

### Implementation

**File:** `workflow-ui/src/routes/workflows/-components/workflow-drawer/forms/RuleInput.tsx`

```typescript
import { JSONPath } from 'jsonpath-plus';

function validateRuleKey(key: string) {
  if (!key.trim()) return { valid: false, error: 'Rule key cannot be empty' };
  if (key.includes(',') || key.includes(';')) {
    return { valid: false, error: 'Rule key must be a single JSONPath expression (e.g., $.customer.id)' };
  }
  try {
    JSONPath({ path: key, json: {}, resultType: 'value' });
    return { valid: true };
  } catch (e) {
    return { valid: false, error: `Invalid JSONPath syntax: ${e.message}` };
  }
}

export function RuleInput({ value, onChange, onBlur }) {
  const [error, setError] = useState();
  
  const handleBlur = () => {
    const validation = validateRuleKey(value);
    setError(validation.error);
    onBlur?.();
  };
  
  return (
    <div>
      <Input value={value} onChange={onChange} onBlur={handleBlur} status={error ? 'error' : undefined} />
      {error && <div className="text-[12px] mt-1" style={{ color: '#da1e28' }}>{error}</div>}
    </div>
  );
}
```

Disable save button when validation errors exist.

---

## 4. Node Editor — UX Improvements

### Resizable Drawer (Desktop)

**File:** `workflow-ui/src/routes/workflows/-components/workflow-drawer/index.tsx`

```typescript
const MIN_WIDTH = 420;
const MAX_WIDTH_PERCENT = 0.6;

const [width, setWidth] = useState(() => {
  const saved = localStorage.getItem('workflow_drawer_width');
  return saved ? parseInt(saved) : 520;
});

const [isResizing, setIsResizing] = useState(false);

const handleMouseDown = (e) => {
  setIsResizing(true);
  e.preventDefault();
};

useEffect(() => {
  if (!isResizing) return;
  
  const handleMouseMove = (e) => {
    const maxWidth = window.innerWidth * MAX_WIDTH_PERCENT;
    const newWidth = window.innerWidth - e.clientX;
    const clampedWidth = Math.max(MIN_WIDTH, Math.min(newWidth, maxWidth));
    setWidth(clampedWidth);
    localStorage.setItem('workflow_drawer_width', clampedWidth.toString());
  };
  
  const handleMouseUp = () => setIsResizing(false);
  
  document.addEventListener('mousemove', handleMouseMove);
  document.addEventListener('mouseup', handleMouseUp);
  
  return () => {
    document.removeEventListener('mousemove', handleMouseMove);
    document.removeEventListener('mouseup', handleMouseUp);
  };
}, [isResizing]);
```

### Read-Only Default with Edit Button

```typescript
const [mode, setMode] = useState<'view' | 'edit'>('view');

return (
  <div>
    {mode === 'view' ? (
      <>
        <Button type="primary" onClick={() => setMode('edit')}>Edit</Button>
        <ReadOnlyView data={formData} />
      </>
    ) : (
      <>
        <Button onClick={() => setMode('view')}>Cancel</Button>
        <Button type="primary" onClick={handleSave}>Save</Button>
        <EditableForm data={formData} onChange={setFormData} />
      </>
    )}
  </div>
);
```

---

## 5. Canvas Palette — Descriptions

### Data Structure

**File:** `workflow-ui/src/types/plugins.tsx`

```typescript
export const PluginMetadataMap: Record<Plugin, {
  icon: React.ReactNode;
  color?: string;
  description: string; // NEW
}> = {
  [Plugin.CONSUMER]: {
    icon: <CloudDownloadOutlined />,
    color: "#3B82F6",
    description: "Fetch data from external APIs",
  },
  [Plugin.MESSAGE]: {
    icon: <SendOutlined />,
    color: "#7C3AED",
    description: "Send messages to channels",
  },
  // ... more
};
```

### UI Update

**File:** `workflow-ui/src/routes/workflows/-components/workflow-sider/index.tsx`

```typescript
<div className="flex flex-col gap-1 px-2 py-2">
  <div className="flex items-center gap-2.5">
    <span>{child.icon}</span>
    {!collapsed && <span>{displayName}</span>}
  </div>
  {!collapsed && (
    <span className="text-[10px]" style={{ color: '#525252' }}>
      {meta.description}
    </span>
  )}
</div>
```

---

## 6. Canvas Node Actions — Copy and Delete

### Context Menu

**File:** `workflow-ui/src/routes/workflows/-components/worflow-canvas/NodeContextMenu.tsx`

```typescript
export function NodeContextMenu({ node, position, onClose }) {
  const { deleteNode, duplicateNode } = useWorkflowActions();
  
  const handleCopyToClipboard = () => {
    const config = JSON.stringify(node.data, null, 2);
    navigator.clipboard.writeText(config);
    message.success('Node configuration copied to clipboard');
    onClose();
  };
  
  const handleDuplicate = () => {
    duplicateNode(node.id);
    onClose();
  };
  
  const handleDelete = () => {
    const edges = getConnectedEdges(node.id);
    if (edges.length > 0) {
      carbonConfirm({
        title: 'Delete Node',
        content: `This node has ${edges.length} connection(s). Delete anyway?`,
        okText: 'Delete',
        okButtonProps: { danger: true },
        onOk: () => deleteNode(node.id),
      });
    } else {
      deleteNode(node.id);
    }
    onClose();
  };
  
  return (
    <Menu items={[
      { key: 'copy', label: 'Copy to Clipboard', onClick: handleCopyToClipboard },
      { key: 'duplicate', label: 'Duplicate Node', onClick: handleDuplicate },
      { type: 'divider' },
      { key: 'delete', label: 'Delete', danger: true, onClick: handleDelete },
    ]} />
  );
}
```

### Delete with Edge Cleanup

```typescript
const deleteNode = useCallback((nodeId: string) => {
  setNodes((nds) => nds.filter((n) => n.id !== nodeId));
  setEdges((eds) => eds.filter((e) => e.source !== nodeId && e.target !== nodeId));
}, [setNodes, setEdges]);
```

---

## 7. AI Workflow Generator — Quality

### Enhanced System Prompt

```typescript
const WORKFLOW_GENERATOR_SYSTEM_PROMPT = `You are a workflow generation assistant.

Generate valid workflow JSON matching this schema:
{
  "pluginList": [{ "id": "...", "name": "...", "plugin": "Consumer|Message|If-Else|Function_V2|Function_V3", ... }],
  "linkList": [{ "from": "...", "to": "..." }],
  "positionList": [{ "id": "...", "x": 300, "y": 100 }]
}

Node types:
- Consumer: Fetch data from external APIs
- Message: Send messages to channels
- If-Else: Branch based on conditions
- Function_V2: Transform data with logic
- Function_V3: Advanced data transform

Layout nodes vertically (x=300, y increments by 150).
Return ONLY the JSON, no markdown fences.`;
```

### Validation Before Apply

```typescript
function validateWorkflow(workflow) {
  const errors = [];
  
  if (!workflow.pluginList || !Array.isArray(workflow.pluginList)) {
    errors.push('Missing or invalid pluginList');
  }
  
  const validPlugins = ['Consumer', 'Message', 'If-Else', 'Function_V2', 'Function_V3'];
  workflow.pluginList?.forEach((node, i) => {
    if (!validPlugins.includes(node.plugin)) {
      errors.push(`Node ${i}: invalid plugin type "${node.plugin}"`);
    }
  });
  
  const nodeIds = new Set(workflow.pluginList?.map(n => n.id) || []);
  workflow.linkList?.forEach((edge, i) => {
    if (!nodeIds.has(edge.from)) errors.push(`Edge ${i}: source "${edge.from}" not found`);
    if (!nodeIds.has(edge.to)) errors.push(`Edge ${i}: target "${edge.to}" not found`);
  });
  
  return { valid: errors.length === 0, errors };
}
```

---

## 8. Home Page — Intro Copy

**File:** `workflow-ui/src/routes/index.tsx`

```typescript
function Index() {
  return (
    <div className="p-8 max-w-2xl">
      <Typography.Title level={1} style={{ fontFamily: 'IBM Plex Sans', fontSize: '42px', fontWeight: 300, color: '#161616' }}>
        Workflow Studio
      </Typography.Title>
      
      <Typography.Paragraph style={{ fontFamily: 'IBM Plex Sans', fontSize: '16px', color: '#525252' }}>
        Design, test, and deploy message enrichment workflows with visual orchestration
      </Typography.Paragraph>
      
      <Typography.Paragraph style={{ fontFamily: 'IBM Plex Sans', fontSize: '14px', color: '#525252' }}>
        Build conditional logic flows, enrich transaction data, and route messages to multiple channels—all through an intuitive canvas interface.
      </Typography.Paragraph>
      
      <Link to="/workflows">
        <Button type="primary" size="large" style={{ height: '48px', borderRadius: 0, background: '#0f62fe' }}>
          Go to Applications
        </Button>
      </Link>
    </div>
  );
}
```

---

## 9. Brand Identity — Snail Favicon

### Favicon Files
Create:
- `public/favicon.ico` (16×16, 32×32, 48×48 multi-resolution)
- `public/favicon.svg` (scalable vector)
- `public/favicon-16x16.png`
- `public/favicon-32x32.png`
- `public/apple-touch-icon.png` (180×180)

### HTML Head Tags

**File:** `workflow-ui/index.html`

```html
<head>
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
  <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
  <link rel="apple-touch-icon" sizes="180×180" href="/apple-touch-icon.png" />
  <link rel="shortcut icon" href="/favicon.ico" />
  <title>Workflow Studio</title>
</head>
```

### Snail Icon Design
Use Unicode 🐌 or custom SVG:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
  <circle cx="32" cy="32" r="30" fill="#0f62fe"/>
  <text x="32" y="42" font-size="32" text-anchor="middle" fill="white">🐌</text>
</svg>
```

---

## Implementation Order

1. **Phase 1:** 5-layer helpers, Carbon overrides, CarbonModal
2. **Phase 2:** Rule validation, resizable drawer, read-only default
3. **Phase 3:** Palette descriptions, node context menu, edge cleanup
4. **Phase 4:** Enhanced generator prompt, Explain → Generate, home copy, favicon
5. **Phase 5:** Retrofit E2E tests, Carbon audit, screenshot baselines

---

## Testing Strategy

- Unit tests: Rule validation, workflow validation, resize calculations
- E2E tests: All existing tests retrofitted with 5-layer validation
- Manual UAT: Visual Carbon compliance, favicon visibility, home page readability

---

## Rollout Plan

1. Deploy to UAT (workflow-ui-gamma.vercel.app)
2. Run full E2E suite (Desktop + Mobile)
3. Manual Carbon audit
4. Fix gaps
5. Re-run E2E
6. Human UAT approval
7. Merge to main and deploy

---

*End of Architecture Doc v30.0*
