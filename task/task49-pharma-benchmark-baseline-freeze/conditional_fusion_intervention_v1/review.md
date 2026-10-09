# Conditional Fusion Intervention v1：执行前审查

日期：2026-10-08
审查对象：`experiment_card.md`（候选、未冻结、未运行）
审查范围：分支 2 的 B-only 诊断、三份生效评价标准和用户的广义目标。
审查结论：**有鉴别力，但当前不能直接冻结执行；补四个最小合同后可运行。主方法不变。**

## 1. 与分支 2 结果的关系

分支 2 的证据是：在 `K=(a=0,b=3,d1=拒答)` 的 fold 0、1、3 中，S 和同键 U 都出现风险差、Brier 差同向变差，支持量达到卡内阈值。它只支持优先检查“终局融合”，不支持高斯分布、第二工具一般有价值或在线收益已经成立。新卡保持了这个边界，且把首选、已取得 pair、gain learner、阈值、成本和反馈更新冻结，方向正确。

## 2. 逐项审查

### 2.1 是否真正只改变终局融合

**条件通过。** 只要以下三点写死，CGEF 与原 masked-logistic 的差异可以归因于终局融合器：

- 首动作和已付费 pair 流逐行复用；不重新排序、不给 CGEF 新的候选或新调用；
- A fit 只用于一次性估计 CGEF 参数；B 只评价，不能回流阈值、参数或 mask；
- 两个规则用同一动作阈值、同一三动作损失和同一成本口径。

当前卡有一个必须澄清的估计对象问题。`ignore-second` 是**已经付费并拿到 pair 后的终局控制**，因此应与 CGEF、masked-logistic 一样计入这次已发生的追加成本；它不能被解释成调用前 stop（调用前 stop 的增量成本为 0）。请在卡中加入这句固定定义，否则 CGEF−ignore-second 的 utility 差没有唯一含义。

卡第 4.B 的 `$D=0$` 与 `$D\in\{1,4\}$` 不是这张固定 B 流融合实验的可执行因素；它们只在后续释放和在线更新中有意义。建议改成“本卡不引入 D；若复用带 release label 的同一缓存，D 不得改变当前已取得 pair 的终局动作；闭环 delay 比较另立实验卡”。保留现在的写法会把一个未运行的动态因素混入单因素静态干预。

### 2.2 CGEF 数学和信息权限

**数学主体可实现，信息边界基本正确，但需要三个定义才能达到可复核状态。**

1. 将类别先验写成拟合量，而不是总体真值：
   `$\hat\pi_A=\frac1{|A|}\sum_{i\in A}Y_i$`，`$\alpha=\operatorname{logit}(\hat\pi_A)$`。同时写明每个 fold 若 A 中缺少某一类则 fail-closed。
2. 对 `$u=\operatorname{logit}(p)$` 写明边界策略：`$p\in(0,1)$` 才允许转换；`p=0/1` 或非有限值按预登记 failure 处理，不能运行时静默 clip。若实现实际要 clip，必须提前固定 epsilon 并在所有方法共享该输入变换。
3. 明确 mask 和失败语义：B 只能使用该行已经返回且有限的 selected provider 输出；未选 provider、当前真值、未来 release 和 selector/gain 特征都不可读。selected output 超时或失败时不能伪造 logits；需预先指定 fail-closed 的 fallback（例如沿用首动作并按相同已付费成本记账）或将该 fold 标记为无效，并在结果中报告分母。

共享协方差的边缘化公式是可计算的：`$\Sigma+0.10I$` 在数值上正定，`$\Sigma_{MM}$` 取同一 joint model 的子矩阵，避免为每个 mask 单独拟合。文中应把 `posterior` 限定为“模型诱导的 evidence score/probability”，不要把它写成真实后验已被证明；卡已经正确声明不证明高斯假设。

### 2.3 Baseline、指标和停止线是否足以改变下一步

**能回答“融合器是否值得继续”，但还不能支撑一项完整的方法比较。**

- `ignore-second`、原 masked-logistic、CGEF 和 evaluator-only Oracle 是这个固定终局问题的必要角色；Oracle 只能作上界，不能进入部署排名。主结果应为全 B 的 paired CGEF−masked-logistic 和 CGEF−post-paid-ignore-second，`K` 只能作预登记条件诊断。
- 分支 2 报告还要求“同预算 confidence cascade 和当前 strongest legal control”。新卡没有列这两个角色。这里不能悄悄用较弱的三行对照替代原停止条件。若 confidence cascade 会改变是否发起第二次调用，它不属于本卡的“固定 acquisition、只改融合”估计量，应在卡中明确“本卡不比较；另立同预算闭环卡”，并把当前停止线相应改写。若存在保持相同已取得 pair 和预算的终局 cascade，则必须把它加入同一 B 流并记录其信息权限。
- 当前 `$+0.01$ utility/item` 是合理的开发路由线，但不是显著性证据。至少要输出五个 fold 的 paired delta、每个 fold 的 molecule/entity 支持和 pooled 加权值；不能把 pair 行当独立样本。若用它决定下一张闭环卡，建议固定为“pooled delta 达到 +0.01 且至少三个 fold 非负”，否则只记为条件诊断；论文或最终方法主张还需要预登记的 entity/fold-cluster uncertainty。
- Utility、正确率、拒答、Brier、calibration、rescued/harmed 和 calls 的集合是合适的。由于 calls 在这张重放里固定，必须明确 cost 已在三种终局规则中同口径计入，不能把固定 calls 当作“调用前节省”。failure、unknown、非有限输入和无效 fold 的分母也要逐项列出。

## 3. 对三份标准的局部评分判断

这不是对整篇论文重新评分，只判断该实验卡能否成为下一步证据。

- **故事线标准：H2/H5/H6 基本满足，H4 仍未闭合。** 分支 2 给了可复现的具体退化和可反驳的融合假设；CGEF 是否带来 integration gain，要靠 matched terminal comparison 和负结果，而不能由高斯名称本身得到。
- **方法论标准：M6 可运行但需补先验、边界输入和 failure fallback；M7/M14 只有在独立 verifier 能从 A 参数快照和 B raw event 完整重建后才算通过；M13 的消融/停止方向正确。** 本卡不产生 selected-only online update 证据，不能替代当前 MIRA-Relay 的实时主张。
- **Benchmark/Baseline 标准：目标隔离 R2 做得好；R3 需要补齐或解释 strongest legal control；R4 目前缺 fold/entity 级不确定性口径；R6 的负结果和停止规则已写清。** 因此它可作为 Stage A 开发干预，不可关闭 Gate C，也不可打开 confirmation。

## 4. 冻结前最小修订清单

1. 写死三种规则的 post-paid cost estimand，并把调用前 stop 明确排除在本卡之外；删除或改写不可执行的 D 行。
2. 补 `$\hat\pi_A$`、`p=0/1` 的 fail-closed 或固定 clip、缺类和 selected failure 的处理；保存逐 fold 参数和实际 mask。
3. 对分支 2 报告中的 confidence cascade/strong legal control 作出范围决定：加入同合同对照，或明确另立闭环卡且不得用本卡结果声称超过它们。
4. 预登记 fold/entity 聚合和 uncertainty 输出；至少保存每 fold paired delta、支持量和 pooled 值，并把“+0.01”定位为开发路由线而不是显著性结论。

完成后才建议生成新的 card hash 并冻结一次 CPU-first B-only 运行。运行结果只决定是否进入同 gain learner、同预算的闭环融合卡；不改生效方法、论文主表或 confirmation。
