# PILOT offline prior fit v1：适配 smoke

日期：2026-09-27

## 完成内容

新增 `src/probe_headless/pilot_offline_fit.py`，按预登记协议实现两阶段 `PILOT-adapted` 先验：

1. fit 区 provider-conditional query triplet projection；
2. 冻结 query projection 后，使用所有 correct–wrong provider pairs 学习 provider vectors；
3. 用 fit 区 provider accuracy 的倒数初始化 online ridge precision。

它没有使用 evaluation 标签，也没有复制原论文的文本 preference、hard-negative cost 或 online cost policy。正式表格中必须把它标成医药任务适配版，不能叫官方复现。

## CPU smoke

使用 frozen fingerprint input 的 fold 0 fit 区（444 个分子、33 provider），100 epochs、64 维 projection/provider prior：

- triplets：12,550；correct–wrong pairs：33,234；
- projection final loss：`6.667888e-05`；provider final loss：`0.568319`；
- fit pair accuracy：`0.852531`；
- prior 数组形状：projection `64×1024`、provider vectors `33×64`、precision `33`。

结果文件：`experiments/logs/task49_pilot_offline_fit_20260927/run_v2/result.json`，先验压缩文件 hash：`f30595e5eec1015f20e7ad33f78eb885046e37f4b13a0455cc6e6dc0d501f753`。同一 seed 重跑后 `prior.npz` 逐字节一致，结果还记录了源码 hash。

## 真实缓存接口 smoke

把 fit-only prior 接入已有 `PilotOnlineKernel`，在 fold 0 evaluation 的前 64 个真实 BBB 分子上跑 delay `0/1/4`：每条流 64 decision、64 terminal、64 feedback，最终版本均为 64，状态有限。独立恢复器重新核对 cohort、provider binding、delay、版本和 prior hash 通过。

证据目录：`experiments/logs/task49_pilot_prior_adapter_20260927/run_v2/`；恢复结果：`recovered_v1/summary.json`。

这只是 baseline 适配和反馈合同验证，没有做完整 development 效果估计，没有新的 provider/API 调用，没有 Nebula 任务，confirmation 保持关闭。

## 下一步

将该 prior 接入统一 development runner，先和 Frozen/Online-SGD/Replay-SGD/BayesianRouter-style 做同一输入、同一 delay 的 CPU 小回放，检查实际选择接口和超参记录；通过后再决定是否占用一条 A800 做完整 baseline 诊断。
