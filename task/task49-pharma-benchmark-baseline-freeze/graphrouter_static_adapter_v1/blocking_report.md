# GraphRouter 静态适配审查 v1

日期：2026-09-29  
状态：**适配可定义，但尚未运行；不进入效果主表。**

## 1. 为什么单独审查

GraphRouter 是当前 baseline roster 中唯一尚未决定是否值得实现的静态强 router。它的作用是检查：如果允许训练区的分子--provider 成功边，图结构静态路由能否超过 Frozen selector。它不能回答 selected-only 延迟更新，也不能替代动态 JEV 对照。

## 2. 上游实现的真实接口

已核对的上游快照是 MIT 许可、commit `0edfb8758d45acd93c8029354867ffb49bb8a97f`，文件 hash 见 `baseline_audit/graphrouter/SOURCE.json`。原实现需要：

- query node embedding；
- provider/LLM description embedding；
- 训练、验证和测试 edge；
- edge 的 effect/cost 特征；
- `torch_geometric.nn.GeneralConv`。

本机 `torch`、`sklearn`、`pandas`、`yaml` 可用，但 `torch_geometric` 不可用。直接运行上游入口会改变依赖环境，而且原入口的多 task、多 scenario 和 LLM description 语义不能直接映射到单一 BBB 性质。

## 3. 合法的 BBB 适配合同（候选）

若继续实现，必须另建适配版本，不能把上游脚本直接改名后当作 GraphRouter：

1. query node：固定 Morgan-1024 分子表示；provider node：只能用运行前冻结、可公开追溯的 provider/version/family 描述。若只用 provider one-hot，必须把它报告为身份特征并做无 provider-feature 消融。
2. edge label：只从 outer-fold 的 fit 区 `system_success` 构图；validation 只用于 epoch 选择；evaluation 不回流。失败记 0，未知不造化学标签。
3. selection：对 evaluation 分子只输出当前 33-provider menu 内的 argmax；原生 index tie-break 和 tie-aware sensitivity 都要预登记。
4. 任务是静态对照。不得向 GraphRouter 注入 selected-only feedback，也不得把它放进 online JEV 的更新曲线。
5. 与 Frozen/Best Fixed 使用完全相同的 outer scaffold folds、输入 hash、菜单、failure denominator 和 confirmation freeze。

## 4. 当前阻塞是否值得解

目前有两个独立阻塞：

- **实现阻塞**：缺 `torch_geometric`；若改用独立 NumPy/PyTorch 重写，必须逐层对照上游并重新做 fidelity audit，否则只是一个自定义图模型。
- **科学阻塞**：provider 描述 embedding 和 BBB 单 task 图的节点语义尚未冻结。没有这两个原始输入，运行数字无法说明 GraphRouter，而只能说明某个临时图路由器。

因此本轮不安装依赖、不提交 A800、不生成伪效果。只有在 provider descriptor、edge 权限、tie-break 和实现 fidelity 都写进新预登记卡后，才值得跑一次 CPU/A800 静态 development。

## 5. 可复用结论

GraphRouter 可以保留为“静态图路由强对照”的候选；它不是在线 JEV baseline。若最终论文切口只聚焦 selected-only 实时适应，GraphRouter 可以在资源不足时保留论文/源码审计，并在正文明确未纳入的输入/依赖原因；若要声称静态路由全面比较，则必须完成上述适配并报告负结果。
