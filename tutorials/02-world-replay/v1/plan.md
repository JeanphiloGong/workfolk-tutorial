# world-replay/V1 · 用另一进程重放真实运行

[事件日志与回放](../plan.md) · 后端 · 实施规划 · 项目 V02

World 的事件只在当前进程可见。完成后，第二个进程能从日志核对同一段运行。

前置版本　[world-replay/V0](../v0/plan.md) · [world-runtime/V3](../../01-world-runtime/v3/plan.md)。实施基线以上述前置实际验收的源码为准。

## 实施顺序

在 `journal.ts` 的 `Journal` 记录初始快照和每次转换，在 `replay.ts` 的 `replay` 逐条复用 World 转换。把一次真实协作运行和独立进程的读取入口接进检查。 [现有章节草稿](replay.md)

## 验收场景

在 `workfolk/` 中运行下列检查。对应正文需要交付完整代码和能满足验收场景的检查文件。

```bash
node --experimental-strip-types tests/test_v4_replay.mjs
```

两进程的最终状态和哈希相同。日志不完整或版本不支持时指出失败位置。回放只使用已记录的输入和结果。

查看器 V1 从已校验的日志生成回放帧。
