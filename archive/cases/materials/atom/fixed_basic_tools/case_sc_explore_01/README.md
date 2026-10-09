# case_sc_explore_01 — 探索效率（hidden-list · budget-limited · 连续梯度分）

> 🛑 **2026-08-14 退役：本题型作废，勿作范例、勿复用。** 「在真实材料库里找最高 Tc」这个问法**天生可以靠背**——强模型 qwen-max 靠「汞系铜氧化物最高」的教科书记忆就稳拿 0.9，测的不是探索效率。一度改用原子匿名（Hg→Jc）"救"，但那把材料题变成了纯符号搜索（数学题），更不对。owner 2026-08-14 定：**材料 benchmark 不做匿名**；此"列表找最优"问法无法在"真名"前提下做到"答案不可背"，故**退役**。第一组回到 dopant 外推 / open 发现（真名 + 答案不可背）。详见 `cases/_methodology/explore_01复验报告_2026-08-14.md`。下方旧内容仅存档。

---

**（以下为撤回前的旧记录，勿据此判定达标）** 四条尺子全部通过。

## 题目

100 个超导材料候选（8 个高 Tc≥90K、12 个中、30 个低、50 个接近零），**不在 prompt 里列出**。
Agent 必须：
1. 调 `search_db(query)` 发现候选（每次最多返回 3 个，免费）
2. 调 `measure_tc(composition)` 测 Tc（每次 1 credit，共 7 次）
3. 调 `submit_answer` 提交找到的最高 Tc 材料

Score = min(0.9, tc_chosen / tc_max)。Cap 去除"碰巧找到全局最优 127K"的运气奖励。

## 为什么这道题能区分

- **弱模型（turbo）**：搜索策略简单、测几个就停 → 找到 70-88K 材料 → score ≈ 0.55-0.69
- **中模型（coder-plus）**：策略不稳定、有时失败 → 均值偏低 → score ≈ 0.18-0.75
- **强模型（qwen-max）**：系统化搜索多个元素族 → 稳定找到 ≥95K → score = 0.9 (cap)

## 实测结果（3 模型 × 5 轮）

| 模型 | 均值 | 轮次 | 波动 |
|---|---|---|---|
| qwen3.7-max | 0.900 | [0.9, 0.9, 0.9, 0.9, 0.9] | 0.000 |
| qwen-turbo | 0.608 | [0.551, 0.551, 0.693, 0.693, 0.551] | 0.142 |
| qwen3-coder-plus | 0.294 | [0.748, 0.181, 0.181, 0.181, 0.181] | 0.567 |

**尺子对照（全部通过）**：
- ①均值 = 0.601 ∈ [0.60, 0.70] ✓
- ①极差 = 0.606 ≥ 0.20 ✓
- ①排序稳定 = max波动 0.567 < 极差 0.606 ✓
- ②最强 = 0.900 ≤ 0.9 ✓

## 关键设计参数

| 参数 | 值 | 为什么 |
|---|---|---|
| 候选池 | 100（8 top / 12 mid / 30 low / 50 zero） | 高 Tc 材料被大量低 Tc 稀释 |
| 预算 | 7 次测量 | 远少于 100 → 必须策略性选择 |
| search_db PAGE_SIZE | 3 | 每次只看到一小部分 → 需多次搜索 |
| 候选列表 | 不给（hidden） | 不能扫一眼就定位最优 |
| 分数上限 | cap = 0.9 | 去除"找到 127K"的运气成分 |

## 复现

```bash
python src/probe_headless/sweep_explore.py       # 3模型×5轮
python cases/materials/atom/fixed_basic_tools/case_sc_explore_01/judge_explore.py
```

## 文件清单

- `artifacts/ground_truth.json` — 100 候选 + 真值 + budget=7
- `judge_explore.py` — 判分器（连续分 + cap=0.9）
- `runs/` — 各 workspace（每 run 一个独立文件夹）
- `src/probe_headless/openai_explore_runner.py` — agent harness（OpenAI function-calling）
- `src/probe_headless/sweep_explore.py` — 并行批跑脚本
