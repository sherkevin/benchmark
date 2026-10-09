# MIRA relative-reliability v1：A800 full matrix 前置闸门复核

- 复核日期：2026-09-30
- 复核范围：relative-reliability CPU development gate 是否足以申请一次 A800 full matrix
- 复核性质：独立技术审查，不修改生效的方法、benchmark 或 claim
- 结论：**CPU mechanism gate 通过；A800 full matrix 条件性 HOLD**

## 1. 复核材料

本复核重新读取了以下材料，而不是只看报告中的汇总数字：

- 预注册与报告：`task/task49-pharma-benchmark-baseline-freeze/mira_relative_reliability_v1/{preregistration.md,report.md}`；
- 权威 CPU 收据：`experiments/logs/task49_mira_relative_reliability_cpu_20260930/run_v1/`；
- 独立动作/状态重放：`experiments/logs/task49_mira_relative_reliability_cpu_20260930/verification_v1/verification.json`；
- 篡改测试：同目录 `tamper_tests_v1.json`；
- 实现与测试：`src/probe_headless/mira_relative_reliability.py`、对应 runner/verifier 和单元测试；
- 生效验收标准：`docs/研究主线/生效/评价标准/02-方法论评价标准.md`、`03-Benchmark与Baseline评价标准.md`。

本地针对候选和相关 SafeSwitch/在线 baseline 的测试为 **36 passed**。独立 replayer 不导入策略实现，重算 60/60 candidate streams、8,664/8,664 个 decision/terminal/feedback；两个故意篡改均以退出码 1 被拒绝。

## 2. CPU gate 判定

| 检查 | 判定 | 依据 |
|---|---|---|
| 数学规则可计算 | PASS（开发级） | 后验均值、相对因子和 hard availability channel 都有实现、边界测试和逐事件重算 |
| selected-only 与延迟时序 | PASS | 360 streams；decision/terminal/feedback 各 51,984；delay 为 0/1/4；replayer 检查 due/release/version |
| 失败语义 | PASS | runtime failure 未进入 correctness ledger；correctness hard block=0；all-blocked fallback=0 |
| 预注册效果条件 | PASS（开发级） | no-event 相对 Frozen 为 +0.14/+0.14/+0.07pp，均在 ±1pp ROPE；controlled-event 三个 delay 均保留 streak-only 的切换收益 |
| 相对强 baseline | 未通过“优越性” | matched Online-SGD 的 controlled-event post delta 为 +5.51/+4.82/+5.50pp，高于 relative-reliability 的 +4.67/+4.39/+3.98pp |
| 可复现性 | PASS（候选流） | 独立 NumPy/SciPy replayer、源/输入/snapshot hash、篡改拒绝收据齐全 |

因此，本轮可以把 relative-reliability 作为**唯一 Proposed development candidate**继续验证；不能把它写成已超过强 baseline 的最终方法结果。

## 3. 为什么还不能放行 A800 full matrix

### 风险 1：运行源没有绑定到不可变的远程代码身份

CPU `config.json` 有逐文件 `source_sha256`，但没有把这组 hash 与本轮执行使用的 `code_commit`/提交树绑定。独立 replayer 证明的是“当前工作区文件与收据一致”，还不是“远程 A800 任务使用了同一棵提交树”。这正是此前 Nebula gitless archive 导致训练尚未开始的故障类型。

**必须补的动作：**

1. 将 relative candidate、runner、verifier、预注册和配置收口到一个干净提交；
2. 在 A800 任务元数据和 config 中同时记录 `declared_commit`、逐文件 source hash、依赖版本和输入 SHA；
3. 提交前用 Nebula 的 `--code-commit` 固定该提交，提交后从任务元数据和 worker 日志核对 commit/source manifest；
4. 将完整 raw JSONL 和恢复收据写入独立 OSS 前缀，不能只保留 summary。

**放行证据：** 一个只读 provenance checker 能从任务元数据、提交树、config 和 raw log 得到全部 matched，且没有依赖当前未提交工作区。

### 风险 2：Baseline 仍未闭合统一的公平比较合同

relative CPU matrix 的六个变体共享 snapshot 和 event contract，但现有 Gate B 仍是 HOLD（最近状态快照为 11/16）。已有审计显示不同 baseline 的 raw JSONL 字段、terminal/read-set/state hash 和独立恢复能力不一致；PILOT、BayesianRouter、Greedy 等的 source fidelity/适配边界也还没有在同一 runner 中闭合。候选目前还弱于 matched Online-SGD，因此直接扩大计算量不能替代公平近邻比较。

**必须补的动作：**

1. 在 A800 full matrix 之前冻结统一 schema：`decision → terminal → delayed feedback`、menu/read-set、provider binding、state/version、failure/unknown 和 cost 字段；
2. 在同一输入、菜单、feature、fold/seed、delay、预算和初始化权限下，至少闭合 Best Fixed、Frozen、matched Online-SGD、Random、Greedy、PILOT、BayesianRouter 和 MIRA；Oracle 只作上界；
3. 为每个 baseline 保存论文/源码版本、适配差异、许可证、配置和独立 action recovery；缺字段的旧日志只保留为历史 development evidence，不当作本轮 full-matrix 主比较；
4. 预先固定 paired accuracy/post-accuracy、failure/unknown 分母、p50/p95 decision/update cost、memory 和 grouped uncertainty，不能用一次 A800 结果临时选指标。

**放行证据：** Gate B 的四个顶层子项均达到 3 分以上，且每个主 baseline 都有同一 schema 的 raw run 与 recovery receipt。

### 风险 3：benchmark 与 dynamic scope 仍只覆盖 BBB development 和受控事件

本轮 722 个 BBB 分子、33 provider 是可审计的 development routing layer；controlled event 是预登记的 provider-version degradation 压力，不是自然 drift。hERG/AMES 的 endpoint equivalence、provider training membership/model-fresh lineage 尚未全部收口。因而一次 BBB CPU 正控不能单独支撑跨 endpoint、自然生命周期或普遍 provider adaptation 的结论。

**必须补的动作：**

1. 完成 hERG/AMES truth、阈值、duplicate/unknown、provider lineage 和训练成员审计；明确哪些 endpoint 进入本轮 A800，哪些只作为待确认扩展；
2. 在 full matrix 预注册中把 `feedback-adaptive` 与 `environment-nonstationary` 分开：前者至少固定 delay/pending/selected-only，后者另列 arrival/retire/version/shift 控制；
3. 保留 BBB controlled event 作为机制压力测试，同时预留未读 scaffold/endpoint confirmation，不把受控事件改名为自然漂移；
4. 固定 confirmation 只读、标签不回流，开发结果不能用于改 `tau`、`n_min`、事件位置或 baseline 配置。

**放行证据：** Gate A 的 A1/A3/A4 和 Gate C 的 C1/C4 达到生效标准；确认集、provider lineage 和 dynamic scope 均有 manifest/hash/恢复收据。

## 4. 最终裁定

当前不是“候选失败”，也不是“只要上 A800 就能证明方法”。裁定如下：

- **允许继续**：把 relative-reliability 保留为 Proposed development candidate，继续补统一 schema、source provenance 和 benchmark qualification；保留 matched Online-SGD 更强这一负结果。
- **暂不允许**：把下一次任务称为已经冻结的最终效果实验，或在 Gate B/A 未补齐前提交 A800 full matrix 并据此写主结果。
- **满足三个放行条件后**：只提交一条 A800 full-matrix 任务，一次完成固定矩阵；任务结束后先独立恢复和审计，再决定是否进入 confirmation。confirmation 前不回流任何标签或指标。

本复核不改变论文目标 claim。它只把“继续逼近目标”和“可以把一次远程结果当作最终证据”分开，避免把一个可复现的 CPU 候选误写成已完成的 AAMAS 方法。
