# world-runtime/V2 · 两个 Agent 共享资源

> 原型草稿。受审查后的范围和重放要求约束，见[本版实施计划](plan.md)。

## 目标和基线

V1 中 Agent 可以独立完成工作，但没有资源冲突。本版加入一个容量为 1 的浏览器
资源，验证一个 Agent 等待时另一个 Agent 仍能继续。

## 实现

在 `workfolk/src/workfolk/resources.ts` 用 `exclusiveResource` 创建容量为 1 的
资源。`World.dispatch` 的 `reserve_resource` 检查 `reservedBy`：资源已经被其他
Activity 占用时，当前 Activity 进入 `waiting`，并产生 `resource_unavailable`。

成功取得资源后，Activity 可以继续 `start_execution`。完成工作后必须显式发送
`release_resource`，再让等待中的 Agent 重试预留。

关键状态规则位于 `workfolk/src/workfolk/world.ts`：

```ts
if (resource.reservedBy.length >= resource.capacity) {
  activity.status = 'waiting';
  activity.blockedReason = `resource ${resource.id} is occupied`;
}
```

这个规则只修改共享事实。Agent 自己决定等待、改做其他 Activity，还是在资源释放
后重试。

## 检查点

运行：

```bash
node --experimental-strip-types tests/test_v2_shared_resource.mjs
```

预期输出为 `V2 shared-resource coordination passed`。测试确认只有一个活动预留
浏览器，另一个活动进入 `waiting`，资源释放后它可以重新进入 `ready`。

接下来可阅读[world-runtime/V3 的实施计划](../v3/plan.md)。
