# E3 机制消融 v3：family-label permutation sensitivity

日期：2026-10-06

## 目的

补齐 E3 的 `family permutation` 轴，检查 FamilyGuard 的动作是否依赖真实 provider-family 分组。这个实验是**对抗性敏感性诊断**，不是主效果实验，也不把置换后的 family 当作 benchmark 真值。

## 固定合同

- 输入：已封存的 722 条 BBB development panel、5 个 scaffold-disjoint folds、4 个固定 provider snapshots。
- 反馈：selected-only，`release_tick = decision_tick + 1 + delay`，本次只用 `fold=4, seed=0, delay=0`，transient/persistent 两种事件。
- 事件：沿用封存的 Frozen event-tick action；只在选中 provider 的 synthetic family 与 target family 相等时注入 deterministic failure。未选 provider 不改动。
- 置换：对 33 个 provider 的 family label 做 seed `20261006` 的确定性 shuffle，多重集合不变；不产生新的 provider 预测。
- 方法：Frozen、streak-only、relative-reliability、matched-SGD、FamilyGuard。
- 资源：本地 CPU replay，无 A800，无新模型训练，无外部 API。

## 先验的最小审查

| 轴 | 现有证据 | 判断 |
|---|---|---|
| interface removal | 同一 runner 的 Frozen/no-update 控制 | 已覆盖更新接口的语义；没有另造空 API runner，不应写成 literal API-null |
| independent residual | 同一 veto schedule 的 matched Online-SGD | 已覆盖最近邻 selected-only 独立更新控制；不宣称与 MIRA 残差数学等价 |
| family permutation | v1/v2 缺失 | 本卡补一个 CPU sensitivity replay；不用于普通 superiority |

## 预注册判据

- 通过条件：事件、反馈时钟、provider binding、selected-only/no-leakage、封存输入和 source hashes 全部通过独立复核。
- 解释条件：只报告 action changes、注入次数和 descriptive accuracy；不计算 pooled CI，不把置换结果写成方法效果。
- 失败条件：任何 hash、event conservation、event injection 或 feedback clock 不一致；失败收据保留。

## 实际命令

见 `research/experiment_section_style_20261006/e3_mechanism_audit/` 下的 `prepare_permuted_manifest.py`、`verify_permuted_manifest.py`、`verify_permuted_replay.py` 及 run receipt。Replay 使用固定 commit `ddfcf8bd66aabfd109c689f4ed143d98fd14c0b4`。

## 产物

- `permuted_registry_seed20261006_v2.json`
- `permuted_target_manifest_v3.json`
- `permuted_manifest_verification_v3.json`
- `permuted_replay_fold4_seed0_delay0_v3/`
- `permuted_replay_verification_fold4_seed0_delay0_v3.json`

## 边界

生产 FamilyGuard verifier 的 prefix-equality 检查针对真实 family registry；family-label 置换会在事件前改变 family state，所以该检查对此诊断不适用。我们保留该失败，不绕过它冒充主收据；本卡使用独立 verifier，只验证 replay 生命周期、封存 panel 绑定和 synthetic event contract。
