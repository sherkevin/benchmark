# 投稿 Gate 缺口优先级审查：arrival 完成、CGEF 终止之后

- 日期：2026-10-08
- 性质：只读方法审查与下一小任务建议
- 范围：只核对当前生效 Benchmark + Baseline 口径、`current_acceptance_audit_v2`、`baseline_readiness_v1`、Stage-A/B scorecard、到达顺序复核和 CGEF 负结果。
- 本次没有运行实验，没有修改 `paper/aamas2027/main.pdf` 或三份生效主线文档。

## 结论先行

当前最值得做的下一件小任务不是继续寻找新的融合模型，也不是扩展 hERG/AMES，而是做一次**投稿范围锁定后的 baseline receipt join 和 gate 状态收敛**。具体名称建议为：

> `baseline_scope_lock_and_receipt_join_v1`

这项任务用已经存在的原始回执、恢复文件、`baseline_contract_v3` 和 Stage-B v3 scorecard，生成一张机器可核对的主 baseline 矩阵，并明确哪些方法进入论文主比较，哪些只作为 adapted、ablation 或 audit-only。它同时解决两个审稿风险：

1. 旧审计和新 Stage-B 对同一项目给出不同门状态，读者无法知道哪个是当前有效结论；
2. `baseline_readiness_v1` 仍把 GraphRouter/Laya-online 的未完成状态放在总体 baseline 名单里，而当前论文已经把它们放在 audit-only/out-of-scope，角色边界还没有在一份投稿级证据表中闭合。

这是一项 CPU/文档审计任务，不需要新的 provider、API、GPU 或 confirmation 标签。它不能凭空提高 MIRA 的效果数字，但最可能直接提高 Benchmark/Baseline gate 的可审查性，并防止审稿人把“适配不完全”误判为不公平比较。

## 一、证据：已经完成的部分

下表只列当前能由文件和独立回执直接支持的事实。

| 事实 | 证据 | 对投稿 gate 的含义 |
|---|---|---|
| 当前投稿切片已经明确 | `docs/研究主线/生效/03-Benchmark与Baseline.md` v004：TDC BBB truth、四个 checkpoint-bound fresh providers、157 个 scaffold-disjoint molecules、一次调用、selected-only delay `{0,1,4}`、controlled correctness drift | Benchmark 不是完全未定；应按这个 declared scope 审查，不要把 33-provider development 或 hERG/AMES 混进同一张主表 |
| 狭义 Stage-B 已通过 | `stage_b_scientific_submission_gate_v3/scorecard.json`：Gate A=15/16、Gate B=13/16、Gate C=14/16，状态 `PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE` | 在已声明的 BBB 一次调用 scope 内，主要 baseline 角色、任务匹配、共同协议和恢复证据已有可投稿基础 |
| 自然 fresh 结果是 null，受控 stress 是正结果 | `provider_fresh_confirmation_v1/report.md`、`stage_b_scientific_submission_gate_v3/report.md` | 可以讲“受控可靠性变化下的 recovery”，不能讲一般自然漂移或 stationary online superiority |
| 主要 baseline 的角色已被部分固定 | `baseline_contract_v3/contract.json`、`baseline_role_roster_v1/roster.json`、`baseline_eligibility_matrix_v2/` | Random、Best Fixed、Frozen、Matched SGD、Greedy、BayesianRouter、MIRA-Relay 可进入 declared-scope 主审查；不同 likelihood/适配层不能直接 raw 排名 |
| GraphRouter/Laya-online 仍未完成同合同效果回执 | `baseline_readiness_v1/report.md`、`laya_online_adapter_v1/blocking_report.md`、GraphRouter blocking report | 这两个方法不能被写成已完成的在线 baseline；应明确保留为 audit-only 或 separate static audit |
| 到达顺序复核已经独立完成 | `primary_arrival_robustness_v1/report.md`、`experiments/logs/task49_primary_arrival_robustness_20261008/independent_full_v2/verification.json` | 补强 C4 的固定 cohort order uncertainty；它不是新 benchmark、不是新 confirmation、也不是新 baseline |
| CGEF 干预是负结果且已停止 | `conditional_fusion_intervention_v1/report.md`、`cgef_v1/independent_verification.json` | 关闭一个终局 Gaussian fusion 候选，避免继续调参；它不改变 baseline 选择，也不支持第二工具一般有效或无效 |

## 二、当前材料之间的冲突和真正缺口

### 1. 旧审计状态与新 Stage-B 状态没有收敛

`current_acceptance_audit_v2/report.md` 的历史主体仍写着 Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`，随后只追加到 Stage-A `13/16`、`12/16`、`12/16`。Stage-B v3 又在更窄、明确的 scope 下给出 `15/16`、`13/16`、`14/16`。这些数字并非必然互相矛盾：它们的时间、scope 和 confirmation 状态不同；问题在于当前没有一份最新机器表明确说明**哪个状态用于论文、哪个状态仅作历史审计**。

这是审稿风险，而不只是文档美观问题。审稿人看到旧表会问：到底 baseline 是否已通过？四个 fresh provider 是不是已经做过 confirmation？GraphRouter/Laya 是主比较还是未完成实验？如果正文、baseline readiness 和 scorecard 各自给出不同答案，B3/B4/C2 的信任会下降。

### 2. Baseline readiness 和当前论文主表的角色边界仍不够硬

`baseline_readiness_v1` 把 GraphRouter 和 Laya/JEV-online 列为未完成/阻塞，这是事实；当前生效 Benchmark 文档和论文却把它们放到 audit-only/out-of-scope。两种表述都可以保留，但必须在**唯一投稿 baseline 矩阵**中变成可机器检查的角色字段，而不能让读者自行推断。

尤其要避免以下三种混淆：

- 把静态 Laya 表示诊断当成在线 JEV baseline；
- 把适配后的 Greedy/BayesianRouter 当成原论文的原生复现；
- 把 Replay-SGD 的旧 BT prior receipt 与当前 common-direct-prior Matched SGD 做 superiority 排名。

### 3. Arrival 和 CGEF 都不是当前最缺的 baseline 证据

Arrival 复核回答的是固定 cohort 上随机到达顺序的策略不确定性；它支持 C4 和边界解释，不能代替同合同 baseline fidelity。CGEF 只替换已经取得的 pair 流上的终局融合，pooled 相对原规则约为 0、相对 `ignore-second` 为 `-0.000659`，按预登记停止线结束。继续增加这两条线的实验不会补上 baseline role、source fidelity 或公平合同缺口。

## 三、按“能提升投稿 gate 的幅度”排序

### P0：锁定投稿 scope，并做 baseline receipt join（下一小任务）

这是当前收益最大的最小任务。输入只读使用：

- 生效 `03-Benchmark与Baseline.md`；
- `baseline_contract_v3/contract.json`；
- `baseline_eligibility_matrix_v2/` 和 `baseline_role_roster_v1/`；
- Stage-B v3 scorecard；
- fresh natural/stress 的 raw receipt、verification 和 aggregation；
- arrival v2 与 CGEF 报告，仅作为 scope/边界证据，不进入 baseline 排名。

输出应包含：

1. `scope`：明确论文主比较只覆盖 BBB、四个 fresh providers、一次调用、selected-only、delay `{0,1,4}`；
2. `role`：逐行标记 Random、Best Fixed、Frozen、Matched Online-SGD、Greedy LinUCB、BayesianRouter、MIRA-Relay、Retrospective Best Fixed；MIRA-Relative 标为 ablation；GraphRouter/Laya-online 标为 audit-only；Replay-SGD 标为 separate historical control；
3. `fidelity`：源码/论文版本、commit/checkpoint、适配差异、likelihood、warm start、许可证和独立 recovery 指针；
4. `fairness`：menu、feature、fit-only 权限、selected-only feedback、delay、call budget、seed、failure/unknown/refusal、cost/state 字段；缺失字段写 `MISSING`，不推算；
5. `uncertainty`：paired molecule/scaffold interval、seed/stream 分母和 recovery 状态；不同 likelihood stratum 不做 raw cross-ranking；
6. `claim_scope`：每一行写明能支持的 claim 和明确不能支持的 claim；
7. `status_reconciliation`：解释旧 `current_acceptance_audit_v2`、Stage-A、Stage-B 的日期和 scope，不删除历史状态，只给当前投稿 scope 一个唯一状态。

最小机器验收条件：

- 论文主表中的每个 baseline 都能指到 raw receipt、独立 recovery 和同一 input/menu hash；
- 没有任何主比较 baseline 仍带 `MISSING` 的关键公平字段；若确实缺失，就从主排名降为 separate/audit-only，不用文字掩盖；
- GraphRouter/Laya-online 不再同时出现在“主 baseline 已完成”和“未完成阻塞”两个角色；
- `stage_b_scientific_submission_gate_v3` 的 narrow PASS 与 broader historical HOLD 同时可解释；
- arrival/CGEF 只出现在对应的 uncertainty/negative-result 边界，不被误接进主效果表。

**决策测试：**如果上述 join 发现某个声称的主 baseline 没有完整回执，最小修复是降级该 baseline 的 claim scope；只有当它是论文不可替代的最近邻时，才另写一张该 baseline 的实验卡。不能为了填表重新解释旧收据或新增未经冻结的适配。

### P1：以 P0 的结果重新生成论文用主 baseline 表和一句话边界

P0 通过后，再把同一机器矩阵压成正文表：主表只列可部署比较；adapted、ablation、audit-only 放脚注或附录。正文对 MIRA-Relay 的 claim 必须和 Stage-B v3 一致：

- natural fresh menu 上是 null/stability result；
- controlled correctness drift 上是 recovery result；
- 不声称自然 provider drift、hERG/AMES、post-call refusal 或 full 33-provider model-fresh 泛化。

这一步不需要新实验，但会直接修复摘要、表格、baseline 段落和三份标准之间的语言漂移。

### P2：只在 P0 发现不可补的关键缺口时，做一个定向 baseline 实验

候选优先级应是“最强合法近邻缺失”而不是“再找一个名字”。若 Matched Online-SGD 或 BayesianRouter 的 fresh receipt 不能满足 P0 的统一字段，再为**同一 fresh menu、同一四 provider、同一 delay、同一预算**写一张小型复现实验卡。若 P0 已证实它们完整，则不要重复跑。

GraphRouter/Laya-online 只有在论文重新把它们写成主比较时才值得实现；在当前 declared scope 下，已有 audit-only 边界足够，运行弱化版会降低而不是提高 B10。

### P3：暂不扩展的事项

- 不把 hERG/AMES 当作当前 BBB 投稿 gate 的必要条件；它们的 endpoint equivalence/lineage 尚未闭合，扩展会引入新的 Gate-A 风险。
- 不继续调 CGEF covariance、Gaussian basis 或第二工具融合阈值；候选已按卡内负结果停止。
- 不把 arrival fixed-cohort 结果写成自然生命周期漂移或新 confirmation。
- 不用 public cross-domain router 数字填充 BBB 主表；只能作方法家族动机，不能当本任务结果。

## 四、预期收益、风险和停止条件

### 预期（预测，不是已观察结果）

若 P0 的所有主 baseline receipt 均能闭合，最可能的收益是：

- 让论文的 Benchmark/Baseline 章节只有一个当前状态，消除 11/16、13/16、15/16 等历史数字造成的歧义；
- 把 B3/B4/C2/C4 的证据指针从“分散可找到”提升为“逐行可核对”；
- 使 arrival 的不确定性和 CGEF 的负结果各自落到正确的证据位置；
- 让 reviewer 能区分 MIRA 的主比较、现代 adapted comparator、ablation 和 audit-only 方法。

这不能预测新的效果增益，也不能把自然 null 结果改成 positive。

### 失败和停止条件

任意一项出现以下情况，P0 应停止并记录，而不是补写数字：

- 主 baseline 的 raw receipt 与论文声明的 menu/feature/delay 不一致；
- recovery 不能独立重算，或只能从 aggregation 反推；
- confirmation 标签进入了 fit、epoch、阈值或 baseline 选择；
- adapted likelihood/prior 与 strict matched control 混在同一排名；
- 关键字段只能由事件数量或模型维度推算；
- 角色边界仍需依赖读者猜测。

失败时的正确输出是降级 claim 或新增一个具体 blocking report，不是再次运行 CGEF、arrival 或未经登记的 GPU 任务。

## 五、最终审查判断

**证据判断：**在声明的一次调用 BBB scope 内，Stage-B v3 已经说明 benchmark 和主要 baseline 具备投稿基础；arrival 完成后 C4 更强，CGEF 负结果补充了方法边界。旧 `current_acceptance_audit_v2` 的 HOLD 不能直接代表最新 narrow scope，但它保留的宽范围问题仍然有效。

**方法判断：**当前最大风险不是缺少一个更 fancy 的 baseline，而是把不同 scope、likelihood 和完成状态的 comparator 混在一起，使主张无法追溯。继续寻找新 baseline 的边际收益低于先完成 receipt join。

**推荐：**立即执行 P0 `baseline_scope_lock_and_receipt_join_v1`，完成后再决定是否需要一个定向 baseline 实验。若 P0 通过，论文应以 Stage-B declared-scope 结果为当前投稿口径；若 P0 暴露关键缺口，先降级对应 baseline/claim，再做最小的同合同修复。三份生效文档和唯一主版 PDF 在 P0 前保持不变。
