# task27 — T2/T3 Stationary + Passive New-Arm Baseline Replay

> 开始：2026-09-21
> 对应：实验矩阵 v2 的 T2/T3/F1/T4 前置 baseline
> 状态：⚡️ 进行中

## 目标

不设计 Ours。先把 published/canonical baselines 在冻结 protocol 上跑通，回答两个问题：
1. fixed pool 时，contextual online baseline 能到什么水平；
2. 一个零历史新 arm 到达时，经典方法的 cold-start penalty 到底多大。

## 主 benchmark

- LLMRouterBench Performance：20 arms，11,481 prompts。
- ASlib SAT12-ALL：31 solver arms，1,614 instances。
- BBB：等前两者 protocol 跑通后作为 scientific hard stress。

## 冻结 new-arm protocol

- history / calibration / test 分离；
- held-out new arm 在 history 中 outcome 完全隐藏；
- incumbent arms 可以用 history outcome 初始化；
- test 阶段每轮只 reveal selected arm reward；
- 每个 arm 轮流作为 new arm；
- 主结果不筛掉“新 arm 很差”的 arrival，另报 new-arm oracle contribution 分层。

## 第一批 baseline

- Best Fixed Incumbent
- Thompson Sampling
- LinUCB
- LinTS
## 评价量

- post-arrival regret
- normalized regret
- Oracle Gap Closed
- native task quality
- new-arm selection rate
- new-arm oracle contribution
- new-arm-best rate
- beneficial / harmful new-arm selections

## Representation

LLMRouterBench：history-only TF-IDF -> TruncatedSVD 32 + intercept。
ASlib：official feature values -> history-only median impute / standardize -> 32D PCA（若有效维数不足则保留原维）+ intercept。
BBB：Morgan fingerprint -> scaffold split；后续接入。

## 关键边界

B=0 时没有 tool-side competence metadata，new arm specialization 不可识别；因此 B=0 只衡量 prior/exploration，不作为“识别能力” superiority claim。
真正的 few-history 曲线从 B=1/4/8/16/32 开始。

## 第二批 baseline

MTSS / UCB-AA / ContextRoute 在 core replay 验证无 bug 后接入。

## 产物

- src/probe_headless/t3_coldstart_baselines.py
- data/tool_results/divergence/t3_*_baselines.json
- 本目录 report.md