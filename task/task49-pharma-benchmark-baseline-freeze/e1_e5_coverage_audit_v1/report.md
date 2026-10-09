# E1–E5 coverage audit v1

日期：2026-10-04  
状态：只读审计；不重跑策略、不申请 GPU、不读取 confirmation。

## 这份审计要回答什么

Gate C 的 `gate_c_unified_matrix_v1` 已经逐收据标出 `PASS`、`PARTIAL` 和 `MISSING`，但还不能直接回答“E1–E5 每一项离最终论文验收还有多远”。这份审计把三份生效评价标准、当前验收审计、baseline eligibility/cost 和 endpoint qualification 重新放在同一张证据表中。

这里的状态是**最终角色合格的主表状态**：

- `covered`：声明的 scope 有原始收据、独立恢复和必要控制，能支撑对应实验问题；
- `partial`：至少一条 scope 已通过，但仍缺主表所需的控制、字段、统一合同或边界；
- `missing`：当前没有可用于该轴的合格证据；不能用另一 scope 的数字填充。

因此，某个收据中的 E4=`PASS` 不会把整个 E4 自动变成 `covered`；它只能关闭该收据声明的受控压力子项。

## 总表

| 实验 | 当前状态 | 已覆盖的轴 | 仍为 partial 的轴 | 明确 missing 的轴 |
|---|---|---|---|---|
| E1 任务有效性 | **partial** | BBB truth/菜单/headroom 线索、静态与在线角色分离 | Random/Best Fixed/Oracle 与在线方法尚未同收据、E1 专用不确定性 | 单一冻结的五控制 E1 manifest |
| E2 主比较 | **partial** | Frozen/Matched 同合同、common-stratum paired receipt、现代适配方法已分层 | likelihood/prior/state/refusal 合同、跨方法主表、普通任务区间跨 0 | 最终角色合格的统一 E2 receipt/table |
| E3 机制消融 | **partial** | Veto 消融、family-outage 消融、独立 verifier、Graph Relay 负诊断 | 普通主任务的完整 integration gain、同 schedule 的所有 proposed ablation | full/remove-interface/remove-state/permutation 统一表 |
| E4 动态与压力 | **partial** | delay `{0,1,4}`、selected-only pending queue、transient/persistent family outage | 受控收据尚未并入最终主表，paired 成本/分母仍需统一 | arrival/retire/version-shift/natural drift；多端点动态确认 |
| E5 错误、成本与恢复 | **partial** | failure/terminal/feedback 记账、unknown/refusal 语义 smoke、post-call utility、部分成本 receipt | common receipt 没有 unknown/refusal 事件，adapted/anchor 字段不全，utility 不能回填 selector | 统一主 E5 表、unknown censoring effect |

机器副本：[`audit.json`](audit.json)。其中保留每一项的 covered/partial/missing 子轴、证据路径、最小下一步和停止条件。

## 逐项判断

### E1：任务有效性和 headroom

BBB 已经具备进入 development primary 的真值和 provider routing 层：TDC truth 固定，8 个冲突行被排除，716 个可评分分子、33 个 provider 和 7 个 family 都有矩阵记录。Best Fixed=`0.9155`、evaluator-only Oracle=`0.9972` 也说明当前 provider 菜单存在可计算 headroom。静态控制、strict online 控制和 evaluator 上界的比较权限已经在 eligibility matrix 中分开。

缺口是控制没有进入同一份冻结 E1 收据。当前 common-stratum receipt 的六个方法各有 4,332 次 decision/terminal/feedback，但它不含 Random、Best Fixed 和 Oracle 的同收据分母；把各处数字拼在一起不能替代 exact stream identity 和 E1 专用配对分析。下一步先做只读 exact-join；无法证明同一 stream 时，再做一条 CPU contract receipt。若统一控制关系或预登记 headroom 条件失败，停止 routing-effect 主张。

### E2：主比较

Frozen direct 与 Matched Online-SGD 是当前 strict common-prior logistic 主控制，输入、64 维特征、fit-only prior、selected-only feedback 和 delay `{0,1,4}` 均已绑定，独立 verifier 和 paired aggregation 也存在。MIRA candidates、Gaussian reward-ridge、Greedy/Bayesian/PILOT-adapted、Replay separate control、GraphRouter/Laya audit-only 的角色也已经写入机器矩阵。

这仍不是最终主比较：Replay 的历史 BT prior 与当前 direct prior 不同；adapted rows 的 likelihood、state bytes 和 refusal/unknown 字段不统一；GraphRouter/Laya 没有在线收据；common-stratum paired intervals 跨 0。下一步是冻结 role-qualified E2 schema 并尽量从已有收据生成主表，只对确实缺字段的一组做一次 CPU contract run。若 proposed method 没有超过最强合法 matched control，就停止 superiority wording。

### E3：机制消融

`mira_veto_ablation_v1` 已经回答了一条局部机制问题：streak ledger 负责 controlled failure response，beta gate 反而带来静态损失；FamilyGuard family-outage receipt 也有 persistent/transient、unexposed equivalence 和独立恢复。Provider-Graph Relay 的 fold0 负结果说明“action 变化”本身不等于 utility gain，这个反例必须保留。

但是，普通 common-stratum 的 E3 仍是 partial。还没有一张最终表同时放下 full method、去掉新增 state/interface、ProbeNoUpdate、IndependentResidual 以及 family/provider permutation，并用同一 schedule、同一预算和成对不确定性检验 integration gain。下一步只做这张 CPU 机制表；若 full 不优于最强去模块对照，停止该机制的主贡献主张。

### E4：动态与压力

在当前明确的 `feedback-adaptive` scope 内，delay `{0,1,4}`、selected-only release、pending queue、transient/persistent family outage 都有可恢复收据；family-outage 行在其声明 scope 的 E3/E4 是 PASS。这已经足以支持“受控 delayed feedback/family outage stress”这个窄实验问题。

E4 仍不能写成自然环境非平稳适应。固定 provider pool 没有 arrival、retire、version shift 或自然分布漂移；common-stratum 普通任务与 controlled event 也必须分表。下一步是把已验证的 event receipt 接入最终 role-qualified E4 表并统一成本/失败分母。除非扩展 claim，否则不额外支付 lifecycle 实验成本。

### E5：错误、成本和恢复

E5 的 accounting 基础最完整：common/family raw receipt 保留 attempts、terminal、feedback 和 runtime failure；unknown/refusal semantic smoke 由独立 verifier 确认 fail-closed；post-call utility audit 单独保存 policy receipt 与 evaluator-only truth；strict common 和 Gaussian receipt 也有部分成本数据。

但 common-stratum 的 unknown/refused 都是 0，说明“没有静默丢字段”，不能说明 refusal utility。adapted receipt 没有 state bytes 与逐事件 refusal/unknown reason；Best Fixed、Oracle、static controls 没有可比的 event timing/state 字段；post-call utility 是 verifier 扩展，不能回填成 pre-call selector 效果。下一步先按统一 schema 聚合已有字段，只有仍缺 utility 才做一次 CPU matched-rate utility receipt。若拒答/unknown utility 不超过 matched control 或成本超过 break-even，停止 utility-superiority wording。

## 依赖顺序和停止规则

推荐顺序不是再堆 baseline 或直接申请 A800：

1. **E5 schema/denominator closure**：先统一 failure/unknown/refusal/cost/recovery 字段；
2. **E2 role-qualified main table**：只在同合同 strata 内比较；
3. **E1 exact control join**：确认 headroom 和控制关系；
4. **E3 matched mechanism table**：证明整体机制的 integration gain；
5. **E4 scope-bound stress table**：把 controlled event 与普通任务分开；
6. 以上都不再是 partial 后，才讨论 E6 confirmation。

硬停止条件：

- 不跨 strict、adapted、separate-control、anchor、static strata 合并 raw accuracy；
- 不把 controlled family stress 写成 arrival/retire/version-shift 或 natural drift；
- 不把 `unknown=0/refused=0` 当成 refusal utility 正结果；
- E1–E5 任一仍 partial/missing 时，不打开 E6 confirmation。

## 证据完整性

本次只读来源的 hash 和路径都在 `audit.json` 的 `source_manifest` 中。审计没有改主文档、没有重跑策略、没有读取 confirmation，也没有提交 GPU 任务。
