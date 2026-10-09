# Greedy LinUCB 开发 baseline 适配候选 v1

日期：2026-09-29  
状态：**候选预登记；未运行，未打开 confirmation，未提交 Nebula/A800 任务。** Gate A 的冲突政策和统一 runner 公平合同未冻结前，不执行本轮效果回放；本卡不改生效 benchmark。

## 1. 这一轮要回答什么

这一轮只把 C6 Greedy LinUCB 适配版做成可复核的同任务 baseline，回答两个问题：

1. 在 BBB 固定 provider 菜单上，selected-only 的二值正确性反馈能否让一个标准上下文 bandit 超过同一冷启动的 Frozen 控制；
2. 延迟反馈 $d\in\{0,1,4\}$ 是否改变它的选择轨迹和累计正确率。

这不是新方法实验，也不把“能实时递推”写成创新。结果若为负，仍然是有效的 baseline 结果。

## 2. 固定输入

- 输入：`fingerprint_diagnostic_v1/inputs.json`，722 个已排除 unresolved 的 BBB development 分子，33 个具体 provider，round 1。
- 特征：canonical SMILES 生成的 Morgan radius=2、1024 bit、原始 0/1 向量；不归一化，不使用 truth、provider output 或 status 生成特征。
- 划分：输入中固定的 5 个 Bemis--Murcko scaffold-disjoint outer folds。每个 fold 的 `fit` 只用于初始先验；`validation` 只保留在输入审计中，不用于选超参或改状态；`evaluation` 只用于评估。
- 顺序：完全沿用输入中的 `fold["evaluation"]` 顺序，与已有 PILOT-adapted 和 structured CPU runner 一致；不新增 evaluation permutation，不看结果改顺序。seed 0/1 保留统一矩阵槽位，但该确定性实现和同一 fit-only ridge 先验不会产生两个独立随机重复；汇总时不得把复制记录当作独立样本。当前 baseline contract 若要求真正的 seed 随机性，必须另开版本并让所有方法共用同一顺序随机化。
- 菜单：每个事件使用输入中同一个 33-provider 菜单，固定全菜单，不做事后删 provider。

输入文件的 SHA-256、各源结果文件的 SHA-256 和代码源文件 SHA-256 必须写入 `config.json`。输入变更即产生新版本，不能覆盖本版本日志。

## 3. fit-only 冷启动

本版本是 **fit-prior LinUCB 适配候选**，不是原论文的零先验算法复现。原论文的 Greedy LinUCB 为无离线标签的 cold-start 场景；本版本对每个 outer fold，用共同允许的 `fit` 区完整 provider panel 计算 ridge 初始均值，evaluation 的 provider 标签完全不参与该计算。共同监督权限必须与 PILOT、BayesianRouter 和 Proposed 对齐；不能只给本方法 fit 标签，或把不同初始化条件直接混排。

令 $X_F\in\{0,1\}^{n_F\times1024}$ 是 fit 分子特征，$R_F\in\{0,1\}^{n_F\times33}$ 是 evaluator 侧的 system-success 矩阵，且 $\lambda_{fit}=1$ 固定。第 $a$ 个 provider 的初始向量是

$$
\mu_{a,0}=(X_F^\top X_F+\lambda_{fit}I)^{-1}X_F^\top R_{F,:,a}.
$$

实例：若 fit 区分子 `z000282` 的 CNN 运行状态为 `ok` 且输出等于 BBB truth，则该 fit cell 的 $R_{F,:,a}=1$；若运行失败，则该 cell 为 0，不能把失败恢复成化学标签。该行只有在 `z000282` 属于当前 fold 的 fit 集合时才进入初始均值。

online learner 的 prior precision 另固定为 $\lambda_{prior}=1$，所以 $P_{a,0}=I$。这是一个以 fit ridge 均值为中心、将 online 不确定性重置到 $I$ 的新适配约定，不是保留离线 Gram 矩阵的 exact warm-start。fit 标签只压缩到初始均值，不在 online 阶段重复加入；后续只接收被选 provider 的已释放 reward。每个 fold 的初始均值单独保存并哈希，独立 verifier 会从 `inputs.json` 重新计算。正式矩阵冻结前，需把“零先验原算法”与“fit-prior 适配版”的角色分别列明；当前不能用较近论文名掩盖差异。

若未来需要 canonical zero-prior 版本，应另建 v2；不能把本版本的 fit-only 结果和 zero-prior 结果混在一张主表里。

## 4. 选择与反馈合同

对当前分子特征 $x_t$，provider $a$ 的分数为

$$
s_{t,a}=x_t^\top\mu_{t,a}+\alpha\sqrt{x_t^\top P_{t,a}x_t},\qquad
a_t=\arg\max_{a\in A_t}s_{t,a},
$$

其中 $\alpha=0.5$ 固定，平分时按菜单中的 provider 顺序取第一个。episode 终止且反馈到期后才释放
$r_t=\mathbb{I}(\text{selected output is a valid and correct return})$。失败保留为 $r_t=0$；失败时 `chemical_correct` 和 `recovered_label` 仍为 `null`。

选中的 provider 到期时，才执行

$$
g_t=\frac{P_{t,a}x_t}{1+x_t^\top P_{t,a}x_t},\qquad
\mu_{t+1,a}=\mu_{t,a}+g_t(r_t-x_t^\top\mu_{t,a}),\qquad
P_{t+1,a}=P_{t,a}-\frac{P_{t,a}x_tx_t^\top P_{t,a}}{1+x_t^\top P_{t,a}x_t}.
$$

实例：如果 tick 3 选择了 `graphb3`，该次调用在 tick 3 结束，$d=1$，反馈在 tick 5 的决策前释放；tick 4 的选择不能使用这条反馈。未选 provider 的输出不进入 `observe`。

## 5. 成对 Frozen 控制

每个 `fold × seed × delay` 同时运行两个策略：

- **Greedy-LinUCB-online**：使用上述 fit-only $\mu_{a,0}$，按到期 selected-only reward 更新；
- **Greedy-LinUCB-Frozen**：使用完全相同的 fit-only $\mu_{a,0}$、$\alpha$、特征、菜单、顺序和 delay，只消费 feedback，不改变 $\mu$、$P$ 或版本。

两者每个 event 使用相同的分子和初始状态，主成对量是同一 event 的 `online_success - frozen_success`。Best Fixed 只作为额外 fit-only 参考：provider 由 fit 区的 system-success 均值选出，不能看 evaluation；Oracle 仅作 evaluator-side 上界，不进入可部署排序。

## 6. 实验矩阵

候选矩阵为 $5\ \text{folds}\times2\ \text{seed\ slots}\times3\ \text{delays}\times2\ \text{policies}=60$ 条流。每条流覆盖该 fold 的全部 evaluation 分子；每个 event 必须恰有一个 decision、一个 terminal 和一个 feedback。两个 seed slot 对本确定性算法应相同，属于协议对齐的重复执行，不是两个独立随机重复。

主指标：按 fold、seed slot、delay 报告 selected system-success accuracy，以及 online 相对 Frozen 的 per-event paired accuracy difference。附带报告 valid-return accuracy、failure/unknown 分母、provider/family 分层、decision/update p50/p95 和最终状态 hash。旧 722 面板已被反复查看且 Gate A 尚未通过，本轮即便执行也只能称为开发回放，不能升级为正式主表或 confirmation。

不计算逐日志行独立置信区间。若需要不确定性，按 molecule/scaffold 做成对 bootstrap，并在报告中保留 5 个 fold、2 个 seed 的原始分层结果。

## 7. 信息边界和独立复核

runner 的 policy 输入只有当前 $x_t$、当前菜单和已经释放的 selected-only reward。policy 不接收完整 `rows`、truth、未选 output 或 evaluation panel。evaluator 可以在 terminal 日志中保存可恢复字段，但这些字段不能传入 `observe`。

`events.jsonl` 逐事件记录：stream、tick、event/query/provider、菜单、特征 hash、状态 version、selected output、terminal reward、release tick、更新前后 version 和 update time。`config.json` 记录参数、顺序、输入/源/代码 hash、Python/numpy/rdkit 和硬件。初始先验单独保存并哈希。

独立 verifier 不导入 runner 或 `GreedyLinUCB` 实现，使用输入数据和递推公式重算：

1. fit-only 初始均值；
2. 每个 tick 已释放 feedback 的数量和更新顺序；
3. 每次 UCB 分数、provider 选择、selected output 和 reward；
4. online/Frozen 的最终状态、事件守恒和成对 accuracy。

任何未选输出进入 policy、evaluation 标签进入 fit、反馈提前释放、重复/丢失 event、状态重算不一致或 hash 不一致，整轮标记 `BLOCKED`，保留原始日志，不重跑到得到好看的数字为止。

## 8. go / no-go

### 技术 GO（只完成可复核适配，不自动取得主表资格）

- 60 条流全部生成 raw JSONL；
- 独立 verifier 为 `verified`；
- 事件守恒、selected-only、delay、provider 绑定、fit-only hash 和状态递推全部通过；
- online 与 Frozen 使用相同输入、顺序和初始状态，Best Fixed 不使用 evaluation 调参。

### 结果信号（只决定是否值得继续投入动态收益分析）

- **正向信号**：online 相对 Frozen 的 molecule-weighted paired accuracy 至少高 0.01，并且至少 4/5 个 fold 上为正，且该条件在至少 2/3 个 delay 上成立。seed 0/1 必须相同且只按一次有效运行计入推断；这才值得继续分析其在线反馈曲线。
- **中性/负向**：技术 GO 但未满足正向信号。保留为合法 baseline，不写成动态学习有效；下一步优先查 warm-start、coverage 和 delay，而不是调 evaluation 超参。
- **STOP**：任一信息泄漏、独立复核失败、主分母改变或只剩不完整日志。停止结果解释，先修合同。

这些阈值是开发资源闸门，不是显著性检验，也不用于删除负结果。任何 confirmation 主张仍需在所有 benchmark、baseline、冲突政策和方法冻结后单独开 Stage B。
