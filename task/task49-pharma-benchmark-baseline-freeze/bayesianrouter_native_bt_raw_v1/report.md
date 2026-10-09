# BayesianRouter native BT prior unified raw receipt v1

日期：2026-09-30  
状态：开发阶段 baseline 合同收据；独立重放通过；不进入 confirmation。

## 为什么另建这条收据

上一条 `bayesianrouter_unified_raw_v1` 误用了 Matched receipt 的
`direct_prior`，虽然实现可重放，但不能叫作 native BayesianRouter prior。该目录
保留为 scope 失配现场，不作为 C8 主证据。本收据改用已核验的 BT+CLS checkpoint
中的 `score.weight` 作为 provider prior，并用同一 checkpoint 的 `encoder.0` 产生
64 维 molecule representation。它仍是 adapted baseline，不声称完整复现论文中
所有训练细节。

## 固定合同

- 输入为 `fingerprint_diagnostic_v1/inputs.json`：722 个 BBB development molecules、
  33 个 provider、5 个 scaffold-disjoint folds、2 个 protocol slots；
- evaluation 使用原冻结 evaluation indices，反馈只释放 selected provider 的
  system-success reward，delay 为 `{0,1,4}`；
- `frozen_bt_mean` 只用同一 BT prior 的 posterior mean；
- `bayesianrouter_online` 使用 prior variance `.02`、observation variance `1.0`，
  provider-independent rank-one Gaussian update 和固定 seed
  `20260926 + 100*fold + 10*seed + delay`；
- 不读取未选 provider 输出，不打开 confirmation；故障 reward 为 0，unknown 按
  stop-batch 语义处理。

## 证据

权威目录：`experiments/logs/task49_bayesianrouter_native_bt_raw_20260930/full_v1/`。

- 60 streams（2 methods × 5 folds × 2 seeds × 3 delays）；
- decision/terminal/feedback 各 8,664；
- native checkpoint manifest 20/20，checkpoint 和 snapshot hash 均进入 manifest；
- 独立 strict verifier `verify_bayesianrouter_native_bt_raw_v1.py` 通过 60/60 streams、
  8,664/8,664 decision-terminal-feedback triples，逐次重算 Thompson action、terminal cache output、reward、
  due/release clock、posterior state hash、版本、菜单/read-set/事件身份和 root summary；
- strict verifier receipt：`full_v1/verification_strict_v2.json`；source provenance 从声明 commit 的 git tree 校验，当前工作区不作为证据；
- 当前 receipt 的 source commit：`445611a6b69a8599ca810253595e0033dcf79285`；
- events SHA-256：`9d86adf20f41b895d5f91f010b57b2b6647060431a2d3694d09f32a4a3b1870e`；
- verifier 输出：同目录 `verification_strict_v2.json`，状态 `verified`；文件
  SHA-256：`387e5926e431103d39fb516d84f07f5caf989a09eb6b6fa1b745f7c86612fb98`。

## 描述性结果

每个值为 10 个 fold/seed streams 的平均 selected accuracy；不作最终显著性结论。

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| frozen BT mean | 0.891355 | 0.891355 | 0.891355 |
| BayesianRouter online | 0.888587 | 0.889272 | 0.890675 |
| online − frozen | −0.277 pp | −0.208 pp | −0.068 pp |

这组 native-BT 结果与旧 development 主表的数量级一致，但由于初始化来源已被
严格区分，不能把 common-direct-prior receipt、旧 A800 receipt 和本 receipt 静默
合并为一组。当前结果仍说明该 adapted Bayesian posterior 在固定 BBB 菜单上没有
超过自己的 frozen prior，也没有超过 fit-only Best Fixed `0.9155`。

## 解释边界

这条收据关闭 BayesianRouter 的一项统一 raw-run/recovery 缺口：native BT prior
来源、feature/checkpoint lineage、延迟反馈和逐事件状态都可复核。它不关闭
provider training membership、hERG/AMES endpoint equivalence、Laya/GraphRouter
范围、动态自然漂移或冻结 confirmation；也不把 adapted implementation 写成原论文
的完整复现。
