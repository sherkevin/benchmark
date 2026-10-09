# Structured CPU full_v1：统计与回放独立审查

日期：2026-09-29  
范围：只读审查 `full_v1/run/events.jsonl`、`summary.json`、`config.json`；未跑 A800，未修改历史结果、主线文档或 preregistration。

## 发现

现有 `aggregate_structured_cpu_effect_v1.py` 的点估计可由事件重算，但其 bootstrap 在每个 `(fold, seed, delay)` 配对流内部独立重采样 scaffold。相同 evaluation entity 在两个 seed 和三个 delay 中反复出现，独立抽样会把重复观测当成新的 cluster 实现，低估跨实体不确定性。v2 用全局 scaffold cluster 一次抽样，并把该抽样复用于所有 seed/delay，保持重复观察的配对关系。

现有 `verify_structured_cpu_effect_v1.py` 验证了事件集合、字段边界和版本增量，但没有回放每个 stream 的事件顺序。缺口包括：decision tick 连续性；terminal 必须紧跟对应 decision；release 前不能早于 due tick；延迟队列中 due/ordinal 顺序；terminal 与 feedback 的 provider、status、selected label 一致；`decision.version` 和按事件顺序恢复的 policy version 一致；`stream_complete` 的 n、correct、valid_returns、accuracy、final_version 与事件重算一致。full_v1 中 delay=1 的合法模式是：decision tick 1 仍看到 version 0，反馈 tick 2 释放 event tick 0 并更新为 version 1；因此不能把同一 event 的 decision.version 直接与 feedback.version_before 比较，必须按日志顺序回放。

## 新增独立回放器

`src/probe_headless/recover_structured_cpu_effect_v1.py` 不导入 runner。它校验 fit manifest 文件哈希和 encoder shape，按 stream 重放 pending queue，检查上述时间、身份、状态和 summary 守恒，并输出每个 event 的 outcome、scaffold、provider、runtime 与 release tick。输入只用于 entity→scaffold 映射，不把 truth 读入 policy。
提供 `--config` 时还核对 input hash、expected stream 数和 method/fold/seed/delay roster。

## 新增保守聚合器

`src/probe_headless/aggregate_structured_cpu_effect_v2.py` 只消费回放 JSON。它保留 method/delay 汇总和 structured−matched 的 fold/seed 配对，并以全局 scaffold cluster bootstrap 计算每个 delay 及所有 delay 的 CI。

对 full_v1 的实际回放：180 streams、25,992 sample records，回放通过；events SHA-256 为 `349223a10e741ad4e2e389932a7049f83038be38ab121c16d24428d5331fadc9`。v2 点差值为 delay 0 `-0.005540`、delay 1 `+0.004155`、delay 4 `-0.000693`，所有 delay pooled `-0.000693`。全局 382 scaffold clusters、10,000 replicates 的所有 delay pooled CI 为 `[-0.006294, 0.005377]`，P(delta>0) 为 `0.3868`；逐 delay CI/P 以及完整 hashes 已落盘在 `experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/recovered_v1/aggregate_v2.json`。这属于开发期复核，不改变原 preregistered CPU go/kill 判定。

## 可复核命令

```bash
python3 src/probe_headless/recover_structured_cpu_effect_v1.py \
  --events experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/run/events.jsonl \
  --inputs task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json \
  --config experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/run/config.json \
  --output experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/recovered_v1/recovery.json
python3 src/probe_headless/aggregate_structured_cpu_effect_v2.py \
  --recovery experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/recovered_v1/recovery.json \
  --output experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/recovered_v1/aggregate_v2.json \
  --bootstrap-replicates 10000
```

没有重跑 runner、没有读取 A800、没有覆写 `full_v1/aggregate.json`。

## 复核补充：argmax 一致性

审计器随后增加了 `scores` 长度、provider 菜单顺序和 `provider_id == argmax(scores)` 检查；在不改原始日志的前提下重新生成 `recovered_v2/recovery.json` 与 `recovered_v2/aggregate_v2.json`，仍通过 180 streams、25,992 sample records。结果与 recovered_v1 相同。该检查只验证日志中的选择与记录分数一致，不等同于从模型权重重新计算分数。

变异检查：把首个 feedback 的 `release_tick` 加 1 后，recover 以 `ValueError: release tick must equal next decision tick` 拒绝。收据：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/recovered_v2/tamper_rejection.json`。

为便于复核，32 MB 的 JSON recovery 另保留 gzip 副本 `recovered_v2/recovery.json.gz`，SHA-256 `065fcfb38102c6c76a3da82b4a9bc2e1abb601bbd1d222a50003ce2e266baaab`；未删除原 JSON。

最终 recovery v2 重新生成后，gzip 副本 SHA-256 更新为 `6ff35d8440ac99c7d7bab152349e22e652094a264e4bc750fdcf736c6fd4a9fb`；之前的临时 gzip hash 不再使用。
