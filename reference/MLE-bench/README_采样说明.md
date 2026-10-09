# MLE-bench — 采样说明【榜单代表：Kaggle 竞赛，奖牌阈值分级（对照：终态无中间卡点）】

## 论文与来源
- 论文：*MLE-bench: Evaluating Machine Learning Agents on Machine Learning Engineering*（OpenAI，arXiv:2410.07095，ICLR 2025）
- 代码：https://github.com/openai/mle-bench
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是。
- 一次性输入：是（竞赛说明+数据一次给全，agent 自主完成建模、产出提交文件）。
- hard 中间 checkpoint：**否——按最终提交在 Kaggle 排行榜上的名次给奖牌等级，无中间卡点**。作为"榜单主流范式"对照收录。

## Benchmark 是什么
75 个精选 Kaggle 竞赛，考察 agent 端到端 ML 工程能力（读数据、特征、训练、调参、产出 submission）。是 OpenAI 官方 agent 长程能力主评测之一，被众多前沿模型报告引用。分 Low/Medium/High 复杂度三档（`split_low_22comps.txt` 是低复杂度 22 题清单）。

## checkpoint / 评分机制
**分级但非中间卡点**——这是与本项目需求的关键区别：
- 每个竞赛（采样 `example_petfinder-pawpularity-score`）配 `config.yaml`（`awards_medals: true`、奖牌阈值）、`grade.py`（判分函数，如 RMSE）、`leaderboard.csv`（真实人类选手排行榜）；
- 评分：agent 的 submission 用 `grade.py` 算分，再对照 `leaderboard.csv` 换算成 Bronze/Silver/Gold **奖牌等级**；
- 判定 hard（数值指标比对），但**只作用于最终提交**，中途建模过程无卡点。

## 采样内容
- `example_petfinder-pawpularity-score/`：一个竞赛的 `config.yaml` + `grade.py` + `leaderboard.csv`（奖牌阈值就藏在真实排行榜分位里）。
- `split_low_22comps.txt`：低复杂度 split 竞赛清单。
- `REPO_README.md`：官方说明。
- 注：竞赛数据集大（需 Kaggle 下载），此处采样机制与配置文件。

## 对我们的参考价值
两点：(1) 作为"榜单主流=终态奖牌分级、无过程卡点"的代表，界定了我们要补的空白；(2) 其"用真实人类排行榜分位做分级阈值"的思路，可迁移为 checkpoint 的难度分层依据。奖牌分级是"结果分层级"而非"过程分层级"——正是我们要超越的地方。
