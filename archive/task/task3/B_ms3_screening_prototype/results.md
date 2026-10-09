# MS-3 筛选阶段过程评分 — 真实数据运行结果

- 数据:`prd/molbench_repo/data_full/molbench-ms-3/molbench-ms-3.csv`,**25** 个任务(靶点),每个 60 候选。
- 指标在 5 个参考排序器上的均值(证明指标正确且对排序质量单调敏感)。
- `oracle`=活性按 pChEMBL 降序置顶(上界);`reversed`=活性置底(下界);`random`=随机基线;`noisy(N swap)`=在 oracle 上做 N 次相邻交换。

| ranker | hits@3 | hits@5 | recall@10 | ef@10pct | ndcg@10 | auroc | bedroc@20 | checkpoint通过率 |
|---|---|---|---|---|---|---|---|---|
| oracle | 3.000 | 5.000 | 1.000 | 7.689 | 1.000 | 1.000 | 1.000 | 100% |
| noisy(20swap) | 3.000 | 5.000 | 0.992 | 7.622 | 0.988 | 0.999 | 0.997 | 100% |
| noisy(80swap) | 3.000 | 4.920 | 0.992 | 7.308 | 0.972 | 0.997 | 0.984 | 100% |
| random | 0.400 | 0.600 | 0.166 | 0.903 | 0.136 | 0.490 | 0.129 | 4% |
| reversed | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0% |

**读法**:
- 所有指标随排序质量 oracle → noisy → random → reversed **单调下降**,说明评分正确、有区分度;
- 官方只报 `hits@3` 一个数;本原型额外给出 `ef`/`ndcg`/`auroc`/`bedroc`,其中 `ndcg@10` 用 **pChEMBL 作分级相关度**,能区分'命中弱活性'与'把最强结合排在最前';
- `checkpoint通过率` = 在我们设的筛选 checkpoint 阈值下(hits@3≥1 且 auroc≥0.6 且 ef@10%≥2)的任务占比,即对照裁判在该阶段判'通过'的比例;
- 接入真实 agent:调用 `evaluate_ranking(candidates, agent_ranked, actives, scores)`,即得该 agent 在'筛选 checkpoint'的过程评分与通过判定。

> 真实性:candidates/actives/pChEMBL 为真实下载数据;checkpoint 阈值为作者撰写(▽),专家可调。
