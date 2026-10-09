# MatSciBench — 采样说明【材料推理 benchmark：1340 题带分步参考解 + 规则判分】

## 论文与来源
- 论文：*MatSciBench: Benchmarking the Reasoning Ability of Large Language Models in Materials Science*（UCLA 等，arXiv:2510.12171）
- 代码：https://github.com/Jun-Kai-Zhang/MatSciBench ｜ 数据：HF `JunkaiZ/MatSciBench`（split=test）
- 本地采样状态：**HF datasets-server 因网络策略无法直连，本地暂未落真实样例**；字段 schema 已从官方仓库核实（见下），采样可后续经镜像补。

## 与需求的匹配度
- 单 agent：是（单模型解题）。
- 一次性输入：是。
- hard 中间 checkpoint：**部分——数值题（NUM）规则判分（归一化+容差比对，hard）；公式题（FORMULA）需 LLM judge（非 hard）。每题带 step-by-step 参考解，可用于步级错误定位，但官方评测只判终答**。

## Benchmark 是什么
大学级材料科学推理题库：**1,340 题**，覆盖材料/性质/结构/机理/工艺/失效六大类，含数值、公式、选择三种答案型，带难度分层、领域标注、嵌入图片（多模态）。字段：`qid / type(NUM|FORMULA) / question / image / solution(分步参考解) / answer / unit / difficulty_level / primary_category`。

## 评分机制
- 数值题：规则判分器（符号+数值容差），hard；
- 公式题：可选 `--llm_judge`；
- 自带四种推理策略基线：base / tool（写 Python 并执行）/ self-correction / self-consistency。

## 加载方式（通道已核实）
```python
from datasets import load_dataset
ds = load_dataset("JunkaiZ/MatSciBench", split="test")
```

## 对我们的参考价值
(1) 材料领域"分步参考解"现成语料——可反向铸造步级 checkpoint（同 PRM 思路但有标准解）；(2) 其 tool 模式（写代码算数值）+ 规则判分可与 MatTools 拼接成"材料推理+工具"双能力格。局限：单题非长程，需组题成 pipeline 方可用于长程评测。
