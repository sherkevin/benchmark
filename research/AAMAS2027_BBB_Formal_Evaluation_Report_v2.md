# BBB 动态验证门 post-freeze 对照结果 v2

日期：2026-09-25

## 结论

v2 加入 no-update control 后，结论更清楚：当前 M2 的收益主要来自“允许更多二次调用”，不是稳定的在线学习收益。

在主成本 `call_cost=0.1` 下，dynamic gate、random probe 和 no-update gate 三者完全相同。`call_cost=0.05` 下 dynamic gate 比 no-update gate 的平均 net utility 低 `0.0136`，但 95% seed 区间包含 0，不能说明在线更新本身有稳定贡献。

固定 Morgan 仍然是最强策略，当前 provider-marginal M2 不足以作为论文主方法。

## v2 设置

- 722 个 resolved BBB 分子：616 calibration，106 held-out。
- Bemis–Murcko scaffold 切分：429 个 scaffold，80 个 held-out scaffold。
- 20 个随机种子，调用成本 `0/0.05/0.1/0.2`。
- 新增 `no_update_gate`：与 M2 使用相同探索和门逻辑，但把所有反馈延迟到完整 stream 结束，决策过程中不发生在线更新。
- 共 174,552 条逐 episode raw 记录。

这批分子已经参与过此前的 runner 调试和参数选择，因此仍然是 post-freeze retrospective evaluation。仓库内没有可证明此前未参与调参的新分子池和完整 provider snapshot，不能把当前 scaffold split 称为 fresh confirmation。

## 结果

| call cost | policy | accuracy | calls / episode | net utility |
|---:|---|---:|---:|---:|
| 0.00 | always Morgan | **0.8962** | 2.000 | **0.1038** |
| 0.00 | random probe | 0.7745 | 1.795 | 0.2255 |
| 0.00 | no-update gate | 0.7745 | 1.795 | 0.2255 |
| 0.00 | dynamic gate | 0.8269 | 1.979 | 0.1731 |
| 0.05 | always Morgan | **0.8962** | 2.000 | **0.2038** |
| 0.05 | random probe | 0.7745 | 1.795 | 0.3152 |
| 0.05 | no-update gate | 0.7745 | 1.795 | 0.3152 |
| 0.05 | dynamic gate | 0.7915 | 1.863 | 0.3017 |
| 0.10 | always Morgan | **0.8962** | 2.000 | **0.3038** |
| 0.10 | random probe | 0.7745 | 1.795 | 0.4050 |
| 0.10 | no-update gate | 0.7745 | 1.795 | 0.4050 |
| 0.10 | dynamic gate | 0.7745 | 1.795 | 0.4050 |
| 0.20 | always Morgan | **0.8962** | 2.000 | **0.5038** |
| 0.20 | random probe | 0.7745 | 1.795 | 0.5844 |
| 0.20 | no-update gate | 0.7745 | 1.795 | 0.5844 |
| 0.20 | dynamic gate | 0.7745 | 1.795 | 0.5844 |

dynamic gate 相对 no-update gate 的 paired net utility 差：

- `call_cost=0.00`：`-0.0524`，95% seed t 区间 `[-0.0641,-0.0406]`。
- `call_cost=0.05`：`-0.0136`，95% seed t 区间 `[-0.0339,0.0067]`。
- `call_cost=0.10`：`0.0000`，95% seed t 区间 `[-0.0199,0.0199]`。
- `call_cost=0.20`：`0.0000`，95% seed t 区间 `[-0.0192,0.0192]`。

## 数据审计

- raw JSONL 174,552 行，无重复 `(call_cost, policy, seed, entity)`。
- held-out episode 全部 `scored`。
- 所有选择的 propensity 满足 `0 < π ≤ 1`，最小值为 `0.2`。

## 方法判断与下一步

no-update 对照说明，当前 one-hot provider 状态没有稳定识别“在线反馈什么时候改变下一步决策价值”。下一版应把状态改成最小的 context-conditioned JEV：用首工具已经可见的分子结构和首工具输出形成固定 context cell，在 `(context, provider)` 单元内做 HT/Freedman 更新；新 cell 不使用未经独立验证的 global pseudo-count 作为置信保证。

这个版本先做小规模、可证明的 8-cell 分层，不直接上神经 backbone。真正 fresh scaffold 实验需要先补一批新的分子和同版本 provider snapshot；在此之前不把现有 hash split 的数字写成泛化结论。

fresh split 的训练成员重叠和进入条件见：[BBB_fresh_split_audit_20260925.md](BBB_fresh_split_audit_20260925.md)。context-conditioned MVP 的符号、case 和门合同见：[AAMAS2027_ContextConditioned_JEV_MVP_Spec_v1.md](AAMAS2027_ContextConditioned_JEV_MVP_Spec_v1.md)。

## 产物

- 协议：[AAMAS2027_BBB_Formal_Evaluation_Protocol_v2.md](AAMAS2027_BBB_Formal_Evaluation_Protocol_v2.md)
- 汇总：[summary.json](../experiments/logs/bbb_formal_postfreeze_retrospective_v2_20260925/summary.json)
- 原始记录：[raw.jsonl](../experiments/logs/bbb_formal_postfreeze_retrospective_v2_20260925/raw.jsonl)
- 代码：[evaluate_bbb_formal.py](../src/probe_headless/evaluate_bbb_formal.py)
