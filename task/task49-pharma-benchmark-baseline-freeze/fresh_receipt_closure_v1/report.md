# Fresh mechanism H7 receipt closure v1 — result report

日期：2026-10-08。状态：**通过（PASS_FOR_DECLARED_POLICY_REPLAY）**。

这是一项 policy-layer receipt/recovery audit，不是新的效果比较。它读取已经封存的
fresh mechanism v2 receipt，并生成新的 receipt v3；没有重新选择分子、provider、漂移
规则或超参，也没有改写 v2 的效果数字和主版 PDF。

## 1. 固定范围和输入

- 四个 checkpoint-bound providers、157 个 confirmation molecules、五折、两个 deterministic
  schedule replicas、三种 delay $D\in\{0,1,4\}$、Frozen/MIRA-Relay/Local/fixed cyclic
  permutation 三种 policy，共每条件 90 streams。
- natural 和 controlled evaluator-side correctness drift 各一份；每流 157 decision、157
  terminal、157 feedback，以及一个 `stream_complete` marker，共 472 events。
- policy replay 不调用 provider、LLM、网络或 GPU。额外 provenance audit 使用现有
  `load_checkpoint_predictions`、`fit_prior`、`selector_feature` 对 development 资料做真实
  CPU forward/fit-prior 重建，不读取 confirmation 数据。

## 2. 独立重建结果

| 条件 | streams | events | final-state streams | checkpoint suffix | logical-clock reorder | unique state blobs |
|---|---:|---:|---:|---:|---:|---:|
| natural | 90/90 | 42,480 | 90/90 | 15/15 exact | 270/270 exact | 4,335 |
| controlled drift | 90/90 | 42,480 | 90/90 | 15/15 exact | 270/270 exact | 4,277 |

import-free verifier 从 canonical state bytes 独立核对每条 event 的 state reference、event
chain、decision/terminal/release 生命周期、selected output、due tick、summary metric 和
final state。checkpoint 在 event ordinal 31、79、143 **之后**保存；从保存的 state、pending
queue、terminal/consumed IDs 和 metric 接续 suffix，逐事件核对到最终 hash 和 correctness。
wire stress 使用固定的 identity、17-event block reverse、prefix-to-tail 三种 permutation，
按 `(logical_tick, phase, logical_ordinal, event_id)` canonicalize；它只证明声明的
logical-clock reorder，不外推任意网络乱序。

## 3. Provenance reconstruction

真实 CPU provenance run 的 summary SHA 为
`25ee8a7fc301bb27a815c42cbdfdcfed21961ee1534650ffcbab7ed4bb24aa4d`：

- 四个 checkpoint 对 722 个 development molecules 生成 2,888 个 native probabilities；
- 原始 30 条 Frozen streams 的 4,332 个 decisions 全部重建；
- logit 最大绝对误差 `0.0`，fit-rate 最大绝对误差 `0.0`，预登记容差 `1e-8`；
- fit/confirmation 成员分离检查通过；无 provider/API/GPU 调用。

v3 receipt 的 `config.json` 将 forward 和 fit-prior reconstruction 均绑定为
`COMPLETED`，同时保存 provenance 输入、源码、checkpoint 和输出 hashes。该重建是
sealed development provenance 复核，不是重新训练 confirmation 模型，也不证明训练数据
license 或完整端到端部署 lineage。

## 4. Fail-closed 与恢复

两组各执行 7 种 mutation：drop decision、duplicate event、state hash、reward、due tick、
truncate、schema。**14/14 均被拒绝**，且没有生成最终 verified summary。删除首个 decision
后从对应 immutable spool 按原 wire position 插回，两个条件都恢复为 byte-identical receipt：

- natural events SHA：`0bc01f38ba72efe1e556def92e28540a17870a895733a72f1a99d3b3c1eedfdf`；
- controlled events SHA：`6e657257c814d270213b6c316ad208c3e20958cc05937932dde89b87f5e940`。

恢复检查 summary SHA：`da3250a4991a1c3d107bdece5164dab1b63ba5b4cc28a5660f3fdec68c57cb7c`。

## 5. 曾出现的失败现场

v1 materialization 在 `natural_v1`/`controlled_v1` 中使用了尚未加入
`stream_complete` marker 的旧 runner；随后更新的 verifier 正确拒绝了 471-event receipt，
要求每流 472 events。这是 schema drift 的真实失败，不删除、不覆盖；v2/v3 重新生成后才
运行正式 verifier。v1 不进入通过统计。

## 6. 结论边界

H7 可以写为 **PASS_FOR_DECLARED_POLICY_REPLAY**，M14 的 receipt/recovery 子项可以升到
4/4（限本声明范围）。这项结果仍不能写成 provider inference、offline optimizer 的
端到端独立重实现，不能外推自然 provider lifecycle drift、family sharing、verification/
refusal 或 agent-level gain；完整方法 Gate 仍需按三份标准联合复核。

## 7. Evidence paths

- natural receipt：`experiments/logs/task49_fresh_receipt_closure_v1_20261008/natural_v3/`
- controlled receipt：`experiments/logs/task49_fresh_receipt_closure_v1_20261008/controlled_v3/`
- natural verification：`.../natural_v3_verification/verification.json`
- controlled verification：`.../controlled_v3_verification/verification.json`
- provenance：`experiments/logs/task49_fresh_receipt_closure_v1_20261008/provenance_reconstruction_v1/`
- delete/reinsert：`experiments/logs/task49_fresh_receipt_closure_v1_20261008/delete_reinsert_recovery.json`
- input preflight：`experiments/logs/task49_fresh_receipt_closure_v1_20261008/preflight_input_manifest.json`
- implementation：`src/probe_headless/run_fresh_receipt_closure_v1.py`,
  `src/probe_headless/verify_fresh_receipt_closure_v1.py`,
  `src/probe_headless/check_fresh_receipt_recovery_v1.py`
