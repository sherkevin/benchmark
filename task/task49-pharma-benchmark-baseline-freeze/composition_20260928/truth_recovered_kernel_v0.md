# Truth-Recovered Structured Kernel v0

状态：CPU 接口原型；未跑 A800，未做效果结论，未进入 confirmation。

## 目的

把候选方法卡中最小的 selected-only delayed-feedback 机制落成可测试接口。固定
特征向量 $v=f(x,e)$，选择前只读当前题目和 provider 菜单。成功调用后先绑定
被选 provider 的二元输出 $z$，直到 correctness 反馈释放时才更新状态。

## 模型和更新

共享性质头为 $q(v)=\operatorname{sigmoid}(\theta^\top v)$。provider $a$ 的条件正确性头为
$h_a(v,y)=\operatorname{sigmoid}(\phi_a^\top[v;y])$。调用前分数使用

$$s_a(v)=q(v)h_a(v,1)+(1-q(v))h_a(v,0).$$

如果输出状态为 `ok`、标签 $z\in\{0,1\}$ 且释放的 correctness 为
$r\in\{0,1\}$，则恢复标签为

$$\tilde y=z\quad(r=1),\qquad \tilde y=1-z\quad(r=0).$$

释放时只对共享头和本次选中的 provider 头做一次梯度更新：

$$\ell=\operatorname{BCE}(q(v),\tilde y)+\lambda\operatorname{BCE}(h_a(v,\tilde y),r).$$

系统失败没有 $z$，unknown 没有可信 $r$；两者只被消费并记录，不生成
$\tilde y$，也不更新状态。未选 provider 的当前输出不会传入接口，也不会更新其头。

## 接口边界

`decide(event_id, feature, query_id, candidates)` 返回选择记录并保存不可变特征快照。

`bind_selected_output(event_id, {status, label})` 只保存被选工具的终止输出；若输出中带有
`provider_id`，必须与 event 中的 selected provider 一致；`status=ok` 必须带二元 label，
失败状态必须带 `label=None`。

`observe(event_id, provider_id, chemical_correct, feedback_status)` 是唯一状态释放边界。
`feedback_status` 取 `scored`、`unknown`、`failure`。它检查 event/provider 身份、输出
状态和 correctness 语义，成功 scored 才更新并递增版本号，所有事件只能消费一次。

实现位于 `src/probe_headless/truth_recovered_structured_kernel.py`，与现有
`pharma_online_baselines.py` 的 Gaussian kernels 独立，不改变现有接口。

## 本地 delayed replay adapter

`src/probe_headless/truth_recovered_selection_stream.py` 新增独立的
`TruthRecoveredFeedbackQueue` 和 `run_truth_recovered_fixture_stream`。队列在
`complete` 时只绑定 selected output，并把 correctness 私存到 due tick；调用
`release` 才将 `chemical_correct` 传给 kernel 的 `observe`。delay 仍按
`due_tick = decision_tick + 1 + delay` 计算，因此 delay 0 也要到下一 tick 才更新。
绑定前会把 response 中的 `chemical_correct` 和 `feedback_status` 字段剥离，kernel 在
释放前只能看到 `provider_id/status/label`。

`src/probe_headless/truth_recovered_bbb_runner.py` 在此队列上复用了真实的
`ReplayEpisode`、`FUNCTION` 和 `TruthStore`。它只从当前 selected provider 的
episode transcript 提取 `status/label`，并在 evaluator 完成终止评分后将结果分为
`scored`、`unknown`、`failure`；三者都先排队，只有 release 才把 scored 的
`chemical_correct` 交给 kernel。unknown 和失败样本继续保留在 stream 中，但不更新状态。
terminal 日志不含 `chemical_correct`、`recovered_label`、truth 或完整 provider panel。

fixture 每个 task 只读取被选 provider 的 response；未选 provider 的 response 不会
进入 learner。`failure` 和 `unknown` 事件会守恒地释放，但 `recovered_label` 为 null、
版本不递增。真实 BBB runner 已在冻结的 replay cache/TruthStore 上完成 8 个样本的
selected-only 合约回放；该回放只验证边界，不提供效果结论。

## 测试覆盖

`src/probe_headless/tests/test_truth_recovered_structured_kernel.py` 覆盖：

- 全概率分数、首个候选平局规则和 deterministic replay；
- 延迟绑定和释放时序；
- 四种 $z,r$ 恢复组合；
- failure/unknown 不更新且不伪造标签；
- 只更新共享头和 selected provider 头；
- 错 provider、未绑定输出、重复消费和非二元标签拒绝。
- adapter 的 delay 0/1/4、释放前不更新、反馈守恒、failure/unknown 语义和
  selected-only response 读取。
- FixtureCache/TruthStore 上的真实 ReplayEpisode runner、未选 provider 不读取以及
  terminal/feedback 日志字段边界。

这些是接口和信息合同测试，不是 benchmark 效果实验。下一步才是为候选方法预注册
CPU 效果实验，与 Frozen、Matched Online SGD、Truth-only、Correctness-only 和
No-integration 做同一信息合同下的对照；在此之前不打开 confirmation，也不能从
evaluator 的完整 provider panel 回填标签。
