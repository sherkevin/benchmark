# FamilyGuard common-stratum full receipt

日期：2026-10-01
状态：development common-stratum receipt；只报告公平输入下的描述性结果和恢复证据，未读取 confirmation。

## 收据

- 同一已封存 common-stratum manifest：722 个 BBB development molecules、33-provider menu、5 folds × 2 seeds、delay `{0,1,4}`。
- 6 methods：Frozen direct、Matched Online-SGD、Greedy、Bayesian、MIRA-relative、MIRA-FamilyGuard。
- 180 streams、25,992 decisions、77,976 raw events；独立 action/state/evaluator verifier `verified`，159 条真实 selected runtime failures 保留在分母。
- Family registry 在运行前固定；FamilyGuard 仅接收 selected terminal 的 delayed feedback。
- 原始回执：`experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/`。
- 历史指标：`metrics_v1.json`；当前 family macro 指标：`metrics_v2_external_registry.json`；paired molecule aggregation：`paired_v1.json`；scaffold-cluster bootstrap：`bootstrap_v2.json`。

## 2026-10-02 registry 口径修正

上一版 `metrics_v1.json` 实际读取的是旧的
`data/bbb_census_v3/tool_registry.json`，而运行配置声明的 registry 是
`task/task38-p01-provider-pool/external_qualification.json`。旧 registry 没有
`fpadmet` 和 `admetlab_3_0`，所以这两个 provider 在旧 family macro 中被计为
`UNKNOWN`。这不是事件回放或策略执行错误，而是指标聚合时的 registry 指针错误。

本节以下的 family macro 以同一份已封存 `events.jsonl` 做只读重聚合，使用配置中声明的
external qualification registry；没有重跑策略、没有读取 confirmation，也没有改变 task
success、valid-return、failure、latency 或 state bytes。机器证据为
`metrics_v2_external_registry.json` 和
`metrics_v2_external_registry_audit.json`。旧 v1 数字保留在历史机器文件中，不再作为
当前 family macro 证据。

## 结果

| method | all-provider task success | valid-return conditional correctness | runtime failure rate | family macro correctness | selection/update/state p95 |
|---|---:|---:|---:|---:|---|
| Frozen direct | 0.901662 | 0.908583 | 0.007618 | 0.873464 | 36.44 / 2.00 / 16,896 |
| Matched Online-SGD | 0.892198 | 0.899465 | 0.008079 | 0.756552 | 35.33 / 13.21 / 16,896 |
| Greedy | 0.887350 | 0.892086 | 0.005309 | 0.886374 | 178.39 / 37.83 / 1,098,504 |
| Bayesian | 0.865420 | 0.870443 | 0.005771 | 0.873853 | 165.93 / 27.31 / 1,098,504 |
| MIRA-relative | 0.902816 | 0.909746 | 0.007618 | 0.888708 | 85.02 / 47.75 / 17,721 |
| **MIRA-FamilyGuard** | **0.907664** | **0.909764** | **0.002308** | **0.883267** | **118.69 / 91.08 / 17,945** |

按 molecule 配对、再按 scaffold cluster bootstrap：

- FamilyGuard − Frozen：`+0.5977 pp`，95% 区间 `[-0.6204,+1.5988] pp`；
- FamilyGuard − MIRA-relative：`+0.4828 pp`，95% 区间 `[-0.6210,+1.1644] pp`；
- MIRA-relative − Frozen：`+0.1149 pp`，95% 区间 `[-0.3728,+0.8063] pp`。

这轮的 task success、valid-return 和 failure rate 没有变化；只有 family macro 因 registry 口径被修正。Matched 的 family macro 下降到 `0.756552`，主要由其在 external registry 下暴露较少的 family support 造成，因此不能把 family macro 当作跨方法总榜。两个 paired 区间仍跨 0，不能把 common-stratum 结果写成已经稳定优于 Frozen 或 MIRA-relative。family-outage 受控压力下的改善和这里的普通 development pooled accuracy 必须分表，不能相互替代。

## 成本

表中的最后一列依次为 selection p95、update p95 和 numeric state p95；时间单位为 µs，状态单位为 bytes。Greedy/Bayesian 的状态约 1.10 MB，不能只比较 accuracy。FamilyGuard 的额外状态很小，主要代价来自对 33 个 provider 的 family factor 和 gate 判断；它相对 MIRA-relative 的 selection/update p95 比值为 `1.42x/1.91x`，numeric state 比值为 `1.013x`。这些是当前 Python/cache replay 的策略开销，不是线上 provider 金钱或 token 成本。

## 评判结论

这份回执补齐了 FamilyGuard 在 common-stratum 主输入上的 failure、family macro、latency 和 memory 证据，也通过了独立恢复。它把 family-outage 的正向修复从单独压力收据接到了主 benchmark 分母上，但不关闭 Gate C：paired uncertainty 仍跨 0，机制消融、完整 baseline fairness contract、unknown/refusal 分母和冻结 confirmation 仍需完成。
