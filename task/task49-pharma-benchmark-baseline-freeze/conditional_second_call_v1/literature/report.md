# Online Cascade Learning 的反馈合同审查

日期：2026-10-08。范围：只读原文与源码审查；无效果实验、无训练、无主稿修改。比对对象为上一层 `design_review.md` 的二轮审查版本；输入哈希见 `source_manifest.json`。

## 结论与适用边界

**Online Cascade Learning（OCL）已经实现按需付费获取末级模型标签、在线更新小模型与 defer gate、在成本与分类性能间取舍。Cascade Routing（CR）已经实现根据已见输出估计后续模型价值并决定继续或停止。** 因此，第二次调用、在线更新、选择性取标签、输出条件决策本身都不是本候选的新贡献。

OCL 的标签是末级 LLM 的预测，理论中假定末级预测的最大概率类别总是正确；它不是外部实验真值的延迟释放。其被学习对象包含小模型本身；候选则冻结四个 provider 和终止规则版本，只学习“对同一个冻结终止规则，第二个实际输出减少多少外部真值损失”。这两者在目标和更新对象上有实质差别。

**在本次核读的两篇原文中，未找到“独立真值在终局后延迟释放 + 仅对付费取得的 selected pair 更新 + 固定 verifier 版本的 rescue/harm/refusal gain + 显式 propensity 校正”这一完整合同的算法或证明。** 这是已审来源的范围判断，不是文献空白证明。标准选择性标签学习、延迟 bandit 和价值信息方法仍可能覆盖组合；不能据此宣布原创性。需要的强对照仍是 **CR + 相同 delayed updater**，并明确区分学习目标差异与一般在线更新收益。

## 原始证据

- OCL 正式会议页：[PMLR 235 / Nie et al.](https://proceedings.mlr.press/v235/nie24a.html)。正式 PDF 20 页：[publisher PDF](https://raw.githubusercontent.com/mlresearch/v235/main/assets/nie24a/nie24a.pdf)，已保存 `sources/online_cascade_icml2024.pdf` 及按版面提取全文 `.txt`。本文页码为 PDF 内页码 1–20。
- 作者官方代码：[Flitternie/online_cascade_learning](https://github.com/Flitternie/online_cascade_learning)，固定提交 `285e03fc98afabdf7768558d33ee7b5f662b5f19`（2025-04-21），完整 source archive 与解压副本已保存。它晚于 ICML 2024；只能证明该固定版本的实现，不能不加说明当成投稿时源码。
- CR：[A Unified Approach to Routing and Cascading for LLMs, arXiv:2410.10347v3](https://arxiv.org/html/2410.10347v3)，本地既有 `task/task40-pharma-nearest-method-audit/evidence/cascade_full.html`，文首版本为 2025-05-22。本轮复制原文件，并抽取 §3、§4、Appendix D.3；稳定 HTML anchors 为 `#S3`、`#S4`、`#A4.SS3`。
- 获取缺口和失败保留在 `retrieval_log.jsonl`。OpenReview PDF 403，但正式 PMLR PDF 已完整获得。未声称运行官方代码或复现其数字。

## 从第一原则看两个问题

终止决定需要比较“现在停止的期望损失”和“付费获取新信息以后再停止的期望损失”。这就是候选的 G_b(E)；该决策原则本身是标准价值信息。OCL 的起点是高速流上 LLM 太贵：把被调用的 LLM 同时当答案提供者和教师，逐步训练较便宜模型来代替它。其成本排序、小模型蒸馏和末级教师都服务于这一需要（pp.1–3）。

当前医药合同的四个 provider 是冻结、都可能出错的外部预测器，独立真值另行到达。直接借用 OCL 的“末级教师正确”会消除第二次调用造成损害这一重要可能性，也会把工具预测替代实验真值。可借用 gate、selective acquisition 和流式更新结构；不能借用标签语义和保证而不改名。

## OCL 的完整执行合同

| 要素 | 原文/源码事实 | 与候选的关系 |
|---|---|---|
| 状态与动作 | §2，pp.2–3：状态为输入 x 与级数 i；动作是类别标签或 defer；固定顺序的小模型直到末级 LLM。没有单独的拒答动作。 | 候选可在多个未调用 provider 中选一个，且停止输出 0/1/拒答。两者动作空间不同。 |
| 调用前信息 | §2–3，p.3：gate f_i 读取本级模型的类别概率向量 m_i(x)。 | 已经是输出条件决策；不能称其只看输入或边际准确率。候选还可读取合法 E 中的分子与 provider ID。 |
| 第二次调用 | Algorithm 1，p.4：defer 进入固定的下一模型；有衰减的 β_t 概率直接跳末级教师。 | 已有探索性付费取标签。固定顺序不等于候选的动态 next-provider 选择。 |
| 标签来源 | §3，p.3：末级 m_N 的 argmax 假定等于 y_t。Algorithm 1，p.4：只有到达 m_N 时把 (x_t, ŷ_t) 加到 D。 | selected teacher labels 已解决；独立实验标签与 paired loss difference 不同。 |
| 停止与答案 | Algorithm 1：本级不 defer 或已到 m_N 时输出本级 argmax 并结束。 | 没有共享冻结 verifier 融合两输出，也没有 refusal loss ρ。 |
| 成本 | §2，p.3：预测 loss 与每次 defer 的 μ c_{i+1}；模型计算成本假定固定且按级递增。Table 1，p.6：预算以最大 LLM 调用数 N 衡量。Appendix C.1，pp.18–19：讨论小模型推断及训练成本。 | 所有 provider 同价且最多两调用的平均预算 1.25 是新增评测约束。不能套用“只计教师次数”的账。 |
| 小模型更新 | Algorithm 1，p.4：先输出本题预测，随后基于 D 用 OGD 更新 m_1…m_{N−1}；末级教师不更新。 | 候选冻结 provider，只更新价值估计；不能称冻结 provider 的 gate-only 适配为完整 OCL。 |
| Gate 更新 | §3，pp.4–5：总 cascade loss J 加在线 confidence calibration；Eq.(5) 的校准 target 为本级 argmax 是否不等于教师 label，MSE，推断阈值 .5。 | 这是教师分歧/当前误差 target，不是相同 d_v 下继续与停止的终局效用差。 |
| 时间合同 | Algorithm 1 在本 query 结束后更新；没有外部标签 release time、pending queue 或 D∈{0,1,4}。源码模型训练有 batch 等待；gate 在 selected teacher 分支同轮更新。 | batch 等待不是独立真值的延迟可见合同。加入 release queue 是适配，不能归为原文已有实验。 |
| 选择概率 | Algorithm 1 有 β 探索；原文和所核代码没有记录每个 pair 的 observation propensity 并作 1/π 加权。 | β 调教师与候选 π_{t,b} 的所有选择路径总概率不能混同。概率为正也不保证短流样本充分。 |
| 理论 | Theorems 3.1–3.2，p.4；Appendix A，pp.12–15：凸可微损失、有界模型空间/梯度、η_t=t^{-1/2}；Lemma A.2 使用损失与 deferral probability 的收敛论证。 | 不能直接搬到非凸 gate、固定有误 provider、refusal、延迟和 IPS paired target。附录没有本合同的 delay/propensity 项。 |

理论允许为省成本而容忍小模型错误；并非要求每次保持教师正确率。实验也承认 LLM 标签有噪声（p.5），表中 LLM 对数据真值准确率低于 100%，但训练仍把它作 reference label。这一区分避免把理想化教师假设误读成实验确有无误真值。

在最简两级、第二级真正完美、无拒答的情形，第二次调用收益退化为 P(首级错误 | E) − κ。当前候选允许 B 同样有错并改变冻结 d_v 的决策，其收益可包含 rescue、harm 和 refusal 转移。更一般的 target 不自动构成新算法。

## 代码交叉核验与直接复用风险

以下路径均相对 `sources/online_cascade_code/`，链接可定位固定原仓版本。

1. [`cascade/online.py:101–122`](https://github.com/Flitternie/online_cascade_learning/blob/285e03fc98afabdf7768558d33ee7b5f662b5f19/cascade/online.py#L101)：模型 cache 满时在下个 item 开始训练；最少模型更新次数小于 10 时强制 LLM warm-up，教师 label 加入所有小模型 cache。当前约 145 eval 分子/折的短流不能不计 warm-up 费用照搬。
2. [`cascade/online.py:176–207`](https://github.com/Flitternie/online_cascade_learning/blob/285e03fc98afabdf7768558d33ee7b5f662b5f19/cascade/online.py#L176)：**先调用所有 wrapper 得到全部小模型概率，再选择停止点**；decay 为 `decaying_factor ** model_update`。这与 paper Algorithm 1 的逐级执行存在实现差异。固定 provider 对照必须把计算移入合法动作分支，不能免费读取其余 provider。
3. [`cascade/online.py:219–252`](https://github.com/Flitternie/online_cascade_learning/blob/285e03fc98afabdf7768558d33ee7b5f662b5f19/cascade/online.py#L219)：仅 LLM 参与分支读取 `llm_label` 并更新 gate；`item['label']` 用于指标。校准 target 实际为 `min(disagreement + calibration, 1)`，与 Eq.(5) 的纯 0/1 有差别；gate 用 Adam（line 34），非理论的 η_t OGD。不能以源码有真值字段推断训练用了独立真值。
4. [`models/wrapper.py:33–61`](https://github.com/Flitternie/online_cascade_learning/blob/285e03fc98afabdf7768558d33ee7b5f662b5f19/models/wrapper.py#L33)：概率向量 → 128-unit ReLU → sigmoid；底层 prediction 在 `no_grad()` 中。这个小 gate 结构可借鉴为低维基线。
5. [`data/__init__.py:47–76`](https://github.com/Flitternie/online_cascade_learning/blob/285e03fc98afabdf7768558d33ee7b5f662b5f19/data/__init__.py#L47)：实时 LLM wrapper 在整数索引取 item 时即调用 `predict`；[`run.py:23–29`](https://github.com/Flitternie/online_cascade_learning/blob/285e03fc98afabdf7768558d33ee7b5f662b5f19/run.py#L23) 将其包装后送入 online loop。故**实时模式的 API 调用可能早于 gate 选择且覆盖所有 items**，`llm_acted` 是逻辑选中数，不能直接当真实 API 账。本轮为静态代码推断，未执行该路径。
6. `run.py:29` 把整个 test stream 文本给 TF-IDF corpus，`models/sklearn.py:43` 拟合词表；当前 fit-only 特征合同必须另做限制。

已保存完整代码供审计；GitHub metadata 的 license 为 null，源树无 LICENSE。可直接复用算法描述与接口思想；若要复制源码进入公开实现，许可证资格仍待核清。本次没有安装或运行其依赖。

## CR 的最近邻强度与两次调用映射

§3 “Quality and Cost” 明确允许估计依赖已见答案，supermodel 成本为成员成本之和，质量为 E[max_l q̂_l]，并通过 validation 估计不确定性。§4 Algorithm 3 从包含已计算模型的 supermodels 中选择；没有新增成员就 stop，否则计算所选集合中尚未算过的最便宜模型。不能把 CR 降格为固定下一个模型的置信阈值。

在本候选已有 A、至多再调用一次时，合法集合就是 {A}、{A,B}、{A,C}、{A,D}；这仍是 Algorithm 3 的直接截断。保留输出条件质量估计、估计不确定性和预算参数 λ，fit/validation 选参数。Appendix D.3 的 quality features 包括已计算模型的 confidence、entropy、agreement 和 JS divergence，并为“已计算历史 × 目标模型”分别训练 logistic regression。**决定是否调用 B 之前不能用 A–B agreement/JS；那时 B 未见。** 二次输出已获得后，这些才可用于终局判定/目标记录。

Appendix D.3 是离线 train/validation/test 合同，未给跨 episode 的 selected-only delayed update。将普通 SGD/replay delayed updater 加给 CR 是必须的强适配。

须区分两种诚实标注：

- **CR-native-2call**：保留原文 E[max q̂] 的质量聚合及选用已有最佳估计答案的语义，给出其原合同和最终 utility；refusal 若作为虚拟终止选项加入，应明确是扩展。
- **CR-utility-adapted-2call + delayed updater**：所有候选共用冻结 d_v；把集合质量映射为 `−E[loss(d_v(E, O_b), Y) | E]`，与 stop 的 `−E[loss(d_v(E),Y)|E]` 比较，增量成本 κ_b。这使固定 verifier/refusal 的目标公平，但已更改原文 supermodel 质量定义；只能叫同任务适配。它与候选的 G_b(E) 在决策形式上等价，若学习器、特征、feedback 也相同，不能只换名称比较。须明确新机制究竟改变什么。

## 最接近且可实施的 OCL 适配

**原样 OCL 没有当前固定 provider 池下的完全忠实实现。** 其核心是在线蒸馏小模型；冻结 provider 已取消该学习对象，外部 Y 又改变教师语义。为了不因不兼容而跳过近邻，建议保留两个级别的合同说明：

1. **OCL-native 参考合同**：有可训练小分类器和指定末级 teacher，顺序固定；只在付费 teacher 调用上收集 label，更新分类器和 gate，原始无拒答。这可作为另一个任务的复现；不能混入当前四 checkpoint 固定 provider 的同任务主表。
2. **OCL-gate-only-delayed（当前可用的受限适配）**：首次 A 沿用 Corrected Frozen；对每个 A 的后继 b_A 仅从 fit/validation 固定，形成两级序列；gate 用已见 A probability 向量，冻结 providers，保留“当前错误/分歧校准 + 追加调用成本”的 gate 思路。冻结共同 d_v；在选择并支付 B 后，只在登记 release time 才更新。若用外部 Y 的 stop-error target，明确替换了 teacher label；若仍用 A–B disagreement target，明确它学习的是 disagreement 而非科学正确性，且延迟是公平性附加限制。所有 paid audits、warm-up、decay 和训练预算与对照登记一致。

该受限 OCL 不是最强竞争者：它约束后继为固定顺序、主要估计 stop error；强竞争者仍是 CR 的输出条件价值估计并配相同 online learner。若将 OCL gate target 也换成 g_t^v，并允许动态 b、相同 d_v、相同 IPS/SGD，那么它已经变成候选本身的等价重述，不能包装为独立基线。

## 可直接复用的资产与下一步

已实际落盘：正式全文 PDF/文本、官方代码固定提交完整 tarball/源码、会议 metadata、CR 原文快照和相关章节文本、逐文件 SHA256 与获取失败记录。可复用的具体组件是小 gate 结构、教师调用后更新的控制流参照、cache/batch 超参表、在线 label-acquisition 成本概念和 CR 的合法集合枚举。

下一步先在执行卡固定：共同 d_v 与版本哈希；各 baseline target；可见特征；全路径 pair 选择概率；调用/真值/terminal/release 的四个时点；实际调用账；fit-only 停止与预算参数。没有 paired observation 的 episode 不补 B 输出；未释放 Y 不构造 target；同一 Y 不能先更新 verifier 再倒算收益。

本轮支持“继续做合法同信息适配与小样本可学性检查”，不支持“delayed paired feedback 已被证明新颖”或“已证明有效”。整体 TODO_LOG 由父代理追加，避免并行写入冲突。
