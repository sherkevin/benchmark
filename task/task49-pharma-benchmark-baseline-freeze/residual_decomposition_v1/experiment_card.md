# MIRA-Relay 残差来源诊断

日期：2026-10-07。状态：分析前固定；仅 development，未替换主方法。

## 目的与决定

对应方法论机制归因与 Gate C 的 E3。当前残差把已选样本的正确率与 fit 总体平均比较，两者的差可能同时包含预测误差和选样组成变化。本轮量化这两部分，决定下一轮是否值得比较不同中心化方式；不把影子动作解释成真实闭环效果。

最近已发表对照是当前 BayesianRouter-adapted 的 selected-feedback 更新；它已在共同协议中运行。本轮是 proposed 内部解释诊断，不新增 superiority 比较。Frozen 提供相同冻结分数，作为无更新对照。既有 FamilyGuard permutation 不能替代本诊断。

## 数学依据

固定 provider $a$，$n_a$ 是当前已释放的已选反馈数；$r_i$ 是其中第 $i$ 条正确性标签；$q_i$ 是选择该条样本时该 provider 的冻结 sigmoid 分数；$\bar r_a$ 是 fit 正确率。$n_0=20$。当前残差满足恒等式

$$\Delta_a=\frac{\sum_i(r_i-\bar r_a)}{20+n_a}=\underbrace{\frac{\sum_i(r_i-q_i)}{20+n_a}}_{\text{预测误差项}}+\underbrace{\frac{\sum_i(q_i-\bar r_a)}{20+n_a}}_{\text{分数组成项}}.$$

实例：A 的易/难分子真实正确率分别为 0.9/0.4，fit 各占一半，故 $\bar r_A=0.65$。若已释放的 20 个易分子中 18 个答对，且其 $q_i=0.9$，两项分别为 0 与 $(18-13)/40=0.125$。后续难分子的得分从 0.4 变为 0.525，可超过另一个得分 0.5 的 provider。这里没有环境漂移；这是反例推导，非实验数据。

任意共同加数 $c$ 都满足

$$\arg\max_a(q_a+c)=\arg\max_a q_a.$$

实例：同一菜单分数 $[0.4,0.5]$ 同加 0.125 后是 $[0.525,0.625]$，选择不变。固定菜单、无裁剪、同一平局规则下，shared-scalar 消融由代数决定，无需重复科学跑批。

实现使用 `LogisticRegression(class_weight='balanced')`；其 sigmoid 分数不保证是自然正确性概率。因此第一项只叫“相对冻结分数的误差”，第二项只叫“分数组成项”，不能直接把二者分别称作真实漂移与纯粹选样偏差。即使分数条件正确、释放不依赖结果，误差和是鞅也不等于随机分母下的残差比值无偏。

## 冻结输入、权限和资源

- 唯一数据目录：`experiments/logs/task49_provider_fresh_dynamic_diagnostic_20261006/v1/`，722 个 development molecules、5 folds、四个 fresh providers。
- `events.jsonl` SHA-256：`4a36d49cf7ca991616505e76d0baf9ba66489b5abbd3455ddf9408fee5ecbffb`。
- `summary.json` SHA-256：`30f9250c8a77bebf569d7eb24a2572c2142c59e6289f3cf3b15bd9bd35024ffc`。
- `config.json` SHA-256：`df5f97edb10e44335728e4eaf25053d3eda68c20f85a80928fd6df8bed6e63ab`。
- 主分析固定 seed0、全部 folds、全部 $D\in\{0,1,4\}$，共 15 条 MIRA stream、2,166 次选择。同一分子跨 delay 重复，不当作独立样本。
- Frozen decision 已记录全部候选的 fit-time logits。只从这些合法 pre-choice 分数求 sigmoid，不读取未选 provider 输出，不重新加载 checkpoint，不读取 confirmation。
- seed1 只核对确定性与重复计数边界；不为扩大样本量重复计入主指标。
- 本地 CPU，单次分析，预计不足 60 秒；无 GPU、训练、API、新 provider 推断或超参搜索。

## 分析与指标

1. 将 Frozen 与 MIRA 按 fold/seed/delay/entity/tick 对齐，核对菜单/feature hash。独立从已释放 selected rewards 恢复 $n_a,s_a$；检查 MIRA 原始分数逐项等于 $q_a+\Delta_a$，以及动作等于 argmax。
2. 按真实释放次序累计两项，校验恒等式；记录每次 decision 的各 provider 计数、两项、总残差、原动作和假设去掉组成项时的 shadow argmax。
3. 报告分数重构最大误差、恒等式最大误差、各折/延迟的实际动作与 Frozen 差异数、shadow 与实际动作差异数、两项绝对值及 signed mean，另报告 released-selected 分数均值/正确率/Brier 作为校准诊断。
4. shadow 只用同一真实轨迹的已释放标签，**不计算 shadow accuracy、counterfactual gain 或 closed-loop reward**；它改变动作后本应获得哪些标签，当前分析无法回答。

## 否证与后续规则

- 任意源 hash 不符、绑定/时钟/菜单不符、反馈重复或遗漏、分数重构/恒等式误差 $>10^{-10}$：停止解释，保留失败，先查实现。
- shared scalar 改变排序：只查浮点/平局实现，不宣称科学现象。
- 若 shadow 与实际所有动作相同：本面板没有分数组成项改变动作的直接证据，不扩大该假设。
- 若有差异：支持进一步调查机制归因；不证明新算法更优。下一步先解决冻结分数校准，再预登记开发集闭环中心化对照。
- 不基于此结果改变已报告 confirmation 方法、超参或表格；任何候选更新需要新实验卡和新的未使用确认数据。

## 留痕

分析脚本、环境、源码/输入/本卡 hash、命令、开始结束时间先写 config；逐事件写 JSONL；成功/失败写 summary；本目录 `report.md` 记录实测结果与下一步。所有旧结果继续保留。
