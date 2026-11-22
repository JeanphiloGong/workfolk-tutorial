# world-runtime/V3 · 事件驱动的动态活动

> 原型草稿。受审查后的范围和重放要求约束，见[本版实施计划](plan.md)。

## 目标和基线

V2 仍然由测试代码预先写出两个 Activity。本版增加 Supervisor Controller，让
产物事件能够打开新的消费活动。世界不再假设一个固定的研究、写作、审核顺序。

## 实现

`workfolk/src/workfolk/activities.ts` 提供 `consumerActivity` 和
`SupervisorController.proposeConsumer`。Supervisor 读取 Artifact 的来源 Activity
和 Goal，再为拥有写作能力的 Agent 提交一个输入绑定到该 Artifact 的 Activity。

若输入还不存在，Activity 会进入 `waiting`。输入产生后，调用
`World.dispatch({kind: 'reconcile'})`，环境把满足输入条件的 Activity 变成
`ready` 并发出 `activity_ready`。

独立 Activity 不会因为消费者等待而被阻塞。这个性质由 World 中逐个检查等待活动
的逻辑保证，而不是由 Supervisor 写死一条流水线。

## 检查点

运行：

```bash
node --experimental-strip-types tests/test_v3_dynamic_activities.mjs
```

预期输出为 `V3 dynamic activity routing passed`。测试确认消费者引用真实
Artifact，独立 Activity 仍然是 `ready`。

接下来可阅读[world-replay/V1 的实施计划](../../02-world-replay/v1/plan.md)。
