# Context residual memory replay v1

日期：2026-09-27

## 目的

provider-transfer 的全局传播没有超过 independent selected-only。这里测试另一种信息结构：反馈只进入被选 provider 的有界局部记忆，未来请求只从同一 provider 的相似 hidden feature 读取 residual correction。

## 固定设置

- 输入：同一 `fingerprint_diagnostic_v1/inputs.json` 与 matched-SGD 恢复的冻结 BT prior/64 维 hidden features。
- 记忆：每个 provider 一个 FIFO，容量 `64`；只存已释放 selected feedback 的 `(normalized hidden feature, frozen residual)`。
- kernel：非负 cosine，取 top-5 正相似邻居；没有邻居时 correction 为 0。
- residual：$e=r-\sigma(\ell_a^0(x))$，不使用动态分数，避免把当前标签写回当前预测。
- score：冻结 logit 加 `clip(memory_correction, -0.5, 0.5)`；系数 `gamma=1`。
- delays：`0,1,4`，释放 tick 为 decision tick 加 `1+delay`。
- 对照：Frozen、independent selected-arm bias、context memory；三者共享冻结 prior、流顺序和反馈规则。

不读取未选 provider 输出，不调用新工具，不训练模型，不打开 confirmation。所有参数在回放前固定，不按 evaluation 调整。

## 否定条件

若 context memory 在三个 delay 中至少两个低于 independent bias，或 top-5 相似度没有超过预先记录的随机邻居对照，则不把 memory 写成主方法；结果保留为表示/局部适应的负证据。
