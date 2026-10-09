# Action-supported failure replay：指标修正版 v2 预注册

日期：2026-09-30
状态：预注册；本文件登记后才运行 v2 metric smoke。它不回写 v1 的主结果，也不把 v1 的已见 raw events 当作确认集。经只读指标审查补充反馈释放前/后的分段定义，详见 `metric_review_v2.md`。

## 1. 为什么需要 v2

v1 的 `post` 从 event tick 开始计分。event tick 被协议强制注入一次 runtime failure，因此 Frozen、streak-only 和 relative-reliability 即使事件后完全恢复，也会共同损失一个被注入样本的正确率。这个下降是 intervention 的直接代价，不能单独作为 recovery failure。

v2 保留 v1 的 raw receipt 和原始判断，同时把两个 estimand 在运行前固定下来：

1. **injected-terminal loss**：被注入的那一次终端失败造成的配对损失；
2. **recovery-only**：从 event tick 后一项开始，策略如何恢复、是否持续避开故障 target。

v2 是指标修复和边界验证，不改变输入、provider menu、方法、超参、target manifest 或反馈权限。

## 2. 固定协议

- 输入、20 个 fit/feature snapshots、target manifest、5 folds、2 seeds、delay `{0,1,4}` 与 v1 完全相同。
- target 仍是每个 `(method, fold, seed, delay)` 的 no-event prefix event-tick incumbent；transient 与 persistent 使用同一 target。
- 事件仍只改变 selected target 的 runtime status；runtime failure 的 chemical correctness 与 chemical label 均为 `null`，不得写入 correctness ledger。
- feedback due tick 仍为 `decision_tick + 1 + delay`；只允许 selected provider 的反馈。
- v2 不读取 event tick 之后的结果来选 target、选方法或调参。
- v2 smoke 只用于检查指标可计算性和事件边界；完整 240-stream v2 replay 需要另行登记，confirmation 继续关闭。

## 3. 预先定义的估计量

令 $t$ 为 event tick，$y^{E}_{s,i}\in\{0,1\}$ 为事件流中第 $i$ 个 decision 的 chemical correctness，$y^{C}_{s,i}$ 为同坐标 no-event control 的 correctness；runtime failure 的事件流样本没有 chemical correctness，按协议记为 $0$ 只用于“任务结果”统计，不把它写成化学标签。$s$ 表示一个固定的 `(method, fold, seed, delay, scenario)` 配对。

### 3.1 注入终端损失

只报告 event tick 的配对任务损失：

$$L_{\mathrm{inj},s}=y^{C}_{s,t}-\mathbb{1}[\text{event terminal at }t\text{ is chemically correct}]$$

在当前协议中，event terminal 在 target 上必为 runtime failure，因此第二项恒为 $0$；同时报告控制流在该 tick 的 $y^{C}_{s,t}$，不把它假设为必然正确。该量回答“本次强制 runtime failure 使这一项任务损失了多少”，不回答“策略是否恢复”。

### 3.2 反馈释放前的盲区

令 failure feedback 的 release tick 为 $R=t+1+delay$。反馈释放前的窗口为
$W_{blind}=\{t+1,\ldots,R-1\}$；当 `delay=0` 时为空，必须记为 `NA`，不能填零。定义：

$$\Delta_{blind,s}=\operatorname{mean}_{i\in W_{blind}}(y^{E}_{s,i}-y^{C}_{s,i})$$

它只描述反馈到达前的不可适应窗口，不能归因给更新规则。

### 3.3 反馈释放后的恢复段

恢复段从 $R$ 开始，到该 stream 最后一项结束：

$$\Delta_{adapt,s}=\frac{1}{n_s-R}\sum_{i=R}^{n_s-1}\left(y^{E}_{s,i}-y^{C}_{s,i}\right)$$

当 $R\ge n_s$ 时记为 `NA`。transient 的主要恢复指标是 $\Delta_{adapt}$；persistent 的 $\Delta_{adapt}$ 衡量收到失败反馈后能否离开退化 target。`post_recovery` 可保留作描述性旧指标，但不能替代按 release tick 切分的主量。

### 3.4 故障 target 暴露

令 $q_s=R$ 为第一次 failure feedback release tick，$a_{s,i}$ 为第 $i$ 项实际选择的 provider，$p_s$ 为 target provider。报告：

$$E_{\mathrm{target},s}=\frac{\sum_{i=q_s}^{n_s-1}\mathbb{1}[a_{s,i}=p_s]}{n_s-q_s}$$

如果 failure feedback 尚未释放，则该 stream 的 target exposure 标为 `not_released`，不得用零填充。另报告 `release_to_switch`；没有切换时记为 `censored_at_end`。

### 3.5 配对不确定性

每个 method/scenario/delay 的主差值以 10 个 `(fold, seed)` 配对为基本观察单元。主区间预先固定为按 5 个 fold 聚类的 paired bootstrap：每次重采样 5 个 fold，并保留被抽中 fold 的两个 seed，再对 10 个 slot 等权求均值；20,000 次，使用 NumPy `PCG64`。seed 固定为 `20261000 + 1000*I[persistent] + 100*method_id + 10*delay`。报告均值、95% percentile interval 和每个 pair 的原始值。不得按 molecule 重新抽样，也不得在看到区间后改变 bootstrap 单元。5 个 scaffold cluster 的区间只作配对不确定性描述，不声称正式显著性。

## 4. 预先固定的解释与反证条件

- manifest prefix、event-tick action、raw schema 或独立 state/action replay 任一失败：engineering failure，不解释效果。
- transient 的 $\Delta_{adapt}$ 相对同坐标 control 低于 `-1 pp`，且在至少两个 delay 的 bootstrap 区间上界仍低于 `-1 pp`：relative-reliability 的偶发故障保守性不成立。单独的 $L_{inject}$ 触发不能算作该条件。
- persistent 的 $\Delta_{adapt}$ 相对 streak-only 低于 `-1 pp`，且在至少两个 delay 的 paired 区间上界仍低于 `-1 pp`：relative factor 没有保留持续故障恢复能力。
- 若 transient 的 `post` 下降而 $\Delta_{adapt}$ 不下降，只登记为 injected-terminal loss；不得把它判为恢复失败。
- target exposure 只作为行为读数。若 persistent 中 relative 的 exposure 比 matched 高至少 5 pp，且 matched 的 $\Delta_{adapt}$ 比 relative 高至少 1 pp，则登记为独立 runtime-failure response channel 缺口，不能用 chemical correctness 反馈解释 runtime failure 的即时处理。

## 5. 结果边界

- 本文件不能改变 v1 已观察到的 persistent relative < matched 事实，也不能把 relative 包装成已优于 matched。
- v2 smoke 通过只说明指标实现和边界正确；不能支持方法有效性或 A800/confirmation 开关。
- 只有在 v2 指标复现通过、完整矩阵重新登记并完成 raw receipt、独立 verifier、paired uncertainty 和来源审计后，才讨论是否值得提交一次 A800 full matrix。
