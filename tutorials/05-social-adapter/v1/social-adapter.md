# social-adapter/V1 · 社交内容适配器

> 原型草稿。受审查后的范围和重放要求约束，见[本版实施计划](plan.md)。

## 目标和边界

Workfolk 只负责协作机制。社交内容项目继续拥有 Brief、Research、Draft、Review、
授权和发布记录。本版加入纯映射适配器，让社交 Brief 可以进入 Workfolk，而不把
社交领域对象复制进通用世界。

## 实现

`workfolk/src/workfolk/adapters/social-media.ts` 提供：

- `goalFromBrief`，把主题、读者和验收条件映射为 Goal
- `researchActivity`，创建研究活动
- `writingActivity`，把研究 Artifact ID 绑定为输入
- `socialArtifact`，为不同社交产物创建通用 ArtifactDraft

适配器不执行浏览器、不发布帖子，也不伪造审核结论。真正的研究、写作和发布工具
仍由后续 Controller 通过明确的 Tool 契约接入。

## 检查点

运行：

```bash
node --experimental-strip-types tests/test_v7_social_adapter.mjs
```

预期输出为 `V7 social-media adapter passed`。测试确认 Goal ID、Activity ID 和
研究 Artifact 输入绑定一致。
