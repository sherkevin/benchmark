# OdysseyBench — 采样说明

## 论文与来源
- 论文：*OdysseyBench: Evaluating LLM Agents on Long-Horizon Complex Office Application Workflows*（Microsoft，arXiv:2508.09124）
- 代码/数据仓库：https://github.com/microsoft/OdysseyBench
- 本地采样下载通道：GitHub Contents API，2026-07 抓取

## Benchmark 是什么
办公应用（Word/Excel/PDF/Email/Calendar）上的**跨多日、跨多应用长程工作流**基准，共 602 个任务：
- **OdysseyBench+**：300 个从真实使用场景提炼的任务；
- **OdysseyBench-Neo**：302 个新合成任务；
- 任务被"打散"在多天的对话历史（chat histories）里，Agent 必须从长期多轮交互上下文中提取关键信息才能完成当前任务——考察长程记忆与信息整合，而不是单一 prompt。

## 构建方法（HOMERAGENTS 多智能体自动化流水线）
论文的另一贡献是用多智能体流水线自动生成长程任务（任务分解 → 场景化对话生成 → 质检），显著降低长程 benchmark 的构建成本。

## checkpoint 机制
采用**分解式评价**：每个长程任务拆成若干 `subtasks`（子任务 JSON），每个子任务有独立 `evaluation` 字段（期望的文件状态/答案），整体完成度由子任务通过情况汇总。子任务即中间卡点，但更强调"信息是否被正确回忆并使用"。

## 采样内容
- `task_1-1/subtasks_plus/0.json、1.json`：任务 1-1 的两个子任务定义（含 `task`、`evaluation`、触发该子任务的 `query_sentence`）。
- `task_1-1/chat_histories_plus/`：两天的历史会话记录（任务信息就埋在这些多日对话里）。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
展示了"**长程 = 时间跨度 + 上下文碎片化**"的另一维度（不止步数多），以及用多智能体流水线批量造长程任务的可行工程路径。
