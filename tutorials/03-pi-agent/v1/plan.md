# pi-agent/V1 · 用 Pi Agent 执行已注册工具

[Pi Agent 与工具执行](../plan.md) · 后端 · 实施规划 · 项目 V03

脚本 Agent 能交付成果。现有 Pi 原型的检查只验证了固定回复映射。完成后，真实 Pi 循环会执行所注册的本地工具，并通过 World 保存结果。

前置版本　[pi-agent/V0](../v0/plan.md) · [world-runtime/V3](../../01-world-runtime/v3/plan.md)。实施基线以上述前置实际验收的源码为准。

## 实施顺序

在 `pi-controller.ts` 建立 Pi `Agent` 会话并注册以 TypeBox 声明参数的 `AgentTool`，在 `controllers.ts` 接入 `PiActivityRunner`，复用 `tools.ts` 的本地工具。用 Pi 1.4.0 的流接口在测试里返回带工具调用的助手消息。 [现有章节草稿](pi-controller.md)

## 验收场景

在 `workfolk/` 中运行下列检查。对应正文需要交付完整代码和能满足验收场景的检查文件。

```bash
node --experimental-strip-types tests/test_v5_tool_boundary.mjs
```

真实 Pi Agent 调用实际工具，World 记录成果。只输出文本或工具失败时没有新成果，失败原因可见。离线检查替换的是模型传输。

项目 V03 将这个执行能力与查看器联验。
