# 当前四工具学习型prior：未通过升级线

2026-10-07。状态：真实A800训练、完整回收、独立核验均完成；两种固定Morgan64训练方案均未过继续线。旧157 confirmation未访问。

## 这次解决什么问题

补当前面板缺失的非线性BCE与BayesianRouter离线BT+CLS目标适配，检验“调用前排序不足能否仅通过改变学习目标解决”。网络、初始化、数据、训练预算和validation选型一致。它关闭一项静态学习型baseline缺口，不等于完整BayesianRouter在线复现或新算法贡献。

## 实际执行

- 任务 `01a114f6d306db0a283dce1d4453aa33`，终态success/FINISHED；执行commit `86543eca491814a19634529e27237d80f04083c4`。
- 单A800-SXM4-80GB、PyTorch2.8.0、NumPy1.26.4、float32/deterministic/TF32 off。722分子、4工具、原5fold、2seed；20 fits×200epoch，2888条OOF。
- 训练入口墙时15.932秒，20次优化循环合计14.361秒；峰值CUDA allocated 68,529,152bytes。平台代码下载/依赖初始化另计，不能把训练墙时当端到端部署延迟。
- 10 shared-initial与20 best checkpoints全部返回；共40个清单文件校验通过。没有新增provider/LLM调用。首次科学运行成功；状态查询一次TLS中断、启动期一次no-containers保留在remote_v1，未重复提交。

## 结果

每seed分母722，mixed分母147。两个seed共享分子，不当两个独立数据集。

| 方法 | Seed0正确数 / accuracy | Seed1正确数 / accuracy | Mixed正确数，seed0 / seed1 |
|---|---:|---:|---:|
| Fit-only Best Fixed | 572 / 79.22% | 同一确定性结果 | 101 / 101 |
| Raw Frozen | 573 / 79.36% | 同一确定性结果 | 102 / 102 |
| Corrected Frozen | 579 / 80.19% | 同一确定性结果 | 108 / 108 |
| BCE+CLS | 567 / 78.53% | 570 / 78.95% | 96 / 99 |
| BT+CLS（目标适配） | 569 / 78.81% | 565 / 78.25% | 98 / 94 |

| 对Corrected Frozen | 净差pp | 救回 / 新增错误 | 五折净正确数差 |
|---|---:|---:|---|
| bce_cls/seed0 | -1.6620 | 16 / 28 | -6, -2, -3, 1, -2 |
| bce_cls/seed1 | -1.2465 | 17 / 26 | 2, -5, 0, 1, -7 |
| bt_cls/seed0 | -1.3850 | 14 / 24 | -7, 1, -2, -1, -1 |
| bt_cls/seed1 | -1.9391 | 9 / 23 | 1, -6, -1, -1, -7 |

四组相对Corrected Frozen的五个LOO全部为负。BT−BCE两个seed分别+0.2770pp（救9/损7）、−0.6925pp（救14/损19），不支持排序目标有稳定优势。所有负折保留。

## 决定与解释

两种家族都未达到“两seed至少+1pp且LOO正”的规则，不升级到在线候选，不重调lr/正则/seed/CLS系数。这个结论针对现有Morgan64及固定配方，不能外推为非线性表示或论文目标不成立。

目前可定位的事实是：相同输入表示上，改为非线性双头或BT训练没有救回比Corrected Frozen更多的错误。20fits中有5个best epoch为1；完整200步仍均执行。这不证明训练无用、验证错误或过拟合已被因果确认，不能据此重新选择checkpoint规则。

下一科学问题转向表示保真：现有64-bit归一化chiral Morgan是否丢失区分适用范围所需的信息？可复用当前provider代码的chiral Morgan1024构造，再保持归一化、菜单、split和标签。旧1024缓存无显式chirality且未归一化，不能直接称“只改位数”。这仍需单独数学/容量审查和新卡；尚未提交下一任务。Laya旧33-marker缓存也不作为四工具query缓存。

## 独立核验

root核验器不导入训练器/准备器/RewardHead；使用原生概率重建正确性、原fold与Morgan64，读取30个state_dict并用独立NumPy矩阵运算复算两个head。对齐每条OOF的模型、行、选择、正确性，复算全部配对与LOO、训练源文件与提交Git树、go决定。
- 结果verified，0 issues；2888条OOF、20fit、30checkpoints、4000条epoch记录齐全；核验0.552秒。
- NumPy/GPU logit最大误差1.924e-07，best-validation loss最大误差5.785e-08；独立argmax全部一致。
- epoch审计是按记录重算应选epoch并核对best forward，不是重新执行全部优化轨迹。计时/内存为原运行记录，未独立重测。

## 证据入口

- 冻结合同：`experiment_card.md`、`math_review.md`、`code_review.md`。
- 运行：`experiments/logs/task49_fresh_learned_prior_20261007/{input_v1,remote_v1,remote_output_v1}/`。
- 独立结果：同目录`verification_v1/verification.json`。
- 输入SHA：`33c9d092f840e4f2c75eec562271abe00891de0362a6ec3a8ad9c78a7aeb1cfc`。
- summary SHA：`5782dd96355888208c1716f9efae0e1a23ccc366da51f86cecdb1d1b3be6c50e`。
- verification SHA：`3f62292bc2863319c2a950248a1fc9838ea102ce14e7116e4fd0f807ec0a1607`。
- OSS archive SHA：`2ac16485c3784976521e13b06dc000e5c9321d3efa084e7fe200af16bcdfe3bf`。

## 回看三份标准

E1/E3补了同表示、容量/选型匹配的静态对照，baseline fidelity更清楚；结果未提供新增有效机制，未完成新方法独立确认、广义动态能力与全部现代在线对照。三份完整标准仍HOLD，不改分母、不因本轮失败撤销目标。当前论文8页要求保持。
