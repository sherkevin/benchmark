# PILOT / Greedy unified raw effect plan v1

日期：2026-09-30  
状态：**Greedy 与 PILOT unified Phase 1 均已完成；confirmation 关闭，未提交 Nebula/A800。**

## 结论先说

PILOT-adapted 和 Greedy LinUCB 已经有 CPU development effect 结果，代码也能复用现有的选择、回放和延迟反馈组件；但两份旧 receipt 还不能直接并入当前 MIRA/native Bayesian/matched 的主比较表。最小可行路径是：复用 `run_matched_sgd_unified_raw_v1.py` 的 receipt 骨架和 `run_bbb_stream` 的队列，把两种 baseline 接入同一 unified runner，再用独立 verifier 从输入副本和固定先验重放。这个任务不需要 A800；完整 5-fold CPU 回放只有约 120 条流、约 17,328 个 decision/terminal/feedback 事件，适合本地一次完成。

## 1. 现有证据与缺口

### Greedy LinUCB

已有 `full_v3_fix`：5 folds × 2 deterministic protocol slots × 3 delays × online/Frozen，60 streams、8,664 events，独立 verifier 状态为 `verified`。它使用 hidden-64、fit-only ridge prior、selected-only `system_success`，结果为：online/Frozen 在 delay 0/1/4 分别 `0.8511/0.8566`、`0.8449/0.8566`、`0.8629/0.8566`；fit-only Best Fixed 为 `0.9155`。这是可信的负向开发信号。

但该 receipt 的 `config.input_sha256` 是 P-C1 authoritative manifest（`94580e…`），而 source manifest 仍指向旧的 `fingerprint_diagnostic_v1/inputs.json`（`a73f1c…`）；没有把精确输入副本放进 receipt，也没有 `declared_commit` 和基于不可变 git tree 的源码核对。它不能和当前 unified receipts 直接声称“同一输入”。

### PILOT-adapted

已有 60 streams（5 folds × 2 fit seeds × 3 delays × Frozen/Online），独立 recovery 为 `verified`，结果 online 相对 Frozen 为 delay 0/1/4：`-0.000704/-0.002773/+0.002074`，低于 fit-only Best Fixed。它是必要的现代近邻负结果。

但旧 runner 的 `config` 仍为 `status=started`，没有当前 unified receipt 的 `run_id/method_id/decision_reads/state_hash` 等逐事件 provenance，也没有声明不可变代码 commit；`recover_pilot_adapted_development.py` 主要核对高层结果，不能替代统一的 action/state independent replay。因此只能保留为历史开发证据，不能直接进入统一主表。

## 2. 统一执行合同

### 输入与顺序

第一版为了和当前 MIRA/native Bayesian/matched receipts 严格可比，固定使用它们共同使用的
`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
（SHA-256 `a73f1cf…`）：722 molecules、33 providers、5 个 scaffold-disjoint folds、fold 内原始 evaluation 顺序。P-C1 authoritative manifest（SHA-256 `94580e3f…`）虽然 rows/folds/providers 内容相同，但 metadata 版本不同；若后续把 P-C1 作为生效 benchmark，必须由所有方法共同新开 receipt，不能只替换这两种 baseline 的输入。

每个 baseline 保留 5 × 2 × 3 × 2 = 60 streams；seed 0/1 的统计身份需分开写清：Greedy 的确定性 slots 不是独立随机重复，PILOT 的两个 fit seeds 是不同的离线先验。不得把两者当成同一种 seed 方差。

### 可见信息与延迟

- 决策前只读当前 molecule feature、完整当前 menu、已释放的 selected-only feedback；禁止读 truth、未选 provider output 和 future feedback。
- 终止后只将选中 provider 的 `system_success` 进入队列；失败保留 reward 0，`chemical_correct/recovered_label` 不进入 policy；unknown truth 终止该 batch。
- feedback due tick 固定为 `decision_tick + 1 + delay`，delay 为 `{0,1,4}`，到期后才更新；两种 baseline 与 MIRA/native Bayesian/matched 使用同一 queue、provider binding、事件 ID 和错误语义。
- 每个 stream 必须恰有一个 decision、terminal、feedback；结束时逐条 drain，不能把所有更新做完后再批量写日志。

## 3. 实现方案：复用但不混淆

建议新增一个小型 unified runner（可从 `run_matched_sgd_unified_raw_v1.py` 抽取 receipt 写入层），而不是修改 MIRA 或把旧日志强行转换：

1. **共同外壳**：复用 `load_inputs`、`PredictionCache`、`TruthStore`、`run_bbb_stream`、`TerminalFeedbackQueue` 和 unified raw 字段写入逻辑。
2. **Greedy adapter**：复用 `GreedyLinUCB` 和已有 fit-only prior 公式；使用与 MIRA/native/matched 相同的 hidden-64 checkpoint 与 evaluation feature snapshot。`greedy_online` 与 `greedy_frozen` 共用同一 fit prior，Frozen 只消费 feedback 不改参数。
3. **PILOT adapter**：复用 `fit_pilot_prior`、`project_query`、`PilotOnlineKernel`；fit 区先做已登记的 query triplet/provider preference 两阶段先验，evaluation 只做 selected-only cosine/UCB/ridge 更新。`pilot_online` 与 `pilot_frozen` 共用同一 prior/features/alpha。
4. **receipt**：每条 event 记录 `run_id/method_id/episode_id`、`menu_before_choice`、`decision_reads`、`released_feedback_ids`、`provider_event_ids`、`feature_sha256`、selected output、due/release tick、version、`state_hash_before/after`、update latency。terminal 不写入未选输出；feedback 不写 evaluator-only correctness。
5. **输入副本与身份**：输出目录复制 exact `inputs.json`；固定 checkpoint、fit snapshot、prior 的 SHA；配置必须含 40 字符 `declared_commit`。源码 hash 必须从该 commit 的 git tree 读取并与工作区 hash 比较，工作区不一致则停止。

不建议直接复用旧 `run_pilot_adapted_development.py` 或旧 Greedy `full_v3_fix` 的汇总：它们的结果可用于保留历史负证据，但缺少当前 receipt 的完整 provenance。

## 4. 独立 verifier 与反证条件

verifier 不导入 runner 或两个 policy 类，直接用 NumPy 重建：

- fit-only PILOT projection/provider prior，Greedy ridge prior；
- 每 tick 已释放 feedback 集合和顺序；
- 两种 policy 的 score、tie rule、选中 provider；
- selected cache output、reward、due/release tick；
- update 前后 state/version hash、feedback conservation 和最终汇总。

先跑 1 fold × 1 seed × 1 delay smoke，再跑完整 CPU receipt。必须保留 raw JSONL、配置、输入/先验/源码 hash、verifier JSON 和 tamper rejection。以下任一项出现，整轮标记 `BLOCKED`，不解释效果：输入 hash 不一致、未选输出进入 policy、feedback 提前释放、provider/event 错绑、state/action 重放不一致、事件不守恒、源码 commit 不可核对。

## 5. 与近邻的可比边界

- Greedy 是标准 disjoint contextual LinUCB 的透明适配，不包含论文之外的 cost policy；它检验“普通线性在线更新是否足够”。
- PILOT 原论文使用 LLM preference、共享 query/LLM cosine 空间和 cost knapsack；这里是医药 provider correctness 的两阶段适配，没有 LLM 文本 preference 或成本策略，正文只能标 `PILOT-adapted`，不能写官方复现。
- 两者的 primary effect 都是同一方法内部 `online − frozen` 的 paired selected-correctness 曲线；不能用不同表示的 raw accuracy 直接排名 MIRA。另报 Best Fixed，并在最终统一主表中注明 representation/warm-start 差异。

## 6. 预登记指标与停止线

主指标：按 fold/seed/delay 的 molecule-level selected `system_success` accuracy、累计错误和 online−Frozen paired difference；附带 prefix/suffix 曲线、provider/failure 分层、feedback release 数、decision/update p50/p95、峰值状态大小。

技术 GO：120 streams 全部完成，schema/action/state verifier 全部 `verified`，无泄漏且可从 receipt 重放。它只使 baseline 具备统一主表资格，不代表方法有效。

开发信号沿用各自已登记规则：Greedy 至少两个 delay 的 paired improvement 且多数 fold 为正才值得继续分析；PILOT 需至少两个 delay 均值为正，否则保留负结果。任何调 alpha、epoch、prior precision、顺序或数据筛选以追求正数，均视为 STOP。

## 7. 是否需要 A800

不需要。两个 baseline 都是缓存回放和小型线性/64 维更新；旧全量 CPU 已在分钟级完成。A800 只在后续需要真实 Laya forward、完整动态事件压力或主方法最终 confirmation 时使用。先完成统一 CPU receipt，避免占用远程队列，也不把 baseline 的负结果误包装成 GPU 证据。

## 下一步

1. 由 benchmark 生效版本确认使用 `a73f1c…` 还是 P-C1 `94580e…`，并把选择写入新的 ADR；
2. **已完成 Greedy Phase 1**：当前 `a73f1c…` 输入下 2-method × 5-fold × 2-slot × 3-delay，共 60 streams/8,664 events；独立 NumPy verifier 通过，报告见 `../greedy_unified_raw_v1/report.md`。
3. **已完成 PILOT unified receipt**：当前 `a73f1c…` 输入下 60 streams/8,664 events，独立 NumPy verifier 通过；报告见 `../pilot_unified_raw_v1/report.md`。旧 PILOT receipt 仍作为历史证据保留。
4. 把 Greedy/PILOT receipts 并入 benchmark/baseline acceptance audit；在 Gate A/B/C 和 provider lineage/endpoint qualification 未闭合前，不打开 confirmation。
