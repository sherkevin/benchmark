# Deliverable B — MS-3「筛选阶段」过程评分原型

把 MolBench-MS-3 的 25 个虚拟筛选排序任务,从官方**只看 `Hits@3` 一个最终标量**,
升级为一组**带数值锚(实验 pChEMBL)的分级筛选指标** + **可插入对照裁判的 checkpoint 判定**。

## 为什么(对齐我们的 goal)
在自主性 benchmark 里,"虚拟筛选/排序"是长程任务的一个**阶段(checkpoint)**(见 `../A_worked_example_CK2alpha.md` Stage 2)。
对照裁判要在该 checkpoint 判断"agent 的排序是否走在合理且通向正确结论的路径上"(PRD §2.6),
需要比单一 `Hits@3` 更细、且锚在 ground-truth 数值上的评分。本原型即该实现。

## 文件
- `ms3_screening_eval.py` — 评分器(纯标准库,无 RDKit/numpy 依赖)。
- `results.md` — 在真实 25 任务上的运行结果(已生成)。

## 指标
| 指标 | 含义 | 相对官方 Hits@3 的增量 |
|---|---|---|
| `hits@{3,5,10}` | top-k 命中活性数 | 官方仅 Hits@3 |
| `recall@10` | 召回率 | 新增 |
| `ef@{10,20}pct` | 富集因子(1=随机)| 新增,VS 标准 |
| `ndcg@10` | **以 pChEMBL 为分级相关度**的 NDCG | 新增:区分"命中弱活性"vs"把最强结合排最前" |
| `auroc` | 活性 vs 非活性排序 AUROC | 新增,稳健 |
| `bedroc@20` | 早识别加权(Truchon-Bayly)| 新增,VS 标准 |
| `checkpoint` | 阶段通过判定(pass + 原因)| 新增:接入对照裁判 |

## 运行
```bash
python3 ms3_screening_eval.py --write results.md
```

## 接入真实 agent(公共 API)
```python
from ms3_screening_eval import evaluate_ranking, load_ms3
row = load_ms3("../../../prd/molbench_repo/data_full/molbench-ms-3/molbench-ms-3.csv")[0]   # CK2α
agent_ranked = my_agent.rank(row["candidates"])                  # agent 输出 60 个候选的排序
m = evaluate_ranking(row["candidates"], agent_ranked, row["actives"], row["scores"])
print(m["ndcg@10"], m["checkpoint"])   # 该 agent 在"筛选 checkpoint"的过程评分与通过判定
```
非法/缺失候选会被记入 `invalid_or_missing` 并补到末尾,保证全集排序、评分不崩。

## 结果摘要(见 results.md)
指标随排序质量 **oracle → noisy → random → reversed 单调下降**,且 `random` 的 `hits@3≈0.4`(=3×n_active/60 的解析期望)、`auroc≈0.49`(≈0.5)——两项与理论值吻合,**证明实现正确**。
checkpoint 通过率:oracle 100% → random 4% → reversed 0%,说明该判定可作为对照裁判的阶段门控。

## 真实性标注
- candidates / actives / pChEMBL:**真实**下载数据(`ms3.csv`,源自 CARA→ChEMBL)。
- checkpoint 阈值(hits@3≥1, auroc≥0.6, ef@10%≥2):**作者撰写**(▽),专家/超参可调。
