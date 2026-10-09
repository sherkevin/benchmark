# 33-provider 指纹选择诊断 v1

日期：2026-09-26；完整日志复核补充：2026-09-29
状态：已完成并核验；这是 development 诊断，不是最终确认实验，也不是在线学习效果。

## 这次实验回答什么

在当前真实的 33-provider 任务上，共享 Morgan 指纹和小型选择头能不能学会“哪个具体工具更可能正确”。这一步先检查静态可学习性，再决定是否值得把表示接到动态 JEV；它没有训练 Laya，也没有测试在线更新。

## 固定协议

- 722 个 BBB development 分子，33 个具体 provider；8 个 unresolved reference 排除。6 个分子含有真实 provider error，共 144 个 error cells；这些 failure 保留在端到端 `system_success` 分母中，不能解释成 chemical wrong。
- 选择前不能看当题未调用 provider 的输出或失败状态。
- Morgan radius 2、1024 bit；非手性 Bemis–Murcko scaffold 分组，5-fold outer GroupKFold；每个 outer train 再按固定 seed 划出 20% validation。
- 线性 BCE、64 维 MLP BCE、BayesianRouter 的 BT+0.2 CLS 离线目标、直接性质 MLP 辅助控制；每种方法 2 个 seed，200 epoch，AdamW，固定学习率和权重衰减。
- train-only Best Fixed 只能用 fit 区选择；Oracle 只作 evaluator 上界。没有调参、没有新科学工具调用。

本诊断的训练目标是端到端成功 $r_{\mathrm{sys}}=\mathbf{1}[status=ok\land output=truth]$。它不是 MIRA 新 label contract 中的单独 `r_chem` 或 `r_run`：新方法必须从 [pharma label receipt](../pharma_label_contract_v1/report.md) 读取两者，并在 error cell 对 `r_chem` 使用 null mask。

## 真实 A800 执行与证据

任务 `01a0de1cf322cd9ee1ead48f41569649` 在 `damo_genomic_gene_a800` 上成功完成，worker 退出码为 0，GPU 为 NVIDIA A800-SXM4-80GB。提交 commit 为 `a50d0164cb8d598817fb2483581120cb76ebf95c`，训练 wall time 为 18.26 秒；平台任务总运行约 3 分 47 秒。

完整日志从 Nebula all-logs 接口下载，而不是用有大小上限的 tail 接口。2026-09-29 又用 `nebula-cli logs download --all` 重新取回并逐文件核对；worker stdout 为 19,686,134 bytes，SHA-256 为 `5b1ead687bb825c27e95cc015591c40160504782692ae4f244ca2bdd49abff9c`。恢复程序核对了 1 个 config、5 个 split、8,000 个 epoch、40 个 fit、40 个 checkpoint manifest、1,970 个 checkpoint chunk、5,776 条 evaluation 和 1 个 complete event。40 个 checkpoint 均通过 gzip/raw hash、PyTorch state_dict 加载、独立 NumPy score 重算、选择/失败/固定工具/Oracle 记录核对；当前正式恢复目录是 `recovered_v4/`，其 `summary.json` SHA-256 为 `45ffa7131a35a8c4407d233445b4911c9a293058ad3194d97536c47892574906`，与之前 `recovered_v3/summary.json` 逐字节一致。

随后分析程序又核对了：

1. 执行源码的 18 个原始 hash 与提交 git tree 一致；
2. 输入 hash 与提交 tree 和本地冻结输入一致；
3. 40 个 checkpoint 的 validation loss 与日志最佳 epoch 一致，最大绝对差约 $1.0\times10^{-7}$；
4. 每条选择相对固定策略的救回/损害分解守恒。

原始日志与恢复产物在 `experiments/logs/task49_fingerprint_diagnostic_20260926/`；正式 full-log 收据仍以 `remote_v2/` 下的下载 manifest 为准，worker stdout 解包副本在 `remote_v2/full_download/extracted_v1/`。`recovered_v1` 保留了第一次恢复器的导出失败；它在所有模型和预测核对完成后，只因 NumPy `int64` 没有转换为 JSON 原生整数而失败，不能与远程训练失败混淆。正式核验结果现在统一指向 `recovered_v4/summary.json`；此前 `recovered_v2`/`recovered_v3` 仅作为相同摘要的历史副本保留。

## 结果

训练区选出的固定 provider 在所有 outer evaluation 中为 `admetlab_3_0`，准确率为 $661/722=0.9155$。具体 provider 的 evaluator Oracle 为 $720/722=0.9972$；固定 provider 错的 61 个分子中，59 个至少有一个其它 provider 正确，另 2 个所有 provider 都错。

| 方法 | seed 0 | seed 1 | 相对 Best Fixed |
|---|---:|---:|---:|
| Linear BCE | 0.8726 | 0.8795 | -0.0429 / -0.0360 |
| MLP BCE | 0.8961 | 0.8906 | -0.0194 / -0.0249 |
| BT + CLS | 0.8878 | 0.8947 | -0.0277 / -0.0208 |
| 直接性质 MLP（辅助控制） | 0.7590 | 0.7604 | -0.1565 / -0.1551 |

MLP 比线性头好，但两个 seed 都没有超过固定工具，也没有满足预登记的继续条件（相对 fixed 至少 +0.02，且每个 seed 至少 4/5 个 fold 为正）。逐分子配对分解如下：

| 方法 | seed | 救回 fixed 错误 | 损害 fixed 正确 | 净变化 |
|---|---:|---:|---:|---:|
| MLP BCE | 0 | 20 | 34 | -14 |
| MLP BCE | 1 | 24 | 42 | -18 |
| Linear BCE | 0 | 20 | 51 | -31 |
| Linear BCE | 1 | 24 | 50 | -26 |
| BT + CLS | 0 | 29 | 49 | -20 |
| BT + CLS | 1 | 22 | 37 | -15 |

## 结论和边界

这次实验否定了一个具体的弱假设：把 Morgan 指纹接到普通多 provider 预测头，就足以可靠地替换当前固定 provider。它没有否定 benchmark 的选择空间，也没有否定 Laya 表示或在线学习；相反，59 个可救错误说明 oracle headroom 真实存在，但模型还不能安全判断何时离开强固定工具。

因此当前不能写“动态 JEV 已有效”，也不能把更大的 backbone 当作自然下一步。随后按 [calibrated switch 分析卡](selective_switch_card.md) 做了不训练的阈值复核：MLP seed 0/1 的 outer accuracy 为 `0.8947/0.9100`，仍低于 fixed `0.9155`；10 个 outer fold 没有一个超过 fixed。这个结果排除了“只需给 argmax 加一道保守阈值”这一解释。

下一步才进入两个必要的主线工作：先在能完整读取模型目录后测 Laya 的真实表示，再把冻结表示接到 selected-only 在线流，与 Frozen、Online SGD、Replay SGD、PILOT 和 BayesianRouter 在相同反馈预算下比较。只有普通近邻已经不能解释收益时，才引入新的动态表示更新机制。

Laya forward 仍需先解决模型目录的 Nebula 资源入口；这不影响本次指纹诊断和 calibrated switch 的完整核验。

## 可复现入口

- 实验卡：`preregistration.md`
- 预检审查：`review.md`
- 冻结输入：`inputs.json`，SHA-256 记录在远程 config 和恢复摘要中
- 执行入口：`src/probe_headless/nebula_fingerprint_diagnostic.py`
- 恢复核验：`src/probe_headless/recover_pharma_fingerprint_diagnostic.py`
- 解释分析：`src/probe_headless/analyze_pharma_fingerprint_diagnostic.py`

## 2026-09-29 最终完整日志恢复补充

本次用 Nebula 的 all-logs 接口取回原始压缩包，没有重新提交任务。本次临时下载包 SHA-256 为 `12b5d723035f46cef5536bf06734bd2232e573fa7a1d78bbb01e7729d45c4357`；正式保存的下载收据对应另一份同内容归档包，SHA-256 为 `22ed07708458157b7c5d2e2befad625553b8d95eab6727088f9d402565c022af`，四个解包文件的大小和 SHA-256 完全一致。worker stdout 的 SHA-256 为 `5b1ead687bb825c27e95cc015591c40160504782692ae4f244ca2bdd49abff9c`。独立恢复器本次输出 `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v4/summary.json`；其 SHA-256 为 `45ffa7131a35a8c4407d233445b4911c9a293058ad3194d97536c47892574906`，与 `recovered_v2/summary.json` 和 `recovered_v3/summary.json` 一致。可复用的正式收据是 `remote_v2/download_all_02_receipt.json`。

恢复器状态为 `verified`，计数为：1 个 config、5 个 split、8,000 个 epoch、40 个 fit、40 个 checkpoint manifest、1,970 个 checkpoint chunk、5,776 条 evaluation 和 1 个 complete event。40 个 checkpoint 都通过压缩块/raw hash、state_dict 加载、独立 NumPy 重算和评价记录核对。这个结果证明训练证据完整，不改变该实验是 development static diagnostic 的范围。

### 结果判定

- Best Fixed：`0.915512`；Oracle：`0.997230`。
- Linear BCE：seed 0/1 为 `0.872576/0.879501`，相对固定工具 `-0.042936/-0.036011`。
- MLP BCE：seed 0/1 为 `0.896122/0.890582`，相对固定工具 `-0.019391/-0.024931`。
- BT + CLS：seed 0/1 为 `0.887812/0.894737`，相对固定工具 `-0.027701/-0.020776`。
- 直接性质 MLP：seed 0/1 为 `0.759003/0.760388`，仅作辅助控制。

所有非固定方法都低于 Best Fixed；MLP 虽优于线性头，但两个 seed 都没有达到预登记的继续条件。该诊断因此给出 **go-signal = false**：当前不能把普通静态指纹选择头直接接到动态 JEV，也不能用这组结果支持“表示已经足够”的说法。

### 对主线的影响

这次结果只否定“通用 Morgan 指纹 + 普通选择头能够安全替换强固定 provider”这一具体弱假设，不否定 provider 选择空间，也不否定组合式动态机制。下一步应先完成真实 Laya 表示审计，随后在同一 selected-only 延迟反馈协议下做 Frozen/Matched Online SGD/Replay SGD/PILOT/BayesianRouter 的公平对照；只有出现可重复的状态、接口或成本收益，再进入表示更新与环境事件扩展。confirmation 仍然不开放。

## 2026-09-30 all-logs 再下载与独立恢复

对同一 Nebula task 再次执行只读 `nebula-cli logs download --all`，得到 10,832,734-byte archive；解包后的四个日志文件与 `remote_v2/download_all_02_receipt.json` 中的文件大小和 SHA-256 全部一致。当前下载 archive SHA-256 为 `cd2379926e4bf30196e22671540ebd784064cb3f73215cb8743e4a3c9ad827f`，worker stdout SHA-256 仍为 `5b1ead687bb825c27e95cc015591c40160504782692ae4f244ca2bdd49abff9c`。

使用当前解包 stdout 重新运行恢复器，输出 `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v5/summary.json`，状态仍为 `verified`，计数仍为 config=1、split=5、epoch=8,000、checkpoint manifest=40、checkpoint chunk=1,970、fit_complete=40、evaluation=5,776、complete=1。`recovered_v5/raw.jsonl` 与 `recovered_v4/raw.jsonl` 逐字节一致；40 个 checkpoint 和全部 evaluation 结果未发生变化。当前恢复摘要 SHA-256 为 `45ffa7131a35a8c4407d233445b4911c9a293058ad3194d97536c47892574906`。这次补充只提高证据可追溯性，不构成新训练或新科学结果。

### 当前正式恢复入口

为避免把之前的空目录尝试当成证据，2026-09-30 从同一份已下载 worker stdout 重新运行恢复器，正式入口改为 `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_final_20260930/`。该目录的 `summary.json` 状态为 `verified`，计数仍为 config=1、split=5、epoch=8,000、checkpoint manifest=40、checkpoint chunk=1,970、fit_complete=40、evaluation=5,776、complete=1；摘要与先前完整核验结果一致。此次没有重新提交 Nebula，也没有改变任何模型或数据。

## 2026-10-01 all-logs recovery recheck

再次对同一个成功任务 `01a0de1cf322cd9ee1ead48f41569649` 做只读完整日志下载，保留新的下载收据 `experiments/logs/task49_fingerprint_diagnostic_20260926/remote_v2/download_all_03_receipt.json`，没有重新提交或重训。worker stdout 解包副本为 `remote_v2/stdout_full_worker_20261001.log`，大小 `19,686,134` bytes，SHA-256 为 `5b1ead687bb825c27e95cc015591c40160504782692ae4f244ca2bdd49abff9c`。

用当前恢复器输出到新的 `recovered_v7_full_20261001/`，状态为 `verified`，计数为 config=1、split=5、epoch=8,000、checkpoint manifest=40、checkpoint chunk=1,970、fit_complete=40、evaluation=5,776、complete=1。40 个 checkpoint 均通过压缩块/raw hash、state_dict 加载、独立 NumPy 重算、validation-best 选择和 evaluation 记录核对；新的 post-hoc analyzer 也通过源码 hash、输入 hash 和 5,776 条 paired rows 的守恒检查。

`recovered_v7` 的结果与先前 `recovered_v4/v5/final` 摘要一致：MLP BCE seed 0/1=`0.896122/0.890582`，BT+CLS=`0.887812/0.894737`，Linear BCE=`0.872576/0.879501`，直接性质 MLP=`0.759003/0.760388`；Best Fixed=`0.915512`，Oracle=`0.997230`。这次只提高原始训练证据的可追溯性，不改变该实验是 development static representation diagnostic、go-signal=false，也不提供在线 MIRA 效果结论。

## 2026-10-04 recovery_v2 复核补充

对同一成功任务再次使用已保存的完整 worker stdout 执行恢复器，输出目录为
`experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v2/`。恢复状态为
`verified`，计数为 config=1、split=5、epoch=8,000、checkpoint manifest=40、checkpoint
chunk=1,970、fit_complete=40、evaluation=5,776、complete=1。40 个 checkpoint 均通过压缩块
与原始 hash、`state_dict` 加载、独立 NumPy 分数重算、validation-best 选择和 evaluation
记录核对；worker stdout SHA-256 仍为
`5b1ead687bb825c27e95cc015591c40160504782692ae4f244ca2bdd49abff9c`。

本次复核没有重新提交 Nebula、没有重新训练，也没有改变任何数字。恢复摘要显示：Best
Fixed=`0.915512`；MLP BCE seed 0/1=`0.896122/0.890582`；BT+CLS=`0.887812/0.894737`；
Linear BCE=`0.872576/0.879501`；直接性质 MLP=`0.759003/0.760388`。因此原先的
`go-signal=false` 判断再次成立：普通静态指纹选择头没有通过预登记继续条件。这条证据仍
只支持 development static representation diagnostic，不支持在线收益、动态环境适应或
最终 confirmation 主张。
