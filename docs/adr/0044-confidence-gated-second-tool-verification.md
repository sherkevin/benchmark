# ADR-0044：置信度门控的第二工具复核通过开发闸门

状态：Accepted

日期：2026-09-28

## 背景

调用后 numeric confidence 对正确性有信号。单纯拒答能提高 accepted accuracy，但 agent 也可以在低置信度时追加一个工具调用，直接提高最终答案可靠性，同时只为一部分请求付出额外成本。

## 决定

保留 confidence-gated second-tool verification 作为主 agent 流程候选。第一和第二 provider 都由 fit-only Best Fixed 排序得到；第一 provider 的 fit-only 0.8 coverage confidence 阈值决定是否追加第二次调用。随机追加调用必须使用相同 fallback 数量作为对照。

## 证据

五折 scaffold-heldout cache replay：Best Fixed pooled accuracy `0.915512`，confidence escalation `0.923823`，提升 `+0.008310`；平均调用次数 `1.206`；4/5 folds 不下降，4/5 folds 高于随机追加。500 个随机 seed 的固定数量 null 中，只有 `2.2%` 不低于 confidence escalation。独立恢复通过。证据见 `task/task49-pharma-benchmark-baseline-freeze/confidence_escalation_v1/` 与 `experiments/logs/task49_confidence_escalation_20260928/run_v1/`。

## 边界和后果

这是调用后验证流程的开发证据，不是动态 JEV 或 direct pre-call selector 的效果。下一步可以把 confidence threshold、provider reliability 和 selected-only delayed feedback 纳入小状态更新；若动态版本不能在相同调用成本下稳定超过静态 escalation，就保留静态流程，不把普通校准包装成主创新。confirmation 保持关闭。
