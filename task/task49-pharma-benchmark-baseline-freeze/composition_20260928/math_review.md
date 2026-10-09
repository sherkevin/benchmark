# 直接工具选择的组合式方法审查：从已选输出恢复真值并做结构化在线更新

日期：2026-09-28
状态：独立候选审查稿；不修改生效方法论，不提交训练，不读取 confirmation。
范围：Task49 医药 direct-tool selection；单次调用、二元性质、selected-only delayed feedback。

> 本文不把“组合”预先判定为创新，也不把组合预先包装成创新。它检查的是：几个已有部件是否被一个真实可观测变量、一个共同的决策目标和一个可以逐事件运行的更新式连成了一个有机机制。是否构成论文贡献，要由最近邻差异、消融和独立确认共同决定。
>
> 仓库中已有部分日志使用了 2026-09-29 等未来日期；那是历史执行记录。本文的审查日期固定为 2026-09-28。

## 1. 先把真正可以利用的信息写清楚

当前单次调用 episode 的最小对象只有六个：

- $x_t$：第 $t$ 个分子的结构表示和性质端点编码；
- $A_t$：本题在调用前可用的 provider 菜单；
- $a_t\in A_t$：选择器在调用前选中的 provider；
- $z_t\in\{0,1\}$：被选 provider 成功返回的二元预测；
- $r_t\in\{0,1\}$：终止后释放的 selected-provider correctness；
- $y_t\in\{0,1\}$：性质的真实标签，只在评测器中持有。

选择器可以使用已经释放的历史 $H_t$，但不能使用本题的 $z_t$、$r_t$ 或 $y_t$，也不能使用未选 provider 的当前输出：

$$H_t=\{(x_i,a_i,z_i,r_i,\tau_i):i<t,\ \tau_i\le t\}.$$

这里的 $z_i$ 只表示已经调用并终止的 provider 输出；$r_i$ 只表示已经到期释放的反馈。**实例：** 第 10 个分子调用 `admetlab_3_0`，它的结果到 tick 14 才释放；tick 10 到 tick 13 的选择不能读这个结果，也不能读同一分子的 `graphb3` 输出。

当前 runner 已经有这些字段的对应证据：`selected_output.label`、`chemical_correct`、`system_success`、`recovered_label`、`due_tick` 和 `release_tick`。其中失败调用的 `chemical_correct` 和 `recovered_label` 必须保持 `null`；不能把失败的 `system_success=0` 当成化学标签。

## 2. 候选 A：Truth-Recovered Structured JEV（优先审查）

### 2.1 核心不是偷看未选结果，而是恢复本次已选样本的真值

对一次成功返回的二元输出，correctness 反馈给出了一个可逆关系：

$$\tilde y_t=
\begin{cases}
 z_t,&r_t=1,\\
 1-z_t,&r_t=0.
\end{cases}$$

**实例：** `z_t=1` 且 `r_t=1` 时，恢复标签为 `1`；`z_t=1` 且 `r_t=0` 时，恢复标签为 `0`；`z_t=0` 且 `r_t=0` 时，恢复标签为 `1`。这三种情况都只用当前已选 provider 的返回和已经释放的 correctness，不需要读任何未选 provider 输出。

该式的适用条件必须写死：

1. provider 返回的是有效二元标签；
2. $r_t$ 是该 provider 输出相对于同一个 $y_t$ 的 correctness；
3. episode 已终止且反馈已释放；
4. 不是 `system_failure`、unknown truth 或 unresolved reference。

如果 provider 调用失败，只有 `system_success=0`，没有 $z_t$，因此不能执行上式。若参考标签 unresolved，也不能执行上式。这个边界与 `recovered_label()` 的现有校验一致。

**现有接口的缺口：** `TerminalFeedbackQueue.release()` 当前只向 policy 的 `observe()` 传 `event_id`、`provider_id` 和 `reward`；候选 A 还需要把已经选中的二元输出 `z_t` 绑定在 pending event 上，并在 release 时传给 learner。最小改法是让 `complete()` 在队列项保存 `selected_output.label`，让 `observe()` 接收这个字段；不能从 evaluator 的完整 provider panel 回填 `z_t`。这是一个接口扩展，不是额外信息权限。

### 2.2 用一个共享属性头和每个 provider 的条件正确性头组成一个共同目标

定义两个概率：

$$q_\theta(x,e)=\Pr(Y=1\mid x,e),$$

$$h_{a,\phi_a}(x,e,y)=\Pr(R_a=1\mid x,e,Y=y).$$

$q_\theta$ 是性质真值头；$h_{a,\phi_a}$ 是 provider $a$ 在给定分子、端点和真值时返回正确的概率。这里 $R_a$ 指 provider $a$ 的 correctness 事件，不是当前未调用 provider 的实际输出。对候选 provider 的调用前正确率预测由全概率公式给出：

$$s_a(x,e)=q_\theta(x,e)h_{a,\phi_a}(x,e,1)+\bigl(1-q_\theta(x,e)\bigr)h_{a,\phi_a}(x,e,0).$$

**实例：** 对分子 `z000282` 的 BBB 端点，若当前属性头给出 $q_\theta=0.7$，`b3clf_dtree_common` 的两个条件正确率分别为 $h_a(\cdot,1)=0.9$、$h_a(\cdot,0)=0.6$，则调用前分数是 $0.7\times0.9+0.3\times0.6=0.81$。计算时没有读 `b3clf_dtree_common` 在当前分子上的真实输出；这只是根据训练期 provider 行为和共享属性状态作出的预测。

这个式子没有假设 provider 彼此独立。它只对单个 provider 的边际 correctness 应用全概率公式。共享的分子/端点信号可以由共同的 $q_\theta(x,e)$ 和表示 $v$ 复用，但独立的 $h_a$ 只给出各 provider 的边际，不会自动估计“两个 provider 一起错”的协方差。只有在要同时建模多个 provider 的联合事件（例如一次追加两个工具）时，才需要额外的相关结构。

### 2.3 延迟反馈时的最小在线更新

对每个已释放且可恢复的事件，使用一条联合损失：

$$\ell_t(\theta,\phi_{a_t})=
\operatorname{BCE}\bigl(q_\theta(x_t,e_t),\tilde y_t\bigr)
+\lambda\operatorname{BCE}\bigl(h_{a_t,\phi_{a_t}}(x_t,e_t,\tilde y_t),r_t\bigr).$$

**实例：** 若 `graphb3` 在 `z000282` 上返回 `0`，后来收到 `r=0`，则 $\tilde y=1$；release tick 只更新共享属性头 $q_\theta$ 和 `graphb3` 的 $h_{a,\phi_a}$，不更新未选的 `admetlab_3_0` 头。若 `graphb3` 调用失败，没有 $z$，该事件不进入这条损失。

为满足实时性，先固定一个离线编码器 $v=f(x,e)$，在线只更新小型头。一个最小实现可以是

$$q_\theta(x,e)=\sigma(\theta^\top v),\qquad
h_{a,\phi_a}(x,e,y)=\sigma\bigl(\phi_a^\top [v;y]\bigr),$$

其中 $[v;y]$ 是把恢复出的二元 $y$ 拼到已固定的 $v$ 后面。**实例：** 若 $v$ 是 64 维 Morgan/Laya 表示，则属性头是 64 维，provider 头是 65 维；一次 release 只做共享头和选中 provider 头的梯度计算，未选 provider 的 32 个头没有梯度。

状态只在 feedback release 时改变：

$$S_{t+1}=U\bigl(S_t,x_t,e_t,a_t,z_t,r_t,\tau_t\bigr),$$

其中 $S_t=(\theta,\{\phi_a\}_{a\in A})$。**实例：** 第 10 个事件的 `due_tick=14`，tick 10 到 13 的状态仍是 $S_{10}$；tick 14 执行一次 $U$ 生成 $S_{15}$，同一个 `event_id` 再次到达时直接拒绝。决策分数仍使用 release 前的状态 $S_t$，不能把未来的 $r_t$ 回填到本次选择。

### 2.4 这条候选为什么比“两个独立模型拼起来”更紧

它们之间有三条必须同时存在的关系：

1. **同一恢复标签：** $z_t$ 和 $r_t$ 生成同一个 $\tilde y_t$；属性头和 provider 头不是各自读取不同的伪标签。
2. **同一决策分数：** $q_\theta$ 不是单独报告性质准确率，而是通过 $s_a$ 进入 provider 选择；$h_a$ 也不是单独做 provider 分类，而是和真值不确定性相乘。
3. **同一时间合同：** 两个头都只能在 selected-only correctness release 后更新；训练区可以用 full panel warm start，但 evaluation/confirmation 不能使用未选输出。

因此它有一个可证伪的整体命题：**恢复出的性质真值能否在同一 selected-only 延迟信息合同下，改善后续 provider correctness 预测，并最终改善选择轨迹。** 如果只提升属性头的 AUC，却不提升 provider 选择，整体方法不成立。

### 2.5 计算成本和恢复边界

若 $v$ 维度为 $d$、当前菜单大小为 $K$，且 q/h 都是线性头，则一次新选择的分数成本为

$$T_{\mathrm{select}}=O(Kd),\qquad T_{\mathrm{update}}=O(d),\qquad M_{\mathrm{state}}=O((K+1)d).$$

**实例：** $d=64$、$K=33$ 时，release 更新只计算共享 q 和一个 selected h；下一题打分需要遍历 33 个 provider 的 h。若只保留 top-2 或使用候选 B 的 margin gate，更新仍是 $O(d)$，只能减少不必要的头计算，不能改变选择前必须评估菜单的事实。pending queue 另存每条事件的 $z$、provider、特征摘要和 release tick，空间随 pending 数线性增长；不能把它报告成与运行时长完全无关。

状态快照必须包含 q/h 参数、provider/version 映射、更新计数、队列和输入 hash。恢复器按 release 顺序重放同一 $z,r$ 事件，重算 $\tilde y$、状态 hash 和每条选择；恢复器不能读取评测器真值表。

## 3. 候选 A 的假设、反例和审查闸门

### 3.1 必须显式承认的假设

- **二元可逆性：** provider 输出和 correctness 都是关于同一个二元真值的定义；多分类或连续性质需要另一个恢复规则，不能直接套用。
- **条件头可学习：** provider 的正确性与 $(x,e,y)$ 有足够稳定的关系；如果每个 provider 完全随机，恢复真值也不能预测其未来正确性。
- **反馈可信：** $r_t$ 不是被错误记录、错绑 query/provider 或从全 panel 偷填的。日志必须能核对 event、provider、status 和 release tick。
- **在线覆盖：** 一个 provider 若永远不被选，只有离线先验；没有探索，不能声称它的在线头已被识别。
- **可恢复样本的选择性：** $\tilde y_t$ 只在选中 provider 成功且真值可评分时出现；失败率、unknown truth 和策略选择可能共同决定哪些分子进入 q 头。若缺失不是随机的，q 的在线梯度是 policy-dependent sample，不应直接当作无偏全体属性训练；需要记录 coverage/propensity，或把结论限制为当前调用分布。
- **端点语义一致：** BBB、hERG、AMES 的标签清洗和 provider 输出阈值必须分别冻结，不能把不同 assay 的 0/1 当成一个共同真值。

### 3.2 反例

1. **失败调用反例：** provider 返回 timeout，系统记录 `system_success=0`。没有 $z_t$，把 $\tilde y_t=0$ 会把“工具失败”错误当成“性质为 0”。
2. **unknown truth 反例：** 评测器没有参考标签，但运行状态正常。没有 $r_t$ 的可信来源，不能恢复 $y_t$。
3. **冲突真值反例：** canonical molecule 有两个未经解释的 `0/1` 参考记录。恢复出的标签不能被多数票静默决定；该组应排除或按重复测量模型另行处理。
4. **provider 漂移反例：** checkpoint 更新后 $h_a$ 仍沿用旧 provider 行为。若没有显式 version key 或 event reset，属性头可能正确，但选择头会使用过期的正确性模型。
5. **选择偏差反例：** 早期只选固定赢家，其他 provider 永远没有 selected feedback；$h_a$ 的低方差不代表它可靠。必须报告探索率或把未覆盖 provider 限制在 frozen prior。
6. **共享错误反例：** 若 provider 错误由相同训练集造成，多个 provider 会同时错。逐 provider 独立后验会低估共错；候选 A 至少通过共享真值头保留了共同的 $x,e$ 条件，但仍需在端点/家族消融中检查残差相关。
7. **过度自信反例：** $q_\theta$ 过拟合 development 分子后，$s_a$ 的校准会变差。必须在 scaffold-disjoint split 报告 calibration 或 Brier，不只报 accuracy。

### 3.3 必须做的因果消融

以下消融能区分“恢复标签”“结构组合”和“普通在线更新”：

| 消融 | 保留 | 移除 | 要回答的问题 |
|---|---|---|---|
| A0 Frozen best | train-only 固定 provider | 所有在线状态 | 是否有真实适应 headroom |
| A1 Matched Online SGD | 同一 $v$、同一参数量、selected reward | 恢复真值和联合分数 | 结构化反馈是否超过普通 selected reward 更新 |
| A2 Truth-only | q 头在线更新 | $h_a$ 动态更新 | 恢复标签本身是否改变选择 |
| A3 Correctness-only | $h_a$ 在线更新 | q 头更新；使用现有 provider correctness 目标 | 共享属性状态是否提供额外信号 |
| A4 No-recovery control | 把 $r_t$ 作为唯一 reward | $\tilde y_t$ 分支 | 正确性恢复是否是必要机制 |
| A5 A-without-integration | 分别预测 q、h | 使用 $s_a$ 的概率积分 | “联合”是否比两个独立排名更有用 |
| A6 No-shared-q | 每个 provider 独立真值头 | 跨 provider 共享 q | 共享真值是否带来冷启动/低反馈收益 |
| A7 Delay/selection | 同一 seed 和菜单 | 改变 delay 或探索 | 结果是否依赖同步反馈或隐藏全 panel |
| A8 Version event | 同一方法 | arrival/retirement/reset | 方法是否真的适应 provider 生命周期 |

主结果至少要显示 A1、A2、A3、A5；若 A 不超过 A1，则只能说结构化标签通道没有在该任务上产生额外收益；若 A 只提升 q 的离线指标而不提升 selection accuracy，则不能把它写成工具选择方法。

## 4. 何时它等价于普通多任务 SGD，何时不等价

### 4.1 等价条件

候选 A 在以下条件同时成立时，数学上就是一个带稀疏 provider 头的普通多任务 SGD：

1. 编码器 $v=f(x,e)$ 固定；
2. 每个 release event 的 $(\tilde y_t,r_t)$ 都是确定可用的监督标签；
3. 更新使用上面的联合 BCE，学习率、batch、权重 $\lambda$ 和参数初值固定；
4. 只对共享 q 头和选中 provider 的 h 头反向传播；
5. 没有 Kalman/Laplace posterior、自然梯度、置信上界或特殊状态校正；
6. 使用相同的反馈到达顺序和相同的事件去重规则。

此时

$$\nabla\ell_t=\nabla\operatorname{BCE}(q_\theta,\tilde y_t)+\lambda\nabla\operatorname{BCE}(h_{a_t,\phi_{a_t}},r_t),$$

只是普通多任务梯度的一个 block-sparse 版本。**实例：** 若第 14 tick 只释放 `graphb3` 事件，则梯度中 q 头有一项，只有 `graphb3` 的 h 头有一项，`admetlab_3_0` 等未选头的梯度为零。这个事实不能隐藏；它帮助我们明确真正可主张的内容应是“selected-only correctness 使二元真值可恢复，并由该标签把两个任务连接在同一个实时决策分数中”，而不是“发明了新的 SGD”。

### 4.2 不等价的地方

以下任一项都会使它成为不同的在线估计器，但每一项都需要单独数学和实验理由：

- 用 q 的不确定性积分、校准温度或 posterior predictive，而不是点估计 $q$；
- 用 provider/version 层级的共享参数或低秩先验，使一个 provider 的事件影响同族 provider；
- 用延迟窗口内的事件批量化、trust-region 或自然梯度；
- 发生 arrival/retirement/drift 后对 $h_a$ 做可恢复的状态迁移，而不是普通梯度累积；
- 用 margin gate 只更新会改变 top-1 决策的 provider 对；
- 同时包含 post-call verifier 的 confidence，并明确它只影响下一次选择或当前拒答，不回填当前 pre-call 动作。

这些变化不会自动成为贡献。必须逐项证明：有新的可观测信息、减少了明确的计算/样本代价，或在预登记事件下提高了可复现指标。

## 5. 候选 B：决策差距驱动的稀疏更新

候选 B 不是另一个完整模型，而是候选 A 的效率机制。定义当前最优和次优 provider 的分数差：

$$\Delta_t=s_{a^{(1)}}(x_t,e_t)-s_{a^{(2)}}(x_t,e_t).$$

**实例：** 当前 `admetlab_3_0` 分数为 0.81、`graphb3` 为 0.80，则 $\Delta_t=0.01$；当前 top-1 和 top-2 差距 0.25 时，单次反馈通常不太可能改变动作。

只在 $\Delta_t\le\kappa$ 时执行候选 provider 的额外结构更新，或者只保存 top-2 的 score margin；远离决策边界的事件仍更新共享 q，但不计算所有 provider 头。该规则可把每步成本从全 $K$ 个头降到 top-2/selected 头，但其正确性必须在“同一反馈、同一状态、同一决策”下与 dense update 对照。

**反例：** 一个表面差距很大的 provider 可能刚发生版本漂移；只按 margin 跳过更新会延迟发现漂移。因此 B 不能单独承担动态适应主张，必须和 provider/version event 或 drift detector 一起评估。

B 的最低实验是：dense A、margin-gated A、只更新 selected provider、相同累计错误下比较更新 p50/p95 和峰值内存。若只降低计算但准确率相同，论文中应把它定位成 efficiency contribution；若 margin gate 反而损害漂移恢复，应停止使用。

## 6. 候选 C：可学习表示与历史状态的一致修复

候选 C 针对的是当前方法的另一个真实痛点：在线头可能有信号，但冻结 representation 把不同 scaffold、endpoint 或 provider version 压成不可分的向量。可行的最小形式是保留离线表示 $v=f(x,e)$，为每个 provider/version 维护一个小的 residual 状态 $d_a$，选择时使用

$$s_a^{\mathrm{res}}(x,e)=s_a(x,e)+d_a^\top v.$$

release 后只更新选中 provider/version 的 $d_a$，并对状态做固定范数约束或低秩投影。**实例：** `b3clf` 的新 checkpoint 只获得自己的 residual；旧 checkpoint 的历史状态不被静默覆盖，恢复脚本可以从 checkpoint hash 和 release 顺序重算 $d_a$。

C 的风险比 A 大：

- residual 只是多加一个线性头，容易退化为普通在线 SGD；
- 如果没有真实 version/arrival 事件，$d_a$ 没有可识别的动态来源；
- 当前 Laya/fingerprint 诊断未证明冻结表示有可用的直接选择信号；
- 状态迁移若读取当前测试 full panel，会产生泄漏。

因此 C 只能在 A 的真值恢复通道和真实 provider/version 事件都通过后再做。它的第一轮不是训练，而是事件可识别性审计：同一 provider 的 version hash、训练时间、事件 tick 和 selected-only feedback 是否存在并能逐事件绑定。

## 7. 建议的最小验证路线

不立即提交效果型 A800。先在现有 BBB development replay 做一个严格的 CPU/小规模验证：

1. 复用已经通过的 `recovered_label` 和 selected-only queue；新增 learner 只能接收 `event_id, provider_id, z, r, release_tick`，不接 truth store 或全 panel；
2. 固定一份离线 $v=f(x,e)$ 和一组 train-only q/h 初始化；不调 online learning rate、$\lambda$、探索率和阈值；
3. 跑 A0、A1、A2、A3、A5，delay $0/1/4$，5 个 scaffold folds、2 个 seeds；
4. 每条事件记录选择前菜单、已释放反馈数、选中输出、恢复标签摘要、状态 hash、更新前后版本和 p50/p95；原始 JSONL 能由独立 NumPy 恢复；
5. 先看机制级检查：恢复标签与 evaluator-only truth 的一致率、q/h 的校准、每个 provider 的 selected coverage、没有未选输出读取；再看 selection accuracy；
6. 只有 A 至少超过 matched Online SGD，并且在两个以上 delay、至少一个未参与开发的端点或 confirmation candidate 上保持方向一致，才值得提交一条 A800 任务。

### 建议的停止条件

- 任意一条恢复标签与 evaluator truth 不一致：停止并修复信息合同；
- 失败/unknown 事件被写入 $\tilde y$：该流作废；
- A5 不优于 A1，且 q/h 组合没有在任何 endpoint 或 delay 产生一致收益：停止“结构化组合”主线，不靠调参改名；
- q 离线指标提高但 selection accuracy 不提高：把它降级为属性预测辅助实验；
- A 超过弱 Frozen 但不超过 matched Online SGD/PILOT/BayesianRouter：不能主张动态 JEV 方法成立；
- 没有真实 provider/version 事件时：结果只能叫 delayed online selection，不能叫 dynamic provider adaptation。

## 8. 现阶段判断

候选 A 是目前最干净、最值得验证的一条组合路线，因为它只有一个新增的可观测事实：**已选二元输出和已释放 correctness 可以恢复该题的性质标签**。这个标签同时训练共享性质头和已选 provider 的条件正确性头，再通过全概率公式产生下一题的 provider 分数。它把“属性学习”“工具正确性学习”“实时 JEV”连到同一个变量和同一个 update event 上。

但它目前仍是候选，不是已证创新。若固定表示、在线 SGD 和该结构在同一信息合同下等价，论文贡献就不能靠公式新名字支撑；真正需要证明的是恢复标签带来的样本效率、跨 provider 共享、延迟下的恢复速度或 version event 下的适应收益。候选 B 可作为 A 的稀疏效率模块，候选 C 只在真实 provider/version 事件和表示瓶颈被证实后再进入实验。

本审查不改变现行 benchmark、baseline、方法论或决议文件；它只为下一次双方讨论提供一个可实现、可证伪、可恢复的候选方法卡。

## 9. 证据入口

- 方法与信息边界：`docs/研究主线/生效/02-方法论.md`
- 任务与数据资格：`task/task49-pharma-benchmark-baseline-freeze/report.md`
- 在线事件合同：`task/task49-pharma-benchmark-baseline-freeze/online_adapter_v1/contract.md`
- selected-only runner：`src/probe_headless/pharma_selection_stream.py`
- 真值恢复实现：`src/probe_headless/pharma_evaluation_contract.py`
- 真值评测器：`src/probe_headless/pharma_bbb_judge.py`
- 现有直接选择审查：`task/task49-pharma-benchmark-baseline-freeze/fresh_method_review_20260928.md`
- matched SGD 合同：`task/task49-pharma-benchmark-baseline-freeze/matched_sgd_contract_v1.md`
