# PM Master — Workflow Platform (`pm-doc-master.md`)

**Document version:** 2.6  
**Last updated:** 2026-04-08  
**Status:** Draft — awaiting human approval  

**Purpose:** Single product baseline. **Filename is fixed** (`pm-doc-master.md`); only **content** and **version** change. **This file is the only bilingual PM artifact:** full **English (EN)** + full **中文 (ZH)** must stay aligned. All other `pm-doc-v*.md` slices are **English only** (see root `CLAUDE.md`).

**Domain prefixes:** **APP** = Application management · **REC** = Execution records & online semantics · **CV** = Canvas (authoring, Run, Explain)

**PM rule (see root `CLAUDE.md` → section *PM master baseline*):** Every `workflow-agent-teams/TODO.md` item requires updating this file, bumping **Document version**, and logging the change in **Revision history**.

---

# English (EN)

## 1. Product description

The **Workflow Platform** helps organizations configure **enrichment, conditional logic, and multi-channel dispatch** for transactional messages with minimal bespoke backend code.

Three product pillars:

| Pillar | Prefix | Scope |
|--------|--------|--------|
| Application management | **APP** | Register, settings, copy, configuration history & lifecycle on `/workflows` |
| Execution records | **REC** | Browse/filter/drill into `WORKFLOW_RECORD`; online API semantics that make records explainable |
| Canvas | **CV** | Visual authoring on `/workflows/$applicationName`, JSONPath rules, save, Run, Explain |

Configuration (control plane) and runtime (online plane) are separate responsibilities with shared data.

## 2. Users & roles

| Role | Focus |
|------|--------|
| Integration engineer | **APP** + **CV** |
| Application developer | **REC** (contracts) |
| Ops / QA / Analyst | **REC** + **CV** (test run) |
| Product / business (read-heavy) | **CV** (Explain) |

## 3. User stories & acceptance criteria

### [APP] 3.1 Application management

#### APP-US-01 — Browse applications and open one

> As an integration engineer, I want to browse the application list on desktop and mobile browsers and open a target application so that I am not blocked by device.

**Desktop:** APP-AC-01-D1 name, enabled, description, last modified · APP-AC-01-D2 pagination (default 5) + fuzzy search · APP-AC-01-D3 table scrolls inside layout + total count · APP-AC-01-D4 empty state  

**Mobile:** APP-AC-01-M1 cards with same core fields, scrollable · APP-AC-01-M2 equivalent find/pagination · APP-AC-01-M3 desktop-table vs card toggle, preference persisted · APP-AC-01-M4 empty state  

**Both:** APP-AC-01-G1 explicit “open application” affordance (row vs card body per design) · APP-AC-01-G2 must not regress **CV** pan/zoom/nodes  

#### APP-US-02 — Create application

**Desktop:** APP-AC-02-D1 create entry, unique name, navigate to **CV** · APP-AC-02-D2 duplicate name handling  

**Mobile:** APP-AC-02-M1 FAB (or equivalent), same flow · APP-AC-02-M2 FAB draggable, position persisted, tap vs drag distinct · APP-AC-02-M3 same rules as desktop  

**Both:** APP-AC-02-G1 no device-only completion path  

#### APP-US-03 — Delete application

**Desktop:** APP-AC-03-D1 delete + confirm  

**Mobile:** APP-AC-03-M1 ⋯ menu delete, same semantics as desktop  

**Both:** APP-AC-03-G1 remove entity + workflow definition · APP-AC-03-G2 not blocked by history records · APP-AC-03-G3 delete not conflated with “open **CV**”  

#### APP-US-18 — Edit application settings

**Desktop:** APP-AC-18-D1 settings entry, full `WorkflowEntitySetting` fields · APP-AC-18-D2 save feedback, inline validation  

**Mobile:** APP-AC-18-M1 same field coverage (full-screen form or drawer) · APP-AC-18-M2 same rules  

**Both:** APP-AC-18-G1 list reflects changes · APP-AC-18-G2 settings independent of canvas entry  

#### APP-US-10 — Copy workflow across applications

**Desktop:** APP-AC-10-D1 row/toolbar copy, prefill source · APP-AC-10-D2 modal flow per product  

**Mobile:** APP-AC-10-M1 ⋯ copy, same flow · APP-AC-10-M2 stay in **APP** context; no auto-navigate to **CV** unless user chooses  

**Both:** APP-AC-10-G1 entity settings + full definition including **CV** layout; API per contract  

#### APP-US-11 — Configuration history & rollback

**Desktop:** APP-AC-11-D1 history entry, paginated list/detail · APP-AC-11-D2 rollback workflow definition only, new revision  

**Mobile:** APP-AC-11-M1 ⋯ history, equivalent decision-critical info · APP-AC-11-M2 no auto **CV** unless user opens app  

**Both:** APP-AC-11-G1 **CV** matches rolled-back definition after entry  

### [REC] 3.2 Execution records

#### REC-US-19 — Browse, filter, drill into records

**Desktop:** REC-AC-19-D1 dedicated page/entry, required columns · REC-AC-19-D2 filters, pagination, vertical scroll, total · REC-AC-19-D3 Confirmation/Tracking/Customer ID visible by default policy  

**Mobile:** REC-AC-19-M1 equivalent filter dimensions · REC-AC-19-M2 scrollable list/cards, key fields visible · REC-AC-19-M3 detail parity with desktop commitments  

**Both:** REC-AC-19-G1 parent/child & retry chain understandable · REC-AC-19-G2 read API; no **CV** regression  

#### REC-US-12 — Submit execution (API contract)

*Terminal-agnostic.*

REC-AC-12-1 path, params, JSON/XML, correlation ID · REC-AC-12-2 async/sync per entity setting, documented · REC-AC-12-3 M0001/M0002 semantics, no duplicate business execution  

#### REC-US-13 — Enrichment semantics (explainability)

*Terminal-agnostic.*

REC-AC-13-1 order before dispatch · REC-AC-13-2 JSONPath all-match; merge to payload · REC-AC-13-3 IFELSE Logic only on match · REC-AC-13-4 status interpretable  

#### REC-US-14 — Dispatch semantics

*Terminal-agnostic.*

REC-AC-14-1 after enrichment · REC-AC-14-2 tracking & channel statuses · REC-AC-14-3 extensible channels  

#### REC-US-15 — Idempotency

*Terminal-agnostic.*

REC-AC-15-1 unique key · REC-AC-15-2 duplicate handling · REC-AC-15-3 retry bypass per contract  

#### REC-US-16 — Retry

*Terminal-agnostic.*

REC-AC-16-1 retry records · REC-AC-16-2 counters & terminal state  

### [CV] 3.3 Canvas

#### CV-US-04 — Open & navigate canvas

**Desktop:** CV-AC-04-D1 graph, types (CONSUMER, CONSUMERWITHOUTERROR, MESSAGE, IFELSE, FUNCTION_V2, FUNCTION_V3), no START · CV-AC-04-D2 pan/zoom  

**Mobile:** CV-AC-04-M1 same types, touch pan/zoom · CV-AC-04-M2 escape hatch to **APP** list  

**Both:** CV-AC-04-G1 entry aligned with APP-US-01  

#### CV-US-05 — Add step

**Desktop:** CV-AC-05-D1 sidebar, drag create · **Mobile:** CV-AC-05-M1 equivalent type set, non-drag allowed · **Both:** CV-AC-05-M2 / G1 new node selectable  

#### CV-US-06 — Connect steps

**Desktop:** CV-AC-06-D1 handles, arrows, `logicOrder` · **Mobile:** CV-AC-06-M1 touch-safe wiring · **Both:** CV-AC-06-G1 same persistence rules  

#### CV-US-07 — Configure step (incl. JSONPath)

**Desktop:** CV-AC-07-D1 drawer skeleton + HTTP/logic split · CV-AC-07-D2 reload saved values  

**Mobile:** CV-AC-07-M1 full field parity, scroll/focus · **Both:** CV-AC-07-G1 AND rules match REC-US-13  

#### CV-US-08 — Delete node

**Desktop:** CV-AC-08-D1 keyboard/UI delete · **Mobile:** CV-AC-08-M1 no physical keyboard required · **Both:** CV-AC-08-G1 remove incident edges  

#### CV-US-09 — Save workflow

**Both:** CV-AC-09-G1 API save, success/fail, no data loss · CV-AC-09-G2 save reachable on small screens  

#### CV-US-30 — Three panels & JSON formatting

**Desktop:** CV-AC-30-D1 Description/Rules/Action + JSON blur format · **Mobile:** CV-AC-30-M1 same structure, long JSON usable  

#### CV-US-31 — Cross-audience clarity

**Both:** CV-AC-31-G1 subtitles · CV-AC-31-G2 field mapping hints  

#### CV-US-17 — Run from canvas

**Desktop:** CV-AC-17-D1 modal, execute, response · **Mobile:** CV-AC-17-M1 parity, scrollable modal  

#### CV-US-20 — Explain workflow

**Desktop:** CV-AC-20-D1 Explain beside Run, token flow, rich context, safe rendering · **Mobile:** CV-AC-20-M1 parity  

**Both:** CV-AC-20-G1 no token to operation/online backends · CV-AC-20-G2 Anthropic/GitHub prompt parity · CV-AC-20-G3 `gho_` etc. accepted like PAT where product supports GitHub path  

#### CV-US-21 — GitHub auth for Explain

**Desktop:** CV-AC-21-D1 device flow, auto-continue · **Mobile:** CV-AC-21-M1 same flow on mobile browser · **Both:** CV-AC-21-G1 E2E verifiable on deployed frontend  

## 4. Non-functional (product-facing)

Dual-end core paths for **APP**/**CV**/**REC** UI stories; **REC** contracts documented; audit; data protection; clear error semantics; extensible channels.

## 5. Out of scope (current)

RBAC, batch execution, canvas step replay, Explain server-held tokens / extra providers unless scoped, native apps (web-only baseline).

## 6. Decision log (aligned with v1.2)

**REC** records in scope (**REC-US-19**). **CV** FUNCTION_V2/V3, not v1. **APP** full settings in UI (**APP-US-18**). Retries per app. Channel model extensible.

## 7. Revision history

| Doc ver | Date | TODO / trigger | US/AC touched |
|---------|------|----------------|---------------|
| 2.6 | 2026-04-08 | Policy: only `pm-doc-master.md` is bilingual; `pm-doc-v*.md` English only | Header purpose + CLAUDE.md alignment |
| 2.5 | 2026-04-08 | Baseline: merge APP/REC/CV master + bilingual | Full document initial EN/ZH |

---

# 中文（ZH）

## 1. 产品描述

**Workflow 平台** 支持对交易类消息进行 **可配置丰富化、条件判断与多通道下发**，在少写定制后端的前提下完成集成与运维。

三大功能线：

| 功能线 | 前缀 | 范围 |
|--------|------|------|
| 应用管理 | **APP** | 在 `/workflows` 的应用注册、设置、复制、配置历史与生命周期 |
| 运行记录管理 | **REC** | `WORKFLOW_RECORD` 的浏览、筛选、下钻；在线接口语义使记录可解释 |
| 画布 | **CV** | `/workflows/$applicationName` 可视化编排、JSONPath 规则、保存、Run、Explain |

配置态与运行态职责分离、数据贯通。

## 2. 用户与角色

| 角色 | 侧重 |
|------|------|
| 集成工程师 | **APP** + **CV** |
| 应用开发者 | **REC**（契约） |
| 运维 / QA / 分析 | **REC** + **CV**（试跑） |
| 产品 / 业务（偏只读） | **CV**（Explain） |

## 3. 用户故事与验收标准

### 【APP】3.1 应用管理

#### APP-US-01 — 浏览应用列表并进入目标应用

> **作为** 集成工程师，**我希望** 在桌面与移动浏览器上浏览清单并进入目标应用，**以便** 不因设备受限。

**桌面：** APP-AC-01-D1 名称、启用、描述、最后修改 · APP-AC-01-D2 分页（默认 5）+ 模糊搜索 · APP-AC-01-D3 表格在版心内纵向可滚 + 总数 · APP-AC-01-D4 空状态  

**移动：** APP-AC-01-M1 卡片同等核心信息、可滚动 · APP-AC-01-M2 等价查找/分页 · APP-AC-01-M3 桌面表/卡片切换，偏好持久化 · APP-AC-01-M4 空状态  

**通用：** APP-AC-01-G1 明确的「进入应用」入口（行/卡片主体以设计为准）· APP-AC-01-G2 不削弱 **CV** 平移/缩放/节点  

#### APP-US-02 — 创建应用

**桌面：** APP-AC-02-D1 创建入口、唯一名、进入 **CV** · APP-AC-02-D2 重名处理  

**移动：** APP-AC-02-M1 FAB 等、同流程 · APP-AC-02-M2 FAB 可拖、位置记忆、点按与拖动区分 · APP-AC-02-M3 与桌面规则一致  

**通用：** APP-AC-02-G1 不要求仅在单一终端才能完成  

#### APP-US-03 — 删除应用

**桌面：** APP-AC-03-D1 删除 + 确认  

**移动：** APP-AC-03-M1 ⋯ 删除，与桌面语义一致  

**通用：** APP-AC-03-G1 删除实体与关联定义 · APP-AC-03-G2 不因历史记录禁止 · APP-AC-03-G3 删除与「进 **CV**」入口不混同  

#### APP-US-18 — 编辑应用设置

**桌面：** APP-AC-18-D1 设置入口、完整实体字段 · APP-AC-18-D2 保存反馈、行内校验  

**移动：** APP-AC-18-M1 字段覆盖面与桌面一致 · APP-AC-18-M2 规则一致  

**通用：** APP-AC-18-G1 列表即时反映 · APP-AC-18-G2 设置与进画布分离  

#### APP-US-10 — 跨应用复制工作流

**桌面：** APP-AC-10-D1 行/工具栏复制、预填源 · APP-AC-10-D2 弹窗流程  

**移动：** APP-AC-10-M1 ⋯ 复制、同流程 · APP-AC-10-M2 留在 **APP** 上下文，不自动进 **CV**（除非用户另行进入）  

**通用：** APP-AC-10-G1 实体设置 + 完整定义（含 **CV** 布局），接口按契约  

#### APP-US-11 — 配置历史与回滚

**桌面：** APP-AC-11-D1 历史入口、分页列表/详情 · APP-AC-11-D2 仅回滚工作流定义、产生新修订  

**移动：** APP-AC-11-M1 ⋯ 历史，关键决策信息与桌面等价 · APP-AC-11-M2 不自动进 **CV**  

**通用：** APP-AC-11-G1 进入 **CV** 后与回滚结果一致  

### 【REC】3.2 运行记录管理

#### REC-US-19 — 浏览、筛选、下钻执行记录

**桌面：** REC-AC-19-D1 独立页/入口、列字段 · REC-AC-19-D2 筛选、分页、纵滚、总数 · REC-AC-19-D3 确认号/跟踪号/客户 ID 默认策略下可见  

**移动：** REC-AC-19-M1 同等筛选维度 · REC-AC-19-M2 列表/卡片可浏览、关键字段优先 · REC-AC-19-M3 详情与桌面承诺一致  

**通用：** REC-AC-19-G1 父子/重试链可理解 · REC-AC-19-G2 只读接口；不削弱 **CV**  

#### REC-US-12 — 提交执行请求（接口契约）

*与终端形态无关。*

REC-AC-12-1 路径、参数、JSON/XML、关联 ID · REC-AC-12-2 异步/同步与实体设置一致且文档化 · REC-AC-12-3 M0001/M0002、不重复业务执行  

#### REC-US-13 — 丰富化语义（可解释性）

*与终端无关。*

REC-AC-13-1 顺序在派发前 · REC-AC-13-2 JSONPath 全匹配、合并负载 · REC-AC-13-3 IFELSE 仅匹配时 Logic · REC-AC-13-4 状态可辨识  

#### REC-US-14 — 下发语义

*与终端无关。*

REC-AC-14-1 丰富化后 · REC-AC-14-2 tracking 与通道状态 · REC-AC-14-3 通道可扩展  

#### REC-US-15 — 幂等

*与终端无关。*

REC-AC-15-1 唯一键 · REC-AC-15-2 重复处理 · REC-AC-15-3 重试绕过按契约  

#### REC-US-16 — 重试

*与终端无关。*

REC-AC-16-1 重试记录 · REC-AC-16-2 计数与终态  

### 【CV】3.3 画布

#### CV-US-04 — 打开并浏览画布

**桌面：** CV-AC-04-D1 有向图、类型齐全、无 START · CV-AC-04-D2 平移缩放  

**移动：** CV-AC-04-M1 同等类型、触控平移缩放 · CV-AC-04-M2 可返回 **APP** 列表  

**通用：** CV-AC-04-G1 与 APP-US-01「进入应用」一致  

#### CV-US-05 — 添加步骤

**桌面：** CV-AC-05-D1 侧栏、拖拽创建 · **移动：** CV-AC-05-M1 类型集合等价、允许非拖拽 · **通用：** 新节点可选可配  

#### CV-US-06 — 连接步骤

**桌面：** CV-AC-06-D1 句柄、箭头、`logicOrder` · **移动：** CV-AC-06-M1 触控可连线 · **通用：** 持久化规则一致  

#### CV-US-07 — 配置步骤（含 JSONPath）

**桌面：** CV-AC-07-D1 抽屉骨架与 HTTP/逻辑分组 · CV-AC-07-D2 再开显示已存值  

**移动：** CV-AC-07-M1 字段与桌面一致、滚动/焦点可用 · **通用：** CV-AC-07-G1 AND 规则与 REC-US-13 一致  

#### CV-US-08 — 删除节点

**桌面：** CV-AC-08-D1 键盘/UI · **移动：** CV-AC-08-M1 不依赖物理键盘 · **通用：** CV-AC-08-G1 删关联边  

#### CV-US-09 — 保存工作流

**通用：** CV-AC-09-G1 调管控 API、成功失败提示、不丢编辑 · CV-AC-09-G2 窄屏保存可达  

#### CV-US-30 — 三区与 JSON 排版

**桌面：** CV-AC-30-D1 Description/Rules/Action + JSON 失焦排版 · **移动：** CV-AC-30-M1 结构保留、长 JSON 可用  

#### CV-US-31 — 多角色可读

**通用：** CV-AC-31-G1 副标题 · CV-AC-31-G2 字段映射提示  

#### CV-US-17 — 画布试跑

**桌面：** CV-AC-17-D1 弹窗、执行、响应 · **移动：** CV-AC-17-M1 能力对等、可滚动关闭  

#### CV-US-20 — AI 说明（Explain）

**桌面：** CV-AC-20-D1 Explain 与 Run 并列、令牌引导、上下文完整、安全渲染 · **移动：** CV-AC-20-M1 信息完整度对等  

**通用：** CV-AC-20-G1 令牌不经 operation/online · CV-AC-20-G2 Anthropic/GitHub 提示完整度一致 · CV-AC-20-G3 设备流等令牌与 PAT 同等可用（产品已支持路径下）  

#### CV-US-21 — Explain 的 GitHub 授权

**桌面：** CV-AC-21-D1 设备流、成功后自动继续 · **移动：** CV-AC-21-M1 移动浏览器同流程 · **通用：** CV-AC-21-G1 正式前端环境可端到端验收  

## 4. 非功能性需求（产品口径）

**APP/CV/REC** 界面类故事双端核心路径一致；**REC** 契约文档化；审计；数据保护；错误语义清晰；通道可扩展。

## 5. 范围外（当前）

统一 RBAC、批量执行、画布逐步回放、Explain 服务端代管令牌/扩展模型商（除非单独立项）、独立原生 App（本文以 Web 为基线）。

## 6. 决策记录（与 v1.2 对齐）

**REC** 记录纳入范围（**REC-US-19**）。**CV** FUNCTION_V2/V3，不支持 v1。**APP** 实体设置全量 UI（**APP-US-18**）。重试按应用。通道模型可扩展。

## 7. 修订记录

| 文档版本 | 日期 | TODO / 触发说明 | 涉及 US/AC |
|----------|------|-----------------|------------|
| 2.6 | 2026-04-08 | 规则：仅主文档双语；`pm-doc-v*.md` 仅英文 | 文首说明 + CLAUDE 对齐 |
| 2.5 | 2026-04-08 | 基线：合并 APP/REC/CV 主文档 + 双语 | 全文初版 EN/ZH |

---

*End of `pm-doc-master.md` — **only this file** is bilingual EN+ZH; `pm-doc-v*.md` are English only. Bump **Document version** and **Revision history** on every `TODO.md` change per root `CLAUDE.md` (PM master baseline).*
