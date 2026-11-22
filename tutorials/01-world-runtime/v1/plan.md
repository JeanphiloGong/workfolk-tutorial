# world-runtime/V1 · 让一个 Agent 自主交付简报

[共享世界与 Agent 协作](../plan.md) · 后端 · 实施规划 · 项目 V01

初始化提交 `ca20e2f` 只有 TypeScript 包、依赖锁文件和入口。完成后，一个 Agent 能观察目标、选活动、执行本地工具并交付简报。

前置版本　[world-runtime/V0](../v0/plan.md)。从 Workfolk 初始提交 `ca20e2f` 起步。

## 实施顺序

依次补齐 `model.ts` 的业务类型、`World.dispatch` 的提议和执行转换、`DeterministicSourceTool`、`AgentRunner.run` 及检查文件。源文件位于 `workfolk/src/workfolk/`。本版用一份固定文本贯通流程。 [现有章节草稿](single-agent.md)

## 验收场景

在 `workfolk/` 中运行下列检查。对应正文需要交付完整代码和能满足验收场景的检查文件。

```bash
node --experimental-strip-types tests/test_v1_single_agent.mjs
```

相同输入得到相同成果和事件顺序。直接提交无活跃执行的 Artifact 会被拒绝，工具失败只得到失败事件。

协作 V2 复用成果发布、状态转换和 Agent 执行入口。
