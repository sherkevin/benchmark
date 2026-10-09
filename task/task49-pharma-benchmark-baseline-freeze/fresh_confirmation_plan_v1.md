# 新方法冻结后的 confirmation 计划（候选）

日期：2026-09-29
状态：执行草案，不是决议；需要在方法冻结后逐项复核。

## 目的

把“公开真值”“selector 未读取”“provider 没有看过测试分子”三件事分开验收。当前 722 条 BBB development panel 已被多轮 selector 诊断读取，且与官方 BBB test 有 147 条严格 canonical、152 条最大碎片重叠；因此不能直接把官方 test 写成对现有方法的 untouched confirmation。

## Gate 0：固定公开真值和重复语义

1. 使用已冻结的 TDC ADMET Group archive 和四个文件 hash。
2. 对 BBB 的 8 个 train_val 冲突 canonical group 和 2 个 test 冲突 group 先登记分子及原始行；在合同中选择一种处理并锁定：
   - 以 canonical molecule 为单位排除冲突组；或
   - 将其定义为重复测量，预先规定聚合和不确定性；
   - 不允许保留第一行、按多数票或按模型表现选标签。
3. AMES 没有标签冲突，但仍要固定 23 个 canonical duplicate rows 的 molecule-level 计数规则。
4. 生成去重/排除后的 split manifest、hash 和逐分子审计表；未经这一步，不训练 provider。

## Gate 1：冻结 selector development 和确认区

- 旧 722 BBB panel 及所有由它产生的 cache、checkpoint、阈值和方法卡只可用于 development。
- 新方法冻结后才创建 confirmation manifest；confirmation 分子、scaffold、provider menu、反馈顺序和 seed 一次写入。
- 如果继续使用官方 BBB test，先排除 2 个冲突 group，再排除旧 panel 的 147 个严格 canonical overlap；这样剩下 245 个 molecule-disjoint candidate，但其中 235 个仍与旧 panel 共享非手性 scaffold。若要作更强的结构独立确认，应再排除这些 scaffold，只保留 157 个 candidate。两种口径都要在结果中分别报告，不能把 245 个直接写成 scaffold-independent。
- confirmation manifest 写入前不打开 test correctness；所有 provider 输出先由 evaluator 私有保存。

## Gate 2：只纳入 checkpoint 可绑定的 provider

当前 33 个缓存 provider 的 checkpoint-specific 独立资格仍为 0（GraphB3 为 G3-paper-backed-observational；ADMET-AI/DeepPurpose/LGBM/B3clf 为 G2；FP-ADMET/ADMETlab 为 G1），所以不能直接把它们的旧 checkpoint 当作 model-fresh confirmation。

最小可行路线是从开源实现构造一个小而可审计的 provider menu：每个 provider 在同一份冻结 `train_val` 上重新训练，保存源码 commit、数据行 manifest、内层 train/validation split、checkpoint hash、端点阈值和运行环境；confirmation test 只做前向推理。候选 family 可以覆盖图模型、分子指纹模型、树/线性模型和一个现有 B3DB 系模型，但具体数量要在训练成本和独立性审查后冻结。

旧缓存 provider 可以作为 lineage-aware stress control；不能与重新训练 provider 混在同一“独立主结果”列中，也不能把同一 B3DB family 的 24 个变体当 24 个独立来源。

## Gate 3：在 confirmation 前验收信息边界

对每个 selector run 保存：

- 决策时可见的分子、endpoint、菜单和已释放 selected-only feedback；
- 不可见的当前真值、未选 provider 输出和未释放反馈的证明性日志；
- terminal、release、decision 的事件顺序和 exactly-once 校验；
- provider failure、missing、unknown label 的原始状态；
- 独立 NumPy recovery，可从事件和冻结输入重算每次选择及聚合结果。

任何一个 selector 读到 test label 或完整当前输出面板，整条 confirmation run 作废，不用“重新解释”修复。

## Gate 4：方法和 baseline 一次冻结

在打开 confirmation 前固定 backbone、动态更新式、初始状态、反馈 delay、菜单、budget、seed、主要指标和停止线。至少包括 Random、train-only Best Fixed、Frozen、matched Online/Replay SGD，以及已审计的现代 router 适配；候选动态 JEV 必须先在 development 上通过独立 recovery 和预登记闸门。

若候选方法没有超过强固定策略，保留负结果并停止扩展；不得通过调整 split、删除失败 provider 或选择较好的 checkpoint 追分。

## Gate 5：一次性 confirmation

只有 Gate 0--4 全部通过，才运行一次 confirmation。结果同时报告 molecule-level accuracy、scaffold-level aggregate、反馈数量和 delay、选择/更新延迟、失败率、provider family 相关性，以及与 Best Fixed 的配对差异。confirmation 只回答冻结前写下的主问题，不能返回 development 调参。

## 2026-09-30 provider-specific overlap update

对 157 条候选与 GraphB3 表的只读审计显示：107 条在 GraphB3 train，27 条在 GraphB3 test，23 条不在两张表。这个分层只说明 GraphB3 exposure，不能代表整个 33-provider 菜单的独立性；具体 checkpoint 的 seed/split/member binding 也仍未闭合。机器收据和报告见 `task/task49-pharma-benchmark-baseline-freeze/graphb3_confirmation_overlap_v1/report.md`。

## 当前下一步

Gate 0 已生成一份只读的 scaffold-disjoint BBB 候选清单：`task/task49-pharma-benchmark-baseline-freeze/gate_a_confirmation_manifest_v1/report.md`，对应 157 条候选和固定 hash。它还不是 confirmation，也没有替代冲突处理和 provider lineage 决定。

下一步是完成冲突处理规则和能绑定 checkpoint 的最小 provider menu。这个计划不自动改变 `docs/决议/0001-研究故事线与评价标准.md` 或现有 ADR；它只是把当前审计转成可执行的资格闸门。
