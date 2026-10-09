# MIRA-FamilyGuard candidate v1

日期：2026-10-01
状态：候选修复；尚未运行，不能写入主结果。

## 1. 为什么需要这个候选

`family_outage_v1` full receipt 已经通过独立验收，但 current `relative-reliability` 在共同支持集 S 的 persistent outage 中仍比 matched-SGD 慢，且 family exposure 高。现有 availability ledger 按 provider 计数；同一 family 内换 provider 会绕开单 provider 的 block。

这个候选只修 failure-response channel，不改 molecule representation、fit-only prior、selected-only 权限或 chemical correctness ledger。

## 2. 候选规则

每个 provider 仍只在被选中后产生反馈。设 provider $p$ 属于 availability group $g(p)$。每个 group 维护 selected feedback 的计数 $n_g^{fail}, n_g^{ok}$，先验 $a_0=1,b_0=9$，并计算

$\hat q_g=\frac{a_0+n_g^{fail}}{a_0+b_0+n_g^{fail}+n_g^{ok}}$。

相对可靠性因子按先验归一化：

$\rho_g=\operatorname{clip}\left(\frac{1-\hat q_g}{1-a_0/(a_0+b_0)},0,1\right)$。

provider score 使用当前的 fit-only score 和 provider-local factor，再乘 group factor：

$s_t(p\mid x)=s_{fit}(p\mid x)\,\rho_{provider}(p,t)\,\rho_{g(p)}(t)$。

group consecutive runtime failure 达到 $K_g=2$ 后，在 cooldown $C=4$ 个 release version 内暂时 gate 该 group；cooldown 后重新允许选择。成功 selected feedback 清零 group streak，但不抹掉 posterior 计数。transient 单点 failure 不触发 group gate；persistent failure 会在第二个同组 failure release 后触发。

## 3. 运行协议

- 仍使用 BBB development、33-provider menu、五个 scaffold folds、两个 seed、delay `{0,1,4}`。
- 事件仍是封存的 Frozen event-tick family；不把未选择 provider 的结果喂给 policy。
- 先跑 candidate no-event receipt，确认 event tick prefix 与现有 relative receipt 一致；不能直接复制 target action。
- 再跑 candidate transient/persistent event receipt；每条 stream 必须通过独立 verifier。
- 主比较固定为同坐标 paired：FamilyGuard−relative、FamilyGuard−streak-only、FamilyGuard−matched-SGD；同时报告 ITT、S、family exposure、leave-family latency、p95 selection/update 和 state bytes。

## 4. 预注册成功和证伪条件

成功方向：

1. persistent S 上，FamilyGuard 相对 current relative 的 release-after delta 在至少两个 delay 提高 ≥1 pp，且五-fold paired bootstrap 区间不把该改进完全排除；
2. persistent S 上 family exposure 相对 current relative 降低 ≥5 pp，且 transient 的 release-after delta 不低于 -1 pp；
3. selection/update p95 不超过 current relative 的 2 倍，state bytes 增长不超过 4 倍。

证伪方向：

- transient 在至少两个 delay 的 release-after delta 低于 -1 pp；
- persistent S 的 FamilyGuard 比 current relative 低超过 1 pp，且五-fold paired bootstrap 支持该差异；
- 任一 unexposed stream 不再和 no-event control 完全一致；
- family registry、selected-only、feedback clock、manifest binding 或状态 hash 任一失败。

结果无论正负都保留。该候选若失败，回到 failure channel 的阈值、cooldown 和 group 定义，不改写旧 receipt。
