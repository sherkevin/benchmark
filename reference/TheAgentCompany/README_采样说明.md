# TheAgentCompany — 采样说明

## 论文与来源
- 论文：*TheAgentCompany: Benchmarking LLM Agents on Consequential Real World Tasks*（CMU 等，arXiv:2412.14161，NeurIPS 2025 Datasets & Benchmarks Track）
- 官网：https://the-agent-company.com/
- 代码/任务仓库：https://github.com/TheAgentCompany/TheAgentCompany
- 本地采样下载通道：GitHub Contents API（`api.github.com`，raw 模式），2026-07 抓取

## Benchmark 是什么
模拟一家软件公司（自托管 GitLab、OwnCloud、RocketChat、Plane），175 个职场长程任务（软件工程、PM、HR、财务、行政等）。Agent 需要浏览网页、写代码、执行命令、和模拟同事（NPC）聊天，单任务通常需要几十步。

## checkpoint 机制（本次调研的核心关注点）
**这是目前最典型的"显式 checkpoint 节点"设计**：
- 每个任务附带 `checkpoints.md`，把长任务切分成若干个中间卡点，每个 checkpoint 有分值；
- `evaluator.py` 用确定性代码（查文件、查 GitLab/RocketChat 状态等）验证每个 checkpoint 是否达成，部分 checkpoint 用 LLM 做简单抽取/分类判断；
- 计分采用部分给分（partial credit）：`Score = 0.5 * full_completion + 0.5 * (earned_points / total_points)`，既奖励全程完成，也度量"走到一半偏没偏"。

## 采样内容
- `REPO_README.md` / `docs_EVALUATION.md`：官方评测文档（checkpoint 评分规则原文）。
- `sample_tasks/`：3 个不同角色的完整任务定义，每个含三件套：
  - `task.md` — 任务指令（给 Agent 看的）
  - `checkpoints.md` — 中间卡点及分值定义（如 admin-arrange-meeting-rooms：checkpoint1 = 答案文件 `/workspace/ans.txt` 内容正确得 1 分；checkpoint2 = RocketChat 上把正确答案发给同事得 1 分）
  - `evaluator.py` — 对应每个 checkpoint 的程序化验证函数（`grade_checkpoint1/2...`）

## 对我们的参考价值
checkpoint = "任务里程碑 + 可程序化验证的谓词 + 分值"。验证信号来自环境终态/中间态（文件、服务端数据），而不是对 trace 文本的主观判断，客观且可复现；适合作为我们构建"长程任务中间卡点"的第一参考范式。
