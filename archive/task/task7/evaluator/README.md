# evaluator/ — 跨 POC 共享 grader 模块

## 文件

| 文件 | 职责 |
|---|---|
| `grader.py` | 聚合公式 + LLM judge 封装 + 格式化工具 |

## 用法

```python
from evaluator.grader import (
    binary_pass_at_1,
    weighted_partial_score,
    weighted_by_tier,
    parse_judge_json,
    build_rubric_prompt,
    format_cp_results,
    format_aggregation,
)
```

## 设计原则

- **不修改 src/**：POC 特定逻辑留在各 `run_poc*.py`
- **复用 src/core/judge**：`_judge_value_set` / `_judge_selection` / `_judge_ranking` 通过 `Stage` dispatch
- **LLM judge 独立**：Tier 2 cp 的 LLM 评测不污染 src 主仓的 kind dispatch

## 复用度

| POC | 复用 evaluator/ 哪些功能 |
|---|---|
| POC3 | 聚合公式（binary vs weighted） |
| POC4 | 聚合公式 + parse_judge_json + format_* |
| POC1 | 聚合公式 + parse_judge_json（设计阶段） |
| POC2 | 聚合公式（设计阶段） |
