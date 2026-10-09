# Provider-transfer replay v1

日期：2026-09-27

## 目的

验证 `provider_transfer_method_card_v1.md` 的最小状态更新在真实 selected-only 回放中是否有可见的信息优势。这个回放不是最终效果实验，也不打开 confirmation；它只决定是否值得接入统一 Nebula runner。

## 固定设置

- 输入：`fingerprint_diagnostic_v1/inputs.json`，5 个 scaffold-disjoint evaluation streams。
- 冻结分数：每个 fold 的 fit-only provider accuracy 转成 logit，作为所有方法的共同冷启动分数。
- 请求表示：当前冻结 Morgan bit vector 除以其 L2 norm；这是低成本方法筛选代理，不是最终 backbone 选择。
- provider 几何：fit-only residual covariance 的前 `q=4` 个非负谱方向，$U=V_q\Lambda_q^{1/2}$。
- dynamic 参数：`eta=0.1`、`lambda=0`、`rho=0.5`、`score_clip=0.5`、`n0=0`。不使用 evaluation 调参。
- delays：`0,1,4`；释放规则是决策 tick 加 `1+delay`，只在释放时更新。
- 对照：Frozen、独立 Online-SGD、provider-transfer、固定种子行置换后的 provider-transfer。
- 选择：只使用当前分数和已释放 selected reward；未选 provider 输出不进入状态。

## 记录和否定条件

记录逐题 decision/feedback、累计准确率、跨 provider 更新次数、更新耗时和状态范数。若真实 provider-transfer 在三个 delay 中至少两个低于 Frozen，或真实 `U` 与置换 `U` 没有同方向差异，则删除这条主方法，不申请新的 A800 完整方法任务。
