# Context residual memory v2：校准后的回放协议

日期：2026-09-27  
状态：已执行 CPU 回放；confirmation 关闭。

## 为什么改版本

v1 把 BT provider score 直接放进 $\sigma(\cdot)$，再把概率 residual 加回 raw ranking score。BT 分数只负责排序，单位不对，不能支持 correctness residual 的解释。v2 先在每个 outer-fold 的 fit 区对每个 provider 拟合固定的 affine logistic calibrator：

$$z_a(x)=\alpha_a s_a(x)+\beta_a,\qquad q_a(x)=\sigma(z_a(x)).$$

其中 $s_a(x)$ 是冻结 BT score，$(\alpha_a,\beta_a)$ 用 fit 区的 `system_success` 拟合，斜率非负、slope ridge 固定为 $1$，不使用 evaluation。动态记忆保存 $r-q_a(x)$；读取后在概率空间形成 $\hat q_a=\operatorname{clip}(q_a+\Delta_a,10^{-4},1-10^{-4})$，最后用 $\operatorname{logit}(\hat q_a)$ 选择 provider。这样 residual 和更新后的分数在同一 correctness 语义下。

贯穿算例：若 `admetlab` 的冻结 BT score 是 $s=0.8$，fit 区校准得到 $(\alpha,\beta)=(1.2,-0.4)$，则 $q=\sigma(0.56)\approx0.636$。已释放 selected reward 为 $r=0$，写入 $e=-0.636$。下一条相似请求若 top-$L$ 加权 residual 是 $-0.3$，则 $\hat q=0.336$，选择分数为 $\operatorname{logit}(0.336)\approx-0.68$；没有记忆时分数仍为 $0.56$。

## 对照和否定条件

同一 fit-only calibration、fold、seed、evaluation stream 和 delayed selected-only feedback 下比较 Frozen calibrated、independent provider residual bias、context memory 和 random memory。若 context memory 在至少两个 delay 低于 independent，或没有超过 random memory，则停止这个构造，不做容量/kernel 搜索，不打开 confirmation。

这是一条方法语义修复后的 falsification replay，不是新增工具预测或最终论文确认实验。
