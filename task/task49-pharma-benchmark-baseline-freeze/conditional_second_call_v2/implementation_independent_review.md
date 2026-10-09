# B-only 融合诊断实现：独立合同审查

日期：2026-10-08。审查对象：
`src/probe_headless/analyze_conditional_fusion_diagnostic_v1.py` 当前版本；对照冻结卡
`fusion_diagnostic_card_v1.md`（SHA-256
`b0da0f3f72fbe08d3beacd8073a2f2754f38554d67283fe387e2548aee9fc319`）。

本审查只读了冻结卡、runner 源码和已存在的 verifier 文件（当前为空）；没有读取新的 C 回执、没有运行真实分析，也没有修改 runner。结论是实现尚不能称为完成；以下是必须修正的合同偏差，按阻断性排序。

## 必须修正

1. **U10 面板少了四个 singleton。** runner 第 71--79 行只写入六个 unordered pair，所以每个 entity 产生 6 行而不是卡要求的 4 singleton + 6 pair = 10 行。当前记录还没有 `mask_kind`、`first`/singleton 与 pair 的统一字段，无法分别核对 singleton 和 pair 的均值。应为每个 entity 先写四个 singleton（各 0.1），再写六个 pair（各 0.1），并在输出中显式区分两类。

2. **U10 的损失/基准定义不完整。** 第 79 行把 pair 的损失写成 `loss`，没有保存 singleton 的 `q/action/loss` 作为同一 mask 面板的对照；因此无法按卡第 5.2 节报告 singleton 与 pair 的概率误差分层，也无法核对 U10 权重和每个 entity 恰为 1。补齐 mask、动作、损失、Brier、校准残差和权重守恒字段。

3. **预定分组没有完整生成。** 第 99--101 行只输出 S/U12 的 fold-level overall 两组。冻结卡要求先输出所有 fold 与总体、四个真实首工具、12 个 ordered pair、6 个 unordered pair、三种首动作、`ordered pair × 首动作` 的 36 个槽位、3×3 动作转移表，以及每格的行数、权重、去重 entity、逐 fold 支持和首工具组成。低支持和空格必须显式保留，不能只写非空/overall 组。

4. **没有实现六个 pair 关联审计。** 当前 runner 没有输出 unordered pair 的 native 概率 Pearson、clip-logit Pearson、`.5` 标签不一致率、错误指示协方差/相关，也没有为全 B 与四个真实 `S` 子集分别记录支持量。该审计是卡第 4 节的固定读数，不得以只有 U12 的分组统计替代。

5. **branch 1/2 缺少跨 fold 去重门槛。** 第 121--131 行只要求每个 fold 的 S/U entity 数至少 10，并以同一 K 达到三 fold；没有要求这些合格 fold 的 S entity 跨 fold 去重后至少 30。应先固定同一 `K=(a,b,d1)` 的共同合格 fold 集合，再同时检查 `|S_f|,|U_f|≥10`、至少三 fold、S 合并去重 entity ≥30，最后才计算 branch 1/2 的真值。

6. **branch 4 的 `m_f` 使用了错误的 miss 定义。** 第 90--91、111--114 行只把“base16 全预测 ≤0”记作 `missed_positive`。卡第 5.3/7 节的 `m_f` 还包括：预测为正但支持内预测首名对应的真实 `g≤0` 的 ranking miss；两类要拆开报告后合并为 m_f。第 91 行 `ranking_miss_nonstop = max(basevals) <= max(actual)` 不是该谓词，必须改为先按 provider 顺序选择预测 `argmax`，再检查其真实 g 是否 ≤0。

7. **gain 审计不完整且支持字段永远为空。** 第 69 行通过 `if False` 把 `pair_support` 固定为 `None`。第 80--91 行也没有输出六模型的加权残差、正值符号错配、三候选排序同分率、预测首名真实 g、`has_positive/has_action_change` 及 unsupported 状态。应从冻结 v2 metadata 重算全部六模型（包括投影后的 transformed features），并完整保存每个 entity/candidate 的预测、支持与上述汇总。

8. **B 特征/投影连接没有按卡核验。** 第 55--56 行只比较 `single_q` 与 gain；没有逐行核对 v2 `base_features` 与旧 B features、`raw_added`、`transformed_features` 与每个 fold model 的固定投影，也没有核对 v2 `base_coefficients` 与旧 conditional coefficients、old model hash 和 pair support。任何连接不一致都应 fail closed，而不是继续产生面板。

9. **manifest、快照与 provenance 不足。** 第 44--50 行只计算白名单文件 hash，没有读取并校验两份 B 目录的 artifact manifest，也没有核对 v2 `old_model_sha256` 指向旧模型；第 45 行没有保存 card/source/test 快照、argv、源码 hash、依赖/环境完整记录。应仅按白名单读取 manifest 的对应条目（不能遍历并打开 C），并在运行前绑定所有快照与 hash。

10. **预算与失败关闭合同未实现。** 当前代码没有异常捕获后的 `failure.json`/失败事件，也没有在超出卡定的 ≤2 分钟、≤1 GiB RSS、≤50 MiB 新产物时中止并标记 incomplete；`resource.json` 只在成功末尾写出。`--card-hash` 也只是接受调用者参数，没有同时锁定上述冻结 hash。应在开始即写 config/progress，持续记录资源，任何不匹配/超限都保留失败证据并拒绝 summary/branch 结论。

## 复核范围与通过条件

修复后，独立 verifier 需要在不导入 runner、不读取 C、不使用结果摘要/bootstrap/confirmation 的条件下，重新从两份 B 回执和冻结 model metadata 重建：四 native 输出、S/U10/U12、所有预定分组、六模型审计、pair 关联和 branch 谓词；并检查每个输出文件的 manifest/hash、行键唯一性、权重/动作/损失代数守恒。通过前不得把诊断标为 complete，也不得据此更改论文主张或生效方法。
