# MIRA-Relay contract replay v1 运行卡

日期：2026-10-03  
状态：运行前封存；本卡对应一次小规模实现核验，不是效果实验。

## 要回答的问题

在固定的 fold 0 / seed 0、evaluation 原顺序前 32 个分子和 delay `{0,1,4}` 下，
`ProbeMIRA-Relay`、`ProbeNoUpdate` 与 `NoProbeShield` 是否都满足 selected-only、
terminal-first、延迟 release 和 exactly-once feedback 合同；已释放的 selected feedback
是否能在后续 decision 中改变 policy state/action。

## 运行范围

- 9 条 stream：3 methods × 3 delays；不读取 confirmation 集。
- 使用已封存的 Task36 replay cache/judge；policy 只收到当前分子 frozen encoder feature、
  当前 candidate menu 和已释放的 selected feedback。
- 每个 decision、terminal、feedback 都写入 JSONL；保存 state snapshot/hash、版本、propensity、
  selected output 和 release tick。
- 候选参数必须通过独立 trust-exact verifier；fit precision 由同一 fit cells 的 Hessian 对角线
  重算，作为本 v1 contract 的显式临时假设。

## 通过条件

1. 9 条 stream 都完成，`pending_feedback=0`，release 数等于 decision 数。
2. 每条 stream 的 selected provider 与 terminal provider 一致；未选 provider 不产生 terminal
   或 feedback 事件。
3. 每个 feedback 只 release 一次，`due_tick = decision_tick + 1 + delay`，版本和 state hash
   前后可由独立脚本重算。
4. runner 不输出 selection quality、paired success、置信区间或方法优越性结论。

## 失败条件与处理

- 任意生命周期、身份、state hash 或 selected-only 检查失败：保留原始日志，标记失败，不重跑
  改过语义的版本。
- 没有 action change 只说明这 32 条 stream 没触发可见改变，不能据此否定方法；需要回到
  fit-derived feature、probe 支持或更新实现排查。
- 即使出现 action change，也不能解释为准确率收益；正式效果矩阵仍需另行审核、预登记和
  full CPU replay。

## 预定命令

```bash
python3 -m src.probe_headless.run_mira_relay_contract_replay_v1 \
  --output experiments/logs/task49_mira_relay_contract_replay_20261003/v4 \
  --fit-state-dir experiments/logs/task49_mira_relay_fit_state_20261003/v7 \
  --code-commit c4d2bf638f86b7315a6283ccd551c46578fa5a3f
```

实际以同一提交运行的权威副本是 `.../v4/`；`v1`–`v3` 保留为历史 runner 版本。
原始事件、配置、summary、verification 和失败日志均保留在 `experiments/logs/`，不覆盖旧版本。
