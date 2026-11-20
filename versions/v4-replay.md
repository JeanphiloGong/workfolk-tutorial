# V4：事件日志和确定性回放

## 目标和基线

V3 的状态只存在于当前进程。本版加入 JSONL Journal，使另一进程可以从初始快照
重新执行相同 Action，并比较每个版本的事件和状态哈希。

## 实现

`workfolk/src/workfolk/journal.ts` 保存一行 header 和若干 transition。每个
transition 保存原始 Action、revision、事件类型和 `stateSha256`。它不保存模型
思考，也不在回放时调用工具。

`workfolk/src/workfolk/replay.ts` 读取 header 建立新的 World，然后逐行调用同一个
`dispatch`。revision、事件类型和状态哈希任一不一致都会立即失败。

```ts
const result = world.dispatch(record.action);
if (result.stateSha256 !== record.stateSha256) {
  throw new Error(`state hash mismatch at ${record.revision}`);
}
```

未知日志版本、空文件和截断 JSON 都停止回放。回放是只读验证，不能重新执行 Pi、
浏览器或其他外部副作用。

## 检查点

运行：

```bash
node --experimental-strip-types tests/test_v4_replay.mjs
```

预期输出为 `V4 deterministic replay passed`。测试在临时目录写入 JSONL，再用新
World 重建相同 Artifact 和事件序列。

V5 将把 Pi 接到 Controller 边界，世界仍通过同一个 Action 入口接收结果。
