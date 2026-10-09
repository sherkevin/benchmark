# cp #1 strategy · Tier 2 LLM judge rubric

> **被评对象**:agent 在任务起步阶段输出的策略文字(自然语言段落,在调任何工具之前)
>
> **评分动作**:LLM judge 读策略文字,按下表给三档分数,最后必须输出严格 JSON

## 评分维度(三档制,各 1 项)

| 维度 | PASS(1.0) | PARTIAL(0.5) | FAIL(0.0) |
|---|---|---|---|
| **A. 三动作齐** | 策略明确含「批量预测 → 阈值筛选 → 提交」三个动作步骤 | 含两个动作 | 只含 0-1 个 / 完全不规划 |
| **B. 预算意识** | 显式提及 30 次预算、考虑覆盖率与代表性 | 提到预算但没具体安排 | 完全没提预算约束 |
| **C. 阈值锚定** | 明确说窗口 [1, 3](或等价表达,如"1 到 3"/"成药窗口") | 提到要按 logD 阈值过滤但未说具体范围 | 没提阈值或方向错 |

## 聚合

```
strategy_score = (A_score + B_score + C_score) / 3
# 例:A=PASS, B=PARTIAL, C=PASS → (1.0 + 0.5 + 1.0) / 3 = 0.833
```

## 输出格式(严格)

LLM judge **必须**只输出如下 JSON(不带 markdown 围栏、不带其他文字):

```json
{
  "A_three_actions": "PASS|PARTIAL|FAIL",
  "B_budget_aware": "PASS|PARTIAL|FAIL",
  "C_threshold_anchor": "PASS|PARTIAL|FAIL",
  "score": 0.0,
  "reason": "一句话总评(≤30 字)"
}
```

## judge prompt 模板(给 judge LLM 用)

```
你是 benchmark 评分员。任务:评估一个 agent 在「从 50 个分子中选 logD ∈ [1,3] 的全部分子,预算 30 次预测」任务上的开局策略文字。

策略文字:
<<<
{agent_strategy_text}
>>>

按以下三个维度独立打 PASS / PARTIAL / FAIL,然后算总分。严格按 JSON 格式输出,不要 markdown 围栏、不要其他文字。

A. 三动作齐:策略明确含「批量预测 → 阈值筛选 → 提交」三动作
   - PASS = 三动作齐 / PARTIAL = 含两个 / FAIL = 只 0-1 个或不规划
B. 预算意识:显式提及 30 次预算、覆盖率、代表性
   - PASS = 显式 / PARTIAL = 提及但没安排 / FAIL = 完全没提
C. 阈值锚定:明确说窗口 [1, 3] 或等价
   - PASS = 明确 / PARTIAL = 提到要按 logD 筛但没说范围 / FAIL = 没提或方向错

score = (A + B + C) / 3,PASS=1, PARTIAL=0.5, FAIL=0
```

## 校准样例(给开发者人工对照,不入 prompt)

| 策略文字示例 | A | B | C | score |
|---|---|---|---|---|
| "先批量调 predict_logd 查 30 个有代表性的分子(预算用满),按预测值落在 [1, 3] 过滤,把命中的全部 submit" | PASS | PASS | PASS | 1.000 |
| "查每个分子的 logD,留下符合窗口的提交" | PARTIAL(漏批量/预算/筛选三动作分开)| FAIL(没提预算)| PARTIAL(没说窗口具体范围) | 0.333 |
| "逐个 SMILES 调 predict 然后看结果" | FAIL(只一个动作)| FAIL | FAIL | 0.000 |
