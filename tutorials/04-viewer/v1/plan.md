# viewer/V1 · 从浏览器观察、回放并干预工作

[工作台与玩家干预](../plan.md) · 前端 · 实施规划 · 项目 V03

现有页面只展示固定示例帧。完成后，浏览器可以从本地 World 读取事实、发出指令，并回看同一次运行。

前置版本　[viewer/V0](../v0/plan.md) · [world-runtime/V3](../../01-world-runtime/v3/plan.md) · [world-replay/V1](../../02-world-replay/v1/plan.md)。实施基线以上述前置实际验收的源码为准。

## 实施顺序

在新增的 `src/workfolk/viewer-server.ts` 实现本地 HTTP 入口，在 `viewer/app.js` 和 `viewer/index.html` 接上实时视图和操作反馈。由 `replay-state.js` 管理回放位置。接口按查看器 V0 的草案定稿。 [现有章节草稿](replay-viewer.md)

## 验收场景

在 `workfolk/` 中运行下列检查。对应正文需要交付完整代码和能满足验收场景的检查文件。

```bash
node --experimental-strip-types tests/test_v6_viewer.mjs
```

在真实浏览器发送消息并确认 Agent 消费，再观察暂停或改派生效。旧响应不会覆盖较新的 revision。播放服务端真实运行生成的回放。

项目 V04 可沿用同一工作台观察内容场景。
