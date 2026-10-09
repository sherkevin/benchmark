# 公共医药 benchmark 合同候选 v1

日期：2026-09-28  
状态：候选草案，未作为决议生效

这份草案把 benchmark 拆成两个层次：公开数据集提供真值、版本和切分；我们提供可审计的 provider 菜单和延迟反馈回放。两层不能混称为一个现成社区 benchmark。

## 1. Truth layer

主候选是 TDC ADMET 的 endpoint 数据，第一批优先 BBB 和 AMES。每个 endpoint 单独保留标签语义，不能把 BBB、AMES、hERG 合并成一个二元奖励。

- 冻结下载文件，而不是运行时依赖最新版 PyTDC。
- 保存来源 URL、文件 SHA-256、TDC 版本或 commit、去重和冲突处理规则。
- 采用 TDC 官方 scaffold test 作为固定测试集；只在 `train_val` 内使用预先登记的训练/验证种子。
- 测试集标签只用于终止后的评分和反馈释放，选择前不可见。
- endpoint metric 保留 TDC 的定义；selector 的主指标另行定义为工具调用后的 correctness。

当前审计已核实 TDC BBB_Martins 和 TDC AMES 的公开来源与规模。hERG 的来源和 assay cutoff 已有部分证据，但还不够冻结为同一主 benchmark。

已从 TDC ADMET Group archive 冻结官方文件：

- archive SHA-256：`bd0005246cb6f1672333c28ad202112a8bf071ab65d847848715030308faf1f1`；
- BBB：`train_val` 1,624 行 / 1,581 unique，`test` 406 行 / 394 unique；
- AMES：`train_val` 5,821 行 / 5,802 unique，`test` 1,457 行 / 1,453 unique；
- 两个 endpoint 均为 canonical-disjoint 且 Bemis--Murcko scaffold-disjoint；文件内部重复行分别为 BBB 55、AMES 23。BBB 的 `train_val` 有 8 个 canonical 冲突组、`test` 有 2 个冲突组（同一 canonical 同时有 `0`/`1`），AMES 无标签冲突。

旧的 722 条 selector development panel 与官方 BBB test 有 147 条严格 canonical、152 条最大碎片重叠。因此在新的方法冻结前，官方 test 不能直接被写成对旧开发工作的 untouched confirmation；这些重叠必须隔离，或由新的未读取确认集承担最终确认。该条是当前 benchmark 合同的硬约束。

## 2. Provider layer

provider layer 是在 truth layer 分子上生成的具体工具菜单和输出快照。它至少记录：

- provider id、版本、源码 commit、模型 checkpoint hash、端点定义和输出阈值；
- 训练数据来源、训练成员审计结果、生成时间、运行环境和失败语义；
- 每个分子每个 provider 的原始输出、二元预测、运行状态和来源行；
- test-only 回放收据，证明选择前没有读取未选 provider 的输出或测试标签。

现有 Task36 的 730×33 BBB 缓存可以保留为 development replay 和 stress panel。它不能直接被称为公开 benchmark，也不能把 24 个 B3clf 变体当成 24 个独立模型家族。

正式主表应按 provider family 平衡。B3clf 变体合并计为一个 family；all-33 菜单可以作为相关性和菜单粒度的附录压力测试。

## 3. Online selection task

每个 episode 的输入是分子、endpoint 和冻结的 provider 菜单。selector 在任何 provider 输出不可见时选择一个 provider。工具结束后，系统给出该次选择的 0/1 correctness；反馈可以按预登记 delay 释放，后续 episode 才能使用。

合法信息边界如下：

1. 选择前可以读分子表示、endpoint、菜单和已经释放的历史反馈；
2. 选择前不能读当前分子的真值、任何 provider 的当前输出或未选 provider 的失败状态；
3. 终止后只释放被选 provider 的运行状态、预测和 correctness；
4. 失败、缺失和未知标签不填成 0；未知真值的 episode 不计入 correctness；
5. 如果输出概率本身被用于 verifier，必须标记为 post-call 信息，不能回填成 pre-call selector 特征。

每个 run 报告：按时间的 accuracy、累计 regret、相对 fit-only Best Fixed 的差值、反馈条数、延迟、平均调用数和失败率。确认集只在所有冻结条件通过后打开。

## 4. Baseline set

所有 baseline 必须使用同一个 truth layer、provider menu、输入表示、反馈延迟和预算。最低集合为：

- Random provider：无学习下界；
- fit-only Best Fixed：只用训练区完整 provider labels 选择一个固定 provider；
- Oracle：只作不可部署的上界；
- Frozen static selector：不读取在线反馈的监督 selector；
- Online-SGD 和 Replay-SGD：同 backbone 的普通持续学习对照；
- Greedy LinUCB：直接工具选择的经典在线对照；
- PILOT-adapted 和 BayesianRouter-adapted：按公开方法目标适配，并明确哪些部分不是官方复现；
- 如实现可审计，再加入 GraphRouter 或同等现代静态 router。

baseline 的源码、论文版本和适配差异要随 run 固定。未经成员审计的开源预训练 provider 只能标为 lineage-aware control，不能当作 model-fresh 泛化证据。

## 5. 外部数据用途

- PharmaBench BBB/AMES：单独的数据源扩展或敏感性分析，不能与 TDC 标签逐条合并。当前重叠中 BBB 有 55 条 `0->1`，AMES 有 33 条 `0->1`，其预处理还混合了 B3DB、TDC 和 ChEMBL。
- B3DB：BBB 外部确认候选；B3DB external 只有 175 条且 171 条为正类，必须报告 PR-AUC、coverage 和类别支持。B3clf 已公开说明使用 B3DB 训练，因此不能作为 B3clf 的独立主测试。
- MoleculeNet BBBP：历史 lineage reference；与 DeepPurpose BBB provider 高度重叠，不作独立 confirmation。

## 6. 发布前停止条件

以下任一条件未满足，就不能把结果写成公开 benchmark 上的 model-fresh 主结果：

1. truth 文件、切分和标签语义没有冻结哈希；
2. provider 的训练成员、源码、checkpoint 或训练来源无法审计；
3. 选择前读取了测试标签、未选输出或当前题目的完整输出面板；
4. provider family 相关性没有单独报告；
5. 延迟反馈、失败和缺失值没有逐事件收据；
6. 独立恢复无法从原始日志重算每条选择和聚合指标。

## 7. 尚未决定的事项

这份草案没有替用户确认主 benchmark，也没有 supersede ADR-0021。还需要共同确认：

- 主结果是否采用 TDC BBB+AMES，还是先只做 BBB；
- hERG 是否进入主结果，还是只作端点资格审计；
- provider 是否全部在公开 train split 重新拟合，还是保留 lineage-aware controls 并把独立性限制写进主结果；
- 是否需要一个通用 tool-use benchmark 做外部迁移，而不是把它默认为必需项。
- 官方文件中的 canonical duplicate rows 是保留公开行还是先按 canonical molecule 去重；必须在方法冻结前明确并固定。
- BBB 官方 split 内的 10 个标签冲突 canonical group 如何处理（排除、按重复测量建模，或保留公开行但改变 estimand）；不得按第一行或多数票静默处理。
- 旧开发 panel 与官方 test 的重叠如何隔离，以及新的 provider 输出是否在官方 `train_val` 上重新生成。
