# AAMAS 2027 Benchmark / Baseline 验收审计（2026-09-30）

性质：开发阶段内部审计，不是 AAMAS 官方评分，也不是录用预测。

## 结论

Random 全量收据、task-native static-router 审查以及 Greedy/PILOT 的统一 full receipt 已补齐 baseline 合同证据，但 benchmark 资格、跨方法公平合同和实验矩阵仍未闭合。Gate A/B/C 继续 HOLD；不打开 confirmation，不提交新的效果型 A800。

| 闸门 | 临时分数 | 通过线 | 状态 |
|---|---:|---:|---|
| Gate A：Benchmark 选型 | 11/16 | ≥12，A1/A3/A4 各≥3 | HOLD |
| Gate B：Baseline 选型 | 11/16 | ≥12，四项各≥3 | HOLD |
| Gate C：实验矩阵与结果 | 10/16 | ≥12，四项各≥3 | HOLD |

## 2026-09-30 hERG 资格更新

端点来源补充核验已落盘于 `task/task49-pharma-benchmark-baseline-freeze/endpoint_audit/semantics_update_20260930.md`。Wang 2016 只闭合了 TDC 来源身份，未闭合跨数据集 cutoff；Siramshetty/NCATS 的 8,993 行归档不能直接等同 FP-ADMET 的 9,204 个训练源；FP-ADMET SI 对 Cai blocker 给出 `IC50 ≤ 10 µM`，但把 liability 与 blocker 分开。故 hERG 继续保持候选资格，不能进入主 benchmark 或 confirmation；Gate A 不变。 GraphB3 另已补论文级 split provenance（6029/1771 与 10 seed/split protocol 一致），但具体 checkpoint 的 seed/member binding 仍缺；provider lineage 因此仍未闭合。

## 2026-09-30 LGBM lineage update

LGBM-BBB 的只读重建完整跑过 Mordred，但当前 runtime 得到 7,296 行，而发布 notebook 目标为 7,298 行；实际训练中间文件未随包发布。因此 LGBM 仍按 G2 处理，不能把重建 split 当作 checkpoint-specific 独立资格；Gate A 不变。

## 2026-09-30 provider-fresh menu qualification update

provider-fresh 路线已修正为 v2：
`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_menu_v2/report.md`。
v1 的 verifier 漏掉了 14 条同标签重复源行；v2 将它们全部写入 excluded ledger，并强制检查 1,624 条源行恰好覆盖一次。v2 还绑定 RDKit `2025.09.2` 的 canonical/scaffold 重算，并机器检查 confirmation scaffold overlap。

v2 固定 845 条 provider-train、180 条 provider-validation，train/validation scaffold overlap 为 0，candidate/confirmation scaffold overlap 为 0。candidate 与 development 有 118 个 scaffold 重合（97/21），所以不能把 provider 训练集称为对 development 的 scaffold-disjoint 数据；该边界已保留在 manifest 和报告中。

随后按 v2 合同完成 CPU provider training/validation：4 个 checkpoint、720 条验证预测由独立 verifier 重算通过，最大概率误差小于 `5.0e-11`。这补齐了成员分区和前向收据，但仍没有生成 confirmation 输出，也没有完成完整环境锁定、最终 provider 菜单和 selector 公平合同；Gate A 仍 HOLD，Graph family 的较弱 validation sanity 表现不构成删模型或改 split 的理由。

Gate B 的 common-stratum smoke 也已完成：7 条方法流、210 条事件，统一 sealed direct neural prior、BT+CLS hidden-64 和 selected-only feedback，独立 verifier 通过。smoke 同时发现旧 Greedy ridge prior 与 direct neural prior 并不相同，故没有把旧 receipt 追溯改写；full 60-stream common-prior receipt、paired uncertainty、成本账本仍待完成，Gate B 继续 HOLD。

## Gate B 更新

主表现在包含：Random、Best Fixed、Oracle、Frozen、matched Online/Replay SGD、Greedy LinUCB、PILOT-adapted、BayesianRouter-adapted，以及单独分表的 MLP BCE/BT+CLS static router。Random 的 30/30 streams、Greedy 当前统一 receipt 的 60/60 streams、PILOT 当前统一 receipt 的 60/60 streams 和 Matched 新 unified receipt 的 60/60 streams 均有独立恢复；static router 的 40 fits/5,776 evaluations 有 checkpoint/NumPy recovery。对六类主 baseline 的旧 raw JSONL 已完成只读字段审计（`task/task49-pharma-benchmark-baseline-freeze/baseline_raw_run_schema_audit_v1/report.md`）；Greedy 与 PILOT 已补当前 representation/input 下的统一 full-development receipt，但跨方法 feature/warm-start/budget/cost 公平性仍未闭合。

GraphRouter 不进入可运行主表。它的原始方法需要 provider description embedding、graph edge semantics 和 `torch_geometric`；这些输入在 BBB routing layer 没有冻结。把这些输入删掉后用自定义图模型，不能称作原 GraphRouter。该范围和来源已记录在 `task/task49-pharma-benchmark-baseline-freeze/static_router_sufficiency_audit_v1/report.md` 与 `graphrouter_static_adapter_v1/blocking_report.md`。

## 尚未解决的硬缺口

1. **Benchmark**：hERG assay/threshold equivalence、provider training membership、旧 development panel 与 confirmation overlap、model-fresh lineage。
2. **Baseline**：Matched、native-BT BayesianRouter、Greedy、PILOT 和 MIRA/Matched 共同 receipt 已有统一 raw-run/recovery 收据；native-BT BayesianRouter、Greedy 和 PILOT 的 60-stream receipt 均通过基于声明 commit 不可变 git tree 的逐事件 verifier。PILOT 的统一报告明确其是 adapted baseline，且普通 cosine/UCB/ridge update 在三个 delay 均未超过 frozen control；旧日志仍保留不改写。跨方法 feature/warm-start/budget/cost 公平性和 provider lineage 仍未统一。BayesianRouter、Greedy、PILOT 与 MIRA 的新 receipt 已补齐 read-set/state hash/terminal 快照，但不能由此自动关闭所有 baseline 资格。Laya 只能在正式范围审查后决定是静态 audit-only 还是可运行在线适配。
3. **矩阵**：E1–E5 完整闭合、action-supported 动态事件压力、paired uncertainty、统一成本和一次性冻结 confirmation。新增 action-supported transient/persistent replay 已给出 240 条 CPU stream 和独立 recovery；随后完成 v2 metric-only 审计，把 injected-terminal loss、feedback blind window 和 release 后 recovery 分开。该审计不替代新的完整 effect receipt；成本/统一 paired uncertainty/confirmation 仍未闭合。

4. **公平性审计**：五条 unified receipt 已完成只读 cross-method contract audit。输入/menu/fold/seed/delay/selected-only/feedback clock 一致，但 prior 来源、初始 covariance、PILOT projection 和 update semantics 不一致；因此当前只允许方法内 `online−frozen` paired effect，禁止把 raw accuracy 混成一条 superiority 排名。报告：`task/task49-pharma-benchmark-baseline-freeze/cross_method_fairness_audit_v1/report.md`。

因此新增 baseline 证据提高了 Gate B 的可审计性，也明确了公平比较的边界，但没有改变当前项目的总体 HOLD 状态。
