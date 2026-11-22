# social-adapter/V1 · 让一份 Brief 跑到本地草稿

[社交内容场景](../plan.md) · 后端 · 实施规划 · 项目 V04

现有模块只检查字段映射。完成后，一份 Brief 经过实际的两个活动并产出可核对的草稿。

前置版本　[social-adapter/V0](../v0/plan.md) · [world-runtime/V3](../../01-world-runtime/v3/plan.md)。实施基线以上述前置实际验收的源码为准。

## 实施顺序

在 `adapters/social-media.ts` 复用 `goalFromBrief` 和活动映射。拟在 `adapters/social-workflow.ts` 接上研究和写作的本地工具，通过同一个 World 执行和交付。写作读取实际研究产物。 [现有章节草稿](social-adapter.md)

## 验收场景

在 `workfolk/` 中运行下列检查。对应正文需要交付完整代码和能满足验收场景的检查文件。

```bash
node --experimental-strip-types tests/test_v7_social_adapter.mjs
```

有真实研究成果、关联到正确输入的草稿，以及对应的世界事件。研究失败时没有草稿。验收到本地草稿。

项目 V04 把该场景与已有执行、回放和查看器放到同一条集成路线。
