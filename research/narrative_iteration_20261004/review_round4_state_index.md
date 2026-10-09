# 叙事迭代第四轮：状态索引与 selector 接口审查

- 日期：2026-10-05
- 对象：`paper/aamas2027/main.tex` 的 release-phase 状态递推与 selector 输入
- 审查方式：方法合同审查员先指出索引矛盾，修订后再独立核验；叙事审查员复核根因链
- 结论：**设计合同层面 PASS；pre-results design CONDITIONAL；最终效果论文 FAIL**

## 发现与修复

原式 `S_{t+1}=U(...)` 配合 `\tau_s\leq t` 会让 `D=0` 的事件错过 `S_{t+1}`，与“下一决策可见”的文字不一致。现将 `S_t` 定义为 tick $t$ release phase 完成后的状态：先处理满足 $s<t,\tau_s=t$ 的事件，再由 selector 读取 $S_t$。因此事件 $t$ 在 `D=0` 时进入 `S_{t+1}`，只影响 action `t+1`。

同时把 selector 接口显式写为
`$a_t=\pi_{\mathrm{sel}}(x_t,e_t,A_t,H_t,S_t)$`，避免把可见历史 `H_t` 与可更新的序列化状态混为一谈。

## 复核结论

- `H_t=\{s<t,\tau_s\leq t\}`、`\tau_t=t+1+D`、release queue 和 `S_t` 的索引现在一致，没有当前事件泄漏。
- `D=0` 的事件不改变触发它的动作；release phase 后才影响下一 tick。
- receipt verifier 仍只做 lifecycle/read-set/queue/state-hash/schema-count 检查；score、propensity、MAP 与完整指标重算仍属于 E5 transition replayer。
- 新公式没有打断“根因手术刀”主线，反而把 selected-only delayed feedback 变成可执行的状态转换。

## 尚未通过最终验收的原因

E1–E5 主矩阵、独立 transition replayer、paired cluster uncertainty、strict baseline fairness、provider lineage 和 E6 confirmation 仍未闭合。因此当前可以保留方法目标和实验计划，不能把 MIRA-Relay 写成已证明的实时优势。

## 编译证据

`make draft` clean build 通过；`paper/aamas2027/build/main.pdf` 为 7 页、letter、692886 bytes，无 fatal error。模板 `\\ifx` incomplete warning 仍存在，未改变输出页数。
