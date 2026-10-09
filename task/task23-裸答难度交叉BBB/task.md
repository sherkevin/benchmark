# task23 · 裸答 × 工具难度交叉分析（BBB 族，D2-② 裸答半的决策证据）

> 上游：owner 2026-09-15「写具体文档，提交全部内容到远程」。
> 这是 U-8（裸答基线第一轮触作废线 0.80，下一步须 owner 裁）的**决策证据补强**，
> 不是对 U-8 的裁定。裁定权在 owner（涉预注册扩展与可能的任务作废，仓规第 7 条）。
> 规格出处：ADR-0028 D2-②、入库版提案 §3.4.6 第 2 条、naked_baseline/preregistration.json。

## 1. 目标

一句话：**在不跑任何新 LLM 的前提下，判定裸答 0.80 这个高分到底是「这道题不用工具就能答（任务作废）」，还是「裸答只在简单分子上高、在工具都判不准的难分子上塌掉（任务依然成立）」。**

U-8 登记了一个危险信号：qwen3.7-max 不给工具、只凭化学知识预测 BBB，平衡准确率 0.800，恰踩预注册作废阈值 0.80。但 0.80 这个**总分**本身分不清两种相反的含义：

| 读法 | 含义 | 对项目的后果 |
|---|---|---|
| **(a) 任务作废** | 相当一部分分子根本不需要查工具就能答对 | BBB 端的诊断题被判部分作废（STOP） |
| **(b) 高分集中在简单分子** | 裸答只在「所有工具都与真值一致」的易分子上对，在「工具互相打架」的难分子上塌掉 | 任务依然成立，且有价值的样本正是裸答救不了的那批 |

本任务用一个**工具侧、客观、不依赖裸答模型自报**的难度轴把 0.80 拆开，判定是 (a) 还是 (b)。

## 2. 难度轴：tool_wrong

对每个分子，`tool_wrong = 7 个 BBB 工具族里有多少个的投票与实验真值不一致`。

- `tool_wrong = 0`：所有工具都与真值一致 —— **最简单**，工具共识即正确，agent 不需要怀疑。
- `tool_wrong = 7`：所有工具都与真值不一致且彼此一致（伪共识）—— **最难**，agent 看到的是一片和谐却全错，正是本项目要抓的失败模式（D2-① 在全 730 池量出 14 个）。

这个轴与 D2-① 的 α_i（分子主效应）同源同口径（同一份 7 族投票），因此「难度」不是裸答模型自己说的，是工具集客观刻画的。

## 3. 输入（全部只读冻结，零 API）

| 输入 | 路径 | 用途 |
|---|---|---|
| 裸答答案（n1 第一轮，已落盘 2026-09-01） | `cases/_methodology/tool_conflict_candidate_pool/bbb_v1/runs_naked/<run>/answer.json` | 每模型 40 个 p_y1 |
| 答案键 | `data/.../naked_baseline/answer_key.json` | sample_id → entity_id → 真值标签 |
| 存档结果 | `data/.../naked_baseline/results.json` | per_model 平衡准确率（交叉核对用） |
| 预注册 | `data/.../naked_baseline/preregistration.json` | 阈值 0.80、判定规则 |
| 7 族投票 | `data/tool_results/divergence/血脑屏障 BBB.csv` | tool_wrong 难度轴（复用 D2-① loader） |
| 实验真值 | `screening_ledger.jsonl`（经 `vd.load_truth()`） | 核对答案键标签一致性 |

样本：40 个（20 阳 20 阴），全部在当前 730 池内。

## 4. 交付物

- [x] `src/probe_headless/bbb_naked_difficulty_crossref.py` — 交叉分析脚本（零 API、只读、确定性）
- [x] `data/tool_results/divergence/naked_difficulty_crossref_bbb.json` — 结果 JSON（带输入 md5）
- [x] `src/probe_headless/tests/test_bbb_naked_difficulty_crossref.py` — 20 条测试，三次变异验证
- [x] `task/task23-裸答难度交叉BBB/report.md` — 结论与证据（金字塔）

## 5. 边界

- **不改裸答基线的判定。** 本任务只**重读** n1 的已落盘答案做交叉分析，不补跑 n2–n5、不预注册扩展、不把 INCONCLUSIVE 改成 STOP 或 GO。预注册政策 `INCONCLUSIVE → 须先预注册扩展再补跑` 仍然成立。
- **不替 owner 裁 U-8。** 本任务给出 (a)/(b) 的证据，三选项（补跑/重设计题面/挂起）仍由 owner 裁。
- **interpretation_limit 不变。** 预注册明写「高分只证明无工具可解，分不清是记忆还是无辅助化学推理」——本任务**不**试图区分记忆与推理，只回答「高分落在难分子还是易分子上」。
- **小样本。** tool_wrong≥4 的层每层 n≤2，单调塌缩是 40 分子上的**趋势**，不是逐层显著性断言；脚本与本 task 都显式标为 caveat。
