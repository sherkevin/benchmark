# task30 — T3 Nearest-Baseline Closure

> 日期：2026-09-21
> 状态：⚡️ 进行中
> 上游：task29 Formal T3 challenger-onboarding
> 目标：在设计 Ours 之前，关闭所有与当前 formulation 最贴近的 baseline 风险。

## 1. 本任务只做四条线

1. Action-Centered Thompson Sampling：核验适配 fidelity、协议和参数，形成正式 comparator。
2. PAK-UCB / RFF-UCB：对照官方代码核 adapter，做合理 calibration/tuning，形成正式 comparator。
3. P-LinUCB / P-TS：按 AISTATS 2024 shared affine subspace 方法实现/复现，检验 cross-tool/task transfer 是否已足够。
4. ParetoBandit-style newcomer onboarding：在相同 frozen-incumbent/challenger 协议下实现 brief forced burn-in + UCB / offline-prior warm-start comparator。

## 2. 明确不做

- 不设计 Ours。
- 不扩 benchmark。
- 不启动 T8 LLM RL。
- 不因为某 baseline 难赢就删 baseline。
- 不修改 T1/T2 已冻结的数据与 incumbent policy。

## 3. 完成判据

每个 baseline 必须给出：假设兼容性、适配方式、一手来源/代码、超参来源、LLMRouterBench/ASlib/BBB 结果或明确不适用原因。
正式结果至少报告：
- IOU；
- Challenger Regret；
- beneficial-adoption precision / recall；
- harmful-adoption rate；
- challenger usage；
- B=1/4/8/16/32 的 few-shot 行为（方法适用时）。

只有四条线都关闭后，才能决定 Ours 的算法结构。

## 4. Go / No-Go

若已有最近邻方法在相同信息条件与小 B 下稳定实现正 IOU，则 Ours 必须证明额外、不可替代的增量；若多个最近邻方法仍系统性失败，则从它们共同 failure mode 反推 Ours。

完整结果写入本目录 `report.md`，代码/数据写入现有 `src/probe_headless/` 与 `data/tool_results/divergence/`；task29 只在本任务收口后回链，不并发覆盖。
