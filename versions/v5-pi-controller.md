# V5：Pi Controller 和工具边界

## 目标和基线

V1–V4 使用确定性 Controller 验证世界规则。本版使用当前项目已经采用的 Pi
Agent framework，让模型选择 Activity，同时保留 World 的写权限边界。

运行仓库的 `package.json` 固定：

```text
@earendil-works/pi-agent-core 1.4.0
@earendil-works/pi-ai 1.4.0
typebox 1.3.27
```

## 实现

`workfolk/src/workfolk/pi-controller.ts` 用 Pi 的 `Agent` 创建临时模型会话，向
它提供只读 WorldState，并要求返回结构化 Activity proposal。Pi 回调只能返回
Controller 结果，不能直接写 WorldState。

`workfolk/src/workfolk/controllers.ts` 的 `PiActivityRunner` 接收 proposal，
调用 World 的 `propose_activity` 和 `start_execution`，再把经过工具验证的结果
提交为 Artifact。模型声称已经完成而没有工具结果时，不能产生
`artifact_published`。

测试使用 `PiController` 的 `ask` 注入点返回固定 JSON。这个替身只验证 World 和
Pi 的边界，不声称完成真实模型调用。

## 检查点

运行：

```bash
node --experimental-strip-types tests/test_v5_tool_boundary.mjs
```

预期输出为 `V5 Pi controller and tool boundary passed`。真实联网模型需要后续
配置 provider 和模型 ID，本版离线检查不发送请求。

V6 将在独立查看器中消费回放，不让动画修改运行时状态。
