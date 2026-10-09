# Round 2：Benchmark 与 baseline 的最终裁定

- 审查角色：benchmark / novelty auditor
- 日期：2026-09-29
- 范围：自建 routing protocol 是否可以作为主 benchmark；baseline 是否应继续扩张
- 约束：不修改生效主线文档；结论区分 benchmark 资格、方法主张和待完成证据

## A. 自建 routing protocol 能否作为主 benchmark

### POSITION

**有条件支持。** TDC 的公开分子性质 truth 加上本项目公开的 provider routing contract，可以作为一个领域化的 AAMAS 主 benchmark；但必须准确称为“基于公开 truth 的领域化 routing protocol”，不能把 routing layer 冒称为已有社区 benchmark，也不能用 custom protocol 的存在自动证明方法创新。

### TARGET

争议有两个：

1. 自建 provider menu 和反馈时钟是否天然缺乏权威性；
2. 如果不是天然无效，哪些证据足以让审稿人相信它是一个可复核、可比较、能支持主张的任务。

### EVIDENCE

1. TDC truth layer 有公开来源、版本和可回查的分子标签；当前缺口在 routing layer 的菜单、provider lineage、冲突规则、反馈语义和复核，而不是 truth 本身是否存在。
2. 生效 Benchmark/Baseline 标准把公共 truth 与 provider routing layer 分开：routing layer 可以由论文提出，但必须公开 input/menu/action/feedback/delay/budget/failure/unknown/lifecycle 合同。
3. 当前 Gate A 临时约 9/16，缺口集中在 hERG/AMES 资格、provider lineage、overlap/confirmation 和独立恢复；因此当前 benchmark 仍是 HOLD，而不是因 custom protocol 这个事实被判死。
4. 当前实验范围实际只支持 BBB fixed-menu feedback-adaptive。没有自然 arrival/retire/version 事件，不能把该 benchmark 写成 environment-nonstationary 或普遍动态工具生态。

### REASONING

审稿人要判断的是：另一个研究者能否从公开 truth、固定 manifest 和 raw event log 重建相同的可见历史，并在相同信息、菜单、预算和反馈时钟下复现 baseline 与 Proposed。满足这一点，custom routing layer 就是一个清楚的任务协议；不满足这一点，所谓 benchmark 只是内部脚本。

因此主 benchmark 的资格条件是：

1. 固定 TDC endpoint/version/hash、冲突和 unknown 规则；
2. 固定 provider menu、版本、源码/权重 lineage、输入输出 schema 和失败语义；
3. 公开 `decision → terminal → release → update → next-action` 事件合同，禁止 pre-call label/output leakage；
4. 提供独立 checker，从 raw JSONL 重建 visible history、pending queue、state hash 和每次 action；
5. 完成 P-C1、scaffold-disjoint split、overlap/lineage audit、family macro 和一次冻结 confirmation；
6. 论文 scope 只写 `BBB fixed-menu, selected-only, delayed, feedback-adaptive routing`，除非另有 event extension 证据。

### DECISION TEST

先不提交新的效果型远程任务。完成以下 Gate A 闭合包：

- authoritative truth/routing 双层 manifest 与 hash；
- provider lineage 和 endpoint qualification；
- conflict/unknown/duplicate/restart/recovery contract；
- molecule/scaffold split、overlap 和 family exposure；
- independent replay/checker；
- 一次只读 confirmation，确认 raw run 可由独立脚本重算。

判定：

- H1–H10 全通过，Gate A ≥12/16，且 A2、A3、A4 不低于 3：可以把它作为主 benchmark；
- Gate A 未通过：只能称 development routing protocol，不能用它支撑完整论文主张；
- 仅 truth layer 通过：不能由此推断 provider routing 合法或方法有效。

### CONFIDENCE

0.93。判断直接依照项目评价标准与当前 manifest/审计状态；“有条件支持”不代表当前已经达到条件。

## B. baseline roster 是否继续扩张

### POSITION

**采用按因果角色覆盖的最小 roster，不无限堆论文名称。** 只要每个关键比较角色都有一个保真、同合同、可恢复的代表，更多同角色方法不会自动增加说服力。不可保真的 GraphRouter/Laya 可以登记为 audit-only 或排除，不能靠名称占位。

### TARGET

需要同时避免两种错误：

- 只和 Random/Best Fixed 比，无法证明对现代方法公平；
- 把所有近期 router、不同 backbone 和不可复现实现都堆进表格，造成预算不匹配和结果不可解释。

### EVIDENCE

当前最小角色槽位为 7–8 个：

| 角色 | 最小代表 | 作用 |
|---|---|---|
| 无信息下界 | Random | 校验任务不是随便猜测 |
| 静态强 incumbent | Best Fixed | 直接选择主张的最低强门槛 |
| 不更新控制 | Frozen / Audit-Frozen | 与在线方法共用表示和初始化 |
| 同表示在线控制 | Matched Online / Replay | 分离“更新”与 Proposed 机制 |
| 延迟/上下文最近邻 | 一个可保真的 LinUCB、PILOT 或等价方法 | 检验 delayed/contextual online 贡献 |
| 现代 router | 一个通过 fidelity audit 的 GraphRouter/Laya/同级方法 | 防止只打过时对手；不能强行伪造复现 |
| 上界 | Oracle | 检查 headroom 与标签实现 |
| 调用后控制（仅在对应主张存在时） | static threshold / same-cost escalation / verifier control | 防止把 post-call 信号冒充 selector 增益 |

`Best Fixed`、`Oracle` 和现代 router 不得共享不公平的未来标签；所有在线方法必须共享 menu、feature、delay、failure/unknown、call budget 和 selected-only release。

### REASONING

baseline 的作用是击穿具体反事实，不是展示方法数量。若一个新增 baseline：

1. 只重复同一信息与反馈时钟；
2. 只换 backbone、超参或 provider variant；
3. 无法通过源码/权重/输入输出契约的 fidelity；
4. 没有 raw run、recovery、成本和不确定性结果；或
5. 不会改变任何 claim、kill criterion 或审稿判断，

它属于 pile-on，不应进入主表。需要把 33 个 provider 的相关性做 family macro、exposure 和 paired molecule/scaffold CI；否则 pooled score 会把一个 provider family 的重复变体误当成独立证据。

### DECISION TEST

先完成一页 role matrix 与统一 baseline contract，再决定是否添加方法：

1. 每个槽位给出源码/权重 commit、license、输入表示、初始化、反馈使用和预算；
2. 每个槽位保存 raw JSONL、config、state/recovery hash 和失败分母；
3. 现代 router 只有在 fidelity pass 后进入主比较，否则标 `audit-only`；
4. 主比较报告 family macro、provider exposure、molecule/scaffold paired CI；
5. 用预登记 `full − strongest_matched`、而不是 `full − Random` 作为主方法门槛。

建议的统一 go/no-go：H1–H10 全通过，Gate A/B/C 各 ≥12/16，B7/B9/B10/B14 ≥3；`full − strongest_matched` 在至少 2/3 delays 超出 ROPE，且没有额外标签、调用或未来信息。

若只胜 Random/Frozen，或优势只出现在不匹配预算、单一 family 或不可复核实现上，停止 dynamic JEV 优越性主张；保留 benchmark/protocol 与负诊断。

### CONFIDENCE

0.94。最小 roster 覆盖评价标准要求的因果角色；是否还需第二个 modern router，取决于 fidelity 和它能否改变结论，而不是方法名称数量。

## FINAL POSITION

1. 自建 routing protocol 可以有条件成为主 benchmark，但必须先完成 Gate A 闭合包，并把范围限定为 BBB fixed-menu feedback-adaptive。
2. baseline 采用角色完整、数量最小的 roster：Random、Best Fixed、Frozen/Audit-Frozen、Matched Online/Replay、一个 delayed/contextual nearest、一个通过 fidelity 的 modern router、Oracle；verifier controls 只在对应 RQ 出现时加入。
3. Gate A/B/C 仍是独立闸门；benchmark 资格不能由算法得分抵消，baseline 数量也不能替代 fidelity、recovery 和 confirmation。
