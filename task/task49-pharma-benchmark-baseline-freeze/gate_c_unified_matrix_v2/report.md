# Gate C unified matrix v2

日期：2026-10-06  
状态：Stage A development audit；E6 未打开。

这版把 role-qualified 主表、baseline contract v3、E5 accounting、机制矩阵 v2 和 paired uncertainty v2 接到同一张 E1--E5 coverage matrix。普通任务、受控 family outage、post-call utility 和 semantic smoke 仍分 scope。

## 结论

Stage A 的 E1--E5 已经有完整的证据入口，但仍有局部限制：E3 的 interface-null/family-permutation 边界没有单独 runner；普通 cohort 的 unknown/refusal 都是零观察，拒答效用只在 post-call extension 中测量。E6 confirmation 明确仍为 `MISSING`，没有读 confirmation，也没有申请 A800。

机器表：`matrix.json`。
