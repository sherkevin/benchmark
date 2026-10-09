# Controlled provider-version event positive control v1

## 核验

共 90 条 stream；每条 stream 都按冻结输入重算 terminal reward，并检查了事件后目标 provider 的 failure 注入、selected-only 反馈和 delay。

## 结果

| 方法 | delay | 总体 accuracy | 事件前 accuracy | 事件后 accuracy | 事件前目标 provider rate | 事件后 rate | 首次 failure 后 rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| frozen_joint_success | 0 | 0.8699 | 0.8917 | 0.8485 | 0.0792 | 0.0715 | 0.1192 |
| frozen_joint_success | 1 | 0.8699 | 0.8917 | 0.8485 | 0.0792 | 0.0715 | 0.1211 |
| frozen_joint_success | 4 | 0.8699 | 0.8917 | 0.8485 | 0.0792 | 0.0715 | 0.1187 |
| joint_success_fit_curvature | 0 | 0.8872 | 0.8903 | 0.8842 | 0.0847 | 0.0317 | 0.0432 |
| joint_success_fit_curvature | 1 | 0.8831 | 0.8875 | 0.8787 | 0.0861 | 0.0330 | 0.0479 |
| joint_success_fit_curvature | 4 | 0.8865 | 0.8875 | 0.8855 | 0.0667 | 0.0317 | 0.0341 |
| matched_online_sgd | 0 | 0.8893 | 0.8750 | 0.9036 | 0.0819 | 0.0069 | 0.0000 |
| matched_online_sgd | 1 | 0.8865 | 0.8764 | 0.8967 | 0.0958 | 0.0083 | 0.0000 |
| matched_online_sgd | 4 | 0.8948 | 0.8861 | 0.9035 | 0.0722 | 0.0137 | 0.0000 |

## Paired comparison

- `fit_curvature_minus_frozen/post_accuracy/delay0`: point `0.0357`, CI `[0.0013318112633180946, 0.08363774733637744]`
- `fit_curvature_minus_frozen/post_target_rate/delay0`: point `-0.0398`, CI `[-0.10285388127853881, 0.0]`
- `fit_curvature_minus_frozen/post_target_rate_after_first_failure/delay0`: point `-0.0760`, CI `[-0.17727314925333548, 0.0]`
- `fit_curvature_minus_frozen/post_accuracy/delay1`: point `0.0302`, CI `[-0.001388888888888906, 0.07541856925418569]`
- `fit_curvature_minus_frozen/post_target_rate/delay1`: point `-0.0385`, CI `[-0.09463470319634702, -0.0013888888888888887]`
- `fit_curvature_minus_frozen/post_target_rate_after_first_failure/delay1`: point `-0.0732`, CI `[-0.16034482758620688, -0.007575757575757576]`
- `fit_curvature_minus_frozen/post_accuracy/delay4`: point `0.0371`, CI `[0.004052511415525084, 0.08769025875190258]`
- `fit_curvature_minus_frozen/post_target_rate/delay4`: point `-0.0398`, CI `[-0.10418569254185692, 0.0013888888888888887]`
- `fit_curvature_minus_frozen/post_target_rate_after_first_failure/delay4`: point `-0.0788`, CI `[-0.17394843962008144, -0.007936507936507936]`

## 解释边界

这是在真实冻结 BBB 输出上注入的受控 version degradation 正控。它只回答在线状态在明确退化事件出现时能否利用 selected failure feedback；不证明自然环境存在这种漂移，也不替代主 benchmark。
