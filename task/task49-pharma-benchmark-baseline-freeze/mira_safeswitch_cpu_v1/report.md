# MIRA-SafeSwitch CPU replay v1

日期：2026-09-29  
状态：CPU development candidate replay；不进入 confirmation、主 benchmark 或 A800。

## 这轮要验证什么

MIRA-SafeSwitch 保持 fit-only direct provider prior 不变，只用已经释放的 selected feedback 做 one-sided veto：观察到 provider runtime failure 后暂时阻断；有效 0/1 feedback 达到 $n_{min}=20$ 后，用 Beta error upper bound `delta=.05, tau_fail=.20` 阻断高风险 incumbent。未选 provider 的输出、当前真值、post-call confidence 和 version flag 都不进入 selector。

## 协议

- authoritative BBB development 输入，5 个 scaffold folds、2 个 deterministic protocol slots、33 providers；表示为冻结 BT/CLS hidden-64。
- 场景：`no_event` 原始冻结 panel；`controlled_event` 沿用已登记的 provider-version degradation（每 fold fit-only Best Fixed provider 在 evaluation 中点后变成 failure）。
- 方法：Frozen direct、MIRA-SafeSwitch、matched Online-SGD；delay `{0,1,4}`。
- 共 180 条 stream（2 场景 × 3 方法 × 5 folds × 2 slots × 3 delays），每条 decision/terminal/feedback 与 selected-only 反馈时序一致。
- 未使用 GPU、LLM 或新工具预测。

## 结果

从原始事件独立聚合，accuracy 是每个 decision 的 system success（失败记 0）。

| 场景 / 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| no-event Frozen | 0.9017 | 0.9017 | 0.9017 |
| no-event MIRA | 0.8976 | 0.8962 | 0.8962 |
| controlled-event Frozen | 0.8699 | 0.8699 | 0.8699 |
| controlled-event MIRA | 0.8941 | 0.8900 | 0.8844 |

MIRA 相对 Frozen 的 fold/slot 配对差值：

- no-event：`-0.0042 / -0.0055 / -0.0055`（delay 0/1/4）；
- controlled-event 事件后 suffix：`+0.0481 / +0.0399 / +0.0303`。

controlled-event 中，目标 provider 在第一次合法 failure release 之后的选择率为 `0.0`（三个 delay 的均值），说明 veto 确实发生；delay 增大时事件后恢复幅度下降。no-event 的小幅损失来自冻结 panel 中的自然 provider failure 被立即阻断，提示需要区分瞬时 failure 与持续 version degradation。

## 独立核验

独立 NumPy/scipy replayer 不导入生产 MIRA policy，而是从 fit-only direct snapshot、feature snapshot、terminal/release 顺序重新计算 masked scores、Beta 状态、block reason、version 和每次 action。180 条 stream 全部通过；其中 60 条 MIRA stream 逐事件重放，另外各方法的合同事件也完成 conservation 检查。

篡改前的收据：

- 原始事件：`experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v1/events.jsonl`，SHA-256 `ba49c197b8f743e1beae1f58e8b00c16e5406ae6ed39415b6b0fe261d5132ac8`。
- 汇总：`summary.json`，SHA-256 `5fc6d30c293498c57bf8b047965627467b604cc07459a3e38cd37ce8a783d834`。
- 独立核验：`verification.json`，状态 `verified`。
- 独立分析：`analysis.json` / `analysis.md`。

## 这轮结论和下一步

这轮支持一个窄的机制判断：在预先注入的 provider version degradation 中，selected failure 足以让 MIRA 安全地离开已经被观测到的 incumbent，并在三个 delay 下改善事件后 suffix。它没有证明自然运行一定有 drift，也没有超过 Best Fixed 的主 benchmark 结论。

当前 bug 已定位：对单个瞬时 runtime failure 立即永久阻断，在 no-event 静态 panel 上会损失约 0.4–0.6 个百分点。下一版先做一个最小修复——连续两次 selected runtime failure 才阻断——并在相同 180-stream 合同上预先固定 no-event ROPE、event suffix 和首次 failure 后切换阈值；若不能保留事件恢复同时收回 no-event 损失，则停止这条 SafeSwitch 变体。
