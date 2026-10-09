# MIRA relative-reliability v1：A800 full-matrix 计划

日期：2026-09-30
状态：只读审查完成；尚未提交 Nebula 任务。

## 1. 当前判断

CPU development gate 已经通过：360 条 stream、每条候选 stream 的 decision/terminal/feedback 守恒，relative-reliability 的独立 NumPy/SciPy 回放通过。现有仓库没有可以直接作为 Nebula `--entry` 的 full-matrix 入口；`run_mira_relative_reliability_cpu_v1.py` 依赖本地 `run_v2_fixed` 快照，且明确是 CPU/no-A800 版本，不能直接拿去声称 A800 full matrix。

因此下一步需要新增一个固定入口，再提交**一条** 1 worker × 1 A800 任务。入口应读取并 hash 校验 CPU gate 已封存的 direct-prior/feature snapshots，然后执行六种策略的完整矩阵。不要在本批重新用 GPU 重算 feature 或 direct head：CPU gate 的 NumPy float64 计算和 GPU float32 计算不保证逐位一致，重算会偷偷改变 candidate。snapshot 本身来自已经核验的 A800 BT encoder checkpoint，来源链在日志中保留。

## 2. 需要先固定的资产

### 输入

- `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- 722 个 development 分子、33 个 provider、5 个 scaffold-disjoint outer folds；不加入 confirmation 数据。

### 代码基线

CPU gate 所在提交：`dd66c1efb27b4df95050d8a8eedce52c802141e9`。它还没有 A800 entry，不能直接作为本次提交 commit；新增入口和恢复器完成后，必须形成新的已推送 commit，并在 Nebula 参数和日志中同时写入同一个 40 位 commit。

当前候选代码的 SHA-256（用于新入口的 source manifest）：

| 文件 | SHA-256 |
|---|---|
| `src/probe_headless/mira_relative_reliability.py` | `0857aece056a1e3a790c57517a18f5269f0492d886d54e94bc7c01523ddd8ffe` |
| `src/probe_headless/mira_safeswitch.py` | `2d14532b02f607b8c321e6b8cd010a47c587209638a0f3afef5bd6e769a0836c` |
| `src/probe_headless/run_mira_veto_ablation_cpu_v1.py` | `21c6074c009f937cdb32d9876c793f874b86eccfd6d7420d22892c34c426fea5` |
| `src/probe_headless/run_controlled_version_event_v1.py` | `077494ee92e41c715ad740cc15d26da15a671070fe61fb3871a1173000369244` |
| `src/probe_headless/run_structured_cpu_effect_v1.py` | `2173b33b85e2a8ba464190d4a04e29347f16c1e0ad1b0fd722e6c1e841a1b427` |
| `src/probe_headless/run_joint_success_cpu_effect_v0.py` | `e1d11a85f17ab733e11bb2117b38f19f6a3ff49ef494e48812c7391da7670b68` |

入口还必须记录自身和所有实际 import 文件的 SHA-256；不能只记录 entry 文件。

### 固定的 CPU-gate snapshots（本批真正的 task input）

本批使用 `experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v2_fixed/` 中已经通过独立 verifier 的 20 个 snapshot：10 个 `fit_snapshots/fold{fold}_seed{seed}.npz` 和 10 个 `feature_snapshots/fold{fold}_seed{seed}.npz`。它们包含 CPU gate 的 direct prior 与 evaluation feature，避免 GPU 重算造成数值漂移。

提交前把目录上传到：

`oss://ai4sci-develop-storage/jingwu/benchmark/task49/mira_relative_reliability_snapshots_v2_fixed/`

同时上传该目录的 manifest；入口启动后先顺序复制到容器本地，再逐文件 hash 校验。挂载 OSS 上直接反复读取小文件不作为运行路径。

本地 sealed config 的 SHA-256 为 `f271854938375251b46d8f39c94f44d8180344d805c6a18297f55910d73673ca`；其中列出的 20 个 snapshot hash 必须原样写入远程 config。列表见该 config 的 `snapshot_sha256` 字段，不能以目录 mtime 或文件大小替代 hash。

为避免远程任务引用本机绝对路径，20 个文件及 hash 在这里再固定一次：

| 文件 | SHA-256 | 文件 | SHA-256 |
|---|---|---|---|
| `fit_snapshots/fold0_seed0.npz` | `c533127d24fa170da91d695225e8fe88299b582b486b66413fd14f9c9d2b1d0e` | `feature_snapshots/fold0_seed0.npz` | `5eaf8ba57758e466855e5b88866fff862da90757f426a072927f76463f92c4cb` |
| `fit_snapshots/fold0_seed1.npz` | `a2b6f39fbc94fa2d029ec619ea26e79cc821027bc1e7851fe4a0f8599fd375f5` | `feature_snapshots/fold0_seed1.npz` | `2c45c641628369307b790badf7fa25186486ddd1f57e6a15b383e0a85ebb0ebb` |
| `fit_snapshots/fold1_seed0.npz` | `454f846f0de68090f6318071bdbf4c57d094cd827acaab0a670f9fa8b6d7ff5e` | `feature_snapshots/fold1_seed0.npz` | `76b6c1d50eef24e3f3e1ca6a79bfee3e898ef6ec189082753f827fb894994e71` |
| `fit_snapshots/fold1_seed1.npz` | `999c2fdac076d1652dc6d5280060d0392c4a508654f10c770f687b4a31d3d292` | `feature_snapshots/fold1_seed1.npz` | `f7913480a8b85a2e96b85f2c4c26aac8c58648494534f3975526f1329d30a649` |
| `fit_snapshots/fold2_seed0.npz` | `eecea454aff559fa2523ceef6b8663004e7c9e883922adaa60b2ecad51624326` | `feature_snapshots/fold2_seed0.npz` | `12d29cd5fef05e8c56a0abd810451410121e765c6435d1ffe87b11e19424362a` |
| `fit_snapshots/fold2_seed1.npz` | `9e5dd403fe287337ece702579f3258ac810a8007ffa6bb884fef91428973b80d` | `feature_snapshots/fold2_seed1.npz` | `3efca9b7648e0c3805781a8440d7cd8cad8c9ef8b29a731b46ecde95c126b415` |
| `fit_snapshots/fold3_seed0.npz` | `79b16f3a9d33f0826663e4f3b3aa1a09a511b994cd15b1e21bbb3be88137de36` | `feature_snapshots/fold3_seed0.npz` | `601e83b5f8f9b637aa6486954ffa314304d2072e563a33b8b954d769cf076c97` |
| `fit_snapshots/fold3_seed1.npz` | `01741b15b5f81cbc9b99760fc567f92d87538bb2c437f90b5d8de880843dcfbc` | `feature_snapshots/fold3_seed1.npz` | `6e02515a22729852a010fc84b48d3ca661e0fd212a002b491e38dc27686890bc` |
| `fit_snapshots/fold4_seed0.npz` | `d59b78610ed2862dcdaea4855cf7d992ea57edd8ee5e4a11c6c7025995870576` | `feature_snapshots/fold4_seed0.npz` | `fbd4fb452718e56c37f5c3cfc4db3b2deeef1a43042a1f6e7f3faf6ca60e17cd` |
| `fit_snapshots/fold4_seed1.npz` | `78b74b3df0fbf07da0296d439cf71da1d8006d253bbdc374e8ed116591b45c5c` | `feature_snapshots/fold4_seed1.npz` | `fdd9fd8dc0bdb2a20894a2768ef0131cff0d84aa3ea546af57cac9b516bc467d` |

这些 snapshot 的上游来源是已恢复的真实 A800 fingerprint 任务 `recovered_v5/checkpoints` 的 BT encoder 与 CPU gate direct-head 拟合；上游 checkpoint 只用于 provenance，不作为本批运行时输入。

## 3. 新入口必须实现的运行契约

建议文件名：`src/probe_headless/nebula_mira_relative_reliability_full.py`。

1. 要求真实 `NVIDIA A800`，设置 `CUBLAS_WORKSPACE_CONFIG=:4096:8`、关闭 TF32、记录 Python/PyTorch/NumPy/SciPy/GPU 版本；依赖安装只允许使用平台内网源。
2. 从挂载的 sealed snapshot 目录复制并验证 20 个 `fit_snapshots/` 与 `feature_snapshots/`；验证内容、shape、dtype、entity 顺序和 config 中的 SHA-256。不能用 GPU 重算或重新拟合。A800 任务只负责同一 frozen candidate 的 full-matrix replay，并记录真实 GPU 型号。
3. 在远程事件日志中写出 snapshot manifest（raw hash、shape、dtype）和输入目录 hash；完整日志恢复器必须能把它们重新落地，供独立 replayer 使用。
4. 策略固定为：`frozen`、`streak_only`、`relative_reliability`、`availability_soft`、`full`、`matched_sgd`；参数只能使用 CPU gate 注册值：`n_min=20`、`tau_fail=.20`、`delta=.05`、runtime failure streak `=2`、matched SGD `lr=.05`。
5. 反馈严格在 `decision_tick + 1 + delay` 释放，delay 为 `0,1,4`；只读取 selected provider 的 output/status，runtime failure 不进入 correctness ledger；controlled event 仍对 fit-best provider 在 evaluation 中点后注入，policy 不可读取 event flag。
6. relative-reliability 的每条 decision 记录完整 `base_scores`、`reliability_factors`、`posterior_error_means`、`soft_scores`、masked scores、provider ID、version 和 feature identity；其他五种方法只需记录 provider、version、timing 和 selected outcome，避免把 33 维向量重复写入日志。terminal 不泄露 truth/chemical correctness；feedback 记录 release/version/state。
7. 不调用 LLM、外部 API 或新工具预测；confirmation 保持关闭。

## 4. 矩阵和硬性事件计数

完整矩阵为 `5 folds × 2 seeds × 2 scenarios × 3 delays × 6 methods = 360 streams`。预期：

- decision = terminal = feedback = **51,984**；
- `stream_complete` = **360**；
- fit/direct-prior snapshot = **10**，feature snapshot = **10**；
- relative-reliability 候选子集为 60 streams、8,664 decision/terminal/feedback；必须用现有独立 verifier 逐事件重放；
- 任一计数不一致、snapshot/hash 不全、A800 检查失败或日志截断，整批作 engineering failure，保留原始证据并停止解释。

## 5. 计划中的提交命令（仅 dry-run 模板）

下面的 `CODE_COMMIT` 只能替换成新增入口已推送后的 SHA；当前 `dd66c1ef...` 没有该入口，不能提交。先做 `doctor` 和 dry-run，不带 `--yes`：

```bash
env -u HTTP_PROXY -u HTTPS_PROXY -u ALL_PROXY \
    -u http_proxy -u https_proxy -u all_proxy \
    tools/nebula/neb doctor --mount_oss

CODE_COMMIT=<new_pushed_full_commit>
env -u HTTP_PROXY -u HTTPS_PROXY -u ALL_PROXY \
    -u http_proxy -u https_proxy -u all_proxy \
    tools/nebula/neb submit \
    --job_name jingwu_task49_mira_relative_full_001 \
    --commit "$CODE_COMMIT" \
    --entry src/probe_headless/nebula_mira_relative_reliability_full.py \
    --mount_oss --oss_bucket ai4sci-develop-storage \
    --oss_endpoint oss-cn-zhangjiakou.aliyuncs.com \
    --workers 1 --gpus_per_worker 1 \
    --user_params "--input task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json --input-sha256 a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf --snapshot-dir /data/oss_bucket_0/jingwu/benchmark/task49/mira_relative_reliability_snapshots_v2_fixed --code-commit $CODE_COMMIT --folds 5 --seeds 0,1 --scenarios no_event,controlled_event --delays 0,1,4 --methods frozen,streak_only,relative_reliability,availability_soft,full,matched_sgd"
```

提交前只检查一次 queue；不同时创建 smoke 和 full 任务。dry-run 输出、最终 task id、status、完整 all-logs 下载 receipt 和 sanitized archive 都要写进对应 `experiments/logs/task49_mira_relative_reliability_a800_YYYYMMDD/`。

## 6. 完成后的恢复和验收顺序

1. 用 `nebula-cli logs download TASK --all` 取完整 ZIP；不要用 `--tail` 作为结果来源。
2. 用新恢复器先验证 config/source/input/snapshot hashes、事件计数和 gzip/raw hashes，再落地 snapshots 与 `events.jsonl`。
3. 对 relative-reliability 子集运行 `verify_mira_relative_reliability_actions_v1.py`；它必须在新的 snapshot 目录上通过，并保留篡改 score/state 后的拒绝测试。
4. 独立聚合脚本从 raw events 重算所有 360 streams；只在恢复和独立回放都通过后更新 report。A800 结果只能作为固定合同下的 full-matrix evidence，不因一次结果修改参数或 confirmation 集。

## 7. 不可绕过的卡点

- **入口缺失**：不能把 CPU runner 当作 A800 entry；先实现文件入口、file-entry import fallback 和 gitless provenance。
- **先验来源不一致**：不能悄悄改用 BT score head 或 GPU float32 重算；本计划采用已封存 CPU gate snapshots。若改成重新 GPU 训练或重算 direct head，必须另立 preregistration，不能与本批混写。
- **OSS 资产未核对**：20 个 snapshot 必须上传并逐文件 hash 通过；OSS 挂载路径只作为输入，运行复制到容器本地。
- **证据不可回放**：没有 snapshot manifest/chunks、完整日志或独立 replayer，就没有可写进论文的 A800 结果。
