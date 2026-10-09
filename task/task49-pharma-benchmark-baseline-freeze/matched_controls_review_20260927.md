# Frozen / Online-SGD / Replay-SGD 下一小任务审查

日期：2026-09-27
范围：仅审查现有 `inputs.json`、`train_bt`/`run_stream`、online adapter，未修改代码，未运行 Nebula。本文是临时审查稿，待 owner 决定是否转为正式实验卡。

## 结论

可以做一个很小、可解释、不会打开 confirmation 的 development 诊断。三种控制共享每个 outer fold/seed 的同一个 `bt_cls` fit checkpoint：冻结的 64 维 encoder、`score.weight` provider head、validation 选出的 epoch、evaluation 到达顺序、反馈 delay。在线阶段只允许使用当前流已经释放的 selected `system_success`。

最小实现不需要重新训练 backbone，也不需要 Laya。先把 `train_bt` 返回的 `score.weight` 和共享 hidden `features` 接到三个 head-only policy：

- Frozen：只做 `argmax(score.weight @ h)`，反馈只消费不更新。
- Online SGD：每条 selected feedback 到达后，对被选 arm 的一行做一次 BCE-with-logits 梯度步；encoder 和未选 arm 行冻结。
- Replay SGD：固定大小 FIFO buffer，只保存已经释放的 selected `(h, arm, reward)`；每次释放用“当前样本 + 从旧 buffer 均匀抽取的历史样本”做一次固定 batch 梯度步，更新仍只作用在每个样本对应的 arm 行。

这一步的目的只是把“普通 head 更新”从 BayesianRouter negative baseline 中隔离出来。它不能证明 Proposed 创新，也不能打开 confirmation。

## 统一数学接口

对每个 fold/seed，从 `train_bt` 的 validation-best checkpoint 取得共享隐藏向量 $h_t\in\mathbb R^{64}$ 和 provider 权重 $w_a\in\mathbb R^{64}$。选择分数为 $s_{t,a}=w_a^\top h_t$，菜单暂时固定为全部 33 个 provider；若未来改为逐题菜单，三种策略必须接收同一冻结菜单。

反馈到达时仅有 $(h_s,a_s,r_s)$，其中 $r_s$ 是所选 provider 的 `system_success`。Online SGD 的单步目标是

$$
\ell_s=\operatorname{BCEWithLogits}(w_{a_s}^\top h_s,r_s),\qquad
w_{a_s}\leftarrow w_{a_s}-\eta\,[\sigma(w_{a_s}^\top h_s)-r_s]h_s.
$$

`h_s` 必须是作出该决策时复制的冻结向量；延迟期间不重新编码、不读取该题未选输出。Frozen 的 `observe` 只释放 pending 记录，不修改状态。

Replay 在释放时先用当前样本和释放前 buffer 中最多 `B-1` 条历史样本构成 batch，再执行一次平均 BCE 梯度；之后把当前样本放入 FIFO，超过 `B` 时丢弃最旧样本。这样当前标签只在合法释放后进入模型，buffer 永远不含 fit 区之外未释放的标签。

## 关键公平性选择

1. 初始 head 必须来自同一个 `bt_cls` checkpoint；不能让 Online/Replay 重新用 evaluation 全面板预热。
2. encoder 和 auxiliary head 均冻结；只更新 selected provider 的 `score.weight` 行。这样三者的可训练参数和输入相同，差别只有是否更新及是否回放。
3. Online 每次释放固定 1 个梯度样本；Replay 预注册 `B`、batch 上限 `b`、采样 seed 和学习率。建议第一轮用 `B=64,b=8,steps=1`；它计算更重，必须同时报告 update p50/p95，不得把它当作与 Online 同成本。
4. 反馈延迟沿用已完成诊断的 `d∈{0,1,4}`，同一 stream 顺序和同一 fold/seed；不同策略因选择不同而得到不同 selected labels，这是协议允许的，不应强行对齐 label 序列。
5. SGD 学习率、Replay `B/b/seed`、梯度是否裁剪、weight decay、是否含 bias 必须在看到结果前写入配置。第一轮不做结果后调参。

## 不泄漏 confirmation 的检查

- 输入仍为 `fingerprint_diagnostic_v1/inputs.json`；五个 scaffold-disjoint outer evaluation 流只作 development online stream。confirmation 不在文件、命令、环境变量或日志中出现。
- `train_bt` 的 fit/validation 监督可以使用全 provider panel，且三种策略共同使用；evaluation 只在终止后生成 selected reward。不要把 `rows[idx]['system_success']`、truth、未选 output 传进 `decide`。
- 事件日志分开 `decision`、`terminal`、`feedback`、`update`；`update` 的输入只写 event/provider/reward/feature hash，不写未选矩阵。恢复程序从 frozen input 重算 selected reward，并检查 update 次数等于 released feedback 次数。
- 不以 evaluation accuracy 选择 epoch、学习率、Replay 大小或方法。evaluation 只生成按预登记配置的轨迹结果。所有方法跑完后才可以写开发总结；不以此轮结果决定 confirmation 样本。
- 真实失败保留 reward 0；unresolved truth 仍排除并停止/报错，不能改成 0。

## 最小执行顺序

1. 先本地实现纯 NumPy head policy 和 delayed pending tests：分数、梯度、FIFO、exactly-once、`feature_sha256`、菜单一致性。
2. 在现有 722 行 fixture 上运行本地 adapter smoke，只核对事件守恒与状态递推，不写效果结论。
3. 给 A800 任务加完整 Nebula run contract，先提交一条小 smoke（1 fold/1 seed/1 delay/少量 evaluation）验证入口，再提交完整 development 诊断；不要同时开多条任务。
4. 恢复完整 JSONL，核对每个 fold/seed/delay 的 Frozen/Online/Replay 三条轨迹、更新次数、初始权重 hash、source hash 和 confirmation closed。
5. 只有通过恢复和信息权限检查，才在报告中比较 cumulative error、prefix/suffix accuracy、update/selection latency；结果为负也保留。

## 失败条件与停止线

- 任何策略在 decision 前读取完整 provider panel、truth、evaluation 标签或未来反馈：立即作废该运行。
- `update_count != released_selected_feedback_count`、pending 不守恒、不同方法初始 checkpoint/hash 不同：不比较效果，先修实现。
- Frozen/Online/Replay 与当前 BayesianRouter 共用 Morgan hidden；如果三者都不超过 Best Fixed，结论应是当前表示/任务信号不足，不能继续堆普通 head。
- 若 Replay 只因额外 batch 计算获得收益，必须把它列为更高计算预算 baseline；不能把收益归因于“实时学习机制”。

## 可复用与尚缺

本轮匹配 head 默认沿用前一轮 BayesianRouter 的 `score.weight` BT provider logits；`bt_cls` 中另有 `auxiliary.weight` BCE head，不能在看到结果后切换。因而 Online/Replay 的语义是 BT warm start 后切到 selected BCE 梯度更新，若要研究 BCE warm start，需另登记消融。

可直接复用 `train_bt` 的 5-fold/2-seed scaffold split、`run_stream` 的 delay queue、现有 recovery 事件模式和 Nebula contract。当前仓库没有这三种 head policy 的统一实现，也没有对应结果日志；PILOT/BayesianRouter 的随机 prior smoke 不能替代它们。本文未提交代码、未申请 A800。

## 需要 owner 明确的一项 objective 口径

当前 `bt_cls` checkpoint 有两个 provider 输出：`score.weight` 用 BT pair loss，`auxiliary.weight` 用 BCE。既有 BayesianRouter diagnostic 和静态评估都用 `score.weight`。因此本审查建议为了 matched continuity 先沿用 `score.weight`，但必须在实验卡写明“从 BT warm start 切到 selected BCE 更新”，不能称为 BT 目标的精确在线后验。

若希望把 JEV 明确定义成 correctness logit，则应在开跑前改为使用 `auxiliary.weight` 作为三种策略共同的初始 head，并固定其 argmax/selected BCE 口径；这会是一个新的 matched-head 诊断，需要重新做训练/恢复，不能和当前 BayesianRouter `score.weight` 结果直接混表。两种 head 不得在看结果后切换。
