# Context residual memory v1 独立审查

日期：2026-09-27

审查对象：`context_memory_method_card_v1.md`、`context_memory_replay_v1` 回放、`pharma_context_memory.py`、`run_pharma_context_memory_smoke.py`、`pharma_selection_stream.py`。

结论：候选方法的 selected-only 事件边界和延迟队列基本清楚；零训练回放的结果可以作为筛选证据。但当前数学定义还不能直接支撑“实时 JEV”主实验，主要原因是把 BT 排序分数当成了校准后的正确概率。完整 development 不应在这一定义下重跑。

## 必须先修的科学问题

### 1. BT score 不是正确概率（阻断）

`train_bt` 保存的是 `model.score.weight`。这个 head 的训练目标包含 provider 间的 Bradley–Terry 差分损失，且 `bt_cls` 的 score 没有 bias；它被约束来排序 provider，不能解释为

$$
P(r=1\mid x,a)=\sigma(w_a^\top h(x)).
$$

但 context memory 的写入正是

$$
 e_t=r_t-\sigma(w_{a_t}^\top h_t),
$$

然后把该 residual 直接加回排序 logit。BT 的共同平移不可识别，且 BT/CLS 混合损失没有给 score head 提供校准保证。因此同一条结果在不同的 score 平移下会产生不同 residual；这不是一个可解释的 correctness residual。当前 `auxiliary` CLS head 也没有被保存到回放证据，不能事后修正。

**最小修复：**先冻结共享表示，再用 fit 区标签拟合一个明确的 per-provider correctness logit（至少有 provider 截距，参数只在 fit/validation 选择），令它成为 `ell_a^0(x)`；保存该 head 的全部参数和校准配置。随后用 $q_a(x)=\sigma(\ell_a^0(x))$ 计算 $r_t-q_a(x_t)$，再把 residual 加到同一个 calibrated logit 上。若继续保留 BT head，应把它作为排序控制，不能把它的 raw score 当成 residual 基线。修复后才值得做新的方法回放。

### 2. 归一化约定在公式、代码和实验之间不一致（阻断前的定义问题）

方法卡把 $h$ 定义为归一化 hidden feature，并同时写 $\ell_a^0=w_a^\top h$。实际训练的 `build_features` 返回 ReLU hidden 的原始向量；`ContextResidualMemoryKernel._scores` 用原始向量算 base score，只在 cosine memory 读取时调用 `_unit`。正式零训练回放也使用该原始 hidden；A800 correctness smoke 则使用 L2 归一化 Morgan64 加随机 prior。三者不是同一个输入/评分合同。

**最小修复：**明确区分原始表示 $u(x)$ 与记忆查询 $z(x)=u(x)/\|u(x)\|$：base logit 用训练时的 $u$，cosine 只用 $z$；或者重新训练并冻结全程使用 $z$ 的 score head。方法卡、输入 manifest、snapshot 和测试必须采用同一条定义。A800 smoke 只能称接口/延迟 smoke，不能与 BT 回放结果合并成效果结果。

## 实验有效性与证据

### 3. 已有 5-fold × 2-seed × 3-delay 回放不需要在 A800 原样重跑

`replay_pharma_context_memory.py` 已在完整 development evaluation 流上执行了 30 条设置；重新在 A800 使用同一组冻结 features 和 prior，只会重复选择序列，不会提供新的科学信息。A800 smoke 使用随机 prior 和 Morgan64，能证明队列、反馈守恒和运行时开销，不能证明方法效果。

**建议：**修复校准 score 后再做一次新的离线 fit + 5-fold/2-seed/3-delay 回放；A800 至多保留一条 smoke 作为硬件/延迟证据。不要为了“跑满卡”重做旧回放。

### 4. 当前“独立恢复”在旧 smoke 代码中不是独立实现

旧版 `replay_and_validate` 重新实例化同一个 `ContextResidualMemoryKernel`，再次调用同一套 `_correction`、排序和写入代码。这是重复执行，不是独立 verifier；若实现和 verifier 同时有同一 bug，仍会通过。另一个实际问题是原 smoke 事件写入工作目录的 `events.jsonl`，stdout 只打印最后的短摘要，平台日志截断或容器结束后无法从 stdout 恢复逐条证据。

**最小修复：**使用不导入 kernel 的 NumPy reference verifier，从决策/反馈 JSONL 逐条重算 cosine、Top-$L$、residual、FIFO 和选择，并比对每条 decision、每个 release 和最终 snapshot；将原始事件分片打印到 stdout 或在任务结束时上传到已登记的 OSS 路径，再保存 sha256。修复后的 verifier 和证据需要在报告中明确标注版本。

### 5. 复杂度写法偏小

当前 `_scores` 对所有 $K$ 个 provider 都调用 `_correction`，即完整菜单下每个请求的读开销是 $O(KBd)$，不是方法卡中未加条件的 $O(Bd)$；只有先把候选菜单限制为 $m$ 个并只计算菜单 provider 时才是 $O(mBd)$。状态量 $O(KBd)$ 是对的。

**最小修复：**将复杂度写成完整菜单 $O(KBd)$、候选菜单 $O(mBd)$；若要主张 $O(Bd)$，代码必须按候选 provider 懒计算并记录 $m$。

## 需要在正式表中补清的口径

1. `system_success` 同时把工具失败和化学答案错误记为 0。它是“端到端调用成功”奖励，不等于条件化的 chemical correctness。正式结果至少分开报告 system success、valid return 和在成功返回上的 chemical accuracy，并在方法卡中固定 $r_t$ 的定义。
2. 独立 bias 的学习率 `0.1` 已记录，但必须在 development 预注册，并与其他在线方法按相同反馈时钟和同一 selected-only 约束比较；报告按 molecule/scaffold 配对的 fold 结果和不确定性，而不是只给三列均值。
3. 回放读取完整 `system_success` 行来取被选 arm 的 reward，当前逻辑没有把未选标签写入状态，符合 selected-only；为减少审查歧义，应移除只用于推导 provider 数目的 `y_fit` 读取，直接从 frozen prior 的形状确定 $K$ 并断言形状。

## 是否进入下一步

当前候选通过了“有局部 residual 信号”的筛选，但未通过“可解释的实时 JEV 更新”定义审查。下一步应是：先建立 calibrated correctness logit 和一致的表示合同，补独立 NumPy verifier/可下载事件证据，再做一次新的 development 回放。若校准后的 memory 仍在至少两个 delay 超过独立 selected-arm baseline，才申请 confirmation；否则保留为负结果并停止该主线。
