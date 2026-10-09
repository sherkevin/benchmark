# Structured CPU 预筛负结果诊断与下一候选

- 日期：2026-09-29
- 状态：独立只读诊断；本文档不修改生效主线，不打开 confirmation，不提交 A800 任务。
- 证据：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/`
  的原始 JSONL、独立 verifier、聚合 JSON/Markdown；输入 SHA-256 为
  `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。

## 1. 先给结论

这轮没有显示 `structured` 相对于强的 fit-only/frozen selector 或
`matched_online_sgd` 有可重复增益。它也没有证明“动态 JEV 这个方向没有信号”。能从当前证据支持的较窄结论是：

> 在固定的 64 维 encoder、fit-only q/h warm start、selected-only 延迟反馈和一步常数步长全向量更新下，q 与 provider correctness 两个头同时在线更新，会破坏已经有用的静态排序；全概率积分没有把这种更新损失抵消掉。

因此当前候选不进 confirmation，也不值得把这组结果直接搬到 A800。下一次若继续，只做一个针对“在线漂移过快”的、预先固定的候选 CPU 证伪；不做学习率、表示、探索率或门槛搜索。

## 2. 可复核的结果

全量运行是 180 条流、25,992 个 decision/terminal/feedback 样本；原 verifier 为
`verified`，但它不是逐事件参数重放器，独立 replay 仍待补齐。primary accuracy 把 failure/unknown 记为 0：

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen structured | 0.8982 | 0.8982 | 0.8982 |
| Matched online SGD | 0.8913 | 0.8885 | 0.8968 |
| Truth-only | 0.8975 | 0.8989 | 0.8975 |
| Correctness-only | 0.8878 | 0.8940 | 0.8996 |
| No-integration | 0.8906 | 0.8920 | 0.8913 |
| Structured | 0.8857 | 0.8927 | 0.8961 |

`structured - matched_online_sgd` 的全样本点差为 `-0.00069`。旧聚合器的 scaffold bootstrap CI `[-0.00633, 0.00518]` 与 `P(delta>0)=0.388` 暂作描述性结果；由于它把同一分子的 seed/delay/方法复制进了独立重采样，不能作为最终不确定性证据。运行前 preregistration 没有写入 `+0.01` 或 `4/5` 数值门槛；它们是 proposal 中用于开发资源决策的判据。按该开发判据，三个 delay 都未达到 `+0.01`，正 fold 数最高 `3/5`。独立 replay 后的保守 v2 聚合为：delay 0 `-0.005540`（95% CI `[-0.015257, 0.004006]`，P>0 `0.1088`），delay 1 `+0.004155`（`[-0.004406, 0.013418]`，P>0 `0.8072`），delay 4 `-0.000693`（`[-0.008333, 0.008648]`，P>0 `0.4104`）；三个 delay 合并为 `-0.000693`（`[-0.006294, 0.005377]`，P>0 `0.3868`）。这些是开发期、按 382 个全局 scaffold cluster 的配对复核，不改变停止条件。

## 3. 为什么不像“恢复标签错了”

当前结果与恢复公式或反馈顺序问题不一致，但在逐事件 replay 完成前不能写成排除性结论。

1. 成功返回且有 `r` 时，事件按 `z` 与 `r` 恢复 `y~=z`（`r=1`）或 `1-z`（`r=0`）；原 verifier 检查了终止、provider 绑定、due tick 和更新次数，但没有逐事件重算所有参数状态。
2. 终止记录没有 truth、chemical correctness 或 recovered label；policy 每轮只拿到被选 provider 的一个 cell。真实 BBB replay 的 selected-only 合约 smoke 也已单独通过。
3. `truth-only` 几乎保持 frozen（0.8975/0.8989/0.8975），说明共享 q 的恢复更新本身没有带来明显的排序收益，但也没有造成主要损失。
4. 主要变化来自 provider correctness 头。把每条 `structured` 的选择与同 fold/seed/delay 的 frozen 选择逐条对齐，平均 provider agreement 只有 `0.199`（30 条流，范围 `0.049–0.717`）；`truth-only` 与 frozen 的 agreement 是 `0.964`。`correctness-only` 与 frozen 的 agreement 为 `0.197`，和 structured 接近。

这组对照把主要变化与“更新 provider head 后的重新排序”联系起来；它没有在独立逐事件 replay 或受控反事实下排除标签恢复、延迟 queue 等替代解释。因此它仍然是诊断性证据，不是因果证明。

## 4. 机制解释：现有 TRS 的三个具体问题

### 4.1 一条 selected feedback 改动了完整 64 维 provider head

当前在线更新是

$$
\phi_{a,t+1}=\phi_{a,t}-\eta
\bigl(\sigma(\phi_{a,t}^{\mathsf T}[v_t,\tilde y_t])-r_t\bigr)[v_t,\tilde y_t],
\qquad \eta=0.05.
$$

一个 release 同时改动该 provider 的所有特征方向。一个 provider 只被选择了很少的、由当前 policy 筛出来的样本时，这种更新会把 fit-only 的全局排序变成局部、噪声敏感的排序。证据是 structured 在 30 条流中平均使用约 10.2 个 provider，而 frozen 平均只用约 5.3 个；与 frozen 的分数向量绝对差平均约 `0.0159`，95 分位约 `0.1038`。这是短流上的明显重新排序，不是“实时更新但基本没变”。

### 4.2 q/h 分解比 direct success 目标更容易失配

`matched_online_sgd` 直接优化 selected `system_success`，而 structured 先估计性质 q，再估计条件正确性 h，最后计算

$$
s_a=q h_a(1)+(1-q)h_a(0).
$$

这个分解在真模型成立时是合理的，但固定 64 维线性头和短的 selected-only 流并不能保证两个条件头都被充分识别。对工具选择而言，direct head 的目标和评价指标更接近；分解增加了中间状态，任何 q/h 的局部偏差都会通过积分进入所有 provider 的排名。当前结果正好表现为 structured 不稳定地介于 correctness-only 与 matched direct baseline 之间。

### 4.3 selected-only 更新是 policy-induced 的

恢复的 `y~` 对成功事件是精确的，但 q/h 的样本不是 fit 区的独立样本，而是“被当前 policy 选中的 provider 和分子”。因此这轮能检验的是“在该 policy stream 上，后续选择是否改善”，不能把 q/h 的在线状态解释成完整分布上的真值估计。未选 provider 没有当前反馈；共享 q 不能替未选 provider 的 correctness head 产生梯度。

失败/unknown 也暴露了目标差异：结构化方法跳过 failure，A1 按 `system_success=0` 更新；本轮失败很少（各结构化 aggregate 通常 12/1,444），所以它不是负结果主因，但主指标确实是“系统成功”，下一候选应把可用性和化学正确性分开建模，而不是把失败伪装成化学错误。

## 5. 下一候选：Prior-Anchored Residual JEV（PAR-JEV）

这是一个只改变在线状态参数化的候选。它保留 fit-only 的上下文排序，把实时学习限制在低维、可审计的 provider 校准残差，并显式表示工具是否成功返回。候选不是已冻结方法，下面的公式和消融需在 CPU 运行前另行预注册。

### 5.1 冻结基线函数

由每个 outer fold 的 fit 区训练并固定三个 baseline logit：

$$
\ell^q_0(v),\qquad
\ell^h_{a,0}(v,y),\qquad
\ell^u_{a,0}(v).
$$

其中 $q$ 是性质为 1 的概率，$h_a$ 是 provider $a$ 在有效返回且性质为 $y$ 时正确的概率，$u_a$ 是 provider 有效返回的概率。$u_a$ 只用 `status=ok`/failure 标签拟合；它不读取真值。已有 direct/system-success warm start 不能直接冒充 $u_a$，除非在新实验卡中明确声明其标签定义。

在线只维护标量 offset 和累积曲率：

$$
\ell^j_t=\ell^j_0+c^j_t,\qquad I^j_t>0,
$$

这里 $j$ 是共享 q、某个 provider 的 h 条件（每个 $y$ 各一个标量）或 provider 可用性 u。初始化 $c^j_0=0$，$I^j_0$ 是同一 fit 数据上 baseline logit 的 Fisher 曲率和，并加固定的数值正则；不引入要靠结果挑选的在线 learning rate。

### 5.2 统一的实时更新

对当前事件对应的一个标量状态，令

$$
p_t=\sigma(\ell^j_0(v_t)+c^j_t),\qquad
w_t=p_t(1-p_t),
$$

收到二元目标 $b_t$ 后执行标量 online-Newton 更新：

$$
I^j_{t+1}=I^j_t+w_t,\qquad
c^j_{t+1}=c^j_t+\frac{b_t-p_t}{I^j_{t+1}}.
$$

这个递推的步长由 fit 信息量和已释放事件的曲率决定；它把实时更新锚在 fit-only 函数上，不会像当前全向量常数步长那样每次改写完整表示。每个事件最多更新 q 一个标量、被选 provider 的 h 一个标量和 u 一个标量，选择仍是 $O(Kd)$，释放更新是 $O(1)$（加上读出相应 baseline logit 的成本）。状态大小为 $O(K)$，pending event 仍只保存 selected cell 和 due tick。

### 5.3 面向系统成功的选择分数

调用前对 provider $a$ 使用

$$
\hat q_t=\sigma(\ell^q_0(v_t)+c^q_t),\quad
\hat h_{a,t}(y)=\sigma(\ell^h_{a,0}(v_t,y)+c^h_{a,y,t}),\quad
\hat u_{a,t}=\sigma(\ell^u_{a,0}(v_t)+c^u_{a,t}),
$$

$$
s_{a,t}=\hat u_{a,t}\left[\hat q_t\hat h_{a,t}(1)+(1-\hat q_t)\hat h_{a,t}(0)\right].
$$

它仍然使用同一个恢复标签连接 q 与 h，但把“工具没返回”作为独立的可观测事件。这样优化对象与 primary system-success accuracy 一致，同时不把 failure 当作化学 correctness 标签。

### 5.4 释放时的信息合同

- 决策前：只见 $v_t$、端点、菜单、fit-only baseline logits 和已经释放的标量状态。
- 终止时：绑定最终选中的 $(a_t,status,z_t)$；不绑定 truth、未选输出或 correctness。
- 成功且 reference 可评分：释放 $r_t$ 后恢复 $\tilde y_t$；更新 q 的目标 $b_q=\tilde y_t$，选中 provider/对应 $y$ 的 h 目标 $b_h=r_t$，u 的目标为 1。
- 成功但 truth unknown：只更新 u（目标 1），q/h 不更新。
- system failure：只更新 u（目标 0），不产生 $\tilde y$。
- 事件严格在 `decision_tick+1+delay` release，一次且仅一次；每次 release 记录 state version/hash、$I$、offset、耗时和 selected provider。

PAR-JEV 不声称能从未选 provider 的反馈识别其当前 correctness；若要承担 coverage/探索主张，必须另加所有方法统一的、预登记的 propensity 规则和 Exploration-Frozen 对照。

## 6. 再做一次 CPU 前必须固定的对照与判据

只做一组候选、同一输入、同一 5-fold × 2-seed × delay `{0,1,4}`，不进行超参搜索：

1. 现有 Frozen TRS（q/h、无 u）：保持当前 frozen reference 完全不变。
2. Frozen q/h/u：加入 fit-only 可用性头但所有 offset 仍冻结，隔离“加入 u”本身的影响。
3. q-residual：只更新共享 q offset。
4. h-residual：只更新 selected provider/selected $y$ 的 h offset。
5. q+h residual：不建模 u，检验分解和锚定本身。
6. u-only：只更新可用性 offset，检验 failure 语义。
7. PAR-JEV：q+h+u，使用完整 $s_{a,t}$。
8. 现有 TRS 与 matched online SGD：作为全向量更新和 direct reward 的匹配负/近邻对照。

所有方法共享 fit-only encoder、fold、菜单、delay、事件顺序和（若开启）探索规则。主结果报告 overall system-success accuracy、valid-return accuracy、scaffold-cluster paired bootstrap、provider coverage/menu-order sensitivity、decision/update p50/p95；同时报告 q/h/u 的 Brier 或 log loss，避免只看最终选择率。独立 verifier 必须检查 selected-only、失败更新、release timing、offset 递推和 state hash。

建议的 go/kill 条件沿用当前 CPU 卡，而不是为新候选放宽：PAR-JEV 相对 matched online SGD 至少 `+0.01`，在三个 delay 中至少两个成立，并且每个 seed 至少 `4/5` 个 fold 为正；相对 frozen 不得在 primary accuracy 上系统性下降；若只有 u-only 提升，则结论限定为 failure-aware routing，而不写成 q/h 动态 JEV。任一条件不满足就保留负结果，不提交 A800 效果任务。

## 7. 是否值得再做 CPU

值得，但只值得做这一轮有针对性的 CPU 证伪。理由是当前负结果已经把一个具体可修的问题暴露出来：q-only 几乎不动，h 全向量更新却让选择序列大幅偏离 frozen；因此“保留上下文基线、只学习低维实时残差”是由日志直接导出的机制假设，不是看到分数后随意调参。PAR-JEV 的更新没有自由 learning rate，u 的引入也对应 primary system-success 的真实标签定义。

这轮 CPU 的目的不是寻找一个能赢的配置，而是回答一个二分问题：

> 在 selected-only、延迟反馈和同一冻结表示下，保守的低维实时校准是否比全向量更新更能保留静态排序并利用新反馈？

若答案仍为否，当前 BBB 开发任务上继续堆叠 q/h 结构就缺乏证据，应停止动态方法的 A800 资源投入，转向补充端点资格、provider lineage 或重新定义可证伪的环境漂移任务。若答案为是，也只能先冻结候选、补 baseline roster 和独立 confirmation，再讨论 A800。
