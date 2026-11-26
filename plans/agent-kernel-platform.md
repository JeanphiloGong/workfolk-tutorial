# Workfolk Agent Kernel：可编排 Agent 应用平台方案

## 1. 方案定位

本方案把 Workfolk 定义为一个可安装、可扩展的 Agent 应用平台。平台提供稳定的 Agent Kernel、App SDK、Workflow Runtime、事件与状态 API、权限和资源边界。应用作者使用这些能力构建自己的 Agent 编排应用。

Multica-style App 和 Orca-style App 是 Workfolk 的第一批内置应用，也是 Kernel 能力的验证样本。它们使用 Workfolk Kernel 和 SDK 重新实现。外部的 Multica、Orca、T3 Code、Lody 和 Raft 源码只作为领域和边界参考。

```text
Agent Kernel Platform
  ├─ Host Shell
  ├─ AI Chat
  ├─ Workflow Builder
  ├─ App Registry
  ├─ App Runtime
  ├─ Agent Runtime Adapters
  ├─ Event / State / Replay
  └─ Capability / Approval
       ├─ Multica-style App
       ├─ Orca-style App
       └─ User-created Apps
```

## 1.1 Workfolk 仓库边界

- `workfolk` 是 Workfolk Kernel、Runtime 和默认应用的运行仓库。
- `workfolk-tutorial` 是方案、契约、版本教程和验收记录的唯一文档仓库。
- 当前 `World`、`Action`、`Transition`、`WorldEvent`、`Journal`、`replay` 和 `PiController` 是 Kernel 第一阶段的参考实现。
- `Goal`、`Activity`、`Resource` 和 `Artifact` 属于 Workfolk Shared World 应用层，后续 Multica-style 和 Orca-style 应用使用同一 Kernel 扩展。

## 2. 目标

- 让用户通过对话或 Workflow Builder 创建 Agent 工作流。
- 让应用作者使用同一个 Kernel 构建不同的编排产品。
- 让 Multica-style 和 Orca-style 两个应用在不修改 Kernel 的情况下运行。
- 让用户可以用 Agent 修改应用的 Workflow、UI、Policy 和 Projection。
- 让应用共享统一的任务执行、事件记录、权限、恢复和回放机制。
- 让应用可以独立安装、升级、停用和卸载。

## 3. 非目标

- 复制外部产品的完整 UI、品牌或所有第三方集成。
- 让 Kernel 直接拥有 Issue、Project、Worker 或 Worktree 等产品业务对象。
- 让任意 App 默认拥有文件、网络、进程或凭据权限。
- 让 App 通过修改 Kernel 源码来扩展平台。
- 在第一阶段实现完整的公共应用市场、支付、评分和多租户发布系统。

## 4. 核心设计原则

### 4.1 Kernel 负责执行语义

Kernel 固定任务、运行、Session、Agent、Runtime、Tool、Event、Checkpoint、Lease、Retry 和 Approval 的通用语义。应用使用这些对象表达自己的业务流程。

### 4.2 App 负责产品语义

App 自己拥有业务对象、Workflow、页面、命令、状态投影、数据表和策略。Multica-style App 的 `Issue` 与 Orca-style App 的 `Goal` 可以有不同模型。

### 4.3 事件是观察和恢复事实

每个执行动作产生带有 `event_id`、`run_id`、`sequence`、`type` 和 `payload` 的事件。Projection 从事件构建 App 读模型。Checkpoint 保存可恢复的执行边界。

### 4.4 外部 Agent 通过 Adapter 接入

Kernel 不直接绑定 Codex、Claude 或 OpenCode。每个 Provider 通过 Runtime Adapter 接入，并将初始化、消息、工具调用、工具结果、完成和错误转换为统一事件。

### 4.5 Capability 是唯一的副作用入口

App 和 Agent 通过声明的 Capability 访问文件、Git、网络、进程、Worktree、凭据和远程主机。用户批准和 Policy 决定 Capability 是否可以使用。

## 5. 分层架构

```text
Host Shell
  → App Registry / App Lifecycle
  → App Runtime
       → Workflow Runtime
       → Command Dispatcher
       → Projection Manager
       → UI Host
  → Agent Kernel
       → Task / Run / Session
       → Event Store / Checkpoint Store
       → Runtime Adapter
       → Capability Broker
       → Approval / Review
  → External Agent CLI / Tool / Git / Remote Host
```

### 5.1 Host Shell

Host Shell 提供应用列表、安装入口、统一登录、基础对话、运行状态、权限提示和通知。它不读取 App 的私有状态表，使用 App manifest 和公开 Projection 显示应用入口。

### 5.2 App Registry

App Registry 保存 App 包、版本、Kernel 兼容范围、签名、权限声明和升级记录。第一阶段可以使用本地目录 Registry，之后再增加远程 Registry。

### 5.3 App Runtime

App Runtime 负责加载 manifest、校验 Kernel 版本、创建 App namespace、注册 Workflow、Command、Event Handler、Projection 和 UI 路由。

### 5.4 Agent Kernel

Kernel 负责执行跨 App 共用的状态转换、事件写入、运行调度、恢复、工具审计和权限检查。Kernel 不知道某个 App 是 Multica 还是 Orca。

## 6. Kernel 核心对象

| 对象 | 责任 |
| --- | --- |
| `Task` | 一个需要被执行或等待的工作单元 |
| `Run` | Task 的一次执行尝试 |
| `Session` | Agent 与 Runtime 的交互会话 |
| `Agent` | 一个可被选择的 Agent 身份和配置 |
| `Runtime` | Agent CLI 或远程执行环境 |
| `ToolCall` | 一次工具调用请求 |
| `ToolResult` | 工具调用的结构化结果 |
| `Event` | 可排序、可重放的事实记录 |
| `Checkpoint` | 可恢复执行边界 |
| `Lease` | 某个 worker 对执行权的时间性占有 |
| `Approval` | 用户或 Policy 对副作用的授权 |
| `Artifact` | 文件、Diff、日志、报告和其他运行产物 |

Kernel 对象只保存通用字段。App-specific 数据通过 namespace、metadata 或 App 自己的存储保存。

## 7. App Package 契约

一个 App 包至少包含：

```text
app/
  manifest.json
  workflows/
  commands/
  policies/
  projections/
  ui/
  skills/
  migrations/
  tests/
```

### 7.1 Manifest

```json
{
  "id": "example.orca",
  "name": "Orca",
  "version": "0.1.0",
  "kernel": { "min": "0.1.0", "max": "0.x" },
  "capabilities": ["worktree.create", "agent.dispatch", "review.read"],
  "entrypoints": {
    "workflow": "workflows/orchestration.yaml",
    "ui": "ui/index.ts"
  }
}
```

Manifest 声明 App 身份、版本、Kernel 兼容范围、Capability、Workflow 入口和 UI 入口。安装时 Kernel 先校验 manifest，再创建隔离的 App namespace。

### 7.2 Workflow

Workflow 可以使用声明式 YAML 或 SDK 代码表达：

```yaml
id: issue-delivery
trigger: issue.assigned
steps:
  - authorize
  - select_runtime
  - create_worktree
  - start_agent
  - collect_events
  - run_checks
  - request_review
  - finalize
```

Workflow Runtime 负责步骤状态、依赖、超时、重试、审批、事件和恢复。它不要求每个 App 使用同一种业务流程。

### 7.3 Command

Command 是 UI、Agent 或外部事件可以请求的 App 操作。每个 Command 声明输入 Schema、所需 Capability、幂等键、状态前置条件和产生的事件。

### 7.4 Projection

Projection 将 Kernel Event 和 App Event 转成 App 读模型。UI 读取 Projection，不直接读取执行器或 Provider 内部状态。

## 8. 两个默认验证应用

### 8.1 Multica-style App

它验证 Issue-centric 交付流程：

```text
Workspace
  → Project
  → Issue
  → Agent Assignment
  → Runtime
  → Run
  → Tool Events
  → Review Gate
```

App 自己拥有 `Issue`、`Project`、`ReviewGate` 和 `IssueProjection`。Kernel 提供 Task、Run、Runtime、Tool、Event、Approval 和 Replay。

### 8.2 Orca-style App

它验证 Goal-centric Worker 编排：

```text
Goal
  → Coordinator
  → Worker Tasks
  → Isolated Worktree
  → Worker Session
  → Diff Review
  → Cleanup
```

App 自己拥有 `Goal`、`Coordinator`、`WorkerTask`、`WorktreeReview` 和 `CleanupDecision`。Kernel 提供 Task DAG、Session、Runtime、Capability、Checkpoint、Review 和 Recovery。

### 8.3 双应用冻结验收

- 两个 App 使用同一个 Kernel 运行。
- 两个 App 使用不同的业务模型、Workflow 和 UI。
- 两个 App 可以同时安装并运行。
- 构建两个 App 时不修改 Kernel 核心语义。
- 两个 App 都能产生统一的 ToolCall、ToolResult、Event、Checkpoint 和 Approval 记录。
- 删除一个 App 不影响另一个 App 或 Kernel 的运行记录。

## 9. Agent 参与 App 开发

Agent 通过 App SDK 读取 App manifest、Workflow、Command、Projection、UI 和测试。修改流程是：

```text
用户提出修改目标
  → Agent 读取 App SDK 和目标 App
  → Agent 生成变更计划
  → 用户批准文件和 Capability 范围
  → Agent 修改 App 包
  → App 校验、类型检查和测试
  → Kernel 验证 manifest 与契约
  → 生成新 App 版本
  → 用户安装或回滚
```

Agent 修改的是 App Package。Kernel 的源代码和运行时权限由平台单独保护。

## 10. 版本路线

### K0：Kernel Contract

冻结 Task、Run、Session、Agent、Runtime、Event、ToolCall、Checkpoint、Approval 和 Capability 的最小契约，并提供内存参考实现。

验收：一个最小 Workflow 可以启动 Agent Session、记录 ToolCall/ToolResult、写入 Event，并从 Checkpoint 恢复。

### K1：Workflow Runtime

加入步骤依赖、超时、重试、Lease、取消、审批和事件回放。

验收：同一个 Kernel 可以运行线性 Workflow、并行 Worker Workflow 和失败恢复 Workflow。

### K2：App SDK 与 App Lifecycle

加入 manifest、App namespace、Command、Projection、App storage、安装、升级、卸载和回滚。

验收：两个空业务模型的 App 可以独立安装和运行，Kernel 不需要针对 App 增加条件分支。

### K3：Multica-style App

实现 Issue、Project、Agent Assignment、Run、Review Gate 和 Issue Projection。

验收：完成一次 Issue → Agent → Run → Tool Event → Review 的闭环。

### K4：Orca-style App

实现 Goal、Coordinator、Worker Task、Worktree、Worker Session、Diff Review 和 Cleanup。

验收：完成一次 Goal → Coordinator → Worker → Worktree → Review 的闭环。

### K5：Agent App Builder

让 Agent 读取 SDK、修改 App Package、执行验证并生成可安装版本。增加 App diff、权限审批、版本回滚和测试报告。

验收：不修改 Kernel，Agent 为 Orca-style App 增加一个 Worker Review 能力并成功安装新版本。

### K6：App Registry

加入本地 Registry、版本索引、签名、兼容性检查、发布和安装审计。公共市场和多租户发布延后。

验收：同一个平台可以安装、升级、回滚和卸载多个 App。

## 11. 最小垂直切片

第一条实现切片不做完整 App Store，先验证以下路径：

```text
Kernel Event Store
  → Workflow Runtime
  → Fake Agent Runtime
  → ToolCall / ToolResult
  → Checkpoint / Replay
  → Minimal App SDK
  → Multica-style sample workflow
```

第二条切片加入 Orca-style sample workflow，复用相同 Kernel。两条切片都使用 fake Agent 和临时 worktree，避免第一阶段依赖真实 Provider 或共享仓库。

## 12. 风险与控制

| 风险 | 控制 |
| --- | --- |
| App 通过扩展绕过权限 | Capability Broker 在 Kernel 边界统一校验 |
| App 直接修改 Kernel 状态 | App namespace 和 Command API 隔离写入 |
| Provider 协议差异污染内核 | Runtime Adapter 负责事件归一化 |
| Workflow 失败后无法恢复 | Event Store、Checkpoint、Lease 和幂等 Command |
| Agent 修改 App 时破坏平台 | Manifest 校验、SDK 类型检查、测试和用户审批 |
| App UI 与状态不一致 | UI 只读取 Projection，不直接读取执行器 |
| 不同 App 产生互相冲突的语义 | Kernel 只冻结通用对象，App 业务状态独立命名空间 |

## 13. 当前决策与待确认事项

已确定：

- Multica-style 和 Orca-style 是第一批默认验证应用。
- 两个应用使用本项目自己的 Kernel 和 SDK。
- 外部项目只提供源码和设计参考。
- App 自己拥有 UI，不要求平台自动生成完整产品页面。
- 用户可以通过 Agent 修改 App，但修改需要经过权限、验证和版本安装流程。

待后续确定：

- Kernel 使用 Go、TypeScript 还是跨语言协议加多语言 SDK。
- App UI 使用宿主 Web 技术、独立 WebView 还是远程页面。
- Event Store 使用 SQLite、PostgreSQL 还是可替换存储接口。
- App Registry 首先使用本地目录、Git 仓库还是 HTTP Registry。
- Capability 的审批是否支持一次性、Session 级和长期授权。

这些选择不会改变 K0 的通用对象和两应用冻结验收。
