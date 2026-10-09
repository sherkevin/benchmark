# M1 verification-gain 最小 runner 记录

日期：2026-09-25

## 目的

在不改 Task36 replay contract、TruthStore 或 M0 行为的前提下，验证“固定首 provider + 随机 probe 第二 provider + 延迟学习 verification gain”的最小可运行闭环。

## 实现合同

- 每个 episode 先调用固定的 `first_provider`。
- 在未调用的 provider 中按 Bernoulli `probe_probability` 决定是否 probe；probe 时均匀采样一个 provider。
- 记录 probe 的 exact propensity：`probe_probability / |remaining_menu|`。
- 只有 episode terminal 后，trainer-side TruthStore 才计算两种损失：首 provider 直接作为结果的 `baseline_loss`，第二 provider 作为结果的 `after_loss`。
- 延迟事件中的 `gain = baseline_loss - after_loss`，取值为 `-1, 0, 1`。未解析真值时保留事件但不更新统计量。
- 到达队列使用 event id 去重，反馈 exactly once release。
- 固定非负特征为 `bias + probe-provider one-hot`。每个已释放的 scored 事件以 IPW 更新：
  `G += feature * gain / propensity`，`Z += feature / propensity`。
- 查询估计为 `feature^T G / (feature^T Z + epsilon)`。

## 文件

- 实现：`src/probe_headless/m1_dynamic_jev.py`
- 合同测试：`src/probe_headless/tests/test_dynamic_jev_m1.py`
- 主要非平凡 propensity smoke：`experiments/logs/m1_dynamic_jev_smoke_20260925_v4_{config,raw,events,summary}.{json,jsonl}`
- 另有 gate 行为 smoke：`experiments/logs/m1_dynamic_jev_smoke_20260925_v3_{config,raw,events,summary}.{json,jsonl}`

## 验证

```text
105 passed
```

目前保留三组 smoke：

- v2：3 个分子、2 个 provider、probe probability `1.0`，得到 3 个 scored events，gain `[1, 0, -1]`。它只验证二次调用和延迟更新，propensity 恒为 `1.0`。
- v3：6 个分子、3 个 provider、probe probability `0.5`，得到 6 个 selection events，其中 2 个 probe、4 个不 probe；probe propensity 为 `0.25`，2 条 scored feedback 均 exactly once release，gain `[0, 1]`，flush 后 pending queue 为空。
- v4：4 个分子、3 个 provider、probe probability `0.8`，4 个 episode 全部 probe；均匀抽取后的 propensity 为 `0.4`，4 条 scored feedback exactly once release，gain `[1, 0, -1, 0]`，flush 后 pending queue 为空。v4 的 config 还记录了 claim 和 falsification condition，用来核查 IPW 字段是否被正确传递。

两组结果都只验证合同，不是效果提升结果。

## 边界

该 runner 只验证反馈合同、propensity 记录和有限状态更新。它还不是最终方法：首 provider 固定，特征没有使用历史返回内容，没有 gain/risk gate，也没有 backbone fine-tune 或漂移遗忘。当前 smoke 只回放冻结缓存，没有新工具调用。
