# SUPER 怎么测（用对工具）

**一句话：给 agent 一个冷门研究仓库和一句任务，让它装环境、跑实验、报数字；用正则在它的执行日志里查"必经的关键输出"命中了几条。**

## 一个真实 case（Expert 集 colbert 任务）

- **任务**（大意）：在我的数据上训练 ColBERT 模型，报告最后一个 loss 值，格式 `{"last_loss": 0.0}`。agent 要 clone 仓库、装依赖、改配置、跑训练、抽取结果。
- **landmarks（里程碑正则）**：
  - `Some weights of ColBERT were not initialized...` —— 模型被正确加载的标志
  - `0 \d+\.\d+` —— 训练开始打印 loss 的标志
- **answer**：最终 `last_loss` 数值。

（见 [`reference/SUPER/case_示例.json`](./case_示例.json)）

## 判分机制（怎么算对）

- **过程分 = landmark 命中率**：`evaluate_checkpoints()` 用每条正则去搜 agent 的 observation 历史，命中一条得一分。landmark = 执行必经的确定性 stdout，与具体实现路径无关。
- **终态分 = answer 精确比对**：最终数值与标准答案比对。
- 全程 hard：正则匹配 + 数值比对，无 LLM。

## 属于哪种考法

**执行痕迹谓词（landmark 命中率）**：从执行日志里捕获"必然出现的关键行"作为过程卡点——即使没跑到最后，也能看出走到了哪一步。

> 完整数据与判分脚本：[`reference/SUPER/`](./)（`expert_full.jsonl`  全 45 题、`evaluate_dataset.py` 含 `evaluate_checkpoints()`）
