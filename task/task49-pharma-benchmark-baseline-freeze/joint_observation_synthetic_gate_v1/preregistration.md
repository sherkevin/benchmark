# Joint observation synthetic gate v1

日期：2026-10-05  
目的：在进入真实 BBB 联合预测前，验证小型 mixture-of-products 是否能从 paired observations 中区分“相同 provider 边际、不同联合错误结构”。

## 固定协议

- 两个二分类真值 `y∈{0,1}`，先验相等；每个样本观察两个 provider 的输出。
- 场景 `same_marginal_different_joint`：两类的每个 provider 边际均为 Bernoulli(0.5)，但 `y=1` 时两 provider 高度同向，`y=0` 时高度反向。Independent model 因而看不到区分信号；2-mode joint model 可以表示该结构。
- 场景 `explicit_failure_state`：输出域为 `{0,1,⊥}`，`⊥` 是运行失败，不改写为化学 0/1；联合模型和 Independent model 使用同一三分类观测合同。
- fit/evaluation 各 8,000 个样本，按真值均衡；seed=`20261005`；EM 重启种子为 `0..7`；最大迭代 300；Dirichlet smoothing `alpha=0.5`。
- 训练只看 fit tuples 和真值；evaluation 只用于一次性计算 conditional NLL、posterior Brier 和 accuracy。

## 闸门

Joint model 必须在第一个场景同时满足：

1. held-out conditional NLL 比 Independent model 至少低 `0.05` nat/sample；
2. posterior Brier 至少低 `0.05`；
3. accuracy 至少高 `0.10`。

第二个场景只检查 `⊥` 的观测语义是否被保留，并要求脚本没有把 `⊥` 转成 0/1。这里不以结果证明真实数据可行，也不授权 GPU 或 paper claim。

## 否定处理

若第一场景失败，停止扩展 latent mode、backbone 或 planner，先回到参数化和观测合同。若第一场景通过但第二场景失败，模型只能处理 valid binary outputs，不能进入真实 provider panel，因为真实 panel 存在 runtime failure。
