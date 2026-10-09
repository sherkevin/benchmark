# ADR-0040：保留 33-provider 主菜单，家族代表菜单只作敏感性附录

- 状态：Accepted
- 日期：2026-09-27
- 依据：`task/task49-pharma-benchmark-baseline-freeze/menu_sensitivity_v1/report.md`

## 背景

主 benchmark 有 33 个具体 provider，其中 24 个是同一 B3clf 家族的配置变体。需要确认它们是否让直接选择任务失真，或者只是增加了真实可调用的具体工具选项。

## 证据

固定 7-provider 家族代表菜单后，5-fold scaffold split 的 Best Fixed 仍为 `0.91557`，Morgan 线性选择 head 为 `0.90308`，evaluator Oracle 为 `0.99030`。33-provider 菜单对应数值为 `0.91557/0.89618/0.99723`。7-provider 线性 head 虽高约 `0.00691`，但五个 outer fold 都低于 Best Fixed；oracle headroom 反而变小。

## 决策

保留 33 个具体 provider 作为主 benchmark，因为动作仍然是调用一个具体工具，不能把同一家族的候选在事后合并。7-provider 代表菜单不替换主任务，只作为预先固定菜单敏感性附录。

## 后果

主任务继续报告 33-provider 的失败、延迟反馈和现代 baseline；论文需要说明同一家族配置变体的相关性，不能把 33 个 provider 当作 33 倍独立样本。若后续方法只在 7-provider 菜单有效，也只能作为菜单依赖的补充结果，不能替代主结论。
