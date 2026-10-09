# ADR-0035：校准 provider-transfer 仍未形成动态主方法

- 状态：Accepted
- 日期：2026-09-27
- Supersedes：`provider_transfer` 和未校准 context-memory 的候选状态

## 背景

信息筛选显示 provider residual 有关系，但前三个 transfer 版本使用 raw BT 分数或独立在线 head，不能回答“校准后的 correctness residual 能否安全转移”。本轮用 fit-only affine calibration 和 residual covariance 重做一次固定 CPU 回放。

## 证据

5 folds × 2 seeds × delay `0/1/4` 的 calibrated transfer 准确率为 `0.8755/0.8783/0.8817`，Frozen calibrated 为 `0.8831/0.8831/0.8831`，independent 为 `0.8762/0.8769/0.8630`。真实矩阵优于置换矩阵，但 delay 0 低于 independent，三个 delay 都低于 Frozen。

## 决策

停止该线，不调参数、不申请远程任务、不把“真实矩阵优于置换”写成主方法效果。这个结果只支持一个较窄结论：离线 provider 关系可预测，但在当前反馈量、延迟和固定冷启动下，直接转移会破坏选择。后续候选必须有明确的安全 fallback 或其它可识别信息来源；CPU 预审查通过前不占 A800。
