# DiscoveryBench — 采样说明（深度版）

## 论文与来源
- 论文：*DiscoveryBench: Towards Data-Driven Discovery with Large Language Models*（AI2，arXiv:2407.01725）
- 代码/数据：https://github.com/allenai/discoverybench ｜ 本地采样：gh raw，2026-07（真实任务 metadata + 数据 + 判分脚本）

## 它是什么
264 个"**数据驱动科学发现**"任务，覆盖考古/社会/生物/经济/工程等 6 域。给 agent 一份真实数据集 + 一个 discovery goal，要求做数据分析、**产出一个假设**（变量间的定量关系）。

## 完整 case（真实，`sample_task_archaeology/metadata_0.json`，逐字段）
这是从一篇真实考古学论文反向抽取的任务：
- `domain`：humanities；`workflow_tags`：time series analysis, data cleaning, data visualization；
- `domain_knowledge`：一大段领域背景（如"象征资本=斧剑+匕首+房屋大小；花粉数据 PCA 可作人口密度指标……时间序列按 100 年 bin 分析"）——**这就是给 agent 的"先验知识"**；
- `datasets`：3 个真实数据文件，每个带**逐列语义说明**——
  - `pollen_openness_score_..._2019.csv`（花粉 PCA 时间序列，含 Belau_PC1/SMOOTH_MEAN_100y 等 13 列，每列有 description）；
  - `time_series_data.csv`（工具/房屋/纪念碑等 Z 值随时间变化）；
  - `capital.csv`（各时期各种"资本"考古数据）；
- `queries`：结构化子问题 `{"qid":0, "question_type":"context", "question":"In which century did the axes become quantitatively most frequent?"}`；
- `hypotheses`：**金标准假设**（main/intermediate 分层）——判分锚点。**注意：test split 里 `hypotheses.main=[{}]` 为空**（金标准被隐去防泄漏），train 里才有。

## ★ 怎么打分（真实代码 `discovery_eval.py`，soft 而非 hard）
评测是**用 GPT-4 做假设匹配**，不是 hard 数值比对：
```python
eval_result = run_eval_gold_vs_gen_NL_hypo_workflow(
    query=query, gold_hypo=gold_hypo, gold_workflow=gold_workflow,
    gen_hypo=pred_hypo, gen_workflow=pred_workflow,
    dataset_meta=data_metadata,
    llm_used='gpt-4-1106-preview',     # ← 用 GPT-4 当裁判
    dataset_type=metadata_type, use_column_metadata=True)
```
- 把 agent 产出的假设（`pred_hypo`）与金标准假设（`gold_hypo`），连同数据集元信息（列语义），一起喂给 **GPT-4**，按 facet（context / variables / relationship）分解、逐面判定是否等价，输出结构化分数（写进 `eval_output.json`）；
- **所以判分是 LLM judge（soft），金标准是"目标假设"文本**。这不是我们要求的 hard checkpoint。

## checkpoint 怎么构建（谁定义）
- **从已发表论文反向抽取**：研究者读论文，把"用某数据集验证某假设"的过程整理成 (数据集 + goal + 金标准假设 + 领域知识 + 列语义) 结构；另有合成任务补充。
- **人力**：研究者标注（一次性）。**金标准来源=论文**（与 SciCode 同一条"答案藏在论文里、反推出来"的路线）。

## 判分性质
- 领域高度相关（数据驱动科学发现，含生物/材料类），金标准来自论文反向抽取（构建路线可借鉴）；
- 但判分是 **LLM 假设匹配（soft）**，无 hard 中间卡点——属"符合领域、判分不符 hard 要求"，作过程量/构建路线借鉴，不直接采用。

## 对我们的价值
- **金标准构建路线**：从真实论文反向抽取 (数据+goal+金标准假设+领域知识+列语义)，这条路我们可直接借鉴来做材料/药物任务；
- **一个警示**：它选了 LLM judge 评假设（soft）——再次印证"假设/发现类答案难 hard 判分"，我们要想 hard，就得把 checkpoint 落在**可执行/数值可比的中间产物**上，而非落在"假设文本"上。

## 采样内容
`sample_task_archaeology/`：`metadata_0.json`/`metadata_1.json`（完整任务结构：领域知识+3个带列语义的数据集+queries+金标准假设槽位）+ `capital_head.csv`（真实数据）；`discovery_eval.py`（GPT-4 判分脚本）；`REPO_README.md`。
