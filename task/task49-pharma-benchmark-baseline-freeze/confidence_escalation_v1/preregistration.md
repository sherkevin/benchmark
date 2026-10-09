# Confidence-gated second-tool verification v1

日期：2026-09-28

## 问题

固定的第一工具已经选出后，provider 返回的 numeric confidence 能否决定是否追加第二个工具，从而提高最终工具答案的 correctness，同时控制平均调用次数？这一步检验的是 agent 的选择加验证流程，不把第二次调用伪装成一次 direct selection。

## 固定协议

- 每个 outer fold 只用 fit 区按 `system_success` 选出第一 provider 和第二 provider；不看 evaluation 标签排序。
- 第一 provider 的 confidence 为 $c=2|p-0.5|$。阈值沿用前一项已冻结的 fit-only target coverage `0.8`：按 fit confidence 降序取前 `floor(0.8*n_fit)`，最后一个 confidence 为阈值。
- evaluation 先调用第一 provider。若 $c\ge\theta$，直接采用第一 provider 输出；否则追加第二 provider，并采用第二 provider 输出。第一、第二次调用都计入成本，失败结果保留为错误。
- 随机追加调用对照在每个 evaluation fold 选择与 confidence 分支相同数量的随机样本，使用 seed `20260928`，其余采用第一 provider。
- 记录最终 correctness、fallback rate、平均调用次数、两工具都调用时的结果和两工具 oracle 上界。

## 进入标准

相对 Best Fixed，confidence escalation 的 pooled accuracy 提升至少 `0.005`、平均调用次数不超过 `1.25`，且至少 `4/5` folds 不下降；相对同 fallback 数量的随机追加调用，至少 `4/5` folds 更高且 pooled 提升至少 `0.003`。不满足则停止这条追加验证主线，不申请 A800。

## 边界

这是已有缓存上的开发回放，没有新 provider 调用，也不是 selected-only 在线更新效果。阈值、provider 排序、随机种子和停止条件在运行前固定；confirmation 保持关闭。
