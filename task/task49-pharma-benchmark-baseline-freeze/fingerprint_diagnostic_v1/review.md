# 开跑前检查

2026-09-26，主执行者复核。请求的独立 baseline 审查代理始终处于 pending_init，已停止等待；没有独立审稿结果，不冒称双人审查。

## 数学与实现

- 线性/MLP 使用逐 provider BCE；多个 provider 同时正确是多标签，不能用一个赢家的交叉熵。
- BT 对当前分子所有 correct–wrong provider pairs 求 softplus(loser−winner)，平票不制造胜负；额外 0.2 CLS 对所有格求平均。手工四对示例检查方向、分母和辅助梯度。
- 没有已核成本顺序，故本轮不伪造 PILOT triplet hard-negative 的成本条件。PILOT 离线 fit 继续列为待完成。
- 三个评价量分清：成功调用并正确、失败、直接性质模型输出。直接性质模型只是标签访问诊断，不当工具选择者排名。

## 数据与训练

- 33 个具体 provider 全保留，选择不看当题状态。成功标记只在训练标签和 evaluator 中出现。
- 外层评价、内部验证、fit 的 scaffold 互不相交；ACYCLIC 合并为一组。旧反复查看的 BBB 仍明确 development，不称新鲜测试。
- 每个模型只有 fit tensors 进入损失反传；validation 只选 checkpoint，outer evaluation 在训练结束后一次使用。
- 所有模型相同固定预算；没有调参搜索。Best Fixed 仅从 fit 区选择，不提前知道失败/回退。
- 同一实验 2 个 seed、5 折是描述性开发诊断；无中稿或统计显著性主张。

## 执行、证据与停止

- 验证输入源 hash 并固定 git commit；只接受 A800，禁止本地 CPU 运行替代训练。
- 一个 git-only 任务，无外部模型下载、无需 OSS、无科学工具重新预测。stdout 保存逐 epoch 和每条预测以及压缩 checkpoint 分块，下载后按 hash 还原。
- 新的远程 fit 仍需 doctor 通过、查看 dry-run，再按 ADR-0022 的已有授权提交；一次一条。
- 若 stdout 被平台截断导致 raw/checkpoint 不完整，本轮不满足复现验收，不可只凭 aggregate 宣布完成。保留失败并恢复原任务日志，不因读取超时重启训练。

结论：适合以预登记配置开展一条开发诊断。可能得到负结果，也不保证能找到主创新。它回答现任务的真实可学习性，避免把旧家族投票的结果当作当前任务证据。

## 首次真实执行的入口错误与修复

任务 `01a0de12e42837ed41354d264e90b7bd` 在 2026-09-26 22:21:57 CST、任何 fit 前失败。真实 A800 可用、输入 hash 检查通过；日志配置取 git HEAD 时失败，因为 Nebula 交付的是源码归档，没有 `.git`。

修正仅为执行 provenance：新增必填 `--code-commit`，与提交时的固定 SHA 相同；若是 git checkout 则严格比对，否则记录 `local_git_head=null` 并由 Nebula task 元数据 + 实际源码 SHA-256 核验。新增 gitless 临时目录测试。数据、split、模型、优化器、epoch 和选择判据不变。失败 stdout/stderr/status 已保存于 `experiments/logs/task49_fingerprint_diagnostic_20260926/remote_v1/`；没有训练数据可挑选，不把此次失败算成负效果。
