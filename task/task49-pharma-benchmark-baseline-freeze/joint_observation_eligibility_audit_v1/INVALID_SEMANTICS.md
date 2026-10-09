# Invalid semantics notice

日期：2026-10-05

本目录的 v1 结果将 `system_success` 当作 runtime success。根据 `prepare_pharma_fingerprint_diagnostic.py`，该字段实际是 `status == "ok" and output_label == truth` 的化学正确性指标；真正的 runtime 状态来自 `output_statuses`。因此 v1 的 failure、complete-case 和 pair co-success 数字不能使用。纠正后的审计见 `../joint_observation_eligibility_audit_v2/`。
