# 三个在线核心：计算式、接口和完成边界

2026-09-26。代码：`src/probe_headless/pharma_online_baselines.py` 与 `pharma_selection_stream.py`。

本轮解决的是强 baseline 的在线部分。完整 PILOT / BayesianRouter 仍需离线训练；这里不把它们改名为 Proposed，也不把“可以递推”当成创新。

## 输入只保留必要对象

$x$ 是当前题目的冻结 query 向量；$a$ 是所选具体 provider；$r\in\{0,1\}$ 是题目终止且反馈到期后释放的 system success。各 arm 的均值向量为 $\mu_a$，协方差为 $P_a$。三者均使用同一个 query 向量，而不是每个候选各一份表示。

实例：BBB 分子 `z000282` 的 canonical SMILES 经本轮 RDKit Morgan radius=2、64-bit、L2 归一化得到 $x\in\mathbb R^{64}$。这只是接口检查特征。若选 DeepPurpose-CNN，它返回预测 1，参考标签为 1，反馈 $r=1$。未选 GraphB3 的输出不传给 learner。实际输入向量与菜单存于日志 `inputs.json`。

Laya 提取计划产生的是每候选 marker 向量。不能把它直接塞进这里并声称保持 PILOT 原生 query 接口；后续必须明确公共 query 表示，或把 candidate-specific 版本单独列为适配消融。

## 1. 共享的精确递推

给定正的 prior precision $\lambda_a$、固定 prior $\mu_{a,0}$ 和观测噪声方差 $\sigma^2>0$，在线状态对应以下目标：

$$
\mu_a=\arg\min_w\left\{\lambda_a\|w-\mu_{a,0}\|_2^2+
\frac1{\sigma^2}\sum_{i:\,a_i=a,\ i\text{ 已释放}}(r_i-x_i^\top w)^2\right\}.
$$

实例：若 CNN 已收到两条题后反馈，就只纳入那两条记录；另一条 CNN 结果仍在等待反馈时不进入求和。$\lambda_a=1$、$\sigma^2=1$ 是本轮 Greedy/PILOT 检查值。失败的 system success 为 0，但化学正确性仍为 null；未知真值整批停止，不补成 0。

初始化 $P_a=\lambda_a^{-1}I$。仅被选中的 arm 在反馈到达时更新，右侧全部使用更新前状态：

$$
g=\frac{P_a x}{\sigma^2+x^\top P_a x},\qquad
\mu_a^+=\mu_a+g(r-x^\top\mu_a),\qquad
P_a^+=P_a-\frac{P_a x x^\top P_a}{\sigma^2+x^\top P_a x}.
$$

实例：二维计算例 $x=(1,0)^\top$、$\mu_a=(0,0)^\top$、$P_a=I$、$r=1$、$\sigma^2=1$。得到 $g=(1/2,0)^\top$，$\mu_a^+=(1/2,0)^\top$、$P_a^+=\operatorname{diag}(1/2,1)$。其它 provider 状态不变。这是 Sherman–Morrison / Gaussian conditioning 的标准恒等式，不是我们的新公式。

正则项使批量解唯一；固定特征下，按任意反馈到达顺序累积相同的合法样本，终态相同。决策轨迹仍可因到达顺序不同而改变。本轮以独立批量求逆解检查递推，未把 binary reward 下的 Gaussian surrogate 称为校准的 Bernoulli 后验。

## 2. 三种分数不能混用

**Greedy LinUCB，AAAI 2026 Algorithm 1 的 H=1 适配：**

$$
s_a=x^\top\mu_a+\alpha\sqrt{x^\top P_a x},\qquad a=\arg\max_a s_a.
$$

实例：以上二维更新后，在相同 $x$ 上、$\alpha=0.5$，已更新 arm 的分数为 $0.5+0.5\sqrt{0.5}$，未更新零 prior arm 为 $0.5$。代码与固定官方 notebook 数值函数逐步对齐。H=1 后就是普通 disjoint LinUCB，不能靠近年的论文名掩盖这一点。

**PILOT §2.2.2：**令 $x$ 为单位长度的已学习 query projection。

$$
s_a=\frac{x^\top\mu_a}{\|\mu_a\|_2}+\alpha\sqrt{x^\top P_a x},\qquad
\mu_{a,0}=\theta_{a,\mathrm{pref}}.
$$

实例：$x=(1,0)^\top$，两个 prior 为 $(100,100)^\top$、$(1,0)^\top$，且 $\alpha=0$，PILOT 选择第二个，因为余弦是 $1/\sqrt2$ 和 1；点积会错误选择第一个。更新仍采用上一节的 ridge，不能把后验向量每步直接改写成单位向量。零 query 拒绝；零 arm 向量的余弦延拓为 0，是明确的实现约定，原文没有定义该奇点。

尚缺 §2.2.1 的两阶段 fit：query triplet projection、冻结 projection 后的 provider preference fit；$\lambda_a$ 按原文来自离线准确率倒数。当前随机 prior 不代替这些步骤。医药工具没有已核实成本排序，原文 smaller-model hard negative 如何迁移仍待解决，不凭 provider 名字编排序。

**BayesianRouter 式 4–6：**

$$
s_a=x^\top\mu_a+\sqrt{x^\top P_a x}\,\epsilon_a,\qquad
\epsilon_a\overset{\mathrm{ind}}\sim\mathcal N(0,1),\qquad a=\arg\max_a s_a.
$$

实例：对同一个 query，逐 arm 独立抽一个标准正态数。这个 scalar draw 与先抽 $w_a\sim\mathcal N(\mu_a,P_a)$ 再算 $x^\top w_a$ 的动作分布相同；省去每次对整个协方差做 Cholesky。不能加上观测噪声 $\sigma^2$，也不能把选中动作的 propensity 写成 1（代码记 null）。这只适用于逐 query 独立抽样，不保证与“同一 batch 共享一份权重样本”联合分布相同。

尚缺离线 BT + $0.2$ 倍 auxiliary correctness BCE、共享表示训练和 BT arm embedding prior。本任务移除原方法 DPO loss 的 quantile normalization，直接用二元 system success；论文 prior variance=0.02、noise variance=1，但 full method 不能用随机 prior 代替已学习 prior。源码无声明许可，计算按论文独立实现。

## 3. 时钟与信息边界

$$
\text{release\_tick}=\text{decision\_tick}+1+d,\qquad d\in\{0,1,4\}\ \text{（本轮检查）}.
$$

实例：tick 0 先选择和终止。$d=0$ 的反馈在 tick 1 下一个决策之前更新；$d=4$ 则到 tick 5 才更新。旧样本特征在决策时复制，不能在等待过程中被外部数组修改。原模型版本是 decision version，release 另记前后版本，不能假设二者相邻。

`Selection` 绑定 event/query/provider 和冻结特征摘要；queue 验证终止、query、arm 与所选输出。失败仍有 system reward 0；未执行 provider 没有反馈。诊断日志里可有 evaluator-only 可恢复真值，但 `observe()` 仅接收 event/provider/reward，不接全表或真值。这是可信研究进程的接口隔离，不是针对恶意 Python 调用者的安全沙箱。

## 4. 复杂度和实时性边界

设 feature 维度为 $d$、候选数为 $K$。每次更新时间 $O(d^2)$；全候选评分 $O(Kd^2)$；统计状态内存 $O(Kd^2)$。延迟队列额外存每条 pending feature 的 $O(d)$，exactly-once 去重表另需 $O(T)$ 个历史 ID。不能把总状态写成与运行长度完全无关。

这些成本不含 backbone 编码、离线 fit 和日志。本地 float64 检查不证明 A800 完整服务达到某个毫秒目标；实际延迟和准确率还要在同输入、同反馈、同预算下测。

## 来源和复用

- Online Multi-LLM Selection：`baseline_audit/online_multi_llm/` 的 MIT 固定快照；测试只 AST 提取 `LinUCBModel` 数值函数，未执行 API 代码。
- PILOT：`method_audit/pilot_text.txt`，论文 pp.23936–23937；官方 PDF 在 Task45。
- BayesianRouter：`method_audit/bayesianrouter_text.txt`，式 2–6、Appendix A.1；官方 PDF 在 Task45。
- 复用 Task36 的真实 replay/judge 和 Task42 的 fixed-Agent 终局会计，新增代码仅连接在线状态和到期队列。

完整 URL、hash、commit 在 Task49 `sources.jsonl` 与 `baseline_audit/*/SOURCE.json`。下一步是补齐离线初始化和表示诊断，不把当前 adapter smoke 升格为 baseline 效果或方法创新。
