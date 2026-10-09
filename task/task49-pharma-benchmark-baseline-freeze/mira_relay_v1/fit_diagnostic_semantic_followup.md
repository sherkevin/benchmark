# Fit diagnostic semantic follow-up

日期：2026-10-05

对 `run_mira_relay_fit_diagnostic_v1.py` 与 `run_mira_relay_contract_replay_v1.py` 做字段语义复核后，发现 legacy no-fit-state 分支曾只匹配 `output_statuses == "failure"`。冻结输入的真实词表是 `ok/error`，因此旧分支的 `failure_counts` 漏掉了 144 个 error cells。

这不改变 v4 sealed fit-state（它已经按非-ok统计），也不改变该批 `fit_not_converged` 的主结论；但旧 v1/v2 action/hazard 可行动性数字不能作为证据。代码已改为统计所有 `status != "ok"`，未来必须重新生成完整 receipt 后才能使用该分支。
