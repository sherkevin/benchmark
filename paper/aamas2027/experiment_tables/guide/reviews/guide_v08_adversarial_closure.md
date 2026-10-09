# Guide v08 反方闭环审查

**对象**：`paper/aamas2027/experiment_tables/guide/versions/v08_20261008/guide.tex`（含 Gate-A/B/C 前缀、canonical baseline IDs、closure scorecard、ADR-0047、固定 E6/Table 9）。  
**结论**：**FAIL（尚不能据此保证 Benchmark+Baseline 可关闭）**。v08 已把多数审计要求写成可签收合同；但即使团队逐项填入真实证据，仍有几处会让内部标准或审稿人拒绝“已闭环”的解释。需要先修合同/范围，再按其跑证据；本审查未改源文件、未跑实验。

## 分数与通过判断

| 项目 | 判断 | 原因 |
|---|---|---|
| H1–H10 结构 | **PASS（结构层）** | 条件完整，证据路径/hash/verifier 要求明确；真实数据仍未知。 |
| Gate-A | **PASS（结构层）** | A1–A4 已有 canonical 前缀且覆盖 truth、routing、split、lineage。 |
| Gate-B | **PASS（结构层，定义仍有缺口）** | B1–B4 角色清楚；但“现代近邻”是否至少一个可运行主 baseline、BayesianRouter 与 LinTS 的主/审计地位仍需在 scorecard 先冻结。 |
| Gate-C | **FAIL（阈值和覆盖仍不充分）** | E1 headroom 门槛过弱；E4 未规定逐 delay 的最低证据；E6 的 untouched 前提与项目现状需核实；Table 9 是外部依赖。 |
| 参考分布与结果隔离 | **PASS（文字层），风险仍高** | 明确 v07 只作 anchor，且禁止继承 SD/covariance/win rate；主表仍需逐数字绑定 candidate raw receipt，避免写作回填旧锚点。 |

## 必须在真实运行前修正或冻结的阻塞项

### 1. E6 的“untouched”定义与当前数据状态冲突（CRITICAL）

v08 把固定的 157 条 scaffold-disjoint molecules 写成 untouched confirmation，并以此作为 Stage-B 关闭条件。项目现有记录已说明这 157 条 confirmation molecules 已被查看并用于既有回放/审计；scaffold-disjoint 不等于未读。若标签、provider 输出、顺序或旧结果已被人读取，则它们不能支持“未读泛化”。

**最小解决**：在 ADR-0047/scorecard 中二选一并固定：

- 证明 157 条在 candidate freeze 前从未被任何调参、阈值选择、方法比较或人工检查读取，并保存访问/哈希收据；或
- 将 E6 改名为 `frozen replay confirmation on previously inspected cohort`，删去 untouched/generalization claim；若仍要泛化，另建真正未读 scaffold 集和新版本 ADR。

在没有该裁定前，Gate-A3、E6 和最终 CLOSED 必须为 HOLD。

### 2. E1 “Best Fixed 相对 Random 正向且 ≥3/5 fold”几乎没有 headroom 要求（MAJOR）

E1 只要求方向为正和 3/5 fold 通过，没有预登记最小效应、CI 或 paired denominator。极小的偶然正差即可关闭任务有效性，即使 provider 差异不足以支撑动态 routing。v08 其他地方使用 +2 pp / +0.5 pp，但 E1 没有对应尺度，阈值不自洽。

**最小解决**：在 E1 冻结一个 `Δ_task`（按 molecule/scaffold paired effect），同时要求 ≥3/5 fold 的下界或预登记 practical margin；若只想证明存在信号，应明确 E1 仅为“非零可辨识性”而不能叫 headroom gate，并把结果交给 E2 的最小 headroom。

### 3. E4 允许 pooled 压力通过而某个 delay 完全失败（MAJOR）

E4 要求 pooled contrast 满足 E2 margin，另写“每个 delay 单独报告”，但没有逐 delay 的停止线。D=0 的大收益可以抵消 D=4 的泄漏/失效，仍通过 pooled gate；这与方法标准对 delay 分层和 selected-only release 的要求不够。

**最小解决**：为 D∈{0,1,4} 预登记逐层判定（至少报告 paired CI、coverage、非法提前反馈为硬 FAIL）；若允许某层失败，必须把 dynamic scope 限定到通过的 delay 子集，不能以 pooled “dynamic”概括全部三层。

### 4. E2/E3 的 CI、比较对象和多重判定没有统一统计单位（MAJOR）

E2 使用 controlled contrast 点估计 ≥2 pp 且 paired CI 下界 ≥0.5 pp；E3 使用机制 contrast ≥1 pp 且下界 ≥0；E4 复用 E2 margin；但没有规定 CI 以 128 arrival orders、157 molecules/scaffolds 还是 stream 为独立单位，也没有说明多个 comparator、delay 和 natural/controlled 判定是否进行 multiplicity 控制。团队可以选对自己最有利的单位或层级。

**最小解决**：在 scorecard 固定每个 gate 的 estimand、cluster 单位、CI 方法、primary comparator 和 secondary/exploratory 标记；主结论只用一个 primary contrast，其他 baseline/延迟作为预登记次级结果。不得在结果后切换 pooled、equal-scaffold 或 order weighting。

### 5. 主 baseline roster 仍允许缺失可运行现代近邻（MAJOR）

v08 将 `BayesianRouter-adapted` 标 canonical primary，将 `LinTS-adapted` 标 audit-only（除非另行声明）。这使“现代近邻”可以只剩一个作者适配且未必有可运行官方实现；公开跨域数字不构成 BBB baseline。若 BayesianRouter 适配失败，Gate-B 不能靠 LinTS audit-only 补齐。

**最小 source check**：在 Gate-B1 先冻结“至少一个可运行 canonical modern nearest”的具体 method_id、source commit、适配差异和 fallback 处置；若 BayesianRouter 不可恢复，LinTS 必须升级为主 baseline，或明确 Gate-B FAIL，而不能事后把旧结果/论文数字填入。

### 6. E3 机制消融不够隔离 prior、表示、预算和更新目标（MAJOR）

`full / 去 state / 去 interface / independent residual / family/provider permutation` 是名称清单，不规定每个消融保留什么 feature、prior、初始化、update budget、探索和 compute。一个“去 interface”若连 feature 或预算也改变，E3 的 +1 pp 不能归因于 Relay 机制；permutation 也可能是 evaluator-side control 而非合法 online policy。

**最小解决**：为每个消融写一行 matched contract（唯一差异、可见字段、state hash、budget、seed、compute、是否可部署），并要求 candidate vs own Frozen/SGD/Replay 的同一 paired streams；不能恢复的消融降级为 audit-only，机制 claim HOLD。

### 7. E5 性能阈值同时使用相对和绝对 cap，但硬件/计时层仍可能不一致（MODERATE）

E5 的 `selector+update p95 ≤2× Frozen 且 ≤100 μs (kernel/replay)` 是目标，不替代 B12/B13/B15；但 “Frozen” 的 p95、kernel/replay 的边界、CPU/GPU、batching 和 provider I/O 尚未在矩阵中定义。一个方法可在 kernel cap 内通过，却在端到端调用成本上不可用；也可能因 Frozen 极快而相对阈值失败。

**最小解决**：冻结硬件、batch size、warm/cold、计时起止事件和 p95 统计单位；把 kernel/replay 只作为辅助 feasibility gate，端到端 latency/calls/memory/utility 单独有 PASS/FAIL 或明确不做实时 claim。

### 8. “参考分布不冒充结果”仍依赖正文执行，需机械检查（MAJOR）

v08 正确声明 v07 不能继承 SD、covariance、win rate，但 E2/E4 要求报告 order mean、paired SD、quantile、strict-win，字段名与 v07 完全相同。没有机器规则阻止作者把旧 128-order 文件复制到新 candidate 表。

**最小解决**：每个 candidate 结果表必须含 candidate-specific `run_id/source hash/manifest`，并由 checker 验证 method_id、state/config hash 与 v08 freeze receipt 一致；若只出现 v07 artifact ID 而没有新 raw JSONL，Gate-C4 自动 HOLD。

## 覆盖性审查

- **E1** 覆盖任务信号、joint bits 和 headroom，但需补 practical headroom 与 provider lineage 分层。
- **E2** 覆盖主要动态比较；Permutation 的可部署性和 own candidate comparator 必须固定，不能只用旧 Relay permutation。
- **E3** 有机制消融名目，但需要 matched contract 才能支持 attribution。
- **E4** 覆盖 selected-only、delay、failure 和 controlled flip；缺逐 delay 最低判据，且自然 lifecycle/arrival shift 被正确列为另行范围，不能偷渡成泛化。
- **E5** 覆盖错误、成本、恢复；需把 kernel 和端到端层级固定。
- **E6** 有固定 Table 9 artifact ID，这是进步；但是否 untouched 是当前最大硬门，且只完成 E6 不会自动证明新的 provider model-fresh 泛化。

## 可以保留的闭环设计

- Gate-A/B/C 和 `Gate-*` 前缀消除了旧版 B 编号冲突；scorecard、双人复核、证据 hash 和 canonical IDs 使关闭条件可审计。
- 明确将 v07 降为 anchor，禁止继承旧 SD、covariance、win rate，是必要且正确的防 cherry-picking 条款。
- Random、fit-only fixed、Retrospective fixed、Frozen、matched SGD/Replay、Greedy LinUCB、BayesianRouter-adapted 的角色覆盖基本完整；LinTS 是否主 baseline 需在运行前冻结。
- Table 3–9 与 E1–E6 映射、failure/unknown 分母、paired vectors、recovery 和成本字段形成了可执行矩阵。

## 最终 PASS/FAIL 判定

**当前 v08：FAIL / HOLD。** 不是因为已有结果不好，而是因为以下四项在“真实证据填满”前仍能造成不可判定或过度结论：

1. E6 的 untouched 身份尚未与已查看 157 分子事实对齐；
2. E1 headroom 阈值弱于 E2，允许任务信号过小；
3. E4 pooled gate 没有逐 delay 通过/失败规则；
4. E2/E3 多比较、多层级和 CI 统计单位未预先唯一化。

补齐上述四项，并冻结现代 nearest 的可运行主 baseline、消融合同和 candidate-specific artifact checker 后，v08 才具备申请 `CLOSED` 的结构资格。即使通过，也只能关闭 benchmark/baseline 验收；不能把固定队列参考分布写成 MIRA 的自然部署泛化或普遍 superiority。
