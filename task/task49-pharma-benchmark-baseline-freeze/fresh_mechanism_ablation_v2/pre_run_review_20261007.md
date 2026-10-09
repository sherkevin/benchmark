# v2 pre-run design review

日期：2026-10-07

## 结论

**设计卡 PASS；代码门槛待实现后复核。** 独立审查人确认本卡已经把 v1 的
静态初始重排和估计量混用问题拆开，但执行前仍必须在代码、fixture 和 receipt
中证明两项硬条件：

1. permutation 维护 source-indexed 的 $n_a,s_a$，按源 provider 的 fit-rate
   anchor 计算 $δ_a$，只把该增量写入 $π(a)$ 的 residual 槽；不能使用目标
   provider 的 anchor 或计数器。
2. 单个 reward mutation 只改变 Local 的源 residual 与 permutation 的预期
   destination residual，其他状态、score 和 event sequence 保持不变。

## 已审查的设计约束

- provider 顺序、first-index tie-break、固定无不动点置换和 release 时钟已写入卡片；
- 在每条 stream 的每个有效 feedback release 之前，逐事件比较完整 score vector 和
  argmax，而不是只比较第一条 action；
- failure、unknown、refusal 或缺少 chemical label 不产生 chemical residual；
- 主 estimand 唯一固定为等权 scaffold cluster，点估计和 bootstrap 使用同一聚合规则；
- permutation 只接收 selected identity、released reward 和 release tick，不能读取
  未选 output、当前 truth、未来 feedback 或 evaluator drift 标记；
- v2 不复用 v1 Shared-Credit 的结果或区间。

## 状态

本文件是运行前设计回执，不是实验结果。待实现提交后，需追加代码 hash、fixture
命令和 verifier 回执；在这些证据产生前不得启动 full replay，也不得把 v2 写入主稿。
