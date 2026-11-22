# 教程路线图

五个专题共用一个 Workfolk 运行仓库和一条贯穿案例。玩家给出目标，Agent 交付简报，其他 Agent 消费这份成果。后续加上回放、Pi 工具、玩家干预和本地内容场景。

专题版本如 `pi-agent/V1` 表示一项能力的进度。项目里程碑如 `V03` 表示几个专题组合后可以实际做什么。按专题阅读，按前置依赖动手。[项目总计划](../plan.md)

## 从专题进入

| 专题计划 | 分版本计划 |
| --- | --- |
| [共享世界与 Agent 协作](01-world-runtime/plan.md) | [world-runtime/V0](01-world-runtime/v0/plan.md) · [world-runtime/V1](01-world-runtime/v1/plan.md) · [world-runtime/V2](01-world-runtime/v2/plan.md) · [world-runtime/V3](01-world-runtime/v3/plan.md) |
| [事件日志与回放](02-world-replay/plan.md) | [world-replay/V0](02-world-replay/v0/plan.md) · [world-replay/V1](02-world-replay/v1/plan.md) |
| [Pi Agent 与工具执行](03-pi-agent/plan.md) | [pi-agent/V0](03-pi-agent/v0/plan.md) · [pi-agent/V1](03-pi-agent/v1/plan.md) |
| [工作台与玩家干预](04-viewer/plan.md) | [viewer/V0](04-viewer/v0/plan.md) · [viewer/V1](04-viewer/v1/plan.md) |
| [社交内容场景](05-social-adapter/plan.md) | [social-adapter/V0](05-social-adapter/v0/plan.md) · [social-adapter/V1](05-social-adapter/v1/plan.md) |

V0 是设计和契约入口。当前实现章保留原型草稿，下面的检查是修订后的验收目标。

## 项目里程碑

| 里程碑 | 本次组合的专题版本 | 验收场景 |
| --- | --- | --- |
| V01 · 第一次自主交付 | `world-runtime/V1` | 给定文本和目标，一个脚本 Agent 产出简报。可核对它的执行和成果。 |
| V02 · 协作与回看 | `world-runtime/V2–V3`、`world-replay/V1` | 两个 Agent 处理争用，成果到达后下游开始。另一进程回放这次运行并核对结果。 |
| V03 · Pi 执行与玩家干预 | `pi-agent/V1`、`viewer/V1` | Pi 通过已注册的本地工具交付。玩家在浏览器给正在运行的世界发指令，能核对生效结果。 |
| V04 · 跑通内容场景 | `social-adapter/V1` | 在同一个项目中提交 Brief，核对研究产物、草稿及其输入关系。 |

项目里程碑依次累计。社交场景的首版可在命令行验证，查看器和 Pi 随项目里程碑联验。

## 专题版本与依赖

表中每个实现版也承接自己的前一版设计或代码。跨专题前置指实现和验收真正用到的能力。

| 实现版本 | 跨专题前置 | 可观察变化 |
| --- | --- | --- |
| [world-runtime/V1](01-world-runtime/v1/plan.md) | 初始提交 `ca20e2f` | 一个 Agent 自主交付简报 |
| [world-runtime/V2](01-world-runtime/v2/plan.md) | 无 | 两个 Agent 争用独占资源 |
| [world-runtime/V3](01-world-runtime/v3/plan.md) | 无 | 真实成果让下游活动就绪 |
| [world-replay/V1](02-world-replay/v1/plan.md) | `world-runtime/V3` | 第二进程核对运行轨迹 |
| [pi-agent/V1](03-pi-agent/v1/plan.md) | `world-runtime/V3` | Pi 执行工具，World 记录结果 |
| [viewer/V1](04-viewer/v1/plan.md) | `world-runtime/V3`、`world-replay/V1` | 真实浏览器干预当前活动和查看回放 |
| [social-adapter/V1](05-social-adapter/v1/plan.md) | `world-runtime/V3` | 同一 Workfolk World 交付研究成果与本地草稿 |
