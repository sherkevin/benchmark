# task26 — AAMAS2027 T1 Benchmark Characterization

> 日期：2026-09-20  
> 对应实验矩阵：`docs/AAMAS2027_完整实验矩阵_DynamicToolRouting_v1.md` 的 T1。  
> 状态：✅ 完成（2026-09-21；最终判定见 report §8–§9）。

## 目标

先不做新方法。只回答：

> 我们选的 benchmark 是否真的存在“按 context 选择 provider/tool”可利用的空间，而且这种空间是否有最基本的可学习信号？

T1 不通过，不进入 T3 new-arm cold-start 主实验。

## 当前验收量

每个 benchmark 至少给：
- #contexts
- #arms
- reward
- Best Fixed
- Per-instance Oracle
- routing headroom
- 一个简单 contextual selector 相对 Best Fixed 的增益
- 数据与 split caveat

## 本轮范围

1. BBB：本地完整实跑。
2. BEIR：先用完全本地可复现的 lexical same-function proxy pool 做先验审计；**不替代冻结候选中 LQM 的 GTE/BGE/MiniLM provider pool**。
3. LLMRouterBench：先核 peer-reviewed Table 13 与官方数据包；本地 1.28GB bundle 因本机 HF/Google Drive 连接重置/超时，暂未 replay。
4. 不做 T3/T4/T5；T1 gate 未过前不推进。

## 产物

- `src/probe_headless/t1_bbb_characterization.py`
- `src/probe_headless/t1_bbb_learnability_models.py`
- `data/tool_results/divergence/t1_bbb_characterization.json`
- `data/tool_results/divergence/t1_bbb_learnability_models.json`
- `src/probe_headless/t1_beir_characterization.py`
- `data/tool_results/divergence/t1_beir_characterization.json`
- `data/tool_results/divergence/t1_llmrouterbench_published.json`
- 本目录 `report.md`

## 2026-09-21 第二轮状态

- LLMRouterBench official frozen bundle 已本地完整 replay；prompt-only TF-IDF+Ridge 在 5 seeds 下相对 fold Best Fixed 平均 **+2.20 pp**，T1 PASS。
- BEIR-SciFact GTE/BGE/MiniLM 正式 dense pool 已复现到论文四舍五入一致；20 split seeds 下 contextual gain 平均 **−0.00344 NDCG@10**，qualification FAIL，降为 negative control。
- ASlib SAT12-ALL replacement candidate 按官方 CV 实跑：固定 Ridge alpha=10 将 PAR10 从 **3079.9 降至 1486.9**，相对下降 51.72%，关闭 56.12% Oracle gap，T1 PASS。
- BBB 维持：有 +6.51pp Oracle headroom，但 simple scaffold-OOD routing < Best Fixed，作为 hard scientific stress test。
- 因此 benchmark qualification 已结束并冻结 v2：`docs/AAMAS2027_完整实验矩阵_DynamicToolRouting_v2.md`。T1 不再继续搜 benchmark；下一步进入 new-arm protocol/baseline replay。
