# Biomni — 采样说明（深度版）

## 论文与来源
- 项目/论文：*Biomni: A General-Purpose Biomedical AI Agent*（Stanford SNAP，biomni.stanford.edu）
- 代码：https://github.com/snap-stanford/Biomni ｜ 过程级评测框架：BiomniBench（bioRxiv 2026.05）
- 本地采样：gh raw，2026-07（评测器全文 + 任务加载器 + 工具样本）

## 它是什么
通用**生物医学 agent**：LLM + **150+ 工具**跨 20+ 学科，多步 ReAct 执行科研任务（变异优先级、GWAS 致病基因、扰动筛选设计、罕见病诊断等）。配套评测集 **BiomniEval1**。

## 数据集结构（真实，`eval_biomni_eval1.py`）
- 数据：`hf://datasets/biomni/Eval1/biomni_eval1_dataset.parquet`（一个 parquet，全部任务合并）；
- 每行字段：`task_name`、`task_instance_id`、`instance_id`、`split`(train/val)、`prompt`（题面）、`answer`（金标准）；
- 评测入口：`evaluate(task_name, task_instance_id, user_answer) -> reward ∈ {0.0, 1.0}`，按 `(task_name, task_instance_id)` 查金标准再比对。

## 一个完整 case（真实，评测器 main() 示例）
任务 `gwas_variant_prioritization`：给一个 GWAS 位点，agent 多步调工具（查数据库→序列/功能分析→文献）找出致病变异，输出变异标识；金标准是确切变异 ID，**精确匹配**才得分。

## ★ 机制：怎么打分？（真实代码 `_compute_reward`，逐任务 hard 比对，全表）
**全部是确定性 exact-match，无 LLM judge、无人工**，但**只评最终答案**（reward 打在终答上）：

| 任务 task_name | 答案形态 | 判分规则（真实代码） |
|---|---|---|
| `crispr_delivery` | 选项字母 a–f | `user.lower()==gt.lower()` → 1.0 |
| `gwas_causal_gene*` | 基因名 | `user.upper()==gt.upper()`（大小写不敏感）→ 1.0 |
| `gwas_variant_prioritization` | 变异 ID | `user.strip()==gt.strip()`（**严格精确**）→ 1.0 |
| `hle` | 字母 A–Z | 大小写不敏感字母匹配 → 1.0 |
| `lab_bench*` | 字母 A–Z | 大小写不敏感字母匹配 → 1.0 |
| `rare_disease_diagnosis` | JSON | 解析后比 `OMIM_ID` 字段相等 → 1.0 |
| `screen_gene_retrieval` | 基因符号 | 大小写不敏感匹配 → 1.0 |
| `patient_gene_detection` | JSON{causal_gene:[...]} | 预测基因列表与金标准**有交集**即 → 1.0 |

代码片段（基因任务）：
```python
elif task_name.startswith("gwas_causal_gene"):
    return 1.0 if user_answer.strip().upper()==ground_truth.strip().upper() else 0.0
```
- **hard**：全是字符串/字段精确比对，0/1 二值，无部分分（除 patient_gene_detection 用"交集"放宽）；
- **评终态**：reward 只看最终 `user_answer`，**完全不判中间工具调用对不对、调了几步**。

## ★ 机制：调工具时怎么判"对不对"？
Biomni agent 本体是 ReAct 多步（`biomni/agent/`），工具调用同样**只做可执行性校验**（工具名合法、能跑出结果），**不判语义正确性**；中间步骤无 hard 卡点。**过程级评测是 BiomniBench 才引入的**（主仓库 BiomniEval1 仍只评终答）。

## 工具规模（真实，`biomni/tool/`，agent 调的"反查接口"）
20+ 学科工具模块，体量即覆盖面：`pharmacology.py`(167KB)、`database.py`(198KB，各大生物库检索)、`genomics.py`(112KB)、`molecular_biology.py`(91KB)、`immunology.py`(80KB)、`genetics.py`(66KB)、`microbiology.py`(58KB)、`bioengineering.py`(52KB)、`cancer_biology.py`(52KB)、`bioimaging.py`(53KB)、`pathology.py`、`cell_biology.py`、`biophysics.py`、`lab_automation.py`、`literature.py`、`protocols/`（含 Addgene 实验方案文本）等。

## 对我们的价值与差距
- **价值**：当前可下品里最接近"长程 agent + 自调工具 + hard 判分"的规模化现成品；其**逐任务 hard exact-match 判分器**（`_compute_reward`）是我们"终答 hard 比对"的直接模板；150+ 工具体系是"agent 调 oracle"的规模范例。
- **差距**：① 主评测**只评终答**（过程级在 BiomniBench）；② 任务偏"**有唯一硬答案的科研 QA**"（找基因/变异/字母），而非"开放式分子设计"；③ 工具调用无中间 hard 卡点。
- **可借鉴**：终答 hard 比对模板 + 工具编排范式；我们补"中间过程 hard checkpoint"并把领域聚焦到分子/材料设计。

## 采样内容
`eval_biomni_eval1.py`（**逐任务 hard 判分器全文**）、`task_lab_bench.py`/`task_hle.py`/`task_base.py`（任务加载器，含 case 结构）、`tool_pharmacology_sample.py`/`tool_molecular_biology_sample.py`（工具样本，截断）、`README.md`。
注：LAB-Bench 原始 QA 行在 HuggingFace 为受限（gated）数据集，未取原始行；case 结构见 `task_lab_bench.py`（question + distractors + ideal，输出 `[ANSWER]X[/ANSWER]`）。
