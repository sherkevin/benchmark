# Fresh mechanism ablation v2 — pre-run card

状态：**设计冻结草案，尚未运行**。本卡先于任何 v2 执行保存；它不继承
v1 的 Shared-Credit 解释。目标是只检验 released state 是否带来动态选择，
不把静态初始重排误算成在线学习。

## 唯一问题

在同一 fresh menu、同一 prior、同一反馈时钟下，保持首次动作与 Frozen
完全一致时，正确消费 selected-only released outcome 是否比不消费反馈更能
应对声明的 evaluator-side correctness drift？

## 固定输入和信息边界

- 四个 checkpoint-bound provider、157 个 confirmation molecule、原有 molecule
  顺序、五折、两个 deterministic schedule replicas、$D\in\{0,1,4\}$ 和一次调用
  预算全部复用 sealed receipt；两个 replica 是固定执行表中的重复观测，不解释为
  随机 seed 稳健性；
- selector 只读取 $x,e,A$ 与已释放的 selected outcome；不读取当前 truth、未选
  provider output、未来 terminal 或未到期反馈；
- 使用原有 terminal/reward receipt，不重新调用 provider、LLM 或 GPU；
- 受控条件仍只在 tick 78 之后、且仅当 fit-best provider 被选中时翻转其返回标签。

## 匹配方法

令 $q_a^0(x)$ 为冻结 prior 分数，令 $\Delta_a=0$ 为冷启动 residual。固定
provider 顺序为 `ecfp_logreg, ecfp_mlp, ecfp_random_forest,
graph_message_passing`，所有 `argmax` 使用该顺序的 first-index tie-break。
所有方法在每条 stream 的每个反馈释放前都必须满足

$$q_a(x)=q_a^0(x),\qquad a\in A_t,$$

并逐事件核对完整 score 向量（浮点绝对误差不超过 $10^{-12}$）和
argmax 与 Frozen 相同；不是只核对第一条 action。$D=0$ 的反馈在
$t+1$ tick 释放，其他延迟为 $t+1+D$。

1. **Frozen/no-update**：记录 terminal 和 release 事件，但 scorer 永远读取
   $q_a^0(x)$。
2. **MIRA-Relay/Local**：释放 selected provider $a_t$ 的 reward 后只更新
   $\Delta_{a_t}$，使用当前方法的 selected-only residual。
3. **Fixed-permutation credit**：使用 protocol id `cycle-v1` 固定并在 config
   中明文保存的无不动点置换 $\pi:\{1,\ldots,K\}\to\{1,\ldots,K\}$（本次四个
   provider 的固定结果为 `ecfp_logreg→ecfp_mlp→ecfp_random_forest→
   graph_message_passing→ecfp_logreg`）。先按 Local 对源 provider
   $a_t$ 计算本次 released reward 的 residual 增量
   $\delta_{a_t}(r,n,s)$（仍使用源 provider 的 fit-rate anchor 和计数器），
   再把这一个增量写入目标槽 $\Delta_{\pi(a_t)}$；不替换目标槽的 prior、
   fit-rate anchor 或其余状态。这样只改变 credit destination，避免把目标
   provider 的静态 anchor 混入对照。该 arm 只做错误归因诊断，不是 proposed
   method。

   具体地，令源 provider 的 fit-rate anchor 为 $r_a$、释放前计数为 $n_a$、
   成功数为 $s_a$、释放 reward 为 $y\in\{0,1\}$，则本次写入的增量固定为

   $$\delta_a(y,n_a,s_a)=\frac{n_0r_a+s_a+y}{n_0+n_a+1}-r_a
   -\left(\frac{n_0r_a+s_a}{n_0+n_a}-r_a\right),\qquad n_0=20.$$

   permutation arm 维护 source-indexed 的 $(n_a,s_a)$，并只将该
   $\delta_a$ 加到目标槽 $\Delta_{\pi(a)}$；Local 将同一增量加到
   $\Delta_a$。目标槽不维护或读取自己的 feedback 计数器。

构造案例必须先通过：无反馈时三者对每个事件的 score/action 完全等于 Frozen；
一个 reward mutation 能显示 Local 与 permutation arm 只在不同 destination
residual 槽发生变化；同一 terminal/release event 序列对三者逐条一致。

## 主要估计量和指标

唯一主 estimand 是等权 scaffold cluster 的 selected correctness。对每个
stream $s$ 和 scaffold $g$，先取该 stream 在该 scaffold 的正确率
$m_{s,g}=|I_{s,g}|^{-1}\sum_{i\in I_{s,g}}y_{s,i}$；对方法差值先取
$d_g=|S_g|^{-1}\sum_{s\in S_g}(m_{s,g}^{\mathrm{method}}-
m_{s,g}^{\mathrm{Frozen}})$，再取所有 scaffold 的等权均值
$\hat\Delta=|G|^{-1}\sum_g d_g$。95% CI 用同一 $d_g$ 向量做固定 seed 的
scaffold bootstrap；Local--Frozen 与 Local--Permutation 使用完全相同的
scaffold 向量和重采样规则。不得再把 pooled event 点估计和 scaffold 区间配对。
报告：

- Local−Frozen 与 Local−Permutation 的 paired correctness difference 和 95% CI；
- rescued/harmed decisions、action changes、首次 action 一致率、feedback release
  后的首个可影响 action；
- natural stream 的损失/收益与 controlled stress 的恢复；
- receipt validity、update p95、calls 和 pending queue length。

runtime failure、unknown、refusal 或缺少 chemical label 的 terminal 只写入
ledger，不生成 chemical residual，且所有方法保持相同的未更新规则；本卡的
sealed receipt 没有这些事件，但该规则仍由 fixture 和 verifier 检查。

不得把重复 schedule 当作独立分子，也不得把 v1 的 pooled Local−Shared 区间带入。

## 证伪条件

以下任一成立，v2 不支持“正确 selected-provider credit 是必要机制”：

1. Local 与 Frozen 首次 action 不完全一致；
2. Local 在 controlled paired contrast 不优于 no-update，或不优于固定错配 credit；
3. permutation arm 在 natural stream 产生更小损失且 controlled recovery 不劣；
4. 任一方法读取了未选 output、当前 truth 或提前释放的 reward；
5. pooled 点估计与 bootstrap 区间使用不同 estimand，或 receipt 不能独立回放；
6. permutation 映射、tie-break、failure/unknown/refusal 规则未固定或未写入
   config。

失败时保留全部日志，回到方法步骤，不改名、不重选 benchmark、不调参到赢。

## 运行门槛和产物

运行前需有：本卡的 Git commit、代码 hash、sealed input hash、固定置换 protocol
id 与明文映射、构造案例断言和独立 reviewer 回执。运行后必须保存 raw JSONL、config、summary、
per-scaffold metrics、verifier 输出和 mutation rejection。v2 不申请新 Nebula
任务，除非 CPU replay 已显示现有缓存不足以执行这个匹配对照。
