# world-runtime/V0 · 共享世界如何推进工作

玩家给出目标和验收条件。一个 Agent 看到目标，自主选择能够做的活动。World 接收动作并校验前置，随后执行者调用自己的工具，交付可消费的成果。另一个 Agent 观察到成果后可以提出新活动。没有输入或资源的活动等待，其他活动仍可推进。

| 对象 | 在场景里的作用 |
| --- | --- |
| `Goal` | 定义希望得到的结果和执行预算 |
| `Agent` | 观察世界，并在私有上下文决定下一步 |
| `Activity`、`Execution` | 表示要做的工作和一次实际运行 |
| `Action`、`Event` | 请求一次世界操作，记录已经确认的结果 |
| `Artifact`、`Resource` | 保存可交接的成果版本，协调有限的共享能力 |

World 拥有共享状态。执行消耗计入对应 Goal。World 拒绝业务动作时追加 `action_rejected` 事件并推进 revision，业务集合保持原状。第一版围绕一份文本产生一份简报。资源争用和成果触发的交接分别在 V2 和 V3 展开。

[共享状态契约](../../../contracts/world-state.md) · [Agent 契约](../../../contracts/agent-model.md) · [动作与事件契约](../../../contracts/interaction.md) · [V1 实施计划](../v1/plan.md)
