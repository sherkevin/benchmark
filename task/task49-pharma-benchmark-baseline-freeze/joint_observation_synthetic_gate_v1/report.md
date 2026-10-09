# Joint observation synthetic gate v1

日期：2026-10-05  
状态：**通过合成资格闸门；不代表真实 BBB 效果，也不打开 confirmation。**

## 目的

测试一个小型 two-mode mixture-of-products 是否能从 paired observations 学到 independent model 看不到的联合错误结构。这个测试只验证预测可行性，不解释 latent mode 的化学语义，也不测试调用策略的 utility。

## 协议

- 两个二分类真值，先验相等；每个样本观察两个 provider 输出。
- `same_marginal_different_joint` 中两个 provider 在两个真值下的边际都为 Bernoulli(0.5)。`y=1` 为高度同向，`y=0` 为高度反向；因此 Independent model 没有可用区分信号。
- `explicit_failure_state` 将观测域扩展为 `{0,1,⊥}`，其中 `⊥` 是 runtime failure，脚本不会把它改写为 0 或 1。
- fit/evaluation 各 8,000 行；seed、EM restart、平滑和迭代次数见 `preregistration.md`。
- 比较 Independent Evidence 与同样 provider 数、同样观测域的 2-mode Joint model。

## 结果

### 相同边际、不同联合结构

| model | conditional NLL | posterior Brier | accuracy |
|---|---:|---:|---:|
| Independent | 1.3863 | 0.2500 | 0.5044 |
| Joint | 1.0140 | 0.0884 | 0.9020 |
| Joint − Independent | −0.3722 | −0.1616 | +0.3976 |

三项预登记闸门均通过：NLL 至少下降 0.05、Brier 至少下降 0.05、accuracy 至少提升 0.10。

### 显式 failure state

在 `{0,1,⊥}` 观测域下，Joint 相对 Independent 的 conditional NLL 为 `−0.0505`，Brier 为 `−0.0234`，accuracy 为 `+0.1121`。`⊥` 保留为第三类观测；该场景只证明错误状态可以进入同一观测合同，不能证明真实 provider failure 可由该模型正确预测。

## 判定与边界

1. **通过**：联合观测机制在一个构造的 same-marginal/different-joint 环境中有可检验的预测收益；这给真实 paired holdout 提供了合理的 CPU gate。
2. **未证明**：没有证明 BBB 数据存在同样的结构，没有证明 latent mode 可解释，没有证明联合模型能改善 selector/verifier 的最终 utility，也没有证明 delayed online update 有收益。
3. **下一道闸门**：用真实 BBB 的 fit-only panel 做 scaffold-disjoint held-out pair likelihood/NLL/Brier/ECE，对比 Independent、family-only 和同容量 pairwise conditional control；同时检查 runtime failure、provider family 覆盖和静态 cache 的合法性。
4. **主任务边界**：该结果不改变 E2a 一次调用 primary；H=2 仍是独立的 sequential evidence extension。

原始机器结果：`result.json`。协议：`preregistration.md`。实现：`src/probe_headless/joint_observation_synthetic_gate.py`。
