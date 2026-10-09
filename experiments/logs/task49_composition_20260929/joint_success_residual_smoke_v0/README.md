# Joint-success residual integration smoke v0

这是一个 CPU integration smoke，不是效果实验。配置是 fold 0、seed 0、前 32 个
evaluation molecules、三个 delay `{0,1,4}`，使用冻结 `bt_cls` encoder 和 fit-only
direct system-success heads。

每个 JSON 文件包含两个方法：`frozen_joint_success` 和
`joint_success_residual`。policy 只收到被选 provider 的 terminal status/label；
correctness 只在 feedback release 事件进入更新。terminal 事件未包含 truth、
chemical correctness、recovered label 或完整 provider panel。每个文件 64 个决策、
64 个 terminal、64 个 feedback，pending queue 最终为 0。

观测到的 smoke 数字：

| delay | frozen | residual |
|---:|---:|---:|
| 0 | 28/32 | 29/32 |
| 1 | 28/32 | 27/32 |
| 4 | 28/32 | 28/32 |

这些数字只证明代码在真实冻结输入上完成了 selected-only delayed replay；样本很小，
没有显著性、不代表方法效果，也不打开 confirmation。全量 CPU gate 仍需固定
5 folds × 2 seeds × 3 delays、matched direct controls 和独立 verifier。
