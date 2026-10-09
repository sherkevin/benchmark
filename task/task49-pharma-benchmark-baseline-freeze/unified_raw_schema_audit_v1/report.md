# Unified raw-run schema audit v1

日期：2026-09-30
状态：contract audit；不产生效果排名，不改变 baseline 资格或 confirmation。

## 目的

旧日志审计的主要问题是：不同方法虽然都有结果，但选择、终止、延迟释放和状态转换没有统一落盘。本轮把已经独立验证的 C0/C4–C8 contract smoke 与 MIRA contract smoke 放在同一检查器下，确认它们是否至少共享一份可审计的 raw event 字段合同。

## 输入收据

- baseline：`experiments/logs/task49_baseline_contract_smoke_20260930/mira_queue_regression_v1/`
  - C0、C4、C5、C6、C7、C8；18 streams。
- proposed：`experiments/logs/task49_mira_contract_raw_20260930/run_v5/`
  - MIRA-dev；3 streams；commit provenance 已独立核验。
- 两个收据使用同一个 64-molecule、33-provider 输入，输入文件 SHA 相同。

检查器：`src/probe_headless/audit_unified_contract_schema_v1.py`。

## 检查结果

- decision/terminal/feedback 各 **1,344**，合计 **4,032** 个事件；21 条 stream。
- 两组收据都原生包含：`menu_before_choice`、`decision_reads`、`released_feedback_ids`、`provider_event_ids`、`feature_sha256`、terminal selected output/correctness、due/release tick、version before/after、state hash 和 unknown 语义。
- baseline 与 MIRA 的 cohort/menu 完全一致；contract summary 都显式关闭 effectiveness estimate。
- baseline verifier 和 MIRA verifier 均已先独立通过；本审计只做跨收据 schema/输入一致性检查，不替代两者的动作与状态回放。

机器收据：`experiments/logs/task49_mira_contract_raw_20260930/unified_schema_audit_v1.json`。

## 边界

这一步只证明 C0/C4–C8 与 MIRA 的 contract-only smoke 可以共享同一 raw schema。它没有把旧 development effect runs 自动升级为正式主表，也没有解决：

1. 各 baseline 的论文源码 fidelity 和训练成员 lineage；
2. 旧 full effect runs 与新 contract smoke 的输入/初始化差异；
3. hERG/AMES endpoint equivalence、confirmation overlap 和 dynamic event scope。

## 下一步

把同一 schema 写入正式 development effect runner，并为 Matched Online-SGD、PILOT、BayesianRouter、Greedy 和 MIRA 生成同一输入、同一 warm-start、同一 delay/event 的可恢复 raw run；在 Gate A/B/C 其他条件满足前，不把本 smoke 作为 A800 放行理由。
