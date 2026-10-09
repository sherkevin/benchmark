# 冻结分数修正 × 在线中心化：配对闭环诊断

日期：2026-10-07。阶段 E；开发诊断，不替换现有主方法、confirmation 或论文主结果。执行前冻结。

## 目的、近邻与预算

检验上一轮发现的基准偏移是否限制在线更新：先区分“冻结分数变好”与“在线更新变好”，再检查改变中心化能否减少自然流的有害切换并保留受控错误恢复。它对应方法 E3 和 benchmark Gate C3 的可解释性要求。

最近已发表对照为已有 BayesianRouter-adapted（来源和合同见 `research/experiment_section_style_20261006/nearest_work_matrix.md`）；本轮是内部机制消融，不提供新的方法 superiority 排名。方法变体若有价值，下一阶段才在共同合同下与现代 baseline 比较。标准的类别权重逆变换不是创新点。

固定矩阵：2 个冻结分数版本 × 3 种在线路径 × 5 折 × 3 个延迟 × 2 种环境 = **180 streams，25,992 decisions**。每条决策只选一个 provider，原队列顺序，seed0；无随机策略。不把 seed1 的确定性重复当独立样本。

预算：本地 NumPy/CPU，单次主运行最多 180 秒；只读已存缓存，0 provider forward、0 API、0 GPU。不调参、不搜索、不重跑旧 confirmation。先做小型数学/时钟测试，然后一次完整运行；实现错误保留原失败目录，另开版本修复。

## 冻结输入与信息边界

1. `experiments/logs/task49_fit_score_reference_20261007/v1/summary.json`，SHA-256 `ad5650d68e8e8d7f9ca497f2fa93c20abd6a4dfc41ed5711ebe83ba7391176c0`。依它核验 `prior_cache.json`、`oof_scores.jsonl`、`development_provider_probabilities.jsonl`、features 和 fit references 的 hash。
2. 原 development `fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；722 molecules、原五折 fit/validation/evaluation、原评价行序和 scaffold 组。
3. 原 development MIRA/Frozen events 只供复核原 raw/marginal/natural 和 raw/Frozen/natural 行为。不可拿其未选 reward 喂给 updater。
4. policy 只接本次四个 OOF logits、fit rates、released state 与事件 ID；evaluator 单独持有原生概率、truth、stress target。每个终止后仅被选输出进入队列，delay 为 0/1/4，due tick = decision tick + 1 + delay，决策前按时释放，结束后统一在 `N+6` tick 按 FIFO 排空，复用原runner尾部时钟。
5. 全部输入均为 development。数据不是 model-fresh 确认；当前 panel 的 provider training exposure 资格保持原声明。本轮不能建立未知模型上的泛化。

## 数学与实例

令 $z_a(x)$ 为原始 class-balanced logistic 的 logit，$\pi_a$ 为该折 fit 上 provider 的正确率，两者来自已重建的缓存。分数版本固定为

$$q_a^R(x)=\sigma(z_a(x)),\qquad q_a^C(x)=\sigma\!\left(z_a(x)+\log\frac{\pi_a}{1-\pi_a}\right).$$

实例：某折 provider fit 正确率 $\pi_a=0.8$，分数 logit $z=0$，原分数为 0.5，修正后为 0.8。这里的 logit 是选择头分数，不是 provider 的分子性质预测概率。实际全部 $\pi_a$ 严格位于 0 和 1 之间；不满足就失败，不静默裁剪。

推导：weighted Bernoulli risk 的逐点最优概率满足 $\operatorname{logit}(q)=\operatorname{logit}(p)+\log(w_1/w_0)$。balanced 权重给出 $w_1/w_0=(1-\pi_a)/\pi_a$，逆变换即上式。这只证明理想逐点最优下的权重修正；原 C=1 的有限维正则模型不保证被修正后校准，必须实测 OOF Brier 和均值差。

对每个分数版本分别运行三个路径；$n_a$ 只统计当前已经释放的 selected feedback，先验计数固定 20：

$$s_a^{F}(x)=q_a(x),\quad s_a^{M}(x)=q_a(x)+\frac{\sum_{i:a_i=a}(r_i-\pi_a)}{20+n_a},\quad s_a^{Q}(x)=q_a(x)+\frac{\sum_{i:a_i=a}(r_i-q_a(x_i))}{20+n_a}.$$

实例：fit 正确率 0.8，已释放两次 selected reward 为 1、0，决策时保存的冻结分数为 0.9、0.6，则 M 修正 $(1-1.6)/22$，Q 修正 $(1-1.5)/22$；未释放反馈不计入分子或分母。Q 保存调用当时的冻结分数，不用更新后的 action score 或事后重算的动态分数。空历史时两者严格等于各自 Frozen。

每个时刻选 $\arg\max_a s_a(x)$，按固定 provider 次序取首次平局。不得裁剪最终 action scores，避免引入平局和新因素。状态记录 count、reward sum、decision-time frozen-score sum；Frozen 可保存审计计数，但不改变选择。

Q 分子在条件校准且释放不依赖未观察 reward 时有零条件均值的 martingale 动机；这不是对当前有限模型的保证，也不保证随机计数比值无偏、未选工具可辨识或 greedy 探索充分。

## 两种环境

- **natural**：原生 provider label >=0.5，按真实 BBB truth 判分。
- **controlled flip**：每折只由 fit rates 的 argmax 决定目标 provider；评价流从 `floor(N/2)` tick 起，若选择该 provider，则 evaluator 翻转它的返回二元 label。所有方法使用相同目标、时点和已知参考 truth，policy 不接收这些信息。这是 evaluator-side correctness stress，不是自然漂移。

相同cell的natural/stress在第一条实际受翻转影响的反馈释放前，action、score、state必须完全相同；若始终没选目标，则全流相同。flip按调用的decision tick决定，不按release tick。跨分数版本的Frozen选择偏好与stress暴露率可能不同，匹配Frozen差不能单独证明跨版本机制更优，须同时读绝对/后半准确率和target选择次数。

先检查 raw/Frozen 与 raw/M/natural 的动作、reward、due/release时钟与顺序能精确复现旧日志；M action score误差不超过 $10^{-10}$，Frozen原记录为logit，先sigmoid后核对。自然和受控结果分别报告；本轮不根据结果换 stress 目标、位置、强度或流顺序。

## 输出与衡量

1. 每个 fold/regime/delay/cell 的 selected accuracy、累计错误、pre/post accuracy、相对同分数 Frozen 的 paired rescued/harmed、动作变化、各 provider 选择次数和释放次数、stress target被选次数和首条受影响反馈释放tick（未发生为null）。后半段固定起点floor(N/2)，不随各策略首次反馈变化。
2. 自然 OOF 全面板的 Brier/log loss/平均分数/真实正确率只作分数审计；不输入在线更新，不冒充在线效果。另报各策略实际 selected 的 Brier。
3. 主比较是同分数版本 Q−M、M−F、Q−F；另报 corrected Frozen−raw Frozen，分开解释静态收益与在线收益。给 pooled 和逐fold、逐delay数值。pooled在每个regime内按分子数加权合并5fold，再对三个delay等权；natural与stress不混成一个总分。已使用多轮development，不作显著性或确认性结论；不把重复delay行当独立样本。固定样本描述无需伪造CI或重复seed。
4. 原始 decision/terminal/release JSONL、完整 canonical counter state、state bytes/hash、pending队列完整可重算快照（含decision_q）、decision-time q、unknown/refusal=`null`与实际status、policy operation timer、每次调用1次、环境/命令/源码/输入/卡hash都入档。成本只称CPU策略操作开销，队列/日志/分数编码另列边界。
5. 独立脚本从JSONL重算状态、动作和聚合，禁止导入runner。所有180流必须通过。保留失败/负结果和固定命名的全部六格。

## 否证与后续决策

- 原行为复现失败、queue/selected-only/state重算失败、hash不符：停止解释，先修实现，保留失败版本。
- corrected 分数若OOF Brier不降，则不能称校准改善；若只corrected Frozen提高而对应在线增量不升，则不能称在线机制优化。
- Q相对M的配对效果必须逐fold/delay报告；若优势集中单折，或自然改善伴随受控恢复下降，不认定全面替代。
- 候选值得进一步比较的开发线索：自然 pooled 相对对应Frozen不负，受控 pooled相对对应Frozen为正；若要把Q升级为下一候选，至少一个regime的同分数Q−M pooled达到1pp，另一个regime不负，且两个regime各自5个leave-one-fold-out配对效果都不反号。极小正数只作描述，不达此资源线。此处是资源选择线索，不是显著性、最终gate或论文主方法替换。
- 若六格均无可靠线索，保留否定结果；下一步回到可预测headroom与greedy选择偏差，不扫计数/学习率找正数。

只有新方法在开发合同与现代baseline比较闭合后，才能另选未用于决策的数据确认。旧157个confirmation分子不能为新变体充当未读确认。
