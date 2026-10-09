# Invalid attempt: system_success semantic misread

日期：2026-10-05

第一次 receipt 生成把输入字段 `system_success` 当作 runtime success。该字段实际由 `prepare_pharma_fingerprint_diagnostic.py` 定义为 `status == "ok" and output_label == truth`，因此该尝试把所有错误预测错误地编码成 `⊥`。它没有进入任何训练、审计或论文结果；正式 receipt 已按 `output_statuses` 重建。
