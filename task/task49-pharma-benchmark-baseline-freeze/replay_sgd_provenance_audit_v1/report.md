# Replay-SGD provenance audit v1

- 日期：2026-10-04
- 范围：只读 provenance 审计；没有重跑 policy、没有读取 confirmation、没有新增预测。
- 机器收据：`audit.json`
- 审计脚本：`src/probe_headless/audit_replay_sgd_provenance_v1.py`

## 决定

Replay-SGD 可以保留为 **separate matched control**，但不能进入当前 strict common-direct-prior 主表。

这不是把旧结果删掉，也不是把它改名成 strict baseline。旧回放使用的是历史 `bt_cls.score.weight`；当前严格收据使用的是另一个 fit-only direct prior。两者都是 33×64 的 provider head，但初始化状态不同，直接混排会把 warm start 差异误当成在线更新收益。

## 逐项证据

| 项目 | 结果 | 证据 |
|---|---|---|
| Task49 输入 | 通过 | 旧 config、旧 input manifest、当前 strict receipt 都是 `a73f1c…e3bdf` |
| 旧事件完整性 | 通过 | 1 config、2,000 epoch、10 fit snapshot、12,996 decision、12,996 feedback、90 streams；与预登记一致 |
| 独立 recovery | 通过 | `recovered_v2/summary.json` 为 `verified`，并保存 source hash、event conservation、NumPy policy replay |
| feature 绑定 | 数值通过 | 10 个 fold/seed 均为 722×64；与当前 strict snapshot 的最大绝对差 `3.51×10^-7`，审计阈值 `5×10^-7` |
| 历史 warm start | 通过 | 10/10 个旧 prior 的 hash 与对应 BT checkpoint 的 `score.weight`（float64 封装）一致 |
| strict direct prior 等价 | 不通过，且是预期边界 | 10/10 个旧 prior 与当前 `direct_prior` 不相等；最大绝对差 `0.3328` |
| state bytes | 缺失 | 旧事件只有 update timing，没有 `state_bytes_before/after`，不能补填 `state_bytes_p95` |

## 可进入的比较范围

Replay-SGD 可以报告：

- 旧收据的 selected-only、delay `{0,1,4}`、Replay capacity 64、batch 8、learning rate 0.05；
- 与它自己同一历史 warm start 下的 Frozen/Online/Replay 描述性对照；
- 作为“普通 selected-only logistic update + replay buffer”的 task-native control。

Replay-SGD 不能报告：

- 与当前 Frozen direct / Matched Online-SGD 的 strict superiority 或 raw accuracy 总排名；
- 没有 state bytes 的统一资源成本结论；
- 对当前 common-direct-prior 方法的增益归因。

## 对验收的影响

该审计把 Replay 从“证据尚未判定”收敛为“证据可复核但比较分层不同”。Benchmark+Baseline 的严格主表仍只有 Frozen direct 与 Matched Online-SGD；Gate B 的分数不因这次审计增加，仍为 `11/16 HOLD`。Gate C 也不变，仍缺统一 E1–E5 主矩阵、完整 failure/unknown/refusal 聚合和 frozen confirmation。

后续如果要让 Replay 进入统一成本表，需要重新运行带有 feature/warm-start manifest、`state_bytes_before/after` 和独立 verifier receipt 的版本；不回填旧 receipt。
