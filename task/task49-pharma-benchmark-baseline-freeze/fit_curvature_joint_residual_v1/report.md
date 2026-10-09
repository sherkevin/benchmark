# Fit-curvature joint residual CPU diagnostic v1

日期：2026-09-29
状态：已完成；开发期诊断，不进入 confirmation 或 A800。

## 要回答的问题

上一轮 joint-success residual 用常数初始精度 `1.5`。候选方法的数学描述要求使用 fit-only Fisher 曲率。本轮只替换这个初始化，其余数据、表示、更新递推、反馈语义和比较方法全部保持一致。

## 协议

- 722 个 BBB development 分子、33 个 provider、5 个 scaffold-disjoint outer folds。
- 2 个 seed、3 个 feedback delay `{0,1,4}`；共 150 条 stream、64,980 条 decision/terminal/feedback 事件。
- 方法：Frozen direct head；常数精度 residual（`I0=1.5`）；fit-curvature residual；fixed-step scalar residual；matched direct Online-SGD。
- fit-curvature 对 provider $a$ 使用：$I_{a,0}=1+sum_{iin fit}p_{i,a}(1-p_{i,a})$。$p_{i,a}$ 只由 fit-only direct head 计算。
- 在线更新仍是：$I^+=I+p(1-p)$，$c^+=c+(b-p)/I^+$；只更新被选 provider，failure 为 0，unknown 不更新。
- 没有调学习率、精度、阈值或顺序；没有读取 confirmation，没有使用 GPU。

## 结果

下表是 fit-curvature residual 减 comparator 的按 outer fold 配对结果。区间是 10,000 次 scaffold-fold bootstrap。

| comparator | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen direct | `-0.00002` `[-0.00625, 0.00620]` | `-0.00277` `[-0.00694, 0.00000]` | `-0.00209` `[-0.00832, 0.00619]` |
| constant precision residual | `+0.00484` `[-0.00207, 0.01176]` | `+0.00275` `[-0.00621, 0.01586]` | `-0.00553` `[-0.01315, 0.00278]` |
| fixed-step scalar | `-0.00070` `[-0.00417, 0.00207]` | `+0.00138` `[-0.00138, 0.00483]` | `-0.00209` `[-0.00694, 0.00276]` |
| matched direct Online-SGD | `+0.01034` `[-0.00139, 0.02483]` | `+0.01037` `[0.00139, 0.01588]` | `+0.00277` `[-0.00139, 0.00693]` |

各方法的平均 accuracy：

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen direct | 0.90173 | 0.90173 | 0.90173 |
| constant precision residual | 0.89687 | 0.89621 | 0.90517 |
| fit-curvature residual | 0.90171 | 0.89896 | 0.89964 |
| fixed-step scalar | 0.90241 | 0.89758 | 0.90173 |
| matched direct Online-SGD | 0.89137 | 0.88859 | 0.89687 |

fit-curvature 的初始 provider precision 随 fold/seed 变化，观测范围约为 `37.39–120.90`，不是人为挑选的常数。

## 核验和证据

独立 contract verifier 通过：150 条 stream、64,980 行事件；decision、terminal、feedback 对齐，selected-only read、delay 和 provider identity 均通过。

- 原始事件：`experiments/logs/task49_composition_20260929/fit_curvature_joint_residual_v1/run_v1/events.jsonl`
- 配置：`experiments/logs/task49_composition_20260929/fit_curvature_joint_residual_v1/run_v1/config.json`
- fit 曲率：`experiments/logs/task49_composition_20260929/fit_curvature_joint_residual_v1/run_v1/fit_curvatures.json`
- 核验：`experiments/logs/task49_composition_20260929/fit_curvature_joint_residual_v1/run_v1/verification.json`
- 聚合：`experiments/logs/task49_composition_20260929/fit_curvature_joint_residual_v1/run_v1/aggregate.json`
- 运行脚本：`src/probe_headless/run_fit_curvature_joint_residual_v1.py`
- 独立聚合：`src/probe_headless/analyze_fit_curvature_joint_residual_v1.py`

## 判断

Fisher 初始化解决了常数精度 residual 在 delay=0 下的明显漂移，但它没有带来超过 Frozen 的稳定收益。换句话说，先验强度是负结果的一个放大因素，不能解释为什么当前静态回放没有动态增益。当前更强的解释仍是：静态环境没有可利用变化，selected-only provider support 不足，且 Frozen direct head 已经是强起点。

因此不把 fit-curvature residual 冻结为主方法，不提交 A800 效果任务。后续若继续，只值得做一件事：在预先定义的 provider version/menu 或受控分布事件上测试“有变化时是否能恢复”，同时保留 Frozen 和 matched direct baseline。若没有动态事件，继续调 residual 初始化没有研究价值。
