# 故事线与方法论多 agent 辩论记录

## 目的

这组文件审查当前 AAMAS 2027 故事线、方法候选、benchmark 和 baseline 是否已经达到三份生效评价标准的要求。它是审查材料和中间证据，不是新的生效主线文档，也不是自动决议。

## 参与者与管理方式

| 角色 | 负责问题 |
|---|---|
| `story_aamas_reviewer` | 故事切口、创新必要性、AAMAS 论证链、claim scope |
| `method_theory_auditor` | 信息边界、可识别性、状态/反馈时钟、方法可计算性 |
| `benchmark_novelty_auditor` | benchmark 资格、baseline 角色、最近邻和动态 scope |
| `/root` coordinator | 不投票；汇总证据、保留分歧、决定是否进入下一阶段 |

讨论按 `docs/论文研究与写作流程.md` 的 Round 0 → Round 1 → Round 2 → Synthesis 执行。每条实质判断保留 `POSITION / TARGET / EVIDENCE / REASONING / DECISION TEST / CONFIDENCE`，不能用 agent 多数票替代实验和人工确认。

## 文件索引

### Round 0：独立审查

- `round0/story_aamas_reviewer.md`：临时 62/100，H4/H5/H8 UNKNOWN，HOLD/Major Revision。
- `round0/method_theory_auditor.md`：selected-only P/Q 不可识别、selector/verifier estimand 分离、延迟队列和恢复缺口。
- `round0/benchmark_novelty_auditor.md`：custom routing layer、最近邻和 Gate A/B/C 的初审；challenge/revise。
- `round0/README.md`：Round 0 manifest。

### Round 1：交叉挑战

- `round1/story_aamas_reviewer.md`：挑战过严的固定 epsilon、异常场景、baseline 无限扩张和固定调用数 kill。
- `round1/method_theory_auditor.md`：H4 在方法冻结前应为 UNKNOWN/HOLD；区分 feedback-adaptive 与 environment-nonstationary。
- `round1/benchmark_novelty_auditor.md`：custom routing layer 有条件有效；建议按因果角色构成最小 baseline roster。

### Round 2：最终立场

- `round1/story_aamas_reviewer_round2.md`：主故事先收窄为 protocol/negative diagnosis；selector/verifier 拆 estimand，不立即拆论文。
- `round1/method_theory_auditor_round2.md`：保留有条件的 provider adaptation；fixed-pool 只支持 feedback-adaptive。
- `round1/benchmark_novelty_auditor_round2.md`：custom routing protocol 有条件可作主 benchmark；baseline 采用 7–8 个因果角色槽位。

### 管理者综合

- `synthesis/manager_synthesis.md`：跨轮共识、少数意见、当前评分快照、下一步闸门和停止条件。

## 当前状态

证据状态为 `HOLD / REWORK`，但目标 claim 不降。三份生效主线文档和评价标准没有因为辩论自动改写；Gate A/B/C 仍为 HOLD。当前不把 MIRA 写成已经验证的结果；目标方法继续迭代，待 CPU 闸门通过后使用 Nebula A800 做单任务实验。JEV 只作为历史功能参照，不是论文方法名。

## 解释边界

本轮使用了 collaboration subagent 按项目辩论协议生成可审计产物；Orca CLI 当前不可用，因此不把这次运行冒称为 Orca Run。恢复 Orca 后，可把同一 artifact schema 绑定到正式 DAG。中间报告中的反对意见、未知项和失败条件必须与综合结论一起保留。
