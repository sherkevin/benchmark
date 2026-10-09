# task26 — AAMAS T1 Benchmark Characterization（非 canonical 早期工作单）

> 开始：2026-09-21
> 对应：TODO T-283 / 实验矩阵 T1
> 状态：已由 `../task26-AAMAS2027-T1-benchmark-characterization/` 接管；本文件仅保留早期过程记录。

## 目标

只回答论文最前置的问题：这些 benchmark 是否真的存在可路由空间，而且 context 是否携带可泛化的工具能力信号？

T1 固定输出列：

| Benchmark | #contexts | #arms | reward | Best Fixed | Oracle | Headroom | Simple Contextual Gain |
|---|---:|---:|---|---:|---:|---:|---:|

判据不随结果修改：
1. Oracle > Best Fixed：证明存在 routing opportunity。
2. 严格 split 下简单 contextual selector > Best Fixed：证明 gap 至少部分可学习。
3. 三类 real benchmark family 中至少两类同时满足 1–2，才进入复杂 new-arm method。

## 本轮范围

- BBB Scientific Tool Routing：已开始，必须 scaffold/OOD。
- LLMRouterBench performance setting：下载官方 frozen results，按官方 prompt split。
- BEIR retriever pool：先核对是否能公开复现 LQM 论文使用的 provider outcome；若不能，记录 reproducibility gap，不静默造同名数据。
## 已完成：BBB 第一轮

输入：722 usable molecules，7 tool families；Morgan radius=2 / 1024-bit；5-fold GroupKFold by Bemis–Murcko scaffold。
每个 arm 单独 Logistic Regression 预测 P(tool correct | x)，选择预测正确率最高的 available arm。

实测：
- Best Fixed = ADMETLab = **0.915512**
- Oracle = **0.980609**
- Headroom = **+0.065097**
- Best Fixed 错但其他工具可救 = **47 / 722**
- Simple contextual selector = **0.894737**
- Contextual Gain = **−0.020776**

per-arm correctness predictability AUC：ADMET-AI 0.561；ADMETLab 0.532；B3clf 0.595；DeepPurpose 0.629；FPADMET 0.587；graphB3 0.683；LGBM-BBB 0.599。

第一结论：**BBB 明确有 routing headroom，但当前最简单的 scaffold-OOD selector 没有把 headroom 转成收益，反而比 Best Fixed 低 2.08pp。** 因此不能把“BBB context routing 已经可学”当既成事实。

产物：src/probe_headless/t1_bbb_characterization.py；data/tool_results/divergence/t1_bbb_characterization.json。

## 正在进行

- 官方 LLMRouterBench repo 已拉到 .srcprobe/LLMRouterBench，HEAD c77cb05。
- 官方 HF bundle bench-release.tar.gz = 1,283,503,080 bytes，正在下载；官方 sha256 = b79f8cde1a6f029c2efa663a3a3b6f7748defb22341fe59f328cebef6648c8f1。
- LQM-ContextRoute arXiv source已拉到 .srcprobe/lqm-contextroute-source/，用于核验 BEIR provider 设定。
## 交接更正

本目录是 2026-09-21 开工时重复建立的简版 task 入口；后续 T1 权威记录统一放在 `../task26-AAMAS2027-T1-benchmark-characterization/`。本目录不删除以保留审计痕迹，勿继续双写。
