# BBB 动态验证门 post-freeze 对照协议 v2

日期：2026-09-25

## v2 改动

在 v1 的固定策略之外，加入 `no_update_gate` 诊断控制：它使用和 M2 完全相同的菜单、探索概率、调用成本和随机种子，但把反馈延迟到整个 calibration + held-out stream 之后。因此决策期间不会读到任何在线更新，可以区分“动态更新本身”与“固定随机 probe”带来的效果。

v1 的结果继续保留，不覆盖；v2 只在原冻结回放上补充这一项控制。

## 数据边界

- 当前仓库只有一个 730 分子的 BBB prediction pool，排除 8 个 unresolved 后为 722 个 resolved 分子。
- 这 722 个分子此前已经参与 runner 调试和参数选择，所以 v2 仍然是 post-freeze retrospective evaluation。
- 当前仓库没有一批可证明“此前未参与调参”的新分子及其完整 provider snapshot。Bemis–Murcko hash 切分只能作为结构外推 stress test，不能称为真正 fresh confirmation set。
- 真正 fresh scaffold 测试的进入条件是：新增分子池、同一版本的首 provider 和候选 provider snapshot、独立 truth/evaluator，以及新增数据生成前冻结本协议。

## 切分与顺序

- 对 canonical SMILES 计算 Bemis–Murcko scaffold；无环分子使用 `ACYCLIC:<entity_id>`。
- `sha256(scaffold) mod 5 == 0` 的 scaffold 为 held-out，其余为 calibration。
- 运行顺序为 canonical entity id 顺序。在线策略先处理 calibration，再处理 held-out；反馈延迟一个 episode。

## 策略

1. `never`：只调用 CNN。
2. `always_morgan`：CNN 后固定调用 Morgan。
3. `always_mpnn`：CNN 后固定调用 MPNN。
4. `random_probe`：M1，探索概率 0.8，随机选择候选。
5. `no_update_gate`：M2 的相同探索/门逻辑，但延迟所有反馈到 stream 结束，作为无在线学习控制。
6. `dynamic_gate`：M2 v5 safe-overlap HT/Freedman gate，反馈延迟一个 episode。

## 指标与否证

每个 held-out episode 记录 terminal accuracy、terminal loss、调用次数和 `terminal_loss + call_cost × calls`。报告 molecule-weighted、scaffold-macro 以及跨 seed 的 paired difference。

主成本为 `call_cost=0.1`，敏感性为 `0/0.05/0.2`。如果 dynamic gate 在主成本下不优于 random probe 和 no-update gate，或仍显著劣于固定 Morgan，则不声称当前 provider-marginal 方法有效。

## 结果边界

v2 只验证当前安全 gate 的行为和在线更新是否产生额外收益，不验证 backbone fine-tuning、context-conditioned JEV 或真实线上延迟/价格。任何根据 held-out 结果做的参数修改都必须进入新版本协议和新数据。

