# Baseline eligibility matrix v2：执行前合同

日期：2026-10-04
状态：只读审计；不重跑、不读取 confirmation、不修改任何原始 receipt。

## 唯一问题

当前 BBB benchmark 的方法和 baseline 能否合法地组成一张主实验表？这里的“合法”指：输入、候选菜单、表示、warm-start、likelihood、selected-only 反馈、延迟、预算、failure/unknown/refusal 分母、family registry、verifier 和不确定性证据均有明确边界。

## 输入

1. `baseline_role_roster_v1/roster.json`：角色和比较权限；
2. `baseline_cost_budget_v1/table.json`：方法级输入、似然、更新、预算、失败分母和显式缺失字段；
3. `baseline_raw_run_schema_audit_v1/audit.json`：旧 raw-run 的字段证据；
4. 已封存 receipt 的 config、summary、verification 和 paired-analysis 指针。

只读检查文件是否存在、字段是否自洽、分层是否一致；不从代码推断缺失字段，不把 pooled accuracy 当配对证据，不把 adapted receipt 改名为 strict baseline。

## 输出字段

每个方法一行，至少包含：`method`、`role`、`eligibility_class`、`comparison_scope`、`input_sha256`、`menu_count`、`feature`、`warm_start`、`likelihood`、`selected_only`、`delays`、`budget`、`failure`、`unknown`、`refusal`、`family_registry`、`verifier`、`paired_ci`、`confirmation_scope`、`blockers`、`evidence_paths`。

## 分类规则

- `strict_main`：strict common prior/logistic 主合同；仅 Frozen、Matched 及同一 candidate stratum 可进入 strict common 表，仍需单独报告 paired uncertainty；
- `candidate`：本项目候选方法，不冒充外部 baseline；
- `adapted_stratum`：有完整运行和 verifier，但表示、先验、likelihood 或更新语义与 strict main 不同，必须分表；
- `anchor`：Random、Best Fixed、Oracle 等任务锚点或上界，不与在线方法混排；
- `static_control`：静态 full-panel router，回答静态能力问题，不冒充 selected-only 在线 baseline；
- `separate_control`：收据可独立复核，但 warm-start、likelihood 或成本合同与 strict main 不同；只能在自身合同内做 paired control，不能进入 strict 总榜；
- `audit_only`：只完成源码/表示或输入语义审计；
- `blocked`：本来需要进入比较，但缺少使比较成立的硬字段或 verifier，不能写进主结果。

## 通过和停止规则

- 只要缺少独立 verifier、输入/菜单绑定或关键信息边界，就不能标为 strict_main；
- 缺 `paired_ci` 不否定回放，但该行只能标 `development_only`，不能写 superiority；
- common-direct-prior、strict Gaussian、adapted Gaussian、static 和 anchors 必须分 strata；
- 若所有 strict candidate-vs-control 区间跨 0，矩阵结论为 `evidence_insufficient`，不增加新算法或 GPU；
- 输出中的 `MISSING` 是缺口，不得用理论值或代码推断补齐。
