# Truth-Recovered Structured Kernel：冷启动审查

日期：2026-09-28  
状态：只读代码与代数审查；没有运行效果实验，没有修改生效文档或算法。以下配置是预注册建议，尚未冻结。

## 1. 结论

当前默认初始化不适合作为效果实验的主配置。q_prior=None、provider_prior=None 会把全部参数置零；np.argmax 永远按菜单顺序处理平局。第一次调用一定选择菜单首个 provider。

它不代表任意数据流都会永远选择首个 provider：有效错误会降低被选 provider 的分数，后续可能切换。但存在严格永久锁定反例，尤其是首个 provider 持续 failure/unknown 时完全不更新，永远不能从冷启动平局中走出来。因此零先验 greedy 的结果不能直接解释为“共享真值方法没有效果”。

更严重的是，零 provider 头会让共享真值头对未选 provider 的分数完全不起作用。它从起点切断了候选方法希望利用的共享反馈路径。

最小修正：效果入口必须显式加载 fit-only q/h checkpoint，禁止静默使用零先验；如果主张在线覆盖不同 provider，再给所有匹配方法添加同一条、参数预先固定的探索规则。标签恢复公式和 selected-only 边界不需要改动。

## 2. 证据

审查对象及 hash：

| 文件 | SHA-256 | 位置 |
|---|---|---|
| src/probe_headless/truth_recovered_structured_kernel.py | e223122449edcfb46f01c90e0c66606abcbb7adfbc5e194a013cc468eaff8863 | L117–124 零先验；L167–199 打分/平局；L246–258 selected 更新；L288–302 failure/unknown |
| src/probe_headless/truth_recovered_bbb_runner.py | 147a6ff12c094af6af212e6535883347643868f3ac66730c963c578291bc57dd | L91–113 释放和 selected 调用；没有探索 wrapper |
| src/probe_headless/tests/test_truth_recovered_structured_kernel.py | 只读检查 | 首位平局已被测试写成接口行为 |

本报告数字都是公式实例或预注册建议，不是 BBB 效果结果。

## 3. 锁定机制

调用前 provider 分数为

$$
s_a(v)=q(v)h_a(v,1)+(1-q(v))h_a(v,0).
$$

零先验给出

$$
q(v)=h_a(v,0)=h_a(v,1)=\tfrac12,\qquad s_a(v)=\tfrac12.
$$

案例：菜单 [A,B,C] 的三个分数都是 0.5，np.argmax 选择 A；把菜单改为 [C,B,A] 就先选 C。这是菜单顺序造成的起点差异。

未选 B 的参数不更新，因此即使共享 q 后续改变，B 的分数仍为 0.5，因为

$$
\frac{\partial s_b}{\partial q}=h_b(v,1)-h_b(v,0)=0.
$$

若每次 A 调用失败，kernel 只消费事件，不更新 q/h；后续所有分数仍为 0.5，于是又选 A。B 即使始终可靠也没有机会被调用。unknown truth 同理。不能把 timeout 编成化学真值 0；合法修复是改变选择规则，或另行建模 availability。

若 A 第一次有效反馈错误，一维例子会使 A 的分数下降并可能切换到 B；因此准确说法是“存在顺序偏差与可能永久锁定”，不是“任何数据都会永远首选 A”。反馈延迟为 D 时，第一条反馈释放前有 D+1 个决策使用同一零状态。

## 4. Warm-start 合同

1. 每个 outer fold 只用该 fold 的 fit 训练 q/h 初始化；validation 只用于预先声明的 epoch/超参选择。
2. 编码器、归一化、常数项、provider 顺序和 checkpoint 一起冻结，并对所有方法一致。
3. q 的训练单位是每个分子/端点一次真值，不能因有多个 provider 就复制共享监督。
4. h 只用 fit 中合法、成功、可判分的 $(v,y,a,r)$；failure/unknown 不生成 y。
5. 记录每个 provider 的有效样本数和真值支持；支持不足使用预先规定的 fallback，不临时借 validation/evaluation 标签。

第一版可复用现有开发预算：固定编码器，q/h 线性头，200 epochs，AdamW lr=0.001、weight decay=0.01，seeds 0/1；checkpoint 按 validation objective 选择，平局取最早 epoch。

建议离线目标：

$$
L_{\mathrm{fit}}
=\frac1N\sum_{i=1}^{N}\operatorname{BCE}(q(v_i),y_i)
+\frac{\lambda}{K}\sum_{a=1}^{K}
\frac1{N_a}\sum_{i\in V_a}\operatorname{BCE}(h_a(v_i,y_i),r_{ia}),
\qquad \lambda=1.
$$

$V_a$ 是 provider $a$ 的有效 fit 结果，$N_a=0$ 时该项进入预设 fallback。正式评价前必须一次冻结训练预算和 fallback。

Frozen structured、Truth-only、Correctness-only、Full 使用同一 q/h checkpoint，只改变允许更新的参数块。零先验只作显式 cold-start 消融，不混入 warm-start 主表。若要声称与 Online SGD 同一初始函数，需另写函数匹配合同。

## 5. Exploration 合同

建议 Full、对应 Frozen 和结构消融统一使用固定 $\epsilon=0.05$ 的均匀探索，正式执行前冻结。令 $g_t$ 为确定性 greedy provider：

$$
\pi_t(a\mid x_t,H_t)
=(1-\epsilon)\mathbf1[a=g_t]+\frac{\epsilon}{|A_t|}.
$$

探索和利用都只调用一个 provider；随机数只依赖预登记 seed 与决策序号；pending 反馈不影响当前选择。

案例：33 个 provider、$\epsilon=0.05$ 时，greedy provider 概率为 0.95+0.05/33，其他各为 0.05/33。即使探索分支抽到 greedy provider，propensity 仍记录总概率。

至少记录 selection mode、epsilon、greedy provider、selected provider、selected propensity、RNG seed/state checksum 和菜单 hash。pending event 必须绑定最终实际 provider。

增加 Exploration-Frozen：使用同一探索规则但不更新，以隔离探索成本；同时保留无探索 Best Fixed 和 Frozen。PILOT/BayesianRouter 若保留原生 UCB/TS 探索，单独说明，不加 wrapper 后冒称原版。

均匀探索不保证短流覆盖：

$$
\mathbb E[N_{a,\mathrm{explore}}]=T\epsilon/K,\qquad
\Pr(N_{a,\mathrm{explore}}=0)=(1-\epsilon/K)^T.
$$

案例：T=145、K=33、epsilon=0.05 时，每个 provider 的期望纯探索调用仅约 0.22。warm start 是短流的必要设计，不能因加入 epsilon 就声称识别所有 provider。

## 6. 最小代码修正

1. 效果入口要求显式 priors：新增初始化 manifest/loader，核对 fold、provider 顺序、feature hash、fit split 和 q/h 权重；没有 checkpoint 直接拒绝效果任务。零先验只用于单元测试和显式消融。
2. 在实际 decide 路径增加已冻结的 epsilon 选择规则，choice 与 event 绑定为原子步骤；propensity、RNG 和菜单 hash 随 snapshot 保存。这样 failure/unknown 下也不会因固定 tie-break 永久选首位。

如果只想隔离在线结构更新而不承担 provider coverage 主张，可以登记 epsilon=0 的 warm-start 主实验，但必须明写未选 provider 只凭离线先验，不声称一般可识别性。不能看到结果后在两种协议中挑一个作为主设置。

当前 kernel 学习 chemical correctness，未把 availability 作为状态；若主指标是 system_success，这是独立目标差异，不能用伪造恢复标签掩盖。

## 7. 下一步验收（未执行）

- 冻结 warm-start 与 epsilon 合同，保存 checkpoint 和 hash；
- 验证零先验首位、failure 永久平局和错误后切换三个代数反例；
- 验证 epsilon 的实际 provider、pending/provider/result/feedback 绑定和 propensity 可独立重算；
- 报告每臂 selected/有效更新数量、未覆盖 provider 数、菜单顺序敏感性、初始静态分数，以及 Full 相对 Exploration-Frozen 的变化；
- 通过合同检查后，才开始 development 上的 CPU 效果预筛；本报告没有运行这些检查或产生新分数。
