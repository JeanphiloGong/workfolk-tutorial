# V1：一个 Agent 自主完成一项活动

## 目标和基线

V0 已经确定 Agent 拥有私有执行上下文，World 负责共享事实。本版从
`workfolk/src/workfolk/index.ts` 的空入口开始，完成一条真正运行的链路：一个
确定性 Agent 观察 Goal，选择 Activity，调用本地工具，发布 Artifact，最后结束
Activity。

本版不接 Pi、浏览器、数据库或网络。先验证世界规则和 Agent 私有循环。

## 第一步：定义共享模型

在 `workfolk/src/workfolk/model.ts` 增加 `Goal`、`AgentState`、`Activity`、
`Execution`、`Artifact`、`Action` 和 `WorldState`。共享状态只能通过 `Action`
进入环境。Artifact 由 `ArtifactDraft` 计算哈希后发布，调用方不能直接改写
`WorldState`。

读者检查 `WorldState` 至少能表达以下事实：Goal 仍然开放，Agent 正在处理哪个
Activity，Execution 是否成功，以及 Artifact 的生产者和输入版本。

## 第二步：实现 World 转移

在 `workfolk/src/workfolk/world.ts` 的 `World.dispatch` 中按顺序实现：

```text
propose_activity
  -> start_execution
  -> publish_artifact
  -> complete_activity
```

每个 Action 产生一个 `Transition`。Transition 同时返回新状态、确认事件和状态
哈希。世界只接受有效的 Activity 和 Execution，重复 Artifact ID 或没有开放 Goal
的请求会产生 `action_rejected`。

## 第三步：让 Agent 自己运行

`workfolk/src/workfolk/agents.ts` 中的 `AgentRunner.run` 保存自己的
`privateContext`，然后调用 Controller 选择 Activity。工具返回后，Runner 通过
World 的 Action 发布结果：

```ts
const draft = controller.chooseActivity(world.snapshot(), agentId);
world.dispatch({kind: 'propose_activity', activity: draft});
world.dispatch({kind: 'start_execution', activityId: draft.id, executionId, toolId: tool.id});
const result = await tool.run(input);
world.dispatch({kind: 'publish_artifact', activityId: draft.id, executionId, artifact});
world.dispatch({kind: 'complete_activity', activityId: draft.id});
```

这段代码说明 Agent 可以自主推进内部工作，而共享结果仍然必须经过 World。

## 检查点

运行：

```bash
node --experimental-strip-types tests/test_v1_single_agent.mjs
```

预期输出为 `V1 single-agent execution passed`。测试还确认事件顺序是
`activity_proposed`、`execution_started`、`artifact_published`、
`activity_completed`，并确认 Agent 最后回到 `idle`。

V2 将复用这些模型，加入两个 Agent 对同一资源的竞争。
