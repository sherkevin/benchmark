# Calibrated provider residual transfer v2

日期：2026-09-27  
状态：候选，先做 CPU 回放；confirmation 与 Nebula 关闭。

## 问题与假设

普通独立 selected-only 更新只修正被选 provider。信息筛选显示 fit 区 provider correctness residual 有相关性，但旧 provider-transfer 使用未校准 BT score，残差单位不成立。本版固定 correctness 语义后检验：如果 provider 的错误有稳定的 fit-only 相关结构，选中 provider 的一条反馈能否低成本修正其它 provider 的概率。

## 冷启动与 fit-only 关系

冻结 BT score $s_a(x)$ 先经过每 provider 的 affine logistic calibration：

$$q_a^0(x)=\sigma(\alpha_a s_a(x)+\beta_a),\qquad \alpha_a\ge0,$$

参数只用 outer-fold fit 区 `system_success`。在同一 fit 区定义 $e_{ia}=y_{ia}-q_a^0(x_i)$，中心化协方差 $C=\widetilde E^\top\widetilde E/(n-1)$，并令 $T_{ba}=C_{ba}/\max(C_{aa},10^{-8})$。evaluation 不参与 $T$。

## 在线状态和动作

只对已经释放的 selected feedback 维护每个 source provider 的样本数 $n_a$ 和残差均值 $\bar e_a$。对任意候选 provider $b$，动态 correction 是

$$
\Delta_b=\sum_{a=1}^{K}T_{ba}\frac{n_a}{n_a+1}\bar e_a,
\qquad
\hat q_b=\operatorname{clip}(q_b^0+\operatorname{clip}(\Delta_b,-0.25,0.25),10^{-4},1-10^{-4}).
$$

选择 $\arg\max_b\hat q_b$。反馈到达时只更新被选 source 的 $n_a,\bar e_a$，其中 $e=r-q_a^0(x)$；未选 provider 的 evaluation 标签不进入状态。`T` 的对角保留为 independent residual bias，置零 off-diagonal 是关键消融。

算例：三个 provider 的 $T_{:,A}=(1,0.6,-0.2)^\top$。A 的校准概率为 $0.6$，释放反馈 $r=0$，所以 $e=-0.6,n_A=1,\bar e_A=-0.6$。B 得到 $\Delta_B=0.6\cdot(1/2)\cdot(-0.6)=-0.18$，C 得到 $+0.06$；这两个变化来自 fit-only 关系和一条 selected feedback，不读取 B/C 当前题标签。

## 对照、成本与停止线

Frozen calibrated、independent residual（$T=I$）、transfer、permuted-$T$、Best Fixed 同一 fold/seed/delay 对比。固定 delay `0/1/4`、5 folds、2 seeds；报告 system success、paired delta、state update/selection p50/p95。更新 $O(K)$，菜单评分 $O(K^2)$（可预计算 $T$ 与稀疏化），状态 $O(K)$。

若 transfer 在三个 delay 中至少两个低于 independent，或真实 $T$ 与 permuted-$T$ 无稳定差异，则停止；不扫 shrinkage、clip、context 或 covariance rank。即使超过 Frozen 但不超过 Best Fixed，也只能作为安全适配对照，不能写成主算法创新。
