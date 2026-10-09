# AAMAS 2027 当前验收核对表

日期：2026-09-29  
性质：开发阶段内部审查，不是 AAMAS 官方评分，也不保证录用。  
本次重点：Benchmark 选型、Baseline 选型、实验矩阵与结果是否能支撑主张。

## 1. 当前结论

| 项目 | 当前分数 | 通过线 | 状态 |
|---|---:|---:|---|
| 故事线与创新 | 47.5/100 | ≥85 且硬门槛全过 | FAIL |
| 方法论 | 37/60 | ≥49 且硬门槛全过 | HOLD/FAIL |
| Benchmark + Baseline 总分 | 37/60 | ≥49 且 H1--H10 全 PASS | FAIL |
| Gate A：Benchmark 选型 | 11/16 | ≥12，A1/A3/A4 各≥3 | HOLD |
| Gate B：Baseline 选型 | 9/16 | ≥12，四项各≥3 | HOLD |
| Gate C：实验矩阵与结果 | 10/16 | ≥12，四项各≥3 | HOLD |

三门任一未过，Benchmark + Baseline 不通过；不能用总分抵消某一门的关键缺口。当前不打开 confirmation，也不提交新的效果型 A800 任务。

## 2. 三个问题的直接回答

### 2.1 Benchmark 选型

**开发用途合理，最终泛化资格未闭合。** TDC BBB 是当前主开发 truth；TDC AMES 的官方 archive 已完成版本和切分审计，hERG 仍在端点资格审计中，二者都要通过端点语义、覆盖、冲突和 provider lineage Gate 后才能进入最终多端点证据。Task36 提供了可恢复的 provider routing protocol，属于本项目新建评测层，不能直接称为现成社区 benchmark。

当前硬缺口：BBB canonical 冲突尚未处置；旧 722 development panel 与官方 BBB test 有 147 个 canonical / 235 个 scaffold overlap；33 个 provider 的 model-fresh 资格均未证明，且多个 provider 共享训练来源。固定菜单上的 lineage-aware operational claim 已经有边界，但不能外推为 model-fresh 泛化。证据见 [benchmark authority audit](benchmark_authority_audit_20260928/report.md)、[provider lineage audit](provider_training_membership_audit_20260929.md) 和 [lineage claim matrix](../task/task49-pharma-benchmark-baseline-freeze/provider_lineage_claim_matrix_v1/report.md)。

旧文件 [AAMAS2027_Benchmark_Selection_Review.md](AAMAS2027_Benchmark_Selection_Review.md) 已按 [ADR-0004](../docs/决议/0004-主Benchmark收敛为TDC与Task36医药工具路由.md) 标为历史候选。LLMRouterBench 和 ASlib 只在未来确有需要时另设外部迁移实验，不进入当前主结果。

### 2.2 Baseline 选型

**角色设计合理，主比较尚未闭合。** 当前合同覆盖 Random、Best Fixed、Oracle、Frozen、matched Online/Replay、Greedy LinUCB、PILOT、BayesianRouter、GraphRouter 和 verifier/direct-property controls。C4–C8 已在同一真实缓存 runner 上完成 15 streams 的合同核验，但使用 fixture prior 且不含效果统计。已有完整开发效果证据包括 Best Fixed、Oracle、Frozen、matched Online-SGD、Replay-SGD、PILOT-adapted 负结果和 BayesianRouter-adapted 负结果；Laya 已有静态表示负诊断。Greedy LinUCB、GraphRouter 和在线 Laya/JEV 仍未形成正式同协议效果主表。

因此目前不能声称“已经和先进 baseline 完成公平比较”。Adapter smoke、源码审计和失败任务都保留，但不进入效果排名。

### 2.3 实验矩阵和结果

**矩阵骨架合理，闭合度不够；结果解释目前是负结果和受控正控。** 已覆盖静态 OOF、selected-only 延迟 `{0,1,4}`、family/menu 敏感性、verifier 控制和多组 CPU 证伪；但全部主 baseline 尚未在同一协议跑齐，confirmation 也未冻结。

现有数据说明：静态 selector 没超过 Best Fixed；structured 和 joint-success residual 没有稳定超过 Frozen；人工注入 provider degradation 的 controlled version event 有事件后提升，但不能替代自然动态环境。当前最多能写“固定菜单开发回放中没有稳定动态增益；受控退化事件产生了可学习信号”。

## 3. 下一步顺序

1. 先处理 scope 冲突和 Gate A 的 truth/overlap/lineage，不新增效果型实验。
2. 将主 baseline 全部接入同一 runner；未纳入的方法必须有具体不匹配证据。
3. 用同一 menu、feature、warm start、delay、budget、seed 和 failure semantics 闭合 E1--E5。
4. 复核 R1--R8 结果合理性；只有 Gate A/B/C 全过，才冻结方法和 confirmation。

完整逐项证据在 [Benchmark / Baseline 验收审计](AAMAS2027_benchmark_baseline_acceptance_audit_20260929.md)。
