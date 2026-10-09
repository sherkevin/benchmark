# Matched effect matrix v3：五折 seed0 开发闸门汇总

目的：在不新增回放、不选择性删折的前提下，把 fold0–4 的已核验 receipt 按预登记的 paired molecule 和 scaffold-cluster 口径汇总。此文件不是 confirmation，也不把开发信号写成统计显著或最终论文效果。

## Receipt coverage

- 5/5 fold verifier 通过；每折 9 streams。总 evaluation rows：722，scaffold clusters：382。
- fold0–4 的 raw events、config、summary、独立 verifier 和 manifest 分目录保存；delay 0/1/4 是同一 fold 的延迟复本，不能当作独立样本。
- fold4 的一次旧 verifier 拒绝因浮点阈值口径不一致，原始拒绝文件保留；最新 verifier 对同一 raw receipt 通过，未重跑。

## Fold-level paired deltas

| Delay | MIRA vs Independent fold net | mean (median), t4 95% CI | MIRA vs NoUpdate fold net | mean (median), t4 95% CI |
|---:|---:|---:|---:|---:|
| 0 | [1, 1, 0, 0, 0]；0.400 (0.000)；[-0.280, 1.080] | [1, 1, 1, 2, 0]；1.000 (1.000)；[0.122, 1.878] |
| 1 | [1, 1, 0, 0, 0]；0.400 (0.000)；[-0.280, 1.080] | [1, 1, 1, 2, 0]；1.000 (1.000)；[0.122, 1.878] |
| 4 | [0, 1, 0, 0, 0]；0.200 (0.000)；[-0.355, 0.755] | [0, 1, 1, 2, 0]；0.800 (1.000)；[-0.239, 1.839] |

## Paired rescued/harmed by fold

| Delay | Baseline | Fold0 | Fold1 | Fold2 | Fold3 | Fold4 |
|---:|---|---|---|---|---|---|
| 0 | Independent | 1/0 | 1/0 | 0/0 | 0/0 | 0/0 |
| 0 | NoUpdate | 2/1 | 1/0 | 2/1 | 4/2 | 1/1 |
| 1 | Independent | 1/0 | 1/0 | 0/0 | 0/0 | 0/0 |
| 1 | NoUpdate | 2/1 | 1/0 | 2/1 | 4/2 | 1/1 |
| 4 | Independent | 0/0 | 1/0 | 0/0 | 0/0 | 0/0 |
| 4 | NoUpdate | 2/2 | 1/0 | 2/1 | 4/2 | 1/1 |

## Scaffold-cluster descriptive interval

| Delay | Comparison | Cluster bootstrap 95% percentile CI |
|---:|---|---:|
| 0 | mira_vs_independent | [0.0000, 0.0074] |
| 0 | mira_vs_no_update | [-0.0031, 0.0169] |
| 1 | mira_vs_independent | [0.0000, 0.0075] |
| 1 | mira_vs_no_update | [-0.0031, 0.0168] |
| 4 | mira_vs_independent | [0.0000, 0.0048] |
| 4 | mira_vs_no_update | [-0.0052, 0.0159] |

## What this changes

- 五折完成后，MIRA 对 Independent 的 paired net 为 `[1, 1, 0, 0, 0]`（delay0/1）或 `[0, 1, 0, 0, 0]`（delay4）；没有一折出现负 net，但只有 fold0/1 出现正向 rescued 条目。
- 对 NoUpdate 的 net 为 `[1, 1, 1, 2, 0]`（delay0/1）或 `[0, 1, 1, 2, 0]`（delay4）；fold4 为 0，正向不是所有折都重复。
- MIRA 与 Independent 的 provider action change 很少（fold-level合计 delay0/1 为 8/722、delay4 为 6/722），说明当前共享 family residual 几乎没有改变最终 action；这直接限制了“family transfer 带来额外选择收益”的机制证据。
- 这是一条继续决策信息，不是停止目标或降低主张：下一步应优先做不新增大回放的机制诊断，检查 family residual 是否被 provider bias、fallback/hazard gate 或动作 margin 遮蔽；只有找到可检验的修复并预登记，才值得 seed1/A800。

## Boundary

这些区间是五折/固定 seed 的 development 描述；不能替代独立 confirmation，也不能把 722 个分子或 19,494 个事件当成独立 seed。
