# MIRA unified raw receipt v1

日期：2026-09-30  
状态：development CPU full receipt；独立 schema/candidate recovery 通过；未进入 confirmation。

## 目的

把 Proposed MIRA relative-reliability、streak-only、Frozen 和 matched Online-SGD
放到同一份事件合同中。历史 `mira_relative_reliability_cpu` effect receipt 保留，
本收据只补统一 raw writer、menu/read-set、terminal binding、延迟 release 和
source provenance，不改写历史结果。

## 固定合同

- 722 个 BBB development molecules、33-provider menu、5 个 scaffold folds、2 个
  protocol slots、delay `{0,1,4}`；
- `no_event` 与预登记 `controlled_event` 各一组，后者在 evaluation 中点后让
  fit-only Best Fixed provider 持续返回 runtime failure；policy 看不到 event flag；
- 四个方法使用同一 direct-prior/64D feature snapshots：Frozen、runtime-failure
  streak-only、MIRA relative-reliability、matched Online-SGD；
- selected-only feedback，due tick 为 `decision_tick + 1 + delay`；不读取未选输出，
  不打开 confirmation。

## 证据

权威目录：`experiments/logs/task49_mira_unified_raw_20260930/full_v1/`。

- 240 streams（4 methods × 2 scenarios × 5 folds × 2 seeds × 3 delays）；
- 34,656 decision/terminal/feedback 各一条；
- schema verifier：240/240 streams、34,656/34,656 events 通过；
- relative candidate 独立 NumPy/SciPy action/state replayer：60/60 streams、
  8,664/8,664 events 通过；
- immutable source provenance：9/9 source hashes 通过，commit
  `866fbce5c9673c2c1c059a3d2b227b505da75551`；
- events SHA-256：`9f0aca7b3cd7f74576964de1c0a7678d6299bd8a7299fabf41ada70e6ed81be8`；
- 收据输出：`schema_verification.json`、`candidate_verification.json`、
  `provenance.json`。

## 描述性结果

每个值是 10 个 fold/seed streams 的 overall selected accuracy；这里只用于核对
统一回放，post-event suffix 仍以 `mira_relative_reliability_v1/report.md` 为准。

| 场景/方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| no-event Frozen | 0.901734 | 0.901734 | 0.901734 |
| no-event streak-only | 0.901734 | 0.901734 | 0.901734 |
| no-event relative-reliability | 0.903113 | 0.903113 | 0.902423 |
| no-event matched SGD | 0.891375 | 0.888587 | 0.896868 |
| controlled Frozen | 0.869943 | 0.869943 | 0.869943 |
| controlled streak-only | 0.891346 | 0.889962 | 0.889962 |
| controlled relative-reliability | 0.894109 | 0.892725 | 0.890656 |
| controlled matched SGD | 0.889306 | 0.886513 | 0.894794 |

这些是 overall 数字，不能替代 event 后 suffix，也不构成最终 superiority claim。它们
与旧 effect receipt 的整体分母一致；旧报告把 controlled post 单独拆出，所以数值不应
直接逐项相等。

## 解释边界

这条收据关闭了 Proposed 与 matched control 的 unified raw-run 证据缺口。它不关闭
provider training membership、endpoint equivalence、自然漂移、成本/统计配对和
冻结 confirmation；controlled event 仍是压力测试，不能冒充自然环境结果。

