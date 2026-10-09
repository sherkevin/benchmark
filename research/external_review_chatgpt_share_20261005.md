# 外部方法论讨论阅读笔记：联合可靠性与序贯调用

日期：2026-10-05  
来源：[用户提供的共享对话](https://chatgpt.com/share/6ac3643c-d690-83ee-b5ee-db26b97b7490?ogimg=plain)  
状态：研究笔记；不是生效决议，不改变三份主线文档和当前方法身份。

## 一、这次阅读提出的核心候选

外部讨论建议把方法的核心从“更新每个 provider 的边际可靠性”推进到“学习 provider 的联合错误分布”。在已经调用了一个工具之后，系统不只问另一个工具平均上准不准，而是问：它是否会重复当前错误、是否可能纠正当前错误、得到它的结果后最终接受/拒答损失能否下降。

候选实现由四部分组成：

1. 冻结的任务表示和 fit-only direct reference；
2. 一个小的、可计算的联合观测模型，用共享 latent error mode 表示 provider 的相关错误；
3. 用同一联合模型计算当前答案后验、追加 provider 的预期终止损失和有限步数的调用价值；
4. 真实 correctness 释放后，只更新跨任务可靠性状态；本题的即时后验和跨题学习状态分开记录。

外部讨论明确建议第一版只做有独立真值的二分类端点，最多两次调用，并把接受、追加和拒答放在同一有限决策问题里。JEV 只作为功能参照，不作为论文方法名；候选方法名仍是 MIRA。

## 二、与当前项目已经对齐的部分

- 调用前 selector 不能读取当前 provider 输出、confidence 或未选 provider 的结果；
- 调用后 verifier 才能使用已返回的 output、confidence 和 runtime status；
- correctness 只在 selected call 终止并按 delay release 后，才有资格更新跨题状态；
- runtime failure 与 chemical correctness 分账；unknown/refusal 不能静默填成 0；
- fit-only reference、selected-only feedback、release clock、独立真值和 molecule/scaffold 聚类统计继续保留；
- 不能用状态变化、probe coverage 或 provider switch 数量替代 utility gain；
- 当前 strict common receipt、旧 BT prior、adapted router receipt 和 post-call verification 不能跨合同拼接。

因此，它不是另起一条研究线，而是对现有 MIRA 目标的一个更强方法候选：把“共享 residual 是否能传递信息”具体化为“联合观测是否能支持后续证据价值计算”。

## 三、它真正改变的地方

当前 MIRA-Relay 的主要状态是 family residual、provider bias 和 runtime ledger；外部候选把方法的中心对象换成：

\[
P(o_C\mid x,e,y),
\]

其中 $C$ 是本题实际调用的 provider 集合，$o_C$ 是它们的成功返回。一个最小混合模型可以写成：

\[
P(o_C\mid x,e,y)=\sum_z\omega_z(x,e,y)\prod_{a\in C}P(o_a\mid x,e,y,z,a).
\]

条件于共享模式 $z$ 时可以采用局部条件独立；把 $z$ 积分掉以后，工具输出允许相关。这个形式服务于一个具体决策问题：追加 provider 的价值取决于它是否会改变最终回答，而不是它的边际正确率或是否触发了探索。

这会把论文的机制链改成：

> 延迟且 selected-only 的 correctness → 学习 context-conditioned joint reliability → 计算追加证据的预期终止损失 → 接受、拒答或追加调用 → 后续 release 更新。

## 四、需要先审查的硬问题

### 1. 配对观测是否足够

联合模型不能从每个 provider 各自的边际标签凭空识别。它需要同一分子、同一端点上的多 provider 观测。当前 BBB development cache 有 716 个完整的 33-provider 输出，但必须先审计：这些输出是否可以作为 fit-only joint observation 使用、confidence 缺失比例如何、provider family 覆盖是否足够，以及哪些行只是静态缓存而没有合法的真实调用成本。

### 2. 运行时是否会偷看未选输出

模型可以在选择前预测未选 provider 可能返回什么；选择器不能读取该 provider 在当前分子上的实际缓存结果。实现必须在 adapter 层隔离 predicted distribution 与 observed output，并由独立 receipt 检查 read-set。

### 3. latent error mode 的识别与退化

$z$ 可能塌缩、只记住 provider family 或平均错误率，也可能出现 label switching。必须先做合成环境和 fit/validation 观测测试，报告 joint NLL、独立模型对照、模式利用率、family permutation 和 held-out pair likelihood；不能因为公式可计算就把模式解释成真实化学机制。

### 4. 类条件模式权重的定义

如果使用 $\omega_z(x,e,y)$，运行时必须对 $y=0,1$ 两个假设分别计算并归一化，不能把未知真值直接喂给 selector。训练目标、先验、校准和 online update 的可识别性要在 method card 中固定。

### 5. 两次调用是否成为新的任务定义

两次调用、接受/拒答和追加调用是对现有 direct-selection benchmark 的 extension，不应未经讨论就替换 primary benchmark。更干净的做法是：保留一次调用 direct selection 作为 E2a 主任务，在 E5 或新增 extension 中加入两次调用的 sequential evidence task；两者共享 truth、provider menu、表示和反馈合同，但不混合主指标。

### 6. 贡献不能只写成“相关性 + 贝叶斯 + cascade”

这些组件各自都有近邻。可成立的候选主张应集中在一个机制上：同一个从配对观测和 delayed correctness 学到的联合可靠性模型，同时支撑调用前选择、调用后后验判断和追加调用价值计算，并在相同信息、调用预算和反馈时钟下带来净效用提升。是否成立必须由 Frozen Joint、Independent Evidence、Confidence Cascade、Best Fixed 和现有 strict online controls 逐项证伪。

## 五、当前建议：把它设为 MIRA 的候选版本，而不是立即改生效方法

建议内部暂命名为 **MIRA-Joint Reliability candidate**，保留 MIRA-Relay 作为当前已写入论文的实现规格，直到以下 CPU 闸门通过：

1. 配对数据资格与 confidence 缺失审计；
2. 合成环境中能区分相同边际、不同联合错误的模型；
3. holdout 上联合 NLL/Brier/后验校准优于 Independent Evidence，且没有 family-only 解释；
4. 两次调用有限视界 planner 的 exact enumeration、cost/refusal 语义和 read-set/release 单测；
5. 预注册 Frozen Joint、Independent Evidence、Confidence Cascade 三个 matched controls 的字段和否定条件。

这些闸门都不需要新的 A800。若第 2 或第 3 闸门失败，应保留失败证据，不能靠扩大 latent mode、调阈值或换 backbone 把它改成正结果。只有候选通过后，才讨论是否把论文主方法从 MIRA-Relay 迁移到该版本，并重新评估 E2/E3/E5 的矩阵。

## 六、与当前验收标准的关系

- 故事线：候选明显加强了“问题 → 根因 → 机制 → 可证伪收益”的主线，但最终只能在真实结果闭合后升级 claim；当前仍是候选。
- 方法论：新增了可计算的联合分布、有限 horizon value-of-information 和三个直接机制对照，但参数化、更新和 identifiability 尚未冻结。
- Benchmark + Baseline：一次调用 benchmark 继续有效；两次调用属于扩展任务。baseline 需要增加同 planner、同预算的 Frozen Joint、Independent Evidence 和固定 cascade，不能把现有 adapted routers直接当作该任务的等价 baseline。

本笔记不改变当前三门评分、Gate A/B/C 或 confirmation 状态。

## 七、第一道数据资格审查的实际结果

对 `fingerprint_diagnostic_v1/inputs.json` 做了 CPU-only 的固定宽度和共同成功覆盖审查，纠正后的结果见 `task/task49-pharma-benchmark-baseline-freeze/joint_observation_eligibility_audit_v2/`：722 行都有 33 列结构化记录，716 行 33 个 provider 全部 runtime 成功；6 行含有 144 个 raw `error` cells。另有 3,591 个 runtime-ok 但化学预测错误的 cells，不能与 runtime failure 混称。528 个 provider pair 的 runtime co-success 为 716--722；该输入本身没有 confidence 字段。

因此 complete-case 资格不再是主要覆盖率瓶颈，但正式模型仍应保留 6 行显式 `⊥`，并把 `Independent Evidence` 与 `Joint Reliability` 放在同一缺失处理合同下比较。历史缓存仍只能支持 fit-only 资格诊断，不能被叙述成已完成的 sequential paired-call 成本或 online receipt。旧的 v1 数字因把 `system_success` 当成 runtime status 已作废。

## 八、独立方法审查后的收敛意见

独立审查认为候选比当前 family residual 更贴近问题，但提出一个决定性边界：一次调用任务中没有第二个已观测输出，联合错误相关性很难产生行为收益；因此 H=2 必须是明确的 sequential evidence extension，不能替换 E2a primary。它还要求增加同容量的 pairwise conditional control，避免把 latent mode 的收益和额外参数量混在一起。

审查把最终决策压缩为三个同时成立的条件：

1. scaffold-disjoint held-out pair posterior 的 NLL/Brier/ECE 优于 Independent Evidence，且不靠 family-only 记忆；
2. 相同菜单、fit panel、平均调用预算、selected-only release 下，Joint planner 的 net utility 高于 Independent Evidence 和 matched fixed cascade，同时不靠 refuse-all 或减少调用投机；
3. 若要声称 online adaptation，在预登记的 controlled reliability shift 或 lifecycle event 下，Joint online 相对 Frozen Joint 有 paired gain；单纯改变 delay clock 不能称为 nonstationary learning。

只通过第 1 条时，联合模型可以作为分析或辅助，不足以成为论文中心方法。这个审查结论已纳入下一轮 CPU gates，但没有改变生效方法文档。

随后完成合成 CPU gate（`task/task49-pharma-benchmark-baseline-freeze/joint_observation_synthetic_gate_v1/`）。在“相同 provider 边际、不同联合结构”场景中，Joint 相对 Independent 的 held-out conditional NLL 差为 `-0.3722`、posterior Brier 差为 `-0.1616`、accuracy 差为 `+0.3976`；把运行失败作为第三类 `⊥` 后，脚本仍保持显式观测语义，NLL/Brier/accuracy 差为 `-0.0505/-0.0234/+0.1121`。这只说明参数化在构造环境中可学，不说明真实 BBB 或最终 policy 有效；下一步必须做 scaffold-disjoint real-panel pair prediction。

真实 BBB 的第一轮 scaffold-disjoint pair prediction 已完成：Joint 的 full-panel NLL 在 5/5 folds 改善（宏平均 `-3.8502`），528-pair 平均 NLL 在 5/5 folds 改善（`-0.0716`），但 full-panel posterior Brier 变差 `+0.0185`，pair Brier 基本不变 `+0.0006`，ECE 也变差。结论是“联合 likelihood 有信号，但当前 posterior 尚未可直接用于 planner”；下一道闸门必须加入 family-only、同容量 pairwise control 和 fit-only calibration，不能直接进入 H=2 policy replay。收据见 `task/task49-pharma-benchmark-baseline-freeze/joint_observation_real_panel_gate_v1/`。
