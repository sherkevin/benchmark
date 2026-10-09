# PILOT-adapted development replay v1：结果

日期：2026-09-27

## 运行

使用 5 个 scaffold-disjoint outer folds、2 个 fit seed、3 个 feedback delay，共 60 条真实 BBB development 流。PILOT prior 只用对应 fold 的 fit 区拟合；在线阶段只收到已选 provider 的 delayed `system_success`。逐事件日志和独立恢复均通过。

## 结果

| delay | PILOT Frozen | PILOT Online | Online − Frozen | Best Fixed |
| ---: | ---: | ---: | ---: | ---: |
| 0 | 0.882964 | 0.882271 | -0.000704 | 0.915512 |
| 1 | 0.882964 | 0.880194 | -0.002773 | 0.915512 |
| 4 | 0.882964 | 0.885042 | +0.002074 | 0.915512 |

每个 delay 只有 4/10 paired fold-seed 流为正。预登记要求至少两个 delay 的均值为正，未通过；Online 在 delay 4 的偶然改善仍比 Best Fixed 低约 `0.03047`。

## 裁定

PILOT-adapted 离线先验和普通 selected-only cosine/UCB/ridge 更新不能解释当前任务的稳定收益，不申请 A800 做这条 baseline 的完整远程诊断，也不把它写成动态 JEV 方法。它保留为必要的现代 baseline 和负证据。

证据：`experiments/logs/task49_pilot_adapted_development_20260927/run_v1/`；独立恢复：`recovered_v1/summary.json`；协议：`preregistration.md`。没有新 provider 调用，confirmation 保持关闭。
