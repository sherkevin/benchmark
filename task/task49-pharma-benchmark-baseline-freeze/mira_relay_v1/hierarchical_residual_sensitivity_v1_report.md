# Hierarchical residual sensitivity v1：fold0 机制闸门报告

- 日期：2026-10-04
- 状态：**通过事件完整性核验；候选未形成效果结论**
- 目的：检验在共享 family residual 之外加入 selected-provider local residual，是否能把已观察到的 transfer 变成可观察的动作收益。
- 范围：BBB development，fold0/seed0，145 个 scaffold-disjoint evaluation molecules，delay `{0,1,4}`；不含 confirmation、不含 Nebula/A800。

## 1. 预登记问题和公平合同

候选只改变 v3 MIRA 的 residual state：保留 family residual，并增加 selected provider 的 local residual。输入、provider menu、冻结 encoder、fit-only state、probe seed、epsilon、hazard gate、selected-only feedback、反馈时钟和 fallback 全部沿用 v3。候选没有额外标签、工具调用或历史 full-panel 读取。

候选 fold0 的继续条件是：raw/verifier/state chain 全部通过；至少出现真实的 action-level consequence；更新仍为每次 $O(d)$，且 runtime-valid 不下降。若三个 delay 相对两个 matched update controls 都是 `0 rescued/0 harmed`，则停止该候选。

## 2. 独立核验

独立 verifier 从复制的 `inputs.json` 重算 selected provider cell 的 status、label、truth、reward 和 failure 语义，并检查：

- 3 个 delay stream，145 条 decision、terminal、feedback/stream，共 1,305 条 raw events；
- decision tick 的完整顺序、pending queue、release ordinal 和 `due_tick = decision_tick + 1 + delay`；
- 每个 event 的 state hash 和跨 event hash chain；
- selected-only decision reads；
- 未选 family 的 mean/variance 与未选 provider 的 residual mean/variance 不变；
- summary 的逐 stream metric、event hash、config hash 和最终 state hash。

核验结果：`verified`。原始证据和文件 hash 见：

- raw receipt：`experiments/logs/task49_mira_hierarchical_sensitivity_20261004/fold0_seed0/`
- verifier：`.../verification.json`
- manifest：`.../raw_manifest.json`
- verifier source：`src/probe_headless/verify_hierarchical_residual_sensitivity_v1.py`

## 3. 结果

| delay | candidate correct | vs MIRA action/rescue/harm/net | vs Independent action/rescue/harm/net | runtime-valid |
|---:|---:|---:|---:|---:|
| 0 | 127/145 | 3 / 0 / 0 / 0 | 2 / 1 / 0 / +1 | 145/145 |
| 1 | 127/145 | 3 / 0 / 0 / 0 | 2 / 1 / 0 / +1 | 145/145 |
| 4 | 125/145 | 4 / 0 / 1 / -1 | 2 / 0 / 1 / -1 | 145/145 |

候选的 residual updates / family-transfer events / provider-local updates 分别为：

- delay 0：85 / 84 / 84；
- delay 1：86 / 85 / 85；
- delay 4：84 / 83 / 83。

逐样本配对明细在 `hierarchical_residual_sensitivity_v1_pair_metrics_fold0_seed0.md` 和对应 JSON 中。相对 MIRA 的 3、3、4 次 action changes 都是真实 provider 改选；但 delay 0/1 没有改变正确数，delay 4 使 `z034734` 从正确变为错误，净值为 -1。

## 4. 决策

这个候选通过了“实现和机制可观测性”闸门，但没有证明优于 MIRA，也没有证明 family/local 分解带来稳定收益。根据预登记，action-level consequence 条件已满足，因此可以继续跑 folds1–4；这一步的目标是估计跨 fold 的 paired uncertainty，而不是追求一个正数。

继续执行的停止条件保持不变：若后续 folds 显示相对 MIRA 的净收益不稳定、主要由单个分子贡献，或 delay 压力下持续受损，则把 HRA 记录为负机制诊断，回到 transfer opportunity/margin slice，不申请 seed1/A800。无论结果正负，v3 五折结果和原目标 claim 不改写。

## 5. 验证命令

```bash
python3 -m src.probe_headless.verify_hierarchical_residual_sensitivity_v1 \
  --replay-dir experiments/logs/task49_mira_hierarchical_sensitivity_20261004/fold0_seed0 \
  --output experiments/logs/task49_mira_hierarchical_sensitivity_20261004/fold0_seed0/verification.json
```

代码和 receipt 均记录了 executable commit `5d4b49aa0192e4066b3d3e1d90f5c2784dd91ae2`；本报告不把候选 fold0 当作论文最终效果。

## 更正（2026-10-04）

初版第 4 节把“出现动作级变化”和“可以继续扩展”混为一谈。按同一预登记的析取停止条件，候选必须停止扩展：相对 MIRA 没有 rescued，delay 4 有 1 个 harmed；相对 Independent 的唯一 rescued 是 `z070852`，且 delay 0/1 是同一个分子，属于 single-molecule gain。3/3/4 个 action changes 只能说明候选真的改变了动作，不能覆盖这个停止条件。

因此 HRA fold0 的最终决策是：**通过实现/事件核验，停止 folds1–4，不申请 seed1/A800，不把它升级为主方法。** 后续回到现有 raw 的 transfer opportunity、score margin 和 family size 分层，寻找能产生可重复 action consequence 的机制或 benchmark slice。原始继续判断保留在上一节，不能作为当前执行结论。

另：`family_transfer_events` 原 runner 口径统计的是 family state update，不等同于 sibling transfer；fold0 的非 singleton family update 只有 delay 0/1/4 为 20/20/18。后续报告使用 `family_update_events` 与 `non_singleton_family_updates` 分开命名，避免把 singleton provider 的更新写成共享收益。

### 分层诊断更正（2026-10-04）

只读分层结果显示 HRA 相对 MIRA 的 changed actions 在 delay 0/1 是 3 个 `admet_ai`（family size 2），delay 4 是 1 个 `admet_ai`、2 个 `b3clf`（size 24）和 1 个 `admetlab` singleton。因而只能说 delay 0/1 的变化来自可迁移 family；不能把 delay 4 的全部变化都归因为 sibling transfer。这个更正不改变 HRA 的停止决定。
