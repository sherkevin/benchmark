# BBB 动态验证门 post-freeze 回放结果 v1

日期：2026-09-25

## 先说结论

这一轮没有证明当前 M2 动态门有效。主成本 `call_cost=0.1` 下，M2 和随机 probe 的结果完全相同；固定 Morgan 二次调用明显更好。当前实现还不能作为论文里的最终方法结果。

这轮结果的价值是把问题定位清楚了：安全的 HT/Freedman 置信门在当前 provider-marginal、one-hot 状态上太保守，成本稍高时直接退化成随机探索；成本为零时虽然准确率上升，但调用基本打满，没有形成有效的调用收益权衡。

## 实验边界

- 分析集：冻结缓存中的 730 个分子，排除 8 个 unresolved，剩余 722 个 resolved 分子。
- 切分：按 Bemis–Murcko scaffold 做固定切分，429 个 scaffold 中 80 个 scaffold、106 个分子进入 held-out，其余 616 个分子作为 calibration stream。
- 在线策略先读 calibration，再按固定 entity 顺序进入 held-out；反馈延迟一个 episode。
- 策略：`never`、`always_morgan`、`always_mpnn`、M1 `random_probe`、M2 `dynamic_gate`。
- 运行：20 个随机种子，调用成本 `0/0.05/0.1/0.2`，共 116,792 条逐 episode 原始记录。
- 这 722 个分子此前已经参与 runner 调试和参数选择，所以本结果是 **post-freeze retrospective evaluation**，不是未触碰的确认性测试。
- 调用成本是预注册的 synthetic cost，不代表真实延迟或线上价格。

## 主要结果

`net utility = terminal loss + call_cost × tool calls`，越低越好。

| call cost | policy | accuracy | terminal loss | calls / episode | net utility |
|---:|---|---:|---:|---:|---:|
| 0.00 | never | 0.5943 | 0.4057 | 1.000 | 0.4057 |
| 0.00 | always Morgan | **0.8962** | **0.1038** | 2.000 | **0.1038** |
| 0.00 | always MPNN | 0.7547 | 0.2453 | 2.000 | 0.2453 |
| 0.00 | random probe | 0.7745 | 0.2255 | 1.795 | 0.2255 |
| 0.00 | dynamic gate | 0.8269 | 0.1731 | 1.979 | 0.1731 |
| 0.05 | never | 0.5943 | 0.4057 | 1.000 | 0.4557 |
| 0.05 | always Morgan | **0.8962** | **0.1038** | 2.000 | **0.2038** |
| 0.05 | always MPNN | 0.7547 | 0.2453 | 2.000 | 0.3453 |
| 0.05 | random probe | 0.7745 | 0.2255 | 1.795 | 0.3152 |
| 0.05 | dynamic gate | 0.7915 | 0.2085 | 1.863 | 0.3017 |
| 0.10 | never | 0.5943 | 0.4057 | 1.000 | 0.5057 |
| 0.10 | always Morgan | **0.8962** | **0.1038** | 2.000 | **0.3038** |
| 0.10 | always MPNN | 0.7547 | 0.2453 | 2.000 | 0.4453 |
| 0.10 | random probe | 0.7745 | 0.2255 | 1.795 | 0.4050 |
| 0.10 | dynamic gate | 0.7745 | 0.2255 | 1.795 | 0.4050 |
| 0.20 | never | 0.5943 | 0.4057 | 1.000 | 0.6057 |
| 0.20 | always Morgan | **0.8962** | **0.1038** | 2.000 | **0.5038** |
| 0.20 | always MPNN | 0.7547 | 0.2453 | 2.000 | 0.6453 |
| 0.20 | random probe | 0.7745 | 0.2255 | 1.795 | 0.5844 |
| 0.20 | dynamic gate | 0.7745 | 0.2255 | 1.795 | 0.5844 |

20 个 seed 的 paired net utility 差异（dynamic minus baseline）：

- `call_cost=0.10` 对 `random_probe`：`0.0000`，因为两者动作完全一致。
- `call_cost=0.05` 对 `random_probe`：`-0.0136`，95% seed t 区间 `[-0.0252, -0.0019]`；有小幅收益，但仍比 always Morgan 高 `0.0979`。
- `call_cost=0.00` 对 `random_probe`：`-0.0524`；动态门的准确率提高到 `0.8269`，但平均调用达到 `1.9788`。
- `call_cost=0.10` 对 always Morgan：`+0.1012`，说明当前动态门距离最强固定策略仍有明显差距。

## 完整性检查

- 116,792 条 raw JSONL 记录，无重复 `(call_cost, policy, seed, entity)`。
- held-out 共 106 个分子；所有 held-out episode 都是 `scored`。
- 所有在线选择的 inclusion propensity 都满足 `0 < π ≤ 1`，本轮最小值为 `0.2`。
- 脚本在出现未评分 held-out 时会直接失败，本轮没有触发该条件。

## 对方法的判断

现在的瓶颈不是“有没有实时更新代码”，而是更新后的状态没有足够的上下文区分能力。当前状态只按 provider 做 one-hot 的 provider-marginal gain 估计，同一个 Morgan 或 MPNN 的平均收益被用于所有分子。它能学到“哪个 provider 平均更好”，但不能学到“对当前这个分子，哪个 provider 值得调用”。

同时，确定性 overlap floor 加上逐 look 的 Freedman 半径保证了估计合同，但在 616 条 calibration、80 个 scaffold 的规模上造成较宽的置信区间。`call_cost=0.1` 时 LCB 没有超过成本，门一直关闭；这不是代码错误，而是当前统计状态和门规则共同造成的退化。

因此下一版应该优先做 **context-conditioned dynamic JEV**：保留 HT/Freedman 的反馈和 propensity 合同，把状态从 provider one-hot 扩展为低维、可在线更新的上下文特征，并用冷启动先验控制新 scaffold 的方差。不能直接在这批 held-out 结果上调阈值，然后继续把它称成验证集结果。

## 可复现实验产物

- 协议：[AAMAS2027_BBB_Formal_Evaluation_Protocol_v1.md](AAMAS2027_BBB_Formal_Evaluation_Protocol_v1.md)
- 运行配置：[config.json](../experiments/logs/bbb_formal_postfreeze_retrospective_v1_20260925/config.json)
- 逐 episode 原始记录：[raw.jsonl](../experiments/logs/bbb_formal_postfreeze_retrospective_v1_20260925/raw.jsonl)
- 汇总结果：[summary.json](../experiments/logs/bbb_formal_postfreeze_retrospective_v1_20260925/summary.json)
- 代码：[evaluate_bbb_formal.py](../src/probe_headless/evaluate_bbb_formal.py)

