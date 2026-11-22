# social-adapter/V0 · Brief 如何进入 Workfolk

场景调用者提交包含来源文本和验收要求的 Brief。适配代码建立 Goal，研究活动交付成果，写作活动读取该 Artifact 后交付本地草稿。缺少材料或研究失败时保留已有执行事实，写作保持等待或结束并说明原因。

World 负责 [通用状态与成果归属](../../../contracts/world-state.md)，社交场景负责 Brief、研究输入与正文要求。现有 `SocialBrief` 和 `goalFromBrief` 来自相邻运行仓库的 `src/workfolk/adapters/social-media.ts`。V1 用两个确定性本地工具在真实 World 中走通研究和写作，发布权限留在现有社交应用。[V1 实施计划](../v1/plan.md)
