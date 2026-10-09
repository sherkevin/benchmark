# DSBench — 采样说明【Kaggle 系数据科学 agent，终态指标（对照）】

## 论文与来源
- 论文：*DSBench: How Far Are Data Science Agents from Becoming Data Science Experts?*（arXiv:2409.07703，ICLR 2025）
- 代码：https://github.com/liqiangjing/dsbench
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是。
- 一次性输入：是。
- hard 中间 checkpoint：**否——按最终指标（数据建模比对标准答案 / 数据分析对答案）给分，无中间卡点**。作为 Kaggle 系代表对照收录。

## Benchmark 是什么
540 个真实数据科学任务：466 个数据分析任务（来自 ModelOff Excel/金融建模竞赛，问答式）+ 74 个数据建模任务（来自 Kaggle，含长上下文任务描述与数据文件）。考察 agent 端到端数据科学能力。

## checkpoint / 评分机制
终态判定，hard 但无中间卡点：
- **数据建模**（Kaggle 系，采样 `data_modeling_list.json`）：agent 产出预测提交，用 `score4each_com.py` 算相对性能分（相对该竞赛的基线/最优）；
- **数据分析**（ModelOff 系）：客观答案精确比对；
- 全程无中间检查点，只看最终提交/答案。

## 采样内容
- `data_modeling_list.json` / `data_analysis_list.json`：两类任务全清单（含 Kaggle 竞赛名、URL、数据规模、年份）。
- `data_modeling_sample5.json` / `data_analysis_sample5.json`：各 5 条样本。
- `data_modeling_score4each.py`：数据建模判分脚本（相对性能计算）。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
作为"Kaggle 系数据科学 agent 评测"的代表，与 MLE-bench 同属"终态指标分级、无过程卡点"阵营。其数据建模的"相对性能分"（相对基线归一化）思路，可作为 checkpoint 达标阈值设计的参考。同样是我们要补"过程卡点"的对照对象。
