# pi-agent/V0 · Pi 如何为 World 交付工具结果

一个 Agent 看到可做的活动后，通过 Pi 发起一次受控本地工具调用。Pi 负责会话、工具调用和结果消息，Workfolk 在 World 的执行边界检查前置并接收结果。模型的文字回复可以表述意图，成功工具结果才提供本场景的发布证据。

复用 [Agent 契约的 Pi 边界](../../../contracts/agent-model.md#pi-boundary) 和 [动作与事件契约](../../../contracts/interaction.md)。Pi 版本由运行仓库的锁文件固定为 1.4.0。离线用真实 Pi Agent 循环配模型流夹具检查成功、工具失败和只返回文本的情况。[V1 实施计划](../v1/plan.md)
