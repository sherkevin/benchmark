# USACO — 采样说明【★核心匹配：单 agent + 一次输入 + 测试点部分给分】

## 论文与来源
- 论文：*Can Language Models Solve Olympiad Programming?*（Princeton，arXiv:2404.10952）
- 代码：https://github.com/princeton-nlp/USACO ｜ 数据：HF `princeton-nlp/USACO`（LFS）
- 榜单：**HAL（Holistic Agent Leaderboard, Princeton）在用**，https://hal.cs.princeton.edu/usaco
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是。
- 一次性输入：是（题面一次给全，agent 产出代码，无交互）。
- hard 中间 checkpoint：**是——每题多个官方测试点，通过数即部分给分，纯执行判定**。

## Benchmark 是什么
307 道 USA Computing Olympiad（USACO）竞赛题，分 Bronze/Silver/Gold/Platinum 四难度层。每题配官方高质量单元测试、参考代码。agent 写程序，在沙箱里对多个测试点评测。是主流榜单常用的"硬核编码长程"项。

## checkpoint 机制（本项目关注点）
关键在评测脚本 `evaluation/metrics.py`：
- **`fraction_tests_passed`**：一道题有 N 个测试点，得分 = 通过的测试点数 / N——**这就是天然的部分给分/中间卡点**。低效或只对一半情况的代码能拿到部分分（正如 USACO 真实赛制："前几个测试点可拿部分分"）；
- `pass_at_k` 系列：k 次尝试的通过率；
- 判定完全 hard：沙箱跑测试点，`usaco_batch_judge.py` 批量执行，无 LLM。

## 采样内容
- `judge_usaco_batch_judge.py`：批量判题器原文（如何对每个测试点执行、比对输出）。
- `evaluation_metrics.py`：全部指标函数（`fraction_tests_passed` / `pass_at_k` 等）。
- `REPO_README.md`：官方说明。
- 注：题目与测试点数据在 HF（LFS，gated），此处采样评测机制文件。

## 对我们的参考价值
"**一题 N 个测试点、通过比例即过程分**"是 competitive programming 领域最成熟的 hard 部分给分范式，且被 HAL 主榜采用。虽测试点非"语义中间步骤"，但它证明了 hard 部分给分在硬核长程编码上的标准做法。可与 SciCode 的"子问题级测试"互补：SciCode 卡语义步骤，USACO 卡覆盖度。
