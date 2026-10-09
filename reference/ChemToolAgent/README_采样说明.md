# ChemToolAgent — 采样说明（深度版）

## 论文与来源
- 论文：*ChemToolAgent: The Impact of Tools on Language Agents for Chemistry Problem Solving*（OSU-NLP，arXiv:2411.07228）
- 代码：https://github.com/OSU-NLP-Group/ChemToolAgent ｜ 本地采样：gh raw，2026-07

## 它是什么
一个 **ReAct 多步化学 agent**，核心研究问题：**给 LLM agent 配化学工具，到底有没有用**（对比"有工具 vs 无工具"的答题准确率）。

## 一个完整 case 的形态
给一道化学问题（如"预测分子 X 的某性质""这个分子叫什么""这步反应产物是什么"），agent 多步调工具求解，最后给 `Answer:`。任务本身多为**单问 QA**，长程性来自"多步调工具"而非任务跨多个强依赖阶段。

## agent 怎么调工具（真实循环，`agent_tool_agent.py` 的 `run()`）
严格 ReAct 格式，每轮 LLM 必须输出：
```
Thought:      [下一步推理]
Tool:         [工具名，必须来自给定列表]
Tool Input:   [输入，以 <END_INPUT> 结尾]
→ 系统返回 Tool Output，循环 …… 直到：
Thought:      [汇总结论]
Answer:       [最终答案]
```
循环上限 `max_iterations=40`；每步把工具输出追加进对话，作为下一步的上下文。

## ★ 机制一：调工具时，怎么判断"调得对不对"？（真实代码 `_call_tool`，288–306行）
**关键结论：它只判"工具调用能不能成功执行"，不判"该不该调这个工具/输入对不对/输出有没有用"。** 逐层看：

```python
def _call_tool(self, tool_name, tool_input, conv_id=None):
    # ① 工具名合法性：不在注册列表 → 失败，提示"请从这些里选"
    if tool_name not in self.tool_names:
        return False, '"%s" is not a valid tool. Please select from {...}'
    try:
        r = self.tool_dict[tool_name](tool_input)      # ② 真正执行工具
        # ③ PythonShell 特判：代码跑通但没输出 → 报错要求检查
        if tool_name == PythonShell.name and r.strip()=='[Code executed successfully with no output]':
            raise ChemAgentFatalError('...no output. Need a check.')
    except ChemAgentGeneralError as e:                  # ④ 工具内部抛错 → 失败
        return False, 'Error: ' + str(e)
    return True, r                                      # ⑤ 没崩 = success=True
```
所以"对"的标准只有两条，都是**执行层面**的：
1. **工具名在注册列表里**（不是瞎编的工具）；
2. **工具执行没抛异常**（输入能被该工具消化、跑出结果）。

**它不检查的（重要）**：
- 不检查"这一步选这个工具是否合理"（语义正确性）；
- 不检查"输入对这个工具是否有意义"（比如给性质预测器喂了个名字）；
- 不检查"工具输出是否真的推进了任务"。

这些**全靠 agent 自己的 Thought 推理**，系统只把工具输出原样塞回对话让它继续。格式解析失败（`extract_command` 抛 `ChemAgentOutputError`）会重试，连续失败 `max_error_iterations=3` 次才报错。

**一句话**：这里的"判对"= **可执行性校验**，不是**正确性校验**。真正的"答案对不对"留到最后用答案比对（见下）。**中间没有任何"这步工具调用是否正确"的卡点**——这正是它和我们需求的差距。

## ★ 机制二：最终怎么打分？
- 对 `Answer:` 与金标准做**正确性比对**（数值/名称/结构匹配，hard）；
- 核心实验指标 = **有工具 vs 无工具**两种设置下的**答题准确率差**，用以论证"工具的价值"；
- 全程**只评终答**，不评中间工具调用序列的质量。

## 真实工具（agent 调的"反查接口"，采样自 tools/）
- `property_prediction.py`：**UniMol 神经网络**算分子性质（这是"调接口查分子性质"的典型）；
- `name_conversion.py`（10KB）：名称↔SMILES↔CAS 互转；
- `molecule_description.py`：分子结构描述；
- `chemspace`（化学空间检索）、`ai_expert`（LLM 兜底）、`PythonShell`（跑代码）。

## 对我们的价值与差距
- **价值**：把"agent 调工具"作为一等研究对象，其 ReAct 循环 + `_call_tool` 的"可执行性校验"是我们"agent 调 oracle"那半的直接代码范式；`tool_use_chain`（每步记录 thought/tool/input/output/success）就是现成的 trace 结构。
- **差距**：判分只到终答；中间"工具调用是否正确"无 hard 卡点；任务偏单问。我们要补的正是**在 `tool_use_chain` 的每一步上加 hard checkpoint**（而非只校验"能不能跑通"）。

## 采样内容
`agent_tool_agent.py`（ReAct 主循环 + `_call_tool` 校验）、`agent_tools.py`（工具装配/校验 `verify_tools`）、`tool_base.py`、`tool_molecule_description.py`、`tool_name_conversion.py`、`tool_property_prediction.py`、`README.md`。
