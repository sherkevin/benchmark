# BayesianRouter action replay v1

日期：2026-09-30
性质：对已有 A800 development run 的旁路独立核验，不是新实验。

## 做了什么

正式 recovery 之前已经核对了 selected provider、输入 reward/status、延迟 release、version 和 stream summary；本次补上它没有内置的 action-level replay。核验器从同一份 frozen `inputs.json`、已核验的 BT+CLS checkpoint、原始 recovered events 和固定 seed 公式独立实现 Gaussian rank-one update 与 Thompson draw，不导入在线 kernel 的决策代码。

- input SHA：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- streams：60（5 folds × 2 seeds × 3 delays × frozen/online）
- decisions：8,664
- action mismatches：0
- seed：`20260926 + 100*fold + 10*seed + delay`
- events SHA：`cf66af547cae8d243ae173595fc63faada1d1b1872943b2d50f9547decc13fdf`

## 结论

BayesianRouter development 数字现在有逐次 action 的独立重放收据；之前“正式 recovery 未重放 action”是验证器功能缺口，不是该 run 的最终证据缺失。它仍是一个 adapted negative baseline：action replay 通过不改变 `online_bayesianrouter_effect_result.md` 中的负结果，也不证明 online Bayesian update 有效。

剩余的统一 raw-run 缺口仍存在：旧 JSONL 没有 native `decision_reads`、state hash 和完整 terminal 快照；这些不能由 action replay 反推。它们影响跨方法证据合同的一致性，不否定当前 development 数字。

## 收据

- verifier：`src/probe_headless/verify_bayesianrouter_action_replay_v1.py`
- result：`experiments/logs/task49_fingerprint_online_20260927/action_replay_v1/verification.json`
- result SHA：`8ddc82525cfb3c6492aefc7f0a84a5ad676238e0cb3a1080aaedfcffa5f15f37`
- verifier SHA：`a76dd44dddb75a9aa065ce5ef39f45e19b101b5fbcc122f9e1bb35dfb944048a`
