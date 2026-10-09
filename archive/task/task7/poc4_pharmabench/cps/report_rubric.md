# cp #7 report · Tier 2 LLM judge rubric

> **被评对象**:agent 提交后输出的报告文字(自然语言,含对每个提交分子的预测值依据)
>
> **评分动作**:LLM judge 读报告 + 看提交集,按下表给三档分数,严格 JSON 输出

## 评分维度(三档制)

| 维度 | PASS(1.0) | PARTIAL(0.5) | FAIL(0.0) |
|---|---|---|---|
| **A. 数值落地** | 报告对每个(或绝大多数 ≥80%)提交分子给出具体 logD 预测值 | 给出部分(40-80%)分子的具体值 | 不给数值 / 给数值 < 40% |
| **B. 决策可追溯** | 解释清楚为什么这些分子入选(显式提到阈值 [1, 3] 或预测值落在窗口内) | 有解释但不挂钩具体阈值 | 无解释或解释跑题 |
| **C. 诚实标注** | 提到不确定性 / 边界值 / 漏选的可能(如"未查的 20 个分子可能含遗漏正例") | 部分提及 | 全无;声称 100% 准确无误 |

## 聚合

```
report_score = (A + B + C) / 3
```

## 输出格式(严格)

```json
{
  "A_numerical_grounding": "PASS|PARTIAL|FAIL",
  "B_decision_traceable": "PASS|PARTIAL|FAIL",
  "C_honest_uncertainty": "PASS|PARTIAL|FAIL",
  "score": 0.0,
  "reason": "一句话总评(≤30 字)"
}
```

## judge prompt 模板

```
你是 benchmark 评分员。任务:评估一个 agent 完成「从 50 个分子中选 logD ∈ [1,3] 的全部分子,预算 30 次预测」后的最终报告。

提交分子(SMILES 列表):
{submitted_smiles_list}

报告文字:
<<<
{agent_report_text}
>>>

按以下三个维度独立打 PASS / PARTIAL / FAIL,然后算总分。严格按 JSON 格式输出。

A. 数值落地:报告对每个(≥80%)提交分子给出具体 logD 预测值
   - PASS = ≥80% / PARTIAL = 40-80% / FAIL = <40%
B. 决策可追溯:显式说明为什么选(挂钩阈值 [1, 3] 或预测值在窗口内)
   - PASS = 显式挂钩 / PARTIAL = 解释但不挂钩 / FAIL = 无解释/跑题
C. 诚实标注:提到不确定性 / 边界值 / 漏选可能性
   - PASS = 明确提 / PARTIAL = 部分 / FAIL = 全无或声称完美

score = (A + B + C) / 3
```

## 校准样例

| 报告示例 | A | B | C | score |
|---|---|---|---|---|
| "提交 22 个分子,每个的 logD 预测值如下:[列出 22 个 logD 值] ;选择标准:预测值 ∈ [1.0, 3.0]。注意:未查的 20 个分子中可能含 3-5 个遗漏正例,边界值如 1.05 已含。" | PASS | PASS | PASS | 1.000 |
| "提交以下分子,理由是它们的 logD 符合要求。" | FAIL | PARTIAL | FAIL | 0.167 |
| "我选了这些 SMILES,它们都很好" | FAIL | FAIL | FAIL | 0.000 |
