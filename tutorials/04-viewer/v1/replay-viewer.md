# viewer/V1 · 只读回放查看器

> 原型草稿。受审查后的范围和重放要求约束，见[本版实施计划](plan.md)。

## 目标和基线

回放专题已经有可以回放的世界事实。原型增加了一个静态浏览器查看器，展示 Agent、当前
活动和事件时间线，支持播放、单步和重置。它不拥有领域规则，也不执行 Agent。

## 实现

`workfolk/viewer/replay-state.js` 只负责把帧索引限制在有效范围内。`app.js` 读取
`sample-replay.json`，将当前帧渲染到 DOM。播放按钮只改变查看器的指针，不能调用
`World.dispatch`。

页面入口是 `workfolk/viewer/index.html`，样式在 `styles.css`。浏览器可以用下面
的命令查看：

```bash
python3 -m http.server 8766 --bind 127.0.0.1 --directory viewer
```

## 检查点

运行：

```bash
node --experimental-strip-types tests/test_v6_viewer.mjs
node --check viewer/app.js
node --check viewer/replay-state.js
```

预期输出为 `V6 replay viewer state passed`，并且两个 JavaScript 文件语法检查
通过。浏览器验证应确认播放、单步和重置只改变显示帧。

接下来可阅读[social-adapter/V1 的实施计划](../../05-social-adapter/v1/plan.md)。
