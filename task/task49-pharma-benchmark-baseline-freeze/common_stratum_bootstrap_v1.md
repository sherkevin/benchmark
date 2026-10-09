# Scaffold-cluster paired bootstrap

状态：描述性不确定性分析，不是 confirmation 或 p-value。

实现：`src/probe_headless/bootstrap_common_stratum_v1.py`。每个 molecule 先平均同一方法在 seed `{0,1}` 和 delay `{0,1,4}` 的 paired delta；再在每个 outer fold 内按 Bemis--Murcko scaffold cluster bootstrap，最后对 5 个 fold 等权平均。固定 RNG seed=`20260930`、10,000 draws。

原始结果：`experiments/logs/task49_common_stratum_bootstrap_20260930/v1.json`。

| 对比 | observed delta | scaffold bootstrap 95% percentile interval |
|---|---:|---:|
| Matched SGD − Frozen | −0.00946 | [−0.02291, 0.00219] |
| Greedy online − Frozen | −0.01430 | [−0.02786, −0.00063] |
| Bayesian online − Frozen | −0.03623 | [−0.05090, −0.02133] |
| MIRA − Frozen | +0.00115 | [−0.00373, 0.00806] |
| Greedy online − Greedy no-update | −0.01430 | [−0.02820, −0.00066] |
| Bayesian online − Bayesian no-update | −0.01845 | [−0.02782, −0.00802] |

区间只说明当前 development replay 的 paired uncertainty；它没有修复 Gaussian reward-scale 语义，也没有处理 provider-family macro、order sensitivity 或 confirmation shift。MIRA 的区间跨过 0，当前不能宣称稳定提升。
