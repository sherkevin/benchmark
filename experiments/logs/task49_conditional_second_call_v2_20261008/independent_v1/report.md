# Conditional second-call v2 独立核验

2026-10-08。核验对象为相邻 `full_v1/`，summary SHA256 为
`07724e6b1aab7a280c0aac2d99ccfc78dd6c20588bba258abe75220f666e9e00`。

**机械与数值复算通过；开发修复门槛未通过。** 本报告不把已有 bootstrap
区间当作原始分子随机到达机制的常规置信区间，原因见下文。

## 核验方式与范围

- 独立实现：`src/probe_headless/verify_conditional_second_call_v2.py`，不导入 v1 或 v2 runner。
- 执行前保存 config、源码/card 快照、输入/来源/模型哈希；过程流式保存至 `execution.jsonl`。
- 从冻结原始概率、分子真值、特征、v1 已核验模型恢复首次选择器和冻结终局验证器输出。
- 从 B 的原始行恢复旧 16 列、符号/拒答/多项式原始新增列，再以独立显式截断 SVD 恢复 B-only 投影。
- 检查现有系数的 ridge normal equations、原 16 系数不变量和投影正交性；没有重新拟合模型。
- 每个策略先只用合法首调用证据计算请求，再由 evaluator 使用其余输出计算终局结果。
- 独立按 tick 并行重放每个 bootstrap 序列和每个策略的调用控制器；与 runner 的 replicate/method 循环实现不同。

## 检查规模与误差

| 对象 | 已核验数量 |
|---|---:|
| 冻结 development 分子 | 722 |
| B paired rows | 1,518 |
| B molecule-fold observations / 去重分子 | 506 / 341 |
| B 行 × 新增 basis | 6,072 |
| 新增模型 | 20 |
| Policy / evaluator rows | 5,776 / 5,776 |
| C evaluator panels | 722 |
| 与 v1 完全一致的旧策略对账行 | 2,888 |
| Bootstrap replicates | 10,000 |
| Bootstrap utility values | 80,000 |
| Fold-policy bootstrap trajectories | 400,000 |
| 点估计与 bootstrap contrasts | 10 |

最大 normal-equation residual 为 `5.8382e-15`；原 16 系数最大差为
`3.2353e-16`；B 投影最大正交误差为 `1.4642e-13`。数值容限为 `1e-8`。
旧四策略的保存 payload 和 bootstrap 四列与 v1 完全一致。
全部 bootstrap utility 与独立重放在 `1e-12` 容限内一致。
首次完整核验耗时 6.86 秒；没有意外核验失败。

`verification.json` 保存通过结果；`model_equation_checks.json` 保存逐模型误差；
`recomputed_results.json` 和两份 `recomputed_bootstrap_*.npy` 保存独立复算。
精确零附近的 bootstrap 正值比例会受浮点累计顺序影响，因此保存比例按原始
算术核对，并确认与独立重放的符号分歧仅能出现在绝对值不超过 `1e-12` 的值。

## 实质篡改检查

四个合成篡改均在复制出的输入视图内进行，并重新封装 artifact manifest，
避免只验证“哈希不相等”。未修改原始输入或原始 run。

1. B projection 元素增加 0.01：被独立投影检查拒绝。
2. 新增系数增加 0.01：被 normal-equation 检查拒绝。
3. Policy receipt 加入 truth 字段：被严格合法字段集合检查拒绝。
4. Bootstrap 一个新增策略值增加 0.01：被完整重放检查拒绝。

四项均按预期 fail closed；完整 config、源码、stdout/stderr、错误栈和结果在
`mutations_v1/`。这些是人工故障注入，不是新增科学结果。

## 科学解释边界

`signed_refusal` 相对原样本最强已实现对照 `confidence_cascade` 的 utility
差为 **−0.002119/input**，保存区间为 **[−0.012783,+0.002458]**；预设开发修复门槛失败。
本核验不支持在线收益、agent 收益或新的独立确认。

**到达过程改变：** 原始点估计使用每折分子级随机排列；bootstrap 先抽
scaffold，再将每个 scaffold 的分子连续拼接。预算控制器依赖顺序，因此
该区间同时改变分子组成和到达机制。`refusal_only−base_confidence` 的原始
点值 +0.007382 高于其保存区间上限 +0.006018，与这种偏移相容。
现有区间应描述为 **scaffold-block replay sensitivity interval**，不能未经
额外统计论证就解释为原随机分子到达策略的常规 95% CI。历史数字原样保留。

**对照选择：** 保存 gate 固定原始样本赢家，再与该赢家比较；没有给出
“超过所有对照”的联合统计保证。当前 gate 失败不受这点改变。

**状态覆盖：** 五折各 pair 的最少 B 支持仅为 3/7/4/4/4；pair 有样本不等于
每种符号或终局动作都有支持。新增模型可对未覆盖状态外推，1,518 行不是
1,518 个独立分子。

**投影含义：** 原 16 系数不变只是一条系数不变量；预测仍可写成
`x0·(beta0−T·gamma)+z·gamma`。不能由此声称有效旧基函数贡献或正则几何未变。

可复用产物：独立 SVD/normal-equation 检查、严格合法字段检查、旧策略逐行
绑定、全部 bootstrap 控制器重放和四类 fail-closed 检查。后续若改变到达
协议，应在新实验卡和新目录中规定 scaffold 抽样与分子排序的关系。
