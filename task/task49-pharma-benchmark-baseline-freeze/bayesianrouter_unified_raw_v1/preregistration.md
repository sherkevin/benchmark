# BayesianRouter unified raw receipt v1（common-direct-prior control）

状态：开发阶段预注册，2026-09-30。

## 目的

把 common-direct-prior 控制版本放进与 MIRA 相同的原始事件合同。它检验证据可
审计性，不改变已有 BayesianRouter 数字，也不把适配实现称作原论文完整复现。
由于它不是 native BT prior，这条收据不承担 C8 主 baseline 的 lineage 结论；native
BT 版本见 `../bayesianrouter_native_bt_raw_v1/`。

## 固定协议

- 输入：同一 `fingerprint_diagnostic_v1/inputs.json`、722 molecules、33-provider
  menu、5 scaffold folds、2 seeds、delay `{0,1,4}`。
- representation/warm start：当前 MIRA 使用的 frozen BT+CLS 64 维 features 和
  fit-only direct provider prior；由已核验的 Matched unified receipt snapshot 提供，
  不重训、不读取 evaluation labels。这是 common-direct-prior control，不是 native
  BayesianRouter checkpoint migration。
- `frozen_bt_mean`：同一 prior 的 deterministic mean score，posterior 不更新。
- `bayesianrouter_online`：prior variance `.02`、observation variance `1.0`，按
  provider-independent Gaussian rank-one update 和固定 Thompson seed
  `20260926 + 100*fold + 10*seed + delay`；只在 selected feedback 到期后更新。
- selected-only、失败 reward=0、unknown 停止 batch；不打开 confirmation。

## 反证条件

action、terminal cache output、reward、延迟、version、posterior state hash、RNG
state 或 source/input hash 任一不能由独立 NumPy replayer 重算，receipt 即失败。

## 解释边界

这是 adapted BayesianRouter baseline 的统一审计 receipt。它回答“在同一信息和
表示合同下，强现代 selected-only posterior baseline 的动作与状态能否复核”，
不单独证明 MIRA 或 BayesianRouter 的优越性。
