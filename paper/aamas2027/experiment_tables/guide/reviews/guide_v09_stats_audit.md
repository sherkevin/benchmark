# Guide v09 统计与可复现闭环审查

- 审查日期：2026-10-09
- 审查角色：只读方法与复现审查
- 审查范围：Guide v09、ADR-0048、v09 scorecard/review 模板、现有 Task49 preregistration/report 和 arrival-robustness experiment card
- 结论性质：这是一份审查意见，不是 CLOSED 签收，也没有运行实验或修改正式标准。

## 结论先说

v09 已经把“只看均值就关门”的问题改成了一个明显更严格的合同：有唯一 primary comparator、唯一主要消融、逐 delay 门槛、paired CI、candidate-specific artifact 和 exposure receipt。这些是必要改进。

但按“真实矩阵完成后即可关闭 Benchmark+Baseline”的强含义，当前文字仍不足以支撑无争议关闭。主要原因不是缺一个显著性检验，而是**目标总体、独立统计单位、future-Y、失败/拒答效用、暴露审计和 clean replay 的机器合同还没有完全闭合**。现有 270/120 streams 的报告可以证明事件回放和信息边界在已有 cohort 上可复核，不能把重复配置当作独立样本，也不能证明未见未来分子上的动态泛化。

因此当前判断是：**v09 可作为候选 closure target，但按本审查应保持 HOLD；需要把下列 P0 条款补成可执行合同后，才有资格把“做完即关门”作为事实判断。**

## 已确认的证据

1. `guide/versions/v09_20261008/guide.tex` 和 `docs/adr/0048-guide-v09-closure-contract-correction.md` 已固定 E1--E6、主比较、逐 delay 判定、Holm family、scaffold bootstrap、E5 policy timer 和 E6 exposure receipt。
2. `provider_fresh_context_natural_v1/report.md` 的 270 streams 实际是 5 folds × 2 seeds × 3 delays 的重复配置，输入仍为同一 157 个分子；`provider_fresh_drift_stress_v1/report.md` 的 controlled 结果为 120 streams。它们是同一有限 cohort 上的 trajectory evidence，不是 270/120 个独立分子样本。
3. `primary_arrival_robustness_v1/experiment_card.md` 已经采用“完整 order 是独立单位”的更清楚定义，并明确该 cohort 已被查看、不能充当新 confirmation；它与 Guide v09 当前通用的“stream/order 等权 + scaffold bootstrap”之间存在需要统一的统计合同。
4. 现有 receipts 能证明部分 event conservation、selected-only release 和 state-hash replay；它们尚未等同于 v09 要求的 candidate-specific checker、final exposure receipt、两份独立 closure review 和完整 E1--E6 关闭包。

## P0：必须在 Guide 中补成硬合同

### P0-1. primary estimand 与独立单位仍有歧义

**证据。** E1 写的是“Best Fixed−Random pooled paired effect”，又要求 outer fold 等权、fold 内 scaffold-clustered molecule pair；E2--E4 写的是“完整 stream/order strata 等权”，CI 又统一指定 scaffold-cluster bootstrap。没有给出唯一的数学定义来说明：一个 stream 是否是一条完整 adaptive trajectory、同一 molecule 在不同 seed/delay/order 是否算独立、fold/seed/delay 的权重如何嵌套。

**风险/反例。** 对 adaptive policy，逐 molecule 重采样会切断状态和 pending queue；对同一 157 分子的不同 delay/seed，复制配置不增加 cohort 独立性。若把 270 条 trajectory 当成 270 个独立样本，CI 会过窄。若同时按 stream 等权又按 scaffold cluster bootstrap，结果取决于未声明的先后顺序。

**必须补的决策测试。** 对每个 primary contrast 先写唯一 estimand：

```text
Delta_r = mean_t [Y_candidate(r,t) - Y_comparator(r,t)]
mu = E_{r ~ declared trajectory/order law}[Delta_r]
```

然后固定一种推断单位：

- 若目标是固定 cohort 下的 arrival-law，完整 order/trajectory 是独立单位，用 order-level CI/MCSE；不再对 event 或 molecule 做普通 bootstrap。
- 若目标是 scaffold 泛化，必须说明 bootstrap 的 cluster 层级、是否重新拟合每个 bootstrap、以及 CI 是 conditional-on-fit 还是包含训练不确定性；不能用一个通用“scaffold bootstrap”同时声称两种覆盖。
- E1 的 fold 等权公式、随机基线的重复数和配对方式必须写成公式，并保留每个 fold 的原始 paired vector。

**建议的关门条件。** scorecard 只有在“primary estimand、独立单位、权重公式、重采样算法、随机种子/版本”全部机器化并由独立 verifier 重算后才允许 PASS。否则 E2/E3/E4 只能记 2 分，不能关闭。

### P0-2. CI 的覆盖目标和 trajectory 依赖没有闭合

**证据。** v09 统一指定 10,000 次 scaffold-cluster bootstrap；arrival card 则为 128 个 uniform orders 使用 order-level Student-t/MCSE，并明确这是 order-law 条件区间。两套推断对象不同，但 Guide 没有说明每个实验采用哪一种，也没有禁止把同一 receipt 的两套区间混成一个主结果。

**风险/反例。** 在固定 cohort 上，molecule-level bootstrap 的样本看似很多，实际上 policy state 由完整顺序决定；它不能估计新 order 的变异。反过来，order-level CI 不能证明新 scaffold 泛化。一个区间跨 0 的统计不确定性也不能通过改变 cluster 层级消失。

**建议。** 每个 E2--E4 只能选一个主推断对象并在 preregistration 写明：

1. conditional finite-cohort / order-law：完整 trajectory 为单位，报告均值、MCSE、t 区间、empirical quantiles；
2. scaffold-generalization：外层 scaffold 为单位，报告 outer-fold/cluster bootstrap，并明确其不覆盖 arrival-law；
3. 两者若都报，一个必须标为 secondary，不得用 secondary 区间做 superiority 判定。

Bootstrap 必须保存 paired candidate/comparator vectors、PRNG 算法/版本、seed、draw count、cluster membership 和失败重采样检查；禁止从汇总均值重建区间。

### P0-3. “future-Y”没有被定义成可否证的目标

**证据。** E2 用“未来选择/future correctness”，但 E1--E4 的输入是同一有限 cohort 上一次调用一个分子；反馈在 $t+1+D$ 发布。文档没有固定 future-Y 是下一次 action、未来 $K$ 次 action、时间后半段，还是未读分子上的 held-out correctness。

**风险/反例。** 只要回放中有 delayed release，就可以把同一 cohort 的后续事件叫“future”，但这不证明对真正未见分子或未来 provider 状态有效。controlled flip 是预设的 evaluator-side intervention；它能检验声明的压力机制，不能单独构成自然 future-Y 泛化。

**必须补的决策测试。** 在每个 E2/E4 preregistration 里固定：

- $Y_{t+h}$ 的索引和 horizon（例如首次 release 后的下一个 $K$ 个 eligible decisions，或 held-out future block）；
- 该 $Y$ 在 action 前对 selector 不可见，何时对 evaluator 可见，何时 release；
- 分子 future block、arrival order 和 drift condition 的总体；
- 如果只做 finite-cohort trajectory，正文必须明确 claim 是“conditional on declared cohort/order law”，不能使用 broad online-generalization 表述。

Guide 的 E6 需要一个真正 sealed future-Y（或明确把 E6 限定为一次性 confirmation，而不把它叫动态泛化）。没有这个定义，E2 的“未来改善”不能成为可审计的 primary claim。

### P0-4. E2/E4 的 practical margin 与 CI 门槛逻辑不一致

**证据。** E2 要求点估计至少 `+2.0 pp`、CI 下界至少 `+0.5 pp`；E4 每个 delay 点估计至少 `+1.0 pp`、调整后 CI 下界只要求 `>=0`。文中称这些为 practical margin，但 CI 下界没有达到相应 margin。

**风险/反例。** 一个估计值 `+2.01 pp`、CI `[+0.01,+4.01]` 会通过 E2；它只能拒绝零，不足以证明“至少 +2 pp 的实际收益”。同样，E4 可用接近零的真实效果满足 CI 下界而被写成每个 delay 有实际增益。

**必须先裁定一个逻辑：**

- 若要证明 practical superiority，使用一侧非劣检验 $H_0:\Delta\le m$，CI 下界必须 $ge m$（E2 用 $m=2$，E4 用 $m=1$）；或
- 保留 CI 下界 $ge0$，但把 `+2/+1` 降级为点估计筛选，不得称 practical margin，也不能据此声称达到该收益。

这不是调数字，而是明确 claim 的可否证条件；在裁定前 Gate-C3 应 HOLD。

### P0-5. E6 exposure receipt 目前是必要字段，不是充分审计

**证据。** Guide 允许已查看的 157 条在 exposure audit 证明“没有影响决策”时承担 E6；receipt 只要求 manifest hash、freeze commit/time、truth/aggregate access audit 和 decision-impact audit。现有 Task49 文档明确 157 cohort 已被查看。

**风险/反例。** git commit 时间和作者自报不能证明研究者没有根据 provider 输出、局部结果或记忆修改代码；只审 truth 文件访问也不能排除 provider predictions 或聚合结果已经影响 candidate/threshold/baseline 选择。事后写“没有影响”无法被独立复核。

**建议的硬裁定。** 最终 E6 默认必须使用新 sealed manifest；已查看 cohort 只能作为 historical/robustness evidence。若保留复用例外，receipt 至少要有不可变的 OS/object-store access log、truth hash precommit、candidate/preregistration hash 的时间顺序、独立 auditor 读取日志并进行 mutation test；缺任一项即 `previously_inspected`，不能承担 E6。不要把“decision-impact audit”定义成作者自证。

## P1：当前条款会削弱 baseline 和结果解释

### P1-1. Modern nearest 的“可运行”不足以证明语义保真

BayesianRouter-adapted / LinTS-adapted 可能只是本地接口可调用的近似。Gate-B3 的 raw/recovery 只能证明程序复现，不能证明 posterior、prior、采样次数、tie-break、update timing 与论文算法一致。需要冻结 adapter deviation matrix、官方 reference unit tests（若有）和对每个 deviation 的 scope；否则应把它标作 adapted control，而不是 canonical primary。

### P1-2. Matched Online-SGD 的“matched”没有机器化初始化和更新合同

E3 说保留 feature/menu/prior/seed/budget/release clock，但没有强制记录 parameter shape、初始化字节、optimizer state、learning-rate schedule、gradient clipping、tie-break 和 numerical precision。candidate checker 必须从配置和源快照重算这些字段，不能接受自报字符串。否则 MIRA 与 SGD 的差异可能来自 backbone/初始化/优化器，而不是 relay 机制。

### P1-3. 主 baseline 与 candidate 的公平性没有覆盖 feature extraction 和 offline compute

E5 只对 selector+update policy 计时，E2 的“同 compute”没有定义 offline fit wall-time、parameter count、feature extraction、memory 或 checkpoint load。若 MIRA 使用预先计算的 Morgan/fit rates，而 baseline 每次在线重算，结果会偏向某一方。需要统一 `offline_budget`、`online_nonprovider_budget`、feature cache 规则和硬件/线程，并分别报告 training/selector/provider/total 四种成本。

### P1-4. Failure/unknown/refusal 有记录要求，没有可否证的评分合同

Table 7 提到 failure/unknown/refusal，E5 要求入账，但没有预先定义 primary correctness 的分母、refusal coverage、selective risk、unknown 的 utility 或失败惩罚。一个策略可能拒答大量难题而保持 answered accuracy，通过 E5。必须在开跑前冻结：所有 selected attempts 是否进 primary denominator、coverage、failure rate、refusal utility/penalty，以及“拒答不改变 chemical correctness”的规则。没有 utility 时只能报 Pareto 曲线，不能说“收益值得代价”。

### P1-5. Multiplicity 仍漏了 condition/claim family

E4 对三个 delay 使用 Holm 是正确方向，但 E2 同时有 controlled superiority、natural non-inferiority，E6 又有 natural/controlled 两种结论；方法还会比较多个 secondary baseline、rescue/harm 和多个 endpoint slice。需要在 preregistration 明确唯一 confirmatory claim family 和 gated guardrails，或为每个额外正式 claim 预注册调整。否则“primary comparator 固定”仍可能掩盖在 condition 选择上的多重比较。

### P1-6. Candidate-specific checker 的字段列表还不是验证算法

当前字段（run/method/contract/input/source/primary comparator/exposure）是必要的，但 checker 必须 fail-closed 地：从原始字节重新计算 hash；验证 run ID 唯一、事件/表格行一一对应、method/comparator 属于 freeze manifest、input/scaffold/fold 集合完整、state hash/clock/pending conservation 一致、没有旧 anchor 的 SD/covariance/order 向量复用，并对删除/重排/修改一个事件做 mutation test。仅检查 JSON 字段存在不能形成 Gate-C4 证据。

### P1-7. Clean replay 还缺“独立环境”和“独立实现”硬条件

H10/replicator review 要求独立 verifier，但没有冻结 lockfile/container、离线网络策略、CPU/BLAS/NumPy/RDKit 版本和 source snapshot。若 verifier import runner 的相同聚合函数，它可能复制同一 bug。建议：独立 verifier 禁止导入 runner 和 result aggregator；从 raw event/schema 重新计算 terminal label、release tick、state transition、metrics，并执行一条非法 release、一条 label mutation、一条删除 event 的负例，验证 fail-closed。记录 verifier 自身 commit/hash/环境/运行命令。

### P1-8. Stream 数量和随机性不足以支持当前 CI

现有 seeds `{0,1}` 对策略随机性很弱，且 deterministic methods 的复制不增加独立信息；delay/fold 也不是独立随机重复。Guide 目前没有最小 independent trajectory/order 数，也没有规定两种 RNG 的分离。应固定 order RNG、policy RNG、provider RNG 分离，并预注册最低 independent orders/streams；若只有两个固定 seed，结论必须写成 conditional seed mixture，不能把 seed 复制当不确定性估计。

## P2：可行性与成本风险

1. E5 的 `p95 <= 100 microseconds` 没有硬件、语言、线程、计时器和 warm-up 定义，Python/NumPy/BLAS 下可能不可复现；应固定机器/OS/CPU/thread、warm/cold、计时边界，并至少报告 bootstrap CI。若只需要相对 Frozen，应把绝对阈值改成有依据的 envelope。
2. E5 要求 memory、restart/recovery 和 endpoint cost 同表，但没有 memory/磁盘/总 wall-time 停机上限。把这些变成预注册 resource budget，先跑 one-order smoke，再决定能否执行全矩阵；资源不足只能标记 incomplete，不能删 baseline 或减少 order 后继续声称闭合。
3. 完整 raw event、checkpoint、per-order state 对 8 个以上方法和多 delay 可能很大。Guide 应固定压缩 schema、每条事件的最小字段、decoder hash 和存储预算；压缩不能删掉可重算 state 或失败 traceback。

## 推荐的最小修订顺序

1. 先补一页“Estimand and unit contract”：为 E1--E6 各给公式、总体、独立单位、权重、CI 覆盖和条件范围；解决 trajectory bootstrap 与 scaffold bootstrap 冲突。
2. 再补“Future-Y and failure utility contract”：冻结 horizon、held-out/future cohort、coverage/selective risk、failure/unknown/refusal 分母和 utility/非劣规则。
3. 把 E2/E4 margin 的 CI 逻辑裁定为“CI lower ≥ margin”或明确降级 claim；把 condition/claim family 的 multiplicity 写入同一 preregistration。
4. 将 candidate checker、exposure receipt 和 clean replay 改成带 schema、hash recomputation、mutation tests、access log 和独立环境的 fail-closed verifier。
5. 最后才执行全矩阵：先用一条完整 order/stream 做资源与 replay smoke；所有 primary baseline 做 semantic adapter parity；再跑预注册的 independent order/seed 数量。完成后才填写 scorecard，而不是先按目标均值写 CLOSED。

## 最终判定

**Evidence：** v09 已有较好的目标结构，现有 receipts 证明部分回放链和信息边界；但真实 E1--E6 关闭包尚未完成。

**Prediction：** 如果不补上述 P0，最容易出现的是“同一 cohort 的重复 trajectory 产生过窄 CI、controlled event 被误称 future generalization、拒答/失败改变分母、已查看 157 被事后审计为未读”。这些不是数值好坏问题，而是审稿人可以直接否决的识别与复现问题。

**Recommendation：** 暂不允许把当前 Guide v09 宣布为“做完即可关闭”的最终版本；保持 `HOLD`，先修订统计/暴露/clean replay 合同，再由独立作者与复现审查员按新版本评分。修订完成且真实证据逐项通过后，才可以将 Benchmark+Baseline 标为 `CLOSED`。

