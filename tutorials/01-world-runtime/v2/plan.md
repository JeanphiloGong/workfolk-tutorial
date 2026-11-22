# world-runtime/V2 · 让两个 Agent 协调一个共享资源

[共享世界与 Agent 协作](../plan.md) · 后端 · 实施规划 · 项目 V02

上一版没有资源争用。完成后，一个 Agent 等待独占资源时，另一个仍能完成自己的工作。

前置版本　[world-runtime/V1](../v1/plan.md)。实施基线以上述前置实际验收的源码为准。

## 实施顺序

在 `world.ts` 的 `reserve_resource` 和 `release_resource` 分支处理资源占用。使用 `resources.ts` 的 `exclusiveResource` 创建容量为一的本地资源，接通第二个 Agent 的等待与重新申请。 [现有章节草稿](shared-resource.md)

## 验收场景

在 `workfolk/` 中运行下列检查。对应正文需要交付完整代码和能满足验收场景的检查文件。

```bash
node --experimental-strip-types tests/test_v2_shared_resource.mjs
```

第一个占用者可以继续工作。第二个在释放前无法进入需要该资源的执行，释放后能重新得到资源。

协作 V3 复用资源阻塞条件，区分资源等待与产物等待。
