# world-runtime/V3 · 按产物和事件生成下一项活动

[共享世界与 Agent 协作](../plan.md) · 后端 · 实施规划 · 项目 V02

上一版的活动预先给定。完成后，协调者看见成果才提出消费活动，独立分支照常运行。

前置版本　[world-runtime/V2](../v2/plan.md)。实施基线以上述前置实际验收的源码为准。

## 实施顺序

复用 `activities.ts` 的 `SupervisorController.proposeConsumer` 和 `world.ts` 的 `reconcile`。让协调者消费真实发布事件，把已有 Artifact ID 绑定到下游输入。按阻塞原因推进活动，保留 V2 的资源条件。 [现有章节草稿](dynamic-activities.md)

## 验收场景

在 `workfolk/` 中运行下列检查。对应正文需要交付完整代码和能满足验收场景的检查文件。

```bash
node --experimental-strip-types tests/test_v3_dynamic_activities.mjs
```

缺少指定产物时消费者等待，实际发布后接续。产物出现时，等待其他独占资源的活动仍不能越过资源门槛。

回放读取这条真实的协作轨迹，Pi、查看器和内容场景共用运行规则。
