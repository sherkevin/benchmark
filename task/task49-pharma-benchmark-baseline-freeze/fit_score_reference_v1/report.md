# 冻结分数基准恢复结果

日期：2026-10-07。状态：完成；仅 development 的原参数重建与固定历史诊断。

本轮按 [experiment_card.md](experiment_card.md) 执行了一次真实 CPU checkpoint forward 和五折原 frozen head 重建。全部既有 Frozen logits 和 fit rates 精确对齐，随后才解释分数组成项；没有使用日志逆推缺失 fit 值。

## 重建是否成立

- 四个原 checkpoint 对 722 个 development 分子执行原生 forward，保存 2,888 个真实概率；5 折 frozen heads 使用 C=1、liblinear、class_weight=balanced、max_iter=2000、random_state=20261006。
- 原双 seed、全部 delay 的 30 条 Frozen 流、4,332 次决策全部逐项重建；logits 最大误差 0，fit rates 最大误差 0，均低于 1e-8。先前封存的 2,166 条决策及其 Frozen scores 另做对齐。
- 配置、命令、依赖/源码/卡/原数据/四 checkpoint hash 均在 inference 前落盘。正式运行 7.272 秒，在 180 秒预算内；0 API、0 GPU、没有 provider 重训或新超参搜索。
- 使用原 `load_checkpoint_predictions`、`fit_prior`、`selector_feature`；只调用这三个 helper，不调用包含 confirmation 的 main()，没有读取 confirmation manifest 或数据。输入中的旧 provider 输出字段不进入本轮重建。

## 20 个 fit 参考值

这里的 m 是该折 frozen head 在自身 fit 样本上的 sigmoid 分数均值；r̄ 是四个真实 provider 在同一 fit 集上的正确率。它们是 fit 参考，不是 OOF 校准或泛化成绩。

| fold | provider | n fit | m | r̄ | m−r̄ |
|---:|---|---:|---:|---:|---:|
| 0 | ecfp_logreg | 444 | 0.53969522 | 0.81756757 | -0.27787235 |
| 0 | ecfp_mlp | 444 | 0.54014351 | 0.80405405 | -0.26391055 |
| 0 | ecfp_random_forest | 444 | 0.54218525 | 0.82432432 | -0.28213908 |
| 0 | graph_message_passing | 444 | 0.54017289 | 0.75000000 | -0.20982711 |
| 1 | ecfp_logreg | 483 | 0.53430907 | 0.82401656 | -0.28970750 |
| 1 | ecfp_mlp | 483 | 0.52765346 | 0.79296066 | -0.26530721 |
| 1 | ecfp_random_forest | 483 | 0.53206922 | 0.79503106 | -0.26296183 |
| 1 | graph_message_passing | 483 | 0.53379379 | 0.71014493 | -0.17635114 |
| 2 | ecfp_logreg | 492 | 0.53882826 | 0.79471545 | -0.25588719 |
| 2 | ecfp_mlp | 492 | 0.53271907 | 0.76626016 | -0.23354109 |
| 2 | ecfp_random_forest | 492 | 0.54217139 | 0.78861789 | -0.24644650 |
| 2 | graph_message_passing | 492 | 0.54314565 | 0.71138211 | -0.16823647 |
| 3 | ecfp_logreg | 483 | 0.53551935 | 0.78674948 | -0.25123014 |
| 3 | ecfp_mlp | 483 | 0.52578151 | 0.75362319 | -0.22784168 |
| 3 | ecfp_random_forest | 483 | 0.52898611 | 0.77018634 | -0.24120023 |
| 3 | graph_message_passing | 483 | 0.52614579 | 0.69358178 | -0.16743599 |
| 4 | ecfp_logreg | 480 | 0.53730230 | 0.80000000 | -0.26269770 |
| 4 | ecfp_mlp | 480 | 0.52635372 | 0.77916667 | -0.25281295 |
| 4 | ecfp_random_forest | 480 | 0.52831140 | 0.78333333 | -0.25502193 |
| 4 | graph_message_passing | 480 | 0.52737076 | 0.71250000 | -0.18512924 |

20 个 m 的范围为 [0.52578151, 0.54314565]；r̄ 的范围为 [0.69358178, 0.82432432]；m−r̄ 全为负，范围 [-0.28970750, -0.16743599]。

## 组成项拆分

原分数组成项精确分为 `Σ(q−m)/(20+n)`（相对 fit 的分数组成变化）与 `n(m−r̄)/(20+n)`（fit 基准差）。分母与上一诊断相同：2,166 次 seed0 决策 × 4 providers = 8,664 个描述性状态项，所有 provider 状态等权。

| 项 | signed mean | mean absolute | max absolute |
|---|---:|---:|---:|
| 相对 fit 的分数组成变化 | 0.01635520 | 0.01890807 | 0.06871866 |
| fit 基准差 | -0.07904718 | 0.07904718 | 0.20068559 |
| 原分数组成项 | -0.06269199 | 0.06269199 | 0.17853843 |

恒等式最大绝对误差为 1.1102230246251565e-16，低于 1e-10。fit 基准差项的平均绝对值大于相对 fit 的组成变化项，且两项在 signed mean 上部分抵消；因此上一诊断的全部负组成项不能归为在线选样组成变化。这是数学分解和本面板描述，不是漂移因果归因。

## 固定历史 shadow 敏感性

保持每次实际决策已观察到的反馈不变，单独移除相对 fit 的组成变化后，动作不同 292/2166（13.48%）；单独移除 fit 基准差后，动作不同 374/2166（17.27%）。

这两个计数可重叠、argmax 对两项存在交互，不能相加为上轮 401 次差异，也不能解释成可加的效果贡献。没有计算任一 shadow accuracy、闭环收益、反事实 reward、CI 或候选排名。

| 分组 | 决策数 | 移除相对 fit 组成项：动作不同 | 移除 fit 基准差：动作不同 |
|---|---:|---:|---:|
| fold0 | 435 | 62 | 76 |
| fold1 | 435 | 79 | 61 |
| fold2 | 432 | 63 | 87 |
| fold3 | 432 | 33 | 86 |
| fold4 | 432 | 55 | 64 |
| D=0 | 722 | 96 | 131 |
| D=1 | 722 | 113 | 113 |
| D=4 | 722 | 83 | 130 |


完整的每折/每延迟/每折×延迟 signed mean、mean absolute、max absolute 和各 provider 汇总保存在 summary.json。722 分子跨三个 delay 重复，fit folds 也重叠，不以 15 流或 8,664 状态项作为独立样本。

## 本轮结论与复用

fit 分数基准与 fit 正确率的偏移在当前公式下可改变实际排序；后续应将“分数校准”与“在线中心化方式”设为分开的因素，使用同一校准 surface 和各自匹配的 Frozen。当前诊断没有证明移除任一项更好，也不能把偏移直接称为自然环境漂移。下一轮校准拟合或闭环对照必须先冻结新实验卡；新的效果确认需要未用于选择方案的数据。

本轮已把可复用输入落盘：722 行四 provider 原生概率、722×65 selector features、5 折原 priors/indices/rates、2,382 行 fit 分数、722 行 OOF 分数、20 个 fit 参考均值与 2,166 条逐决策两项分解。下轮可直接从这些 hash 绑定缓存开始，避免再做无必要的 checkpoint forward。没有改旧日志、生产 helper、已报告 confirmation 或论文超参。

## 产物与验证

- 输出目录：`experiments/logs/task49_fit_score_reference_20261007/v1/`。
- `config.json`、`execution.jsonl`、`run_stdout.txt`：事前配置、逐阶段执行记录、真实进程输出。
- `development_provider_probabilities.jsonl`、`development_selector_features.npy`：真实 forward 概率及原 selector features。
- `prior_cache.json`、`fit_reference_means.json`、`fit_scores.jsonl`、`oof_scores.jsonl`：五折原参数与 fit/OOF 分开缓存。
- `composition_decisions.jsonl`：2,166 条原历史分解及影子动作；`summary.json`：分组结果和产物 SHA-256。
- `tests_attempt01.jsonl`、`tests_attempt01_stdout.txt`：5 项纯数学工程测试全部通过（卡内例子、零反馈、有符号抵消、逐项 shadow、平局规则）。这些夹具不作科学结果。
- 命令：`/Users/jingwu/work/benchmark/.srcprobe/beir_venv/bin/python -B -m src.probe_headless.recover_mira_fit_score_reference_v1`。
- Git：`c9f7b2475453c47e1f0b86a456aa1387b1c9f4c9`；开始 `2026-10-07T03:47:52.364998+00:00`，结束 `2026-10-07T03:47:59.833220+00:00`。
- Python 3.9.6；依赖版本：`{"numpy": "1.26.4", "rdkit": "2025.9.2", "scikit-learn": "1.6.1", "torch": "2.8.0"}`。
- 新脚本 SHA-256：`79249a00913dda572027fb3ae20e9d08cbd3a45d41026d8702dca2489f110e0c`；卡 SHA-256：`9ee597ca0438009a0647d1c6c6d85521ec7b3e269029cf38c3e0864ac2ea221c`。
- summary.json SHA-256：`ad5650d68e8e8d7f9ca497f2fa93c20abd6a4dfc41ed5711ebe83ba7391176c0`。

缓存独立读取说明见 `cache_manifest.json`：逐文件 SHA-256、JSONL envelope/schema、native BBB probability 与 selector score 的语义区分、features 行序及 fold indices 映射均已记录。

| 缓存 | SHA-256 |
|---|---|
| `development_provider_probabilities.jsonl` | `e1dabd7ff222b21b1319b73b52d2c47d3a5e861bd3d7231a4e38e9269528bf8b` |
| `development_selector_features.npy` | `274df04cd35823596a9dde92a20bd7a4d47bc20884379804368cb84378559dcd` |
| `prior_cache.json` | `7e76743f5258df4714b23528acce55bdcb926ad429bb40b36bfc9c281a475206` |
| `fit_reference_means.json` | `37bd92618251923f5f955741f42f43d7af1d09b0949194b6bd632db2145bb222` |
| `fit_scores.jsonl` | `c337fc2114729e48a5a990002d1799cadad150208b392e6fac8189d6b7865ee0` |
| `oof_scores.jsonl` | `00ca53857f96415dc0d6e3215e84571dbb7c96b456767c1abe48909aadafb673` |
| `composition_decisions.jsonl` | `f44c695b60d4191091dd7d483cf0c8ebd0c320d02cbc76c2bf615691f7fe5c61` |
| `cache_manifest.json` | `fbd94674b3f38dae07045cd8aefdc32f906c852c33998f2e46b98292311e7ecf` |
