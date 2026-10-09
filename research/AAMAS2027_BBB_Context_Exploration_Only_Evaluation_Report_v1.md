# BBB context exploration-only JEV 回放结果 v1

> 统计口径更正：本报告使用了尚未修正的 Hoeffding 半径和逐 episode 重查 gate，不能作为 coverage 或最终方法结果。请以 [v2](AAMAS2027_BBB_Context_Exploration_Only_Evaluation_Report_v2.md) 为准；本文件保留作历史审计记录。

日期：2026-09-25

## 结论

这是目前第一个在 BBB 回放上表现出稳定净效用改善的候选。它不是把所有二次调用结果都拿来做 IPW，而是只用随机探索分支采到的 gain 更新 context-provider 状态；利用分支的结果不进入估计。

在当前回溯集上，它相对 random/no-update probe 有小幅但一致的收益；不过 always Morgan 仍然更强，而且 722 个分子此前参与过开发，因此还不能写成最终方法或 fresh 泛化结论。

## 方法

- 首 provider 固定为 CNN，候选为 Morgan/MPNN。
- context 为 4 个 cell：`TPSA > 60` × `aromatic_ring_count > 0`。
- 每个 cell-provider 只维护随机探索样本的 bounded mean 和 alpha-spent Hoeffding 半径。
- 利用分支只在 LCB 超过调用成本时打开，探索概率 `0.8`，每 64 个 episode 重新计算一次门。
- 利用分支产生的反馈不更新状态，避免自适应 exploitation 结果污染置信区间。

## BBB 结果

20 个 seed，106 个 scaffold-held-out 分子，成本敏感性如下。括号内是跨 seed 的 95% t 区间。

| call cost | accuracy | calls / episode | net utility |
|---:|---:|---:|---:|
| 0.00 | 0.8274 `[0.8156, 0.8391]` | 1.9528 `[1.9393, 1.9664]` | 0.1726 `[0.1609, 0.1844]` |
| 0.05 | 0.8099 `[0.7913, 0.8286]` | 1.9160 `[1.8863, 1.9458]` | 0.2859 `[0.2684, 0.3034]` |
| 0.10 | 0.7892 `[0.7680, 0.8103]` | 1.8358 `[1.8114, 1.8603]` | 0.3944 `[0.3746, 0.4143]` |
| 0.20 | 0.7745 `[0.7538, 0.7953]` | 1.7948 `[1.7774, 1.8122]` | 0.5844 `[0.5652, 0.6036]` |

相比 v2 的 random/no-update control，paired net utility 差（M3 minus control）为：

- `call_cost=0.00`：`-0.0528`，95% 区间 `[-0.0696,-0.0361]`；
- `call_cost=0.05`：`-0.0293`，95% 区间 `[-0.0436,-0.0150]`；
- `call_cost=0.10`：`-0.0105`，95% 区间 `[-0.0159,-0.0051]`；
- `call_cost=0.20`：`0.0000`，门保持关闭。

`call_cost=0.10` 时，M3 accuracy 为 `0.7892`，random/no-update 为 `0.7745`；M3 平均多调用 `0.0410` 次，但 terminal loss 降低 `0.0146`，净效用仍降低 `0.0105`。

always Morgan 在同一回放上的 accuracy 为 `0.8962`、net utility 为 `0.3038`，所以 M3 还没有超过简单固定策略。

## synthetic 先验检查

固定 potential gain 的 500 seed × 1000 episode 检查中，4000 个有足够 support 的 cell-provider 检查 coverage 为 `1.0`，平均 gate open count 为 `96.6`（成本 `0.1`）。同一规模下，HT/Freedman context 状态 coverage 虽为 `1.0`，但 gate open count 为 `0`。这支持“探索-only 估计更容易形成可用门”的判断，但不替代真实任务验证。

## 边界

- 这是 post-freeze retrospective replay；当前仓库没有 model-fresh 的新分子池。
- 调用成本是 synthetic cost，不能转写成真实线上价格或延迟。
- exploration-only mean 的正式适用范围需要单独写清：它依赖随机探索选择与潜在 gain 的条件独立性。当前 BBB 结果先作为候选方法证据，不宣称最终置信覆盖。
- 还没有接入 Laya/JEV backbone，也没有做 context encoder 或 fine-tuning。

## 产物

- 方法代码：[m3_context_jev.py](../src/probe_headless/m3_context_jev.py)
- 多 seed 评估：[evaluate_bbb_context_explore_only.py](../src/probe_headless/evaluate_bbb_context_explore_only.py)
- 汇总：[summary.json](../experiments/logs/bbb_context_exploration_only_retrospective_v1_20260925/summary.json)
- 原始记录：[raw.jsonl](../experiments/logs/bbb_context_exploration_only_retrospective_v1_20260925/raw.jsonl)
- synthetic coverage：[context_jev_exploration_only_synthetic_v1_20260925](../experiments/logs/context_jev_exploration_only_synthetic_v1_20260925/)
