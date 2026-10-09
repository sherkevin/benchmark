# ADR-0033：context memory v1 因分数语义不成立而停止

- 状态：Accepted
- 日期：2026-09-27
- Supersedes：ADR-0032（候选进入下一轮）

## 背景

ADR-0032 依据 v1 回放把 BT provider score 当作 logit，直接计算 $r-\sigma(s)$ 并把结果加回 $s$。BT 训练目标只规定 provider 排序，未校准 correctness 概率，因此这个 residual 没有成立的单位解释。独立审查还发现 smoke 使用的 Morgan64/随机先验不等于正式 hidden representation，且同一 adapter 的重复回放不能称独立验证。

## 决策

1. 撤回 v1 的正向效果解释，不删除原始结果；它只能作为发现定义问题的历史记录。
2. 用 fit-only per-provider affine logistic calibration 重新定义 correctness score，并在概率空间读取 residual，形成 v2 回放协议。
3. v2 在 5 folds × 2 seeds × 3 delays 中低于 independent residual bias，至少一个 delay 低于 random memory，因此停止 context memory 主线，不再调参、不打开 confirmation。
4. A800 只在下一候选通过 CPU falsification 后做一次接入 smoke；当前未提交 v3 smoke。

## 后果

已有普通 SGD、BayesianRouter、provider-transfer 和 context memory 四条标准/候选线均未形成可写成创新的正证据。后续方法必须先说清楚 score 的统计语义、反馈下可观测信息和更新稳定性，再消耗远程 GPU。
