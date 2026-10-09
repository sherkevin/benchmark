# 下一轮：当前四工具面板上的学习型prior

日期：2026-10-07。历史准备稿；本轮已按后续执行卡完成，结果见[report.md](report.md)，两种方案均未过升级线。以下保留执行前状态：准备，未训练、未冻结全部实验参数。来源为`/root/fresh_headroom_locator`对既有报告/代码的只读范围核查；未读confirmation。

## 为什么推进这一项

当前四provider的调用前prior仍是Morgan64+截距上的逐provider class-balanced logistic。六格校准和context ridge都未达到增量效果规则，继续调整同一在线修正缺少新依据。现有现代baseline的原生离线学习尚未在这四工具上完成，这是方法诊断和Baseline gate共用的一项缺口。

**下一问题：在相同分子表示、数据、容量和训练预算下，学习相对工具排序是否比学习独立正确性更有用？** 同时检查非线性打分器相对已核线性Frozen的表现。排序损失、MLP或标准posterior不单独作为创新；本轮优先补诚实、可比较的现代学习型控制。

## 已有资产与范围

| 资产 | 已完成范围 | 可以复用 / 不能直接继承 |
|---|---|---|
| `run_provider_fresh_confirmation_v1.py::fit_prior` | 当前四工具的class-balanced logistic prior | 当前对照定义；这次只读代码，不再读confirmation |
| `pharma_fingerprint_diagnostic.py::RewardHead/loss_for` | 旧33provider，MLP-BCE与BT+0.2CLS | 64维隐藏层、训练循环、validation选epoch、checkpoint和逐样本日志；旧效果不属于四工具 |
| `bayesianrouter_native_bt_raw_v1/report.md` | 旧33provider、原生BT checkpoint在线适配 | 方法/来源和恢复代码；不能把其0.89结果搬到四工具 |
| `laya_static_diagnostic_v1/report.md` | 旧33provider candidate-marker表示 | 已有工程与模型资产；旧marker与旧菜单绑定，不能重命名成新四工具 |
| `fit_score_reference_20261007/v1/cache_manifest.json` | 当前四checkpoint×722，五折及Morgan64 | 原生概率、truth、fit/validation/evaluation、现有特征，无需再次provider forward |

`ecfp_mlp`是被选择的BBB预测工具，不是correctness selector。当前BayesianRouter使用共同logistic prior的在线适配，不等于四工具原生BT+CLS训练已完成。

## 最小实现方向

1. 薄适配器只从封存四provider原生概率构造正确性，使用原五折和现有Morgan64；保留722全部分母。旧训练入口硬编码33/1024/A800，不能不改资格检查就重跑。
2. 先补MLP-BCE及BayesianRouter BT+CLS静态对照。若要将二者差归因于ranking目标，必须匹配encoder、score bias、head容量、初始化、训练与validation预算；现有两个`RewardHead`分支并不完全匹配。
3. 可用相同BT+CLS网络作为完整对照和BCE消融，只替换主score损失，辅助头、bias和初始化保持一致。是否采用此方式须在执行卡明确；未据此宣称实现等于原论文全部流程。
4. OOF只报告全部/mixed top1、fit-only与已核Frozen锚点、rescued/harmed及全部fold/seed；不靠provider自身BBB分类AUC代替selector效果。
5. 全同对/同错共575/722仅提供loss侧重点的疑问，**不能直接证明BCE梯度被这些样本支配**。如果报告该机制，需按fit三类记录实际loss/梯度贡献；否则只比较真实选择效果。
6. 先固定目标、源版本、loss、epoch/seed/optimizer、validation选择、最小增量与负结果分支；同一Nebula A800任务承载整批，一次一条。开发结论不能进入已读157分子作为“新确认”。

这份准备清单不是新生效方法或决议。下一次工作先冻结一张可执行实验卡并复用训练代码，再运行；不重做已通过的headroom、状态审计或provider推理。
