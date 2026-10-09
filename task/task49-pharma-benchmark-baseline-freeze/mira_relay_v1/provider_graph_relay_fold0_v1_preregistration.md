# Provider-Graph Relay v1：fold0 CPU replay 预登记

- 日期：2026-10-04
- 状态：执行前；只做 development fold0/seed0，不申请 Nebula
- 进入依据：held-out alignment 资源闸门通过，见 `provider_graph_alignment_v1_preregistration.md` 与 `experiments/logs/task49_provider_graph_alignment_20261004/`
- 目的：检验 fit-only provider graph 的 selected-only relay 是否真的改变后续 action，并在相同事件合同下产生 rescued/harmed 结果

## 1. 唯一主问题

在 fold0 的 145 个 evaluation molecules 上，Graph Relay 相对 MIRA family residual 和 provider-local Independent，是否在 delayed selected-only feedback 下产生可复核的 action-level utility？

Held-out alignment 不是效果证据；本批只把它转成一次小规模 replay，不把正结果外推到五折、seed1、其他 endpoint 或 confirmation。

## 2. 固定输入和事件合同

- 输入副本：`fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- fold0 evaluation 顺序固定，145 个 molecule；provider menu 为完整 33 provider。
- frozen encoder、fold0 sealed fit state、fallback、probe seed、epsilon、hazard ledger 和 feature dimension 与 v3 matched matrix 完全相同。
- delays 为 `{0,1,4}`，feedback due tick 固定为 `decision_tick + 1 + delay`；只在 selected provider terminal feedback release 后更新。
- 未选 provider 的 output、truth、future feedback 和 full panel 不进入 policy；failure/status/unknown/refusal 语义沿用 v3。
- 四个方法共享同一输入、菜单、probe schedule、selected-only queue、seed 和 runtime budget：
  1. `probe_mira_relay`：现有 family shared residual；
  2. `probe_independent_residual`：现有 provider-local residual；
  3. `probe_provider_graph_relay`：fit-only graph + selected residual relay；
  4. `probe_degree_matched_random_graph`：同一 graph out-degree、target in-degree 多重集、权重质量和事件合同，只随机置换 source-target 配对。

## 3. Graph Relay 的固定实现

- graph：`m_min=128`、`lambda=32`、`k=2`、`tie_tol=1e-12`；图只由 fold0 fit rows 构造。
- source/target：selected provider $q$ 向 writable target $p$；fallback 永不写入。
- score：沿用 MIRA 的 frozen direct head、fit family residual、fit provider bias 和 hazard/UCB；额外 graph local residual 初始为零，local variance 固定为零，所以四个方法第一步的 frozen score 完全一致。
- release-time residual：$\delta_q=y_q-\sigma(z_q)$，其中 $z_q$ 用 release 时的当前 selected score 计算；不重算历史未选 label。
- update：

  $$u_p\leftarrow u_p+\eta a_{p,q}x_t\delta_q,$$

  `eta=1.0`；若存在 writable neighbors，$a_{q,q}=1-\rho$、$a_{p,q}=\rho W_{p,q}$、`rho=0.5`；无 writable neighbor 时 selected-only self update；selected fallback 不更新。

- uncertainty：Graph local residual 的 variance 固定为零，方法按 fixed-uncertainty relay heuristic 实现；不声称 Bayesian posterior。
- random control 使用同 source 的 relay coefficient multiset，重新分配到 degree-matched targets；总 relay mass 与 Graph Relay 相同。

## 4. 主要估计量

每个 delay 单独报告：

1. selected reward accuracy（分子为统计单位）；
2. 相对 MIRA、Independent 和 random graph 的 paired action changes、rescued、harmed、net；
3. selected-only update count、relay target count、fallback/runtime failure、p50/p95 update cost；
4. action changes 按 provider family、graph degree、failure status 和 scaffold 分层。

不把三个 delay 当独立重复，不把 event 行当独立样本。fold0 结果是 mechanism gate，不是最终主表。

## 5. 停止条件

任一条件出现即停止该候选并保留 raw receipt：

1. 独立 verifier 无法从冻结 provider cell、事件时钟和 state hash 重算；
2. Graph Relay 与 MIRA 在两个或以上 delay 没有 action change；
3. 相对 MIRA 的 rescued/harmed 只来自一个 molecule、一个 delay，或 net 在三个 delay 均为 0；
4. delay4 出现未登记的系统性 harmed；
5. 需要额外标签、未选输出、额外调用、evaluation 调参或超过 matched update budget。

通过 fold0 只允许预登记后续五折 development；不自动打开 seed1、A800、hERG/AMES 或 confirmation。

## 6. 证据要求

运行前写 `config.json` 和 source/input/state/graph hashes；运行中流式写 raw JSONL；运行后独立 verifier 必须重算 selected output、status、reward、queue/release、state hash、paired metrics 和停止条件。失败的启动或旧 verifier 拒绝均保留，不覆盖。
