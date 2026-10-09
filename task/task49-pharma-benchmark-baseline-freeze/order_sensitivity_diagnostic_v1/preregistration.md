# Online policy order-sensitivity diagnostic v1

日期：2026-09-29
状态：development diagnostic；不进入主结果、confirmation 或 A800。

## 问题

当前 Structured 的负结果可能部分依赖 evaluation 到达顺序。已有 full-panel
block permutation audit 没有发现 order-only winner drift；本诊断进一步直接重放
带状态的 policy，检查高维 selected-only 更新是否对 arrival order 敏感。

## 固定协议

- 输入：`fingerprint_diagnostic_v1/inputs.json`，不读取 confirmation。
- fold/seed：fold 0、seed 0；使用已验证的 fit-only `bt_cls` encoder 和同一 q/h/direct
  warm-start snapshot。
- evaluation：同一 145 个分子，固定 RNG seed `20260929` 生成 20 个 within-fold
  permutations；第 0 个 permutation 是原始 entity-id 顺序，其余不替换分子或标签。
- 方法：`frozen_structured`、`correctness_only`、`structured`。
- delay：`0` 与 `4`。
- policy 只接收冻结 feature、menu 和 selected-only delayed feedback；不接收 truth、未选
  provider 或完整 panel。所有方法共用每个 permutation 的顺序。

## 主比较

对每个 permutation 记录 primary system-success accuracy、选择序列、provider coverage、
decision/update runtime 和每条 selected-only event。主比较是 Structured/Frozen 的
paired accuracy difference 在原始顺序与 permutation 集合中的变化；Correctness-only
用于判断变化是否仍由 provider head 驱动。

## 解释规则

- 如果 Structured 在原始顺序明显更差、而 permutation 后差距接近 0，说明当前负结果有
  order amplification，后续方法必须加入顺序敏感性控制。
- 如果 Structured 在所有顺序都接近 Frozen 或都更差，不能把负结果归因于排序；主要问题
  仍是表示、更新方差、selected-only support 或缺少环境事件。
- 这不是环境 drift 证明。Permutation 只改变到达顺序，不创造 provider version、arrival
  或 retirement 事件。

## 停止规则

若任意 stream 出现 event conservation、selected-only 或输入 hash 不一致，诊断作废。
不根据 permutation 结果挑选顺序、学习率或方法；不把一个 fold/seed 的结果写成效果结论。
