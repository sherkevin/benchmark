# Context residual memory JEV：方法卡 v1

> **已停止（2026-09-27）**：该版本把 BT 排序分数当作 correctness 概率，已由 ADR-0033 标记为历史候选。不要用 v1 的回放数字支持方法效果；校准后的 v2 也未通过闸门。

日期：2026-09-27

状态：通过零机时 replay 的候选方法，尚未做正式 A800 效果实验。

## 1. 任务边界

冻结 JEV 对分子性质请求 $x_t$ 和 provider $a$ 给出 logit $\ell_a^0(x_t)$。策略选择一个 provider；工具结束并释放反馈后，只得到本次所选 provider 的 $r_t\in\{0,1\}$。动态模块不能读取本题未选 provider 的输出，也不能在决策前看本题真值。

贯穿算例：本题选择 `admetlab`，冻结 logit 是 $\ell^0=0.8$，所以冷启动概率是 $p^0=\sigma(0.8)\approx0.690$。已释放反馈为 $r=0$，分子 hidden feature 归一化后是 $h=(0.8,0.6)^\top$。这条记录只写入 `admetlab` 的 memory。

## 2. 写入

每个 provider $a$ 保存一个容量为 $B$ 的 FIFO 记忆。反馈释放时保存冻结 residual

$$
e_t=r_t-\sigma\!\left(\ell_{a_t}^0(x_t)\right),
\qquad
m_t=(h_t,e_t)\ \text{写入}\ M_{a_t}.
$$

使用冻结概率而不是动态概率计算 residual，避免用更新后的预测重新解释同一标签。未选 provider 的 memory 不变；未释放的反馈不写入。

算例中 $e=0-\sigma(0.8)\approx-0.690$，所以写入 `admetlab` 的一条记录是 `((0.8,0.6), -0.690)`；其它 provider 仍为空。

## 3. 读取

请求 $x$ 的 hidden feature 归一化为 $h(x)$。对 provider $a$ 的每条记忆计算非负 cosine kernel

$$
k(h,h_i)=\max\{0,h^\top h_i\}.
$$

从 $M_a$ 取 kernel 最大的前 $L$ 条正相似记录，计算局部 residual correction

$$
\Delta_a(x)=
\gamma\frac{\sum_{i\in\operatorname{TopL}(M_a,h)}k(h,h_i)e_i}
{\varepsilon+\sum_{i\in\operatorname{TopL}(M_a,h)}k(h,h_i)}.
$$

没有正相似记录时定义 $\Delta_a(x)=0$。动态选择分数为

$$
s_a(x)=\ell_a^0(x)+\operatorname{clip}(\Delta_a(x),-b,b).
$$

算例：若下一次 `admetlab` 请求仍有 $h=(0.8,0.6)$，cosine 是 1，只有这一条 memory，则 $\Delta_{\mathrm{admetlab}}=-0.690\gamma$；若 $\gamma=1,b=0.5$，修正被截为 $-0.5$，分数从 $0.8$ 变为 $0.3$。其它 provider 没有记录，分数不变。

## 4. 为什么它和 replay SGD 不一样

Replay SGD 把历史样本的梯度混合到同一个全局参数里；memory 保留每条 selected feedback 的局部上下文，只在相似请求上读取。它不需要反向传播，写入成本是 $O(d)$，读取 top-$L$ 的成本是 $O(Bd)$（或由 ANN 降低），状态大小是 $O(KBd)$。容量 $B$、邻居数 $L$、$\gamma$ 和 $b$ 只能用 validation 区固定。

这个方法的前提很具体：相似 hidden feature 的 provider residual 应该相似。若 evaluation scaffold-disjoint 后没有这种局部结构，memory 会退化成随机历史，应该被删除。

## 5. 与 baseline 的消融

- Frozen：所有 $\Delta_a=0$。
- Independent bias：只维护每个 selected provider 的一个全局 bias，不读取相似度。
- Random memory：同样容量和写入规则，但读取随机历史记录，检验收益是不是来自相似性。
- Context memory：真实 cosine top-$L$ 读取。
- Provider-transfer：上一条主线的全局跨 provider 传播，已因正式 replay 未超过 independent 而停止。

主张只能是“有界 selected-only context memory 在医药工具正确性反馈上提供局部适应”。不能把 cosine、FIFO、最近邻或 replay 本身写成新算法。

## 6. 正式实验前的闸门

先在统一 runner 中复核三种 delay，和 Best Fixed、Frozen、Online-SGD、Replay-SGD、BayesianRouter-style 同表比较。若 memory 在三个 delay 中至少两个低于 independent，或真实 top-$L$ 不超过 random memory，则不申请 confirmation，也不继续调容量和 kernel。
