# 主实验到达顺序复核

2026-10-08。状态：**完整运行和独立核验完成**。独立审计 v1 在末尾直方图分箱失败；修复明确的浮点表示合同后，v2 重新独立核对全部轨迹和统计并通过。没有修改生产运行或原始结果。

## 这次解决什么问题

原稿的 scaffold bootstrap 重加权已经记录的结果，没有重新执行自适应策略。它描述固定轨迹对 scaffold 权重的敏感性，不能单独证明新轨迹的总体覆盖率。本实验固定当前已检查过的 157 个分子、四个 provider、五个 prior、两个策略 seed 和三个 delay，在 **128 个独立均匀排列**上重新执行选择、反馈释放和在线更新。

执行前冻结的唯一主比较是：受控漂移下 MIRA-Relay 减 Online SGD 的 pooled correctness。每个完整排列是一个统计单位；30 个配置在排列内平均。预定通过条件要求均值区间下界大于 0，且胜出概率的精确区间下界大于 0.5。该检验只判断方向，不检验预定的非零实际收益门槛。

- [冻结实验卡](experiment_card.md)：输入、完整对照、统计量、否定条件和资源界限。
- [实现审查](implementation_review.md)及[资源 GO](resource_go.json)。
- 原始记录：`experiments/logs/task49_primary_arrival_robustness_20261008/full_v1/`。
- 本实验没有 API、provider forward、离线 refit 或 GPU 调用；在线更新实际执行。

## 主结果与负结果

| 量 | 实测 |
|---|---:|
| 受控 MIRA − SGD 均值 | +4.7363 pp |
| Monte Carlo 标准误 | 0.2120 pp |
| 近似 95% 均值区间 | [+4.3167,+5.1558] pp |
| 正收益排列 | 125/128 |
| 正收益概率精确 95% 区间 | [0.9330,0.9951] |
| 排列间标准差 | 2.3988 pp |
| 中位数 | +4.9045 pp |
| 最小 / 最大 | −2.6752 / +10.2760 pp |
| 排列经验 2.5% / 97.5% 分位数 | +0.4490 / +8.6125 pp |

两个预定方向条件都满足。第 69、86、90 个排列为负，全部保留。均值区间针对固定样本集合的随机到达顺序，不是单个排列的 95% 范围，也不是新分子、新 scaffold 或部署泛化区间。

自然条件下，MIRA 相对 Frozen 的 pooled 差为 **−0.2870 pp**，均值区间 **[−0.3723,−0.2016]**。原顺序的 0.00 pp 不能代表所有到达顺序；这次结果没有支持自然条件下的在线提升。自然条件的负结果与受控恢复同时进入报告和正文，不能切换统计口径来消除它。

## 完整对照

下表为 128 个顺序的平均正确率（%），每个顺序平均同样的 30 个配置。空格表示冻结协议未安排该条件，不是运行失败。

| 方法 | Natural | Controlled |
|---|---:|---:|
| Random | 86.47 | — |
| Fit-only Best Fixed | 88.15 | — |
| Frozen | 89.81 | 75.75 |
| MIRA-Relative | 89.80 | — |
| Online SGD | 89.05 | 82.22 |
| Greedy LinUCB | 89.70 | 76.90 |
| BayesianRouter | 89.72 | 79.60 |
| MIRA-Relay | 89.52 | 86.96 |
| Fixed-permutation credit | 89.86 | 71.40 |
| Retrospective Best Fixed（evaluator-only） | 90.45 | — |

这些是按合同适配的对照，不冒充相关论文的完整原生复现。所有方法共享当题可见信息和一次调用预算；因动作不同，各自获得的反馈内容允许不同。受控条件的翻转始终发生于 ticks 78–156（含两端），因此每个排列被影响的分子也会改变。

## 机制统计与权重区别

机制统计先在每个 scaffold 内平均，再对 124 个 scaffold 等权平均。受控 Local−Permutation 为 **+15.6124 pp**，pointwise 均值区间 **[+14.8146,+16.4101]**。这是另一个估计对象，不与 pooled primary 混写。

自然 Local−Frozen 的等 scaffold 差为 +0.2110 pp，而 pooled 差为 −0.2870 pp。差异说明样本权重影响结论，不能据此选择有利的指标。已单独冻结[事后错误分解卡](error_diagnosis_card.md)，保留所有分子、scaffold 和 singleton/multi 两固定组的救回/损害贡献；它不做新假设检验或调参。

错误分解已经运行，并由另一脚本独立从回执重建，314 条分子记录、248 条 scaffold 记录的整数计数精确一致。每个分子有 3,840 次配对暴露，全部 157 个分子、124 个 scaffold 保留：

| Natural 固定组 | 分子数 | 净正确次数 | Pooled 贡献 | 等 scaffold 贡献 |
|---|---:|---:|---:|---:|
| 104 个单分子 scaffold | 104 | +2,034 | +0.33738 pp | +0.42717 pp |
| 20 个多分子 scaffold | 53 | −3,764 | −0.62434 pp | −0.21614 pp |
| 合计 | 157 | −1,730 | −0.28696 pp | +0.21103 pp |

Pooled 对每个分子等权，因此第二组贡献更大；等 scaffold 则降低大 scaffold 的权重。单分子组也包含净损害 −1,300 次的 `confirm_026`，不能说 singleton 都受益。完整逐分子/逐 scaffold 表在 `error_diagnosis_v1/`；代码不产生新模型预测。下一轮可复用这些配对账目定位需要解释的错误，不能据此删分子或调整旧确认集上的参数。

## 执行与可恢复性

- 完整运行：61,440 条流、9,646,080 次决策；530.51 秒，峰值 RSS 323,665,920 bytes，压缩回执 3,145,050,307 bytes，均在冻结界限内。
- 完整性分母：128 个随机排列全部完成；每个排列 480 条流、每条流 157 次决策和 157 条 selected feedback，共 61,440/61,440 streams、9,646,080/9,646,080 decisions、9,646,080/9,646,080 selected feedback。`failure=0`、`unknown=0`、`incomplete=0`；这些是本次 replay 的事件账目，不是对 provider 的一般可用性承诺。
- 输入收据：执行前一次生成并冻结的 `full_v1/inputs/features.npy` SHA-256=`240f4452db2a9dd2c83daaa1b4a309f07e1c0904aaa84acdb7e2627765fe8a3b`；`full_v1/orders.npy` SHA-256=`993d6ad620e4421a6323c165ccacc87ce1544516f40fdae40111731bd7e6d642`；完整输入/逐 order manifest 位于 `experiments/logs/task49_primary_arrival_robustness_20261008/full_v1/`。features 只生成一次，没有在排列结果上重新拟合。
- comparator 选择：Online SGD 在 128 个排列抽样前，依据已有历史 receipt 被预先登记为 controlled 条件的 strongest strict control；没有查看本次排列结果后挑选赢家。它的先验、学习率、反馈时钟和 seed 随冻结 card 绑定。
- `results.json` SHA-256：`dbaf3e64f9f3695b0d0a20ade3124b25b1732e24cacad46e76e8ec0e12044946`。
- `summary.json` SHA-256：`543445a0e6f170ecd4559e12f40377c875142c75341a4f77f9c0d96130428a61`。
- 原顺序 smoke_v2 保留 480 条流、75,360 次决策。独立检查对原 main/credit 轨迹的动作、终止标签、release 顺序和最终状态一致；sigmoid/logit 变换及旧尾队列任意时刻的差异均按预先声明处理。
- 失败 smoke_v1：旧回执比较逐条重复解压 NPZ，人工中断。修正为每个数组只解压一次；没有改模型、超参或挑选结果。现场、traceback 和新运行均保留。
- 独立 full_v1：128 个 chunk 的选择、标签、分数、状态及标量统计已通过，在 secondary histogram 的整数分箱检查失败。独立诊断发现 168 项中 35 项受不超过 $1.42\times10^{-14}$ 的百分比末位差影响，主比较直方图及所有非分箱摘要未受影响。v1 不标 verified；v2 用明确一致的百分比运算顺序，从独立整数计数重新计算，仍要求分箱整数精确相等，不放宽离散检查。
- 独立 full_v2：61,440 条完整轨迹、38,584,320 个分数、全部最终数值/实例状态、23,040 组 RNG 状态及所有汇总精确一致；错误 release 和 terminal label 仍被拒绝。审计脚本不导入生产 runner/policy。成功一遍 CPU 510.80 秒，连同失败、诊断和测试累计 1,040.78 秒，低于 1,800 秒界限。
- 独立核验文件：`independent_full_v2/verification.json`，SHA-256 `99fb3c3c0d0e9995641bdcf22f4c5e61f5333b9c13c182ae60585dc26e2a87b9`。错误分解另见 `error_diagnosis_independent_v1/verification.json`。原始运行目录、首次失败和诊断均保持原样。

## 对三份标准的影响和下一步

这批证据补的是 benchmark 标准 C4/B13 中的**指定到达分布下的策略完整重放与不确定性**。它不能把人工 correctness flip 变成自然生命周期漂移，不能把已检查队列变成新 confirmation，也不能替代集成的复核/追加/拒答 agent 实验。

完整科学 gate 继续 **HOLD**。独立审计已关闭；表格和区间解释已合入唯一主稿，经编译和全9页视觉核对后提升 `paper/aamas2027/main.pdf`，正文恰好8页、References第9页。来源为 `submission_20261008_114253`，旧版、失败版和完整原表均保留。随后依据错误分解以及条件第二次调用的既有负结果，选择一个能区分原因的后续干预；不对这 157 个分子调参追分。

本轮可复用成果：完整对照的紧凑逐事件回执、独立数学重放、固定顺序下的均值与胜出概率统计合同，以及不混淆 pooled 和等 scaffold 的逐分子贡献账目。后续实验直接复用这些接口，不重新搭建基础设施。
