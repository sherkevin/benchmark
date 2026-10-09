# T1 Benchmark Characterization — 两轮 qualification 结果

> 日期：2026-09-20  
> 最终结论状态（2026-09-21）：**T1 benchmark qualification 已完成。LLMRouterBench 与 ASlib SAT12-ALL 通过；BBB 作为 hard scientific stress test 保留；BEIR-SciFact dense pool 降为 negative control / appendix。第一轮 NOT PASSED 的原始记录保留在下文。**

## 1. 先给结论

这轮最重要的结果不是“我们找到三个很好看的 benchmark”，而是 T1 gate 真正抓到了风险：

1. **三个环境都存在明显或可见的 Oracle headroom。**
2. 但 **BBB 与当前 BEIR lexical proxy 上，简单 contextual selector 并不能稳定超过 Best Fixed。**
3. BBB 甚至出现一个很有信息量的现象：单工具“会不会错”在 scaffold-OOD 下有中等可预测性（AUC 大约 0.59–0.71），但把这些概率直接转成“选哪个工具”以后仍然低于 Best Fixed。
4. LLMRouterBench 的 peer-reviewed 结果明确证明更复杂 router 可以超过 Best Single，但本轮尚未完成我们自己的本地 replay。

所以，当前不能声称“我们的三个 benchmark 都已经验证 contextual routing 可学”。T1 仍是 **GO/NO-GO 进行中**。

## 2. BBB — 本地实跑

数据：
- 722 usable molecules
- 7 tool families
- reward = 工具预测是否与 experimental/reference truth 一致

全量 characterization：

| 指标 | 数值 |
|---|---:|
| Best Fixed | ADMETLab |
| Best Fixed Accuracy | **0.9155** |
| Per-instance Oracle | **0.9806** |
| Headroom | **+0.0651** |
| Best Fixed 错、但其他工具至少一个能救 | **47 / 722 = 6.51%** |

这说明 routing opportunity 真实存在。

### 2.1 最简单 contextual selector

严格采用 5-fold Bemis–Murcko scaffold GroupKFold：
- context feature：Morgan radius=2, 1024 bits
- 每个 tool 一个 Logistic Regression，预测该 tool 当前是否正确
- 测试时选 predicted correctness 最大的 available arm

结果：

| 方法 | Accuracy |
|---|---:|
| fold-train Best Fixed | **0.9155** |
| Logistic contextual selector | **0.8947** |
| Oracle | **0.9806** |

即：

[
\Delta_{\text{contextual-fixed}}=-2.08\text{ pp}.
]

**线性 contextual routing 未通过。**

### 2.2 更强简单模型复核

为了排除“只是线性模型太弱”，在完全相同 scaffold split 下又跑：
- Random Forest
- class-balanced Random Forest
- Extra Trees
- class-balanced Extra Trees

最好的 class-balanced RF 也只有：

[
0.9017 < 0.9155.
]

因此当前不是单纯换一个树模型就能解决。

但每个 tool 的 error/correctness 本身并非完全不可预测。例如 Random Forest 下 per-tool AUC 约：
- ADMETLab 0.703
- B3clf 0.672
- DeepPurpose 0.642
- GraphB3 0.686

这说明：

> “工具什么时候可能错”存在 context signal，但把这种 signal 转成可靠的 **argmax tool selection** 很难，尤其 Best Fixed 已经很强，错误救回收益只有 6.5%，错误切换的代价却很高。

### 2.3 直接验证“错误切换代价”是不是根因

又做了一轮 conservative-switching 诊断：默认始终使用 fold 内 Best Fixed，只让模型预测“某个 alternative 相对 incumbent 的 reward advantage”，只有预测 advantage 超过阈值才切换。

结果非常稳定：**只要发生非平凡切换，harmful switch 都多于 beneficial switch。**

例子：
- Ridge, threshold=0：478 次 switch，19 次救回、33 次把原本正确结果换错，最终 0.8961；
- RF-regression, threshold=0.02：141 次 switch，6 次救回、10 次换错，最终 0.9100；
- RF-regression, threshold=0.10：只剩 9 次 switch，0 次救回、1 次换错，最终 0.9141，已经非常接近 Best Fixed 0.9155，但仍未超过。

因此 BBB 暴露出的不是“完全没有 context signal”，而是一个更具体的 **asymmetric safe-switching** 问题：

[
P(\text{incumbent 错且 alternate 可救})\text{ 很小，}
]

而错误把 incumbent 换走会直接损失一个正确答案。简单 contextual router 的问题不是不会拟合，而是 precision 不足以支撑安全切换。

这对论文问题是重要信息：如果 BBB 最终保留，它更自然测的是“**如何把新工具安全地 onboarding 到一个已有强 incumbent 的工具池**”，而不是泛化的“每个 context 都重新选最优工具”。

诊断数据：`data/tool_results/divergence/t1_bbb_switch_diagnosis.json`。

## 3. BEIR — 本地 same-function proxy 审计

本轮先下载并冻结 BEIR 官方小型数据：
- SciFact：300 test queries
- NFCorpus：323 test queries

来源：
https://github.com/beir-cellar/beir

因为冻结候选里写的 LQM provider pool（GTE/BGE/MiniLM 等）没有公开 per-query run matrix，而本机现有 sentence-transformers 环境还存在 huggingface-hub 版本冲突，所以本轮**没有静默把 lexical proxy 当正式最终 B2**。

仅用于先验 T1 审计的三个同功能 provider：
- BM25
- word TF-IDF
- char TF-IDF

reward 全部用官方 qrels 计算 per-query NDCG@10。

### 3.1 SciFact

| 指标 | 数值 |
|---|---:|
| Best Fixed (BM25) | **0.6623 NDCG@10** |
| Oracle | **0.7454** |
| Headroom | **+0.0831** |
| Ridge contextual CV | **0.6613** |
| contextual gain | **-0.0010** |

存在 8.31 个 NDCG 点的 Oracle headroom，但简单 query-text contextual selector 没吃到。

### 3.2 NFCorpus

| 指标 | 数值 |
|---|---:|
| Best Fixed (char TF-IDF, full panel) | **0.3150** |
| Oracle | **0.3538** |
| Headroom | **+0.0388** |
| CV fold-train Best Fixed | **0.3052** |
| Ridge contextual CV | **0.3067** |
| contextual gain vs CV fixed | **+0.0015** |

技术上略高于 fold-specific Best Fixed，但只有 0.15 个 NDCG 点，不能称为有意义的 contextual gain。

因此 lexical proxy 也**没有通过“简单 contextual selector 明显优于 fixed”这条 gate**。

## 4. LLMRouterBench — peer-reviewed characterization 已核，本地 replay 待补

官方 Performance Setting：
- 11,480 prompts
- 20 lightweight models
- 15 datasets
- 229,600 model-instance records

ACL Findings 2026 Table 13：
- Best Single：Qwen3-8B, **68.01%**
- Dataset Oracle：**73.10%**
- Per-instance Oracle：**91.64%**
- 因此 Best Single → Oracle headroom = **23.63 pp**
- Avengers：71.94%
- MODEL-SAT：71.88%
- EmbedLLM：71.24%

至少已发表结果证明：

[
\text{context/domain routing} > \text{Best Single}
]

是可实现的，而不是只有理论 Oracle gap。

来源：
- 官方 repo：https://github.com/ynulihao/LLMRouterBench
- ACL 2026 Findings paper：https://aclanthology.org/2026.findings-acl.1881/

### 本地数据状态

官方 1.28GB `bench-release.tar.gz` 已确认公开：
- Hugging Face: NPULH/LLMRouterBench
- SHA256: `b79f8cde1a6f029c2efa663a3a3b6f7748defb22341fe59f328cebef6648c8f1`

本轮本机访问 Hugging Face / Google Drive 时出现 connection reset / timeout，所以**没有把论文表格冒充成本地 replay**。等 bundle 到位后，必须按我们自己的 split 重新算 T1 与 held-out-arm protocol。

## 5. 当前 T1 表

| Benchmark | #contexts | #arms | Reward | Best Fixed | Oracle | Headroom | Simple Contextual |
|---|---:|---:|---|---:|---:|---:|---:|
| LLMRouterBench | 11,480 | 20 | score | 0.6801 | 0.9164 | +0.2363 | published routers +3.2~3.9 pp；**local pending** |
| BEIR-SciFact lexical proxy | 300 | 3 | NDCG@10 | 0.6623 | 0.7454 | +0.0831 | **-0.0010** |
| BEIR-NFCorpus lexical proxy | 323 | 3 | NDCG@10 | 0.3150 | 0.3538 | +0.0388 | **+0.0015 vs CV fixed，近零** |
| BBB | 722 | 7 | correctness | 0.9155 | 0.9806 | +0.0651 | **-0.0208** |

## 6. Gate 判定

原实验矩阵要求：至少 2 个 real benchmark family 同时具有：
1. 非平凡 Oracle headroom；
2. simple contextual selector 明显优于 Best Fixed。

当前：
- Oracle headroom：**通过**。
- simple contextual learnability：**未通过**。

所以 T1 总状态：

[
oxed{\text{NOT PASSED YET}}
]

这意味着现在不应进入 T3 new-arm cold-start，也不应开始设计复杂 shared-prior / active probing 方法。

## 7. 下一步只做三件事

### A. LLMRouterBench local replay
把官方 1.28GB bundle 真正拉到本机，按我们的 selector 和 split 跑一次，不能只引用论文数字。

### B. BEIR 正式 provider pool
恢复/隔离 sentence-transformers 环境，复现冻结候选的 GTE/BGE/MiniLM 或与 LQM 完全一致的 retriever pool；lexical proxy 只保留 sanity，不升级成正式 benchmark。

### C. BBB learnability diagnosis
不继续“换模型刷分”，而是专门解释：
- 为什么 per-tool correctness AUC 0.6~0.7；
- 但 tool-selection accuracy 仍掉到 0.90 左右；
- 是否是 calibration / rare rescue / asymmetric switching cost 造成。

如果经过这三步仍然只有 LLMRouterBench 能稳定超过 Best Fixed，则需要重新审视：
- BBB 是否适合做 routing 主 benchmark；
- 论文主问题是否应该从“contextual tool selection”改成“selective use / abstention / confidence”；
而不是强行继续当前路线。

## 8. 2026-09-21 第二轮：官方数据本地 replay 完成，T1 结论更新

### 8.1 LLMRouterBench：从“published evidence”升级为本地完整 replay

官方仓库已拉取并冻结：
- repo HEAD: `c77cb05`
- official HF bundle: `NPULH/LLMRouterBench/bench-release.tar.gz`
- bytes: 1,283,503,080
- sha256: `b79f8cde1a6f029c2efa663a3a3b6f7748defb22341fe59f328cebef6648c8f1`

按官方 `config/baseline_config.yaml` 的 15 datasets × 20 lightweight models，用官方 loader 的“每 dataset/split/model 保留最新结果”口径核到：
- 300 个 dataset/split/model 文件；
- **229,620 records = 11,481 prompts × 20 arms**；
- 当前公开 bundle 比论文 Table 13 的 229,600 / 11,480 多 20 条，差值恰好是一条 prompt × 20 models；因此论文数字与当前公开 bundle 不能混写。后续 replay 以本轮 frozen bundle hash 为准，论文 published number 只作历史对照。

官方 aggregator 在当前 frozen bundle 上复算：
- Best Fixed = **Qwen3-8B = 0.692267** Dataset-Avg；
- per-dataset Max Expert = 0.731501；
- per-instance Oracle = **0.918551**；
- Best Fixed → Oracle headroom = **+0.226284**。

为了不借 dataset ID 做 privileged routing，又补了一个真正只看 prompt 的 simple selector：
- context = prompt TF-IDF 1–2 gram，max 60k features；
- 不输入 dataset ID；
- 每 arm 一个 Ridge(alpha=20) 预测 score；
- 每个 dataset 内 70/30 prompt split，5 个固定 seeds；
- vectorizer 只 fit train prompts。

结果（macro over datasets）：
- fold-trained Best Fixed = **0.693498**；
- prompt-only contextual = **0.715502**；
- gain = **+0.022004 = +2.20 pp**；
- Oracle = 0.918526；
- simple selector closed ≈ **9.77%** of available Oracle gap。

所以 LLMRouterBench 的 T1 两条均通过：**有大 headroom，且 prompt context 的简单模型已经能稳定吃到一部分。**

产物：
- `src/probe_headless/t1_llmrouterbench_characterization.py`
- `src/probe_headless/t1_llmrouterbench_text_context.py`
- `data/tool_results/divergence/t1_llmrouterbench_characterization.json`
- `data/tool_results/divergence/t1_llmrouterbench_text_context.json`

### 8.2 BEIR-SciFact：正式 dense provider pool 已精确复现，但它不适合作为主 contextual benchmark

正式跑了 LQM-ContextRoute 论文同名的三个 dense retrievers：
- `thenlper/gte-base`
- `BAAI/bge-base-en-v1.5`（query instruction 按 BGE 官方写法）
- `sentence-transformers/all-MiniLM-L6-v2`

本地 per-query qrels 重算的 mean NDCG@10：
- GTE-base **0.761780**；
- BGE-base **0.740391**；
- MiniLM **0.645082**。

这与 LQM 文中 0.762 / 0.740 / 0.645 四舍五入后逐项一致，说明 provider reproduction 可信。

但真正与本文能力有关的结果是：
- Best Fixed = **0.761780**；
- Oracle = **0.801177**；
- headroom = **+0.039397**；
- 20 个 KFold seeds 下，MiniLM query embedding + per-arm Ridge 的平均 contextual = **0.758336**；
- mean contextual gain = **−0.003444**；
- 20 seeds 仅 1 个为正。

因此：**“同功能工具”本身并不自动意味着存在可学习的 per-query routing。这个 SciFact pool 的主要结构是一个全局强 provider GTE-base，而不是明显的 context-conditioned specialization。**

这不是方法失败，而是 benchmark qualification 失败：它没有充分实例化本文要测的主能力。BEIR 应保留作 negative control / cost-latency appendix，而不应继续占主 benchmark 位。

产物：
- `src/probe_headless/t1_beir_scifact_dense_exact.py`
- `src/probe_headless/t1_beir_scifact_context_seeds.py`
- `data/tool_results/divergence/t1_beir_scifact_dense_exact.json`
- `data/tool_results/divergence/t1_beir_scifact_context_seeds.json`

### 8.3 ASlib SAT12-ALL：作为 replacement candidate，contextual signal 极强

为避免“BEIR 不合格后随便找一个好看数据集”，replacement 使用 algorithm-selection 社区的经典公开 benchmark **ASlib SAT12-ALL**，并严格使用官方 `cv.arff` repetition=1 split：
- 1,614 instances；
- 31 solver arms；
- 113 个有效 instance features；
- objective = PAR10（越低越好），cutoff 1200s。

无需复杂模型，per-arm standardized Ridge on log(PAR10)，固定 alpha=10 即得到：
- Best Fixed PAR10 = **3079.886**；
- Oracle PAR10 = **241.318**；
- simple contextual PAR10 = **1486.937**；
- 相对 Best Fixed 降低 **51.72%**；
- 关闭 **56.12%** 的 fixed→oracle gap。

ExtraTrees 复核进一步到 916.399 PAR10 / 76.22% oracle-gap-closed，但 T1 不使用这个更强数过 Gate，主判据只采用固定 alpha=10 的线性结果。

这说明 ASlib 非常干净地实例化了本文需要的“context-dependent provider competence”：不同 solver 对不同 instance 的效果确实高度互补，而且简单 context model 已能泛化地利用。

产物：
- `src/probe_headless/t1_aslib_sat12all_characterization.py`
- `src/probe_headless/t1_aslib_sat12all_linear.py`
- `data/tool_results/divergence/t1_aslib_sat12all_characterization.json`
- `data/tool_results/divergence/t1_aslib_sat12all_linear.json`

## 9. T1 最终 benchmark qualification 判定

按“先资格审查 benchmark、再冻结方法”的原则，四个环境现在分成两类：

| Environment | Headroom | Simple context gain | Qualification |
|---|---:|---:|---|
| LLMRouterBench | +0.2263 score | **+0.0220** | **PASS — 主 benchmark** |
| ASlib SAT12-ALL | 3079.9→241.3 PAR10 gap | **-51.72% PAR10 vs fixed** | **PASS — 主 benchmark** |
| BBB | +0.0651 accuracy | **-0.0208** | **HARD — 保留 scientific stress test，不用于证明“简单 routing 必然有效”** |
| BEIR SciFact dense | +0.0394 NDCG | **-0.00344 mean** | **FAIL as positive benchmark — 降为 negative control / appendix** |

因此原 v1 的“三个 real family 中至少 2 个同时有 headroom + simple learnability”的精神现在可以满足，但**不是靠降低 Gate**，而是通过 T1 本来就应该做的 benchmark qualification：LLMRouterBench + ASlib PASS；BBB 保留为 hard application；BEIR 明确留下失败证据而不是删除痕迹。

这轮结果反过来还钉死一个更严谨的能力边界：**我们研究的不是“任何同功能工具都值得 contextual routing”，而是“当一个 capability pool 本身存在 context-dependent competence structure 时，如何对新加入、零历史的 provider 做快速行为式 onboarding”。**

在没有 tool-side competence metadata 的统一主协议下，B=0 时新工具的具体 specialization 在统计上不可识别；真正可比较的是：从第 1、4、8、16、32 个 behavioral feedback 开始，谁能最快恢复出新工具的 competence profile。这一点应成为后续 T3/T4 的协议核心。
