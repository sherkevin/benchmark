# Provider-fresh menu 与 common-stratum 合同审查

日期：2026-09-30  
审查对象：

- `task/task49-pharma-benchmark-baseline-freeze/provider_fresh_menu_v1/report.md`
- `task/task49-pharma-benchmark-baseline-freeze/provider_fresh_menu_v1/training_contract.md`
- `task/task49-pharma-benchmark-baseline-freeze/common_stratum_contract_v1.md`
- 对应实现：`src/probe_headless/prepare_provider_fresh_menu_manifest.py`、`run_provider_fresh_menu_cpu.py`、两个 verifier

性质：只读审查，不改实验结果，不提交 Nebula 任务。评价依据是生效的 `docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md`，重点检查 H2/H3/H4/H6/H8/H10、Gate A/B 和 R1--R8。

## 结论

这两份合同已经把路线从“旧 provider 训练成员不明”推进到了可执行的输入和公平比较草案，但还不能作为 confirmation 前的最终合同。当前有一个实现与合同不一致的硬伤，另外有几项会让审稿人质疑 model-fresh、baseline 公平和复现边界的问题。

建议状态：**HOLD，先修 manifest 完整性，再补 common-stratum 的可执行收据**。

## P0：必须先修的硬伤

### P0-1 同标签重复行没有进入 `excluded_rows.csv`

合同和报告写的是“同 canonical 且标签一致的重复测量只保留 source row 最小的一行，其他行进入 `excluded_rows.csv`”。实现并没有这样做：

- `src/probe_headless/prepare_provider_fresh_menu_manifest.py:105--124` 在非排除组只把 `rows[0]` 放入 `candidates`，没有把 `rows[1:]` 写入 `excluded`；
- `manifest.json` 记录 `source_rows=1624`、`excluded_rows=585`、`candidate_unique_canonical=1025`；
- 实际逐行核对显示 train + validation + excluded 只覆盖 **1610/1624** 个源行，缺 **14** 个源行；
- 缺失行包括 source index `21, 121, 615, 622, 773, 863, 921, 1129, 1277, 1281, 1359, 1474, 1571, 1580`。

这不是仅仅文档措辞问题：当前 verifier 的 `source_rows_recover` 只验证输出行能回到源文件，不能证明每个源行都有唯一处置；因此重复测量被静默丢弃，违反 H2 的不静默规则，也削弱 H3/H10 的完整审计。

**修正：**

1. 为所有保留首行之外的重复源行写入 `excluded_rows.csv`，原因固定为 `same_label_duplicate_first_source_row`；
2. 增加分区不变量：`source_rows = train_rows + validation_rows + excluded_rows`，且 source index 两两不交；
3. 增加 verifier 检查所有 source index 都被恰好处置，并记录每个 canonical group 的全量 source row 清单；
4. 重新生成 manifest、hash、CPU receipt 和 verifier receipt。旧 hash 不应继续作为新合同输入。

### P0-2 provider manifest 没有把确认区的 scaffold 隔离写成可验证不变量

实现只检查 provider train/validation 与 development/confirmation 的 **canonical** 不重叠，没有检查 provider 训练成员与 confirmation 的 scaffold 不重叠。当前数据只读计算得到 train/validation 与 157 条 confirmation 候选恰好没有非手性 scaffold 重叠，但这个事实没有进入 manifest/verifier；将来只要 confirmation manifest 改变，可能静默失效。

**修正：** 将 `no_confirmation_scaffold_overlap`、`no_development_scaffold_overlap`（若选择要求）写入 manifest 和 verifier。若允许 provider 训练与 development scaffold 重叠，必须明确写成“development 只用于 selector development，不能宣称结构独立”；若要把 157 条称作结构独立 confirmation，则 provider train、provider validation、selector development、confirmation 四者的 scaffold 关系必须分别报告。

## P1：会阻止 Gate A/B 通过的证据缺口

### P1-1 canonicalization 版本和实现没有绑定

manifest 只保存数据文件 hash，没有保存 canonicalization 工具、RDKit 版本、参数或实现 commit。`read_panel()` 和 `read_confirmation()` 直接信任输入文件中的 `canonical_smiles`，没有用同一版本重新计算并核对。跨机器/版本的盐处理、手性和芳香化差异可能造成 overlap 漏检。

**修正：** 固定并记录 canonicalization script commit、RDKit 版本、`MolToSmiles` 参数、无效 SMILES 处理、scaffold 算法版本和输入 feature schema；verifier 对 source/development/confirmation 至少抽样或全量重新 canonicalize，发现差异即 fail-closed。manifest 还应记录 provider ID 的固定排序。

### P1-2 source/label 语义没有在本合同内闭合

文件只写“TDC BBB train_val”和 hash，没有指向公开 archive/version、端点定义、标签阈值、license 和冲突审计入口。provider 输出的 `p>=0.5` 是模型决策阈值，不是公共 truth 的端点阈值；两者必须分开写清楚。

**修正：** 在 manifest 中绑定已有 truth audit 的 source URL/record、下载收据、label contract、端点语义、unknown/failure 规则及许可证；训练合同明确模型预测阈值只是 provider 行为，不改变 truth 标签。

### P1-3 “四个 provider”更像自建模型菜单，缺少外部实现身份

`ecfp_logreg`、`ecfp_mlp`、`ecfp_random_forest` 和 `graph_message_passing` 都由本地训练器定义；尤其 graph provider 是自定义两层 GCN，现有代码的邻接矩阵只有二值连接，未使用 bond type/edge feature，而合同表述为“原子和键特征”。这会让审稿人把它看成 toy menu，而不是现有工具生态中的现代 provider。

**修正：** 二选一并在论文中明确：

- 若目标是“可控的 model-fresh routing environment”，将菜单命名为 auditable provider menu，不声称四个独立开源工具；同时把自定义 GCN 的精确 feature schema、源码、许可证和局限写清；
- 若要支撑“真实科学工具选择”，至少纳入可追溯的开源实现（固定 repo commit、训练脚本、许可证、输入/输出语义），并将自建 ECFP/GNN 作为 matched controls，而不是唯一主菜单。

不要把同一 TDC train_val 上训练的四个模型写成四个独立 truth source；它们是相关的 predictor families，必须做 family-level sensitivity。

### P1-4 CPU receipt 的环境和 source tree 绑定不完整

`cpu_v1/config.json` 记录 seed、manifest/train/validation hash 和 `source_code_commit=6962ac6c...`，但没有 Python、RDKit、PyTorch、scikit-learn 版本、依赖 lock、硬件/线程设置或源树 dirty 状态。verifier 的 `declared_commit_is_current` 只在当时工作树恰好位于该 commit 时成立；现在 HEAD 已变化，无法仅凭该收据独立重建环境。

**修正：** 把环境 lock、`pip freeze`/包版本、Python、线程/确定性设置和可执行源码归档到 receipt；用 `git archive <commit>` 或 source-tree hash 验证实际执行文件，而不是比较“当前 HEAD”。重新运行 independent verifier 时允许历史 commit，但要求 commit 存在且文件 hash 与 archive 一致。

### P1-5 stochastic provider 只有单 seed，且 validation 只做一次模型选择

MLP/GCN 只用 seed `20260930`。如果它们要作为 confirmation provider，单个随机初始化可能把偶然性写进工具行为；若根据 validation sanity 选择/删除 family，会把 validation 变成菜单选择器。当前报告已经承诺 graph 表现弱也不删，但这个承诺需要程序化固定。

**修正：** 在 confirmation 前冻结四个 family 全部纳入，或预先固定失败淘汰条件；对随机 provider 至少做 seed sensitivity（例如多个预注册训练 seed），并把 seed 视作同一 family 的相关变体，不当作独立 provider 数量。训练/验证的类别计数和 GroupShuffleSplit 结果也要写入 manifest；当前 train/validation 标签数分别为 663/182 与 137/43，验证集没有任何 group-level stratification 保证。

### P1-6 训练合同与 graph 实现的 feature 语义不一致

合同写“原子和键特征由 RDKit 确定性生成”，代码 `graph()` 实际只使用原子 one-hot、degree、formal charge、aromaticity、total H 和二值邻接矩阵；bond type、stereo、ring 或 edge feature 都没有进入模型。

**修正：** 将合同改成代码真实实现的特征，或实现并固定真正的 bond features。两者必须有 feature schema hash、维度和逐分子重算测试，不能在论文中使用比实际模型更强的描述。

## P1：common-stratum 合同的公平性缺口

### P1-7 `$W_0$` 的定义方向正确，但尚未形成可执行的 fold-level manifest

合同说每个 `(fold, seed)` 使用同一个 fit-only direct prior，并引用 sealed structured CPU fit；但没有在合同中绑定每个 fold 的 checkpoint、provider order、query encoder hash、direct snapshot hash、训练行 manifest、validation epoch 和生成命令。现有 smoke 只有 fold 0、delay 0、10 个任务，不能代替 5 folds × 2 slots × 3 delays 的完整 common receipt。

**修正：** 为每个 fold/slot 生成不可变 `w0_manifest.json`，至少包含：输入 panel hash、fold indices/scaffold hash、encoder checkpoint hash、direct prior bytes hash、provider ID order hash、fit/validation members、best epoch、generator commit 和 source tree hash；common runner 只接受该 manifest，任何缺字段 fail-closed。

### P1-8 MIRA 并不使用共同 covariance，合同需要区分“共享信息权限”和“算法状态”

Frozen/Matched/Greedy/BayesianRouter 使用 `$\Sigma_0$`，当前 MIRA-SafeSwitch/relative-reliability 使用 direct prior 加 Beta/reliability ledger，既不消费 covariance，也带有 `n_min`/`tau_fail` 的结构化伪计数。合同把所有方法写成同一 covariance 下的比较，容易造成“只改变 update/exploration rule”的过强表述。

**修正：** 把共同 stratum 定义为“同一 feature、同一 `$W_0$`、同一可见事件、同一调用/时间预算”；对 covariance 写明只适用于需要 posterior 的方法，MIRA 的 reliability prior 单列并预注册。增加 `MIRA-empty-ledger` Frozen 对照，验证初始分数与 `$W_0$` 完全相同；主表按 state family 分层，不能把 MIRA 的 Beta ledger 当成共享 covariance 的直接等价物。

### P1-9 failure/unknown 的跨方法映射没有固定

合同写“failure 保留在分母，unknown truth 终止 stream”，但没有明确每个方法在 `status=error`、`chemical_correct=null`、`recovered_label=null` 时是否更新、更新成什么、是否进入 runtime-failure ledger。MIRA 有独立 availability hard channel，而 Matched/Greedy/BayesianRouter 的失败处理可能不同；如果 MIRA 看到额外的 failure signal，raw accuracy 不能直接称公平 superiority。

**修正：** 在 event contract 中逐字段定义：所有方法可见的 terminal payload；每个方法对 failure/unknown 的合法 state transition；是否把 failure 视作 reward 0、跳过 chemical update 或只增加 availability count。加入 failure-positive/unknown-positive smoke 和 fail-closed replay 检查。

### P1-10 “common budget”没有资源上限，只规定了调用次数

合同规定每 tick 一次 scoring、一次 provider call 和到期 feedback update，但没有固定 wall-clock、CPU/GPU、峰值内存或状态维护上限。MIRA 的 `O(qd)` update 与 Bayesian/Greedy 的 `O(d^2)` posterior update 不能仅凭相同调用次数证明“高效”。当前 runtime ledger 只记录 p50/p95，尚未形成 cost table。

**修正：** 若论文声称高效，预注册统一计时边界（含 encoder、menu scoring、queue、state serialization、recovery）和峰值内存；若只比较 correctness，明确不作成本 superiority。所有方法使用同一 provider-call budget，额外 exploration、replay 和 historical cache 必须计入。

### P1-11 common-stratum 的统计单位和随机性尚未闭合

合同有 5 scaffold folds × 2 slots，但没有说明 slots 是随机 seed、协议重复还是仅复制 delay；deterministic 方法不能把 slots 当独立 seed。BayesianRouter 的 action randomness 与 MIRA/Greedy 的 deterministic action 也不是同一随机变量。

**修正：** 固定 fold-level paired unit；预注册真正的 stochastic seeds、action RNG、bootstrap cluster（至少 molecule/fold，provider 不能当 iid）；slots 仅作 protocol slots 时不得生成 seed CI。报告 seed variance、fold variance 和 policy randomness 分开。

## P2：应补但不阻止修复后进入下一步的事项

1. `provider_validation.csv` 的 checkpoint selection 规则已写，但 LogReg/RF 只作报告、MLP/GCN 按 validation BCE 选 epoch，模型 family 之间的菜单冻结时点需要写明；不得在看到 validation 后调整 family roster。
2. provider 训练成员与 development scaffold 目前存在重叠（只读统计：train 与 development 共享 97 个非手性 scaffold，validation 共享 21 个）。如果 development 结果要用于结构泛化叙述，应把这个暴露写入报告；confirmation 与 provider train/validation 当前 scaffold overlap 为 0，但要由 verifier 固定检查。
3. provider manifest 的类别计数、invalid SMILES、canonicalization 警告和 source duplicate summary 应成为机器收据，而不是只在人工报告中出现。
4. common-stratum 当前 smoke 只有 `delay=0`、无 failure/unknown，且事件没有统一 raw schema 所需的完整 `decision_reads`、`released_feedback_ids`、`provider_event_ids`、`state_hash_before/after` 等字段；它只能证明 adapter 能运行，不能证明 Gate B 公平性。
5. PILOT 被放在 out-of-stratum 是合理的，但最终主表必须用明显的 adapted 标签，并单独给出 feature/projection/initialization 差异；不能与 strict-stratum raw accuracy 混排。

## 推荐修复顺序

1. 修 P0-1，重新生成并独立核对完整 row partition；同时加入 confirmation scaffold overlap 和 canonicalization version checks。
2. 冻结 provider source/label/feature schema、环境 lock、source archive 和 family roster；明确自建菜单还是开源 provider 菜单。
3. 为 5 folds × slots 生成 `$W_0$` fold-level manifests，补 failure/unknown 语义和统一 raw event schema。
4. 先跑含 delay `{0,1,4}`、failure、unknown、重复/篡改事件的 2-stream smoke；独立 verifier 通过后才跑 60-stream common receipt。
5. 只有 common receipt 的 paired uncertainty、runtime ledger、cost table 和 negative result 完整后，才重新评 Gate B/C；confirmation 继续关闭。

## 结论给项目管理

provider-fresh 路线是有价值的：它确实把旧 provider 的训练成员不明问题变成了一条可审计路线；common-stratum 也把当前异质 prior 的公平性争议转成了可以执行的合同。但当前不能把“manifest verified”或“CPU prediction 可重算”写成 model-fresh confirmation 已经成立，更不能把 common smoke 写成 baseline superiority。优先修复源行分区和 fold-level `$W_0$`/failure 语义，修复后的证据才有资格推进 Gate A/B。


## v2 P0 closure (2026-09-30)

The source-partition defect is fixed in `8afcf7c9`. The active v2 manifest archives all 14 same-label duplicate extra rows, and its independent verifier checks exact coverage of all 1,624 source rows, RDKit 2025.09.2 canonicalization, and confirmation scaffold separation. The v2 CPU training/validation receipt was rerun against the new manifest and independently recovered. The remaining P1 findings in this review—full environment lock/source-tree archive, explicit provider-family qualification, fold-level common-stratum W0, failure/unknown semantics, and budget/cost ledger—remain open.
