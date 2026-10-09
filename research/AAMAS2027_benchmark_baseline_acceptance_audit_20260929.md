# AAMAS 2027 Benchmark / Baseline 验收审计

日期：2026-09-29  
性质：开发阶段内部审计，不是 AAMAS 官方评分，也不是录用预测。  
适用标准：[Benchmark 与 Baseline 评价标准 v005](../docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md)。

## 1. 先给结论

这次审计只回答三个问题：benchmark 选型是否站得住、baseline 选型是否够强且公平、实验矩阵和结果是否足以支撑主张。三项都没有通过，当前整体状态为 **HOLD**。因此暂不打开 confirmation，也不提交新的效果型 A800 任务。

| 顶层闸门 | 得分 | 通过线 | 当前结论 |
|---|---:|---:|---|
| Gate A：Benchmark 选型 | **11/16** | ≥12，A1/A3/A4 各≥3，H1--H4 全 PASS | HOLD |
| Gate B：Baseline 选型 | **9/16** | ≥12，四项各≥3，主表每个方法有完整 raw run | HOLD |
| Gate C：实验矩阵与结果 | **10/16** | ≥12，四项各≥3，E1--E5 不缺 | HOLD |

这不是说现有数据和代码不能用，而是它们目前只能支撑开发期、固定菜单和受控压力测试的结论，不能支撑“benchmark 已确定”“动态 JEV 已领先”或“结果已完成最终确认”。

## 2. Gate A：Benchmark 选型

### 2.1 评分

| 子项 | 分数 | 证据 | 缺口 |
|---|---:|---|---|
| A1 公共 truth 权威性、版本、hash | 3/4 | [benchmark authority audit](benchmark_authority_audit_20260928/report.md)；TDC BBB archive 已作为当前开发 truth 冻结，AMES archive 的 hash 和 split 已审计但尚未启用 | BBB canonical 冲突的最终处置尚未写入最终合同；hERG 仍待 endpoint qualification |
| A2 routing layer 协议和 scope 一致性 | 4/4 | [生效 Benchmark 文档](../docs/研究主线/生效/03-Benchmark与Baseline.md)、[ADR-0004](../docs/决议/0004-主Benchmark收敛为TDC与Task36医药工具路由.md)；Task36 selected-only 回放和延迟合同可复核 | routing layer 仍是本项目新建协议，不能称为已有社区 benchmark；这一边界已写入主线和决议 |
| A3 duplicate、冲突、overlap、split、confirmation 隔离 | 1/4 | 官方 split audit 已报告 BBB 冲突和旧 panel overlap | 722 development panel 与官方 test 有 147 个 canonical / 235 个 scaffold overlap；confirmation 尚未重建 |
| A4 provider lineage、family 相关性和独立性边界 | 3/4 | [provider training membership audit](provider_training_membership_audit_20260929.md)、[provider lineage claim matrix](../task/task49-pharma-benchmark-baseline-freeze/provider_lineage_claim_matrix_v1/report.md) | 33/33 provider 尚不能证明对开发池 model-fresh；24 个 B3clf、3 个 DeepPurpose 等共享来源，不能按 33 个独立模型计；但 lineage grade、family 相关性和禁止的外推主张已经明确 |

### 2.2 选型判断

当前选型已经确认：

> **公共 TDC truth + 可审计的 Task36 provider routing protocol**。

这个组合是合理的开发 benchmark 形态，但 routing protocol 不是一个现成的公共社区 benchmark。论文必须把 truth layer 和 routing layer 分开描述，并按 provider lineage 分层报告。

仓库中的 [旧 benchmark 选择报告](AAMAS2027_Benchmark_Selection_Review.md) 已标记为历史候选。LLMRouterBench/ASlib 的反馈、工具版本和 selected-only 医药 provider 合同没有在本主任务中闭合；如需使用，只能另写外部迁移实验。

## 3. Gate B：Baseline 选型

### 3.1 评分

| 子项 | 分数 | 证据 | 缺口 |
|---|---:|---|---|
| B1 角色覆盖 | 3/4 | [baseline contract v1](../task/task49-pharma-benchmark-baseline-freeze/baseline_contract_v1.md) 含 Random、Best Fixed、Oracle、Frozen、matched update、现代近邻和任务控制 | 仍有方法只完成源码审计或 adapter smoke，尚未进入同一主表 |
| B2 任务与动态 scope 匹配 | 2/4 | 适配合同逐项记录 selected-only、delay、failure 和 warm start；Replay-SGD 已完成 90 条 development stream 的独立恢复；Laya forward 和静态表示诊断已完成；C4–C8 仍有统一合同冒烟 | GraphRouter、Laya/JEV 在线适配、Greedy LinUCB 的正式效果仍未在主表闭合；Laya runtime 本身没有在线 update API，具体边界见 [在线适配审查](../task/task49-pharma-benchmark-baseline-freeze/laya_online_adapter_v1/blocking_report.md)；BayesianRouter 和 PILOT 只能写 adapted，不能称原论文完全复现 |
| B3 实现保真、版本和许可证 | 2/4 | `baseline_audit/`、`baseline_adapters_v0.md`、matched-SGD/Laya 的源码与输入 hash、PILOT/Bayesian 负结果和数值测试；GraphRouter 的具体依赖/输入阻塞见 [适配审查](../task/task49-pharma-benchmark-baseline-freeze/graphrouter_static_adapter_v1/blocking_report.md) | GraphRouter 尚未完成 BBB 适配；Laya 只有静态表示诊断；各方法仍不是同一 implementation fidelity 层级，不能以仓库默认路径代替论文方法 |
| B4 统一 menu、feature、初始化、反馈、预算 | 2/4 | 公平性合同和 selected-only stream 已有验证；C4–C8 共 15 streams、960 条事件通过独立 verifier，使用相同 64 cohort、33-provider menu 和 delay `{0,1,4}`，并记录 `state_hash_before/after` 与 `update_ms` | C0–C3 与现代静态方法尚未纳入同一正式效果 runner；C7/C8 仍使用 fixture prior 冒烟，不能当正式 fit 或 tuning budget |

### 3.2 baseline 判断

名单方向是合理的，当前不能写成“baseline 已完成”。可以进入开发证据的有 Best Fixed、Oracle、Frozen、matched Online-SGD、Replay-SGD、PILOT-adapted 和 BayesianRouter-adapted（均需保留适配边界和负结果），以及 Laya 的静态表示负诊断；C4–C8 的统一合同已接通。Greedy LinUCB 仍是未运行的适配候选，GraphRouter 尚未完成 BBB 适配，Laya/JEV 还缺在线 selected-only 动态效果。源码存在、数值 kernel 通过或 adapter smoke 都不算效果比较。

若一个未纳入的方法与本任务不匹配，必须写出输入、反馈、目标或许可证的具体不匹配；不能只因为实现成本高而删掉最强近邻。

## 4. Gate C：实验矩阵与结果

### 4.1 评分

| 子项 | 分数 | 证据 | 缺口 |
|---|---:|---|---|
| C1 矩阵覆盖 RQ、E1--E6 和 dynamic scope | 3/4 | 生效 Benchmark 文档列出 Gate 0、静态 OOF、online delay `{0,1,4}`、family/menu、verifier、confirmation；Task49 预注册齐全 | 自然 arrival/retire/drift 尚未进入主 benchmark；controlled version event 只能作正控 |
| C2 全部主 baseline 同协议闭合 | 2/4 | structured/joint-success CPU 具备 5 fold × 2 seed × 3 delay 的统一流 | roster 未闭合；无法生成完整主比较表 |
| C3 结果方向、控制关系、成本和 scope 合理 | 3/4 | static fingerprint、structured、joint-success 的负结果均保留；controlled event 报告明确是 synthetic positive control | 固定池没有真实动态事件，尚不能支撑自然动态收益 |
| C4 paired uncertainty、failure/unknown、恢复和负结果 | 2/4 | 多个 CPU run 有 raw JSONL、独立 verifier、paired scaffold 统计和失败语义 | 全 baseline 的成本、恢复和不确定性尚未统一；confirmation 未开 |

### 4.2 已有结果能说明什么

- 静态 fingerprint：Best Fixed 约 `0.9155`，MLP/BT+CLS 低于它；这说明当前表示和静态 selector 还没有超过固定赢家。
- structured CPU effect：180 streams、25,992 decisions；候选相对 matched SGD 的 pooled 差值约 `-0.000693`，95% CI `[-0.006294, 0.005377]`，没有达到预登记开发 go 条件。
- joint-success residual：三个 delay 相对 Frozen 的区间均跨 0，没有稳定动态收益。
- controlled version event：人工注入 provider degradation 后，事件后相对 Frozen 的提升约 `+0.0302`--`+0.0371`；它只证明受控退化事件可以产生可学习信号，不能替代自然 arrival/retire/drift 证据。

这些结果的合理解释是：当前固定 provider pool 和静态分布没有提供足够的动态收益机会；selected-only 更新还会放大 provider head 漂移。不能把负结果包装成方法领先，也不能把正控写成真实环境效果。

还要单独冻结 dynamic scope：目前主回放只有 fixed-pool 下的 delayed selected-only feedback，没有自然 arrival、retirement、version drift 或分布变化。若不新增并冻结其中一种事件，论文主张必须收窄为 **fixed-pool delayed adaptation**；任何在线 baseline 的提升都不能自动解释成 environment-dynamic JEV 收益。

## 5. 必须先做的修复

1. **先收口 Gate A**：等待 P-C1 冲突政策的共同确认；若确认整体排除，写单独 ADR、生成冲突排除后的 authoritative manifest，并把旧 panel overlap 与 candidate confirmation 绑定。不能把候选 manifest 当成已冻结协议。
2. **再补一个最小现代在线近邻**：只运行已经完成候选卡、runner 和独立 verifier 的 Greedy LinUCB CPU development replay；先补 source/hash/state fields，固定 fit-only warm start、同一顺序和 delay `{0,1,4}`。不申请新的 A800。
3. **对不匹配 baseline 明确范围**：GraphRouter 暂保留静态源码/论文审计，在线 Laya/JEV 暂保留静态表示负诊断；若要从主表移到附录或审计层，先记录范围决定，不能静默删除。两者不在当前动态 JEV 任务前抢资源。
4. **最后收口 Gate C**：用已完成的 Replay/PILOT/Bayesian/Online-SGD 和 Greedy 结果形成完整主表；将 controlled version event 单独放在 stress/positive-control，不把它当自然动态 benchmark；在方法和菜单冻结后再做 E6 confirmation。
4. **保留现有负结果**：不继续为已经没有正控证据的普通在线 head 反复调参；下一次实验必须先说明它填补哪个 Gate 缺口。

## 6. 复核规则

本审计只记录当前状态；外部迁移 benchmark 是否实施不属于主线必做项。后续每完成一个小任务，重算 Gate A/B/C；任一门仍低于 12/16，就保持 HOLD。
