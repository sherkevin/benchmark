# AAMAS 2027 论文叙事独立审查 v1

- 日期：2026-10-04
- 审查对象：`paper/aamas2027/main.tex`
- 审查依据：`research/2026_AI顶会最佳杰出论文叙事范式_六大范式_阅读整理.md`、`docs/决议/0008-论文叙事范式作为写作评审基准.md`、三份生效主线文档及三份评价标准。
- 审查方式：独立 reviewer 只读审查；未修改论文。
- 当前状态：论文仍是内部 pre-results draft，不能当作投稿实证稿。

## 1. 总结结论

当前最适合的主线是 **根因手术刀**：动作先于当前输出、未选 provider 不可见、正确性标签只属于 selected call 且可能延迟，导致固定赢家和无约束在线更新分别遗漏不同的信息边界。**新基准暴露失效**可以作为副线，但在 E1/E2 没有结果前，只能说 routing protocol 设计得更锋利，不能说已经测出普通路由器的系统性失效。**反直觉重构**可以作为钩子：把 selector、verifier 和 updater 放进同一 event contract；但尚未有 matched 结果证明它优于普通 online update。

当前引言的六段顺序基本符合 `scene → failure → root cause → insight → method → falsifier`，方法和 benchmark 的信息边界也清楚。主要问题不是再增加背景，而是补齐方法可执行性、统一指标目标、把 verifier 的承诺闭合，并在结果完成后用真实 hero result 取代引言中的计划式表述。

## 2. 逐项判定

| 项目 | 位置 | 判定 |
|---|---|---|
| 标题 | `main.tex:35` | 方向合适，保留；有真实主结果后再重写摘要，不改题目 |
| 摘要 | `main.tex:39–40` | 当前对任务、候选方法、protocol 和比较范围交代清楚，但没有 headline result；内部稿可保留，最终稿必须补真实结果和边界 |
| 引言 | `main.tex:56–73` | 结构合格；`z000282` 只是 ID，没有真实输出/真值/分歧证据，根因仍主要是合理性陈述 |
| 方法 | `main.tex:83–181` | 信息边界清楚，但菜单变化、同 tick 释放顺序、chemical/system label、hazard/verifier 规则尚未完全可执行 |
| Benchmark | `main.tex:183–187` | 正确地区分 TDC public truth 与项目 routing protocol；不能称整个 routing layer 为社区 benchmark |
| Baseline | `main.tex:189–214` | 角色覆盖不错；GraphRouter 的条件行不能进入最终 strict main table，Online/Replay SGD 需拆开或解释合并理由 |
| 实验矩阵 | `main.tex:216–246` | RQ 结构合理，但 E1–E6 仍是占位；E6 的 `CLOSED / TBD` 自相矛盾，应改为 `BLOCKED / TBD` |
| 结果 | `main.tex:248–273` | 当前诊断数字已声明不是在线主结果，边界诚实；最终应把 hero result、机制消融和成本分开 |

## 3. 需要优先修复的问题

### P0：方法无法完全执行

1. **动态菜单的状态维度未定义**（`main.tex:118–135`）。`K=|A_t|` 会随菜单变化，但 provider bias 状态如何保留、新 provider 如何初始化、retire/version 如何迁移没有写清。修复：用全局 provider registry/key，菜单只用 mask；为加入、退出、版本变化定义状态迁移，并在 recovery verifier 中检查。
2. **同一 release tick 的顺序未定义**（`main.tex:105–109`）。多个事件同时释放时，逐事件更新和批量更新可能不同。修复：固定 `event_id` 排序或固定可交换的 batch update，并把顺序写入事件合同和 verifier。
3. **主目标与更新标签混淆**（`main.tex:112`、`138–154`）。primary estimand 写的是 selected correctness，但 updater 使用 `system_success`，runtime failure=0、chemical correctness=null。修复：明确 chemical correctness、system success、runtime hazard 三条账本，主表和更新规则分别说明。
4. **hazard 与 verifier 规则不闭合**（`main.tex:101–103`、`174–178`）。`q_t`、阈值、upper runtime-hazard bound、slack 和 `\upsilon_{a,t}` 未完全定义。修复：给出可执行公式、伪代码、常数来源和一个日志恢复案例；否则把 verifier 明确标作接口/control。

### P1：主故事还缺关键证据

1. `z000282` 需要替换为可审计的开发样例：provider IDs、输出、独立 truth、runtime status 和是否被选择；不能只放 ID。
2. “fixed/static/online 为什么翻车”目前是机制假设。需要 E1/E2 的 paired intervention：full-panel offline、selected-only delay `{0,1,4}`、ProbeNoUpdate、matched Online/Replay。
3. MIRA 同时列 fallback、family residual、bias、probe、hazard、verifier，不能只说“deliberately small”。需要 E3 去掉每个部件、family/provider permutation、rescued/harmed 和 integration utility。
4. verifier 在摘要中属于主机制，但正文只有接口和 controls。要么补完整 verifier 算法和独立 RQ，要么明确它是可插拔 control；不能两种定位混用。
5. baseline 必须按 `strict matched / adapted / audit-only` 分层；每行绑定源码 commit、checkpoint、prior、feature、likelihood 和 tuning budget。

### P2：段落和措辞

1. `main.tex:181` 一个段落同时放五个 invariant、多个拒绝条件和 science gate，违反“一段一个动作”；应拆成 invariants、falsification tests、primary gate 三段。
2. `main.tex:66–72` 的四个 commitments 在最终稿应改成 `claim → nearest → evidence → falsifier`，避免读起来像研究计划。
3. `main.tex:314` 当前 conclusion 是未来式；拿到结果后应改为有范围、有数字、有边界的一条持久教训。
4. `ε=0.20` 必须有预注册来源和敏感性分析，不能成为隐含调参。

## 4. 决定性测试与停止条件

| 测试 | 目的 | 失败时的处理 |
|---|---|---|
| T1 headroom | Random/Best Fixed/Frozen/Oracle、scaffold folds、family macro | 没有 context-dependent headroom 时，停止“任务有效性/benchmark 暴露失效”主张 |
| T2 root-cause isolation | full-panel static vs selected-only delays vs matched online/replay | delay/selected-only 不解释失败时，只把它写成合同特征，不写成根因证明 |
| T3 method utility | MIRA 对 Best Fixed/Frozen/ProbeNoUpdate/IndependentResidual；paired CI | 不满足预登记门槛则停止 adaptive superiority claim，保留协议/诊断结果 |
| T4 mechanism path | release 后 action change、probe 比例、family/provider permutation | 若变化都来自 probe 或 permutation 不影响，停止 shared-residual exploitation claim |
| T5 recovery | 从 raw events + snapshot 重算 visibility/action/release/state/metrics | 任一字段不能恢复，停止 realtime/recoverable claim |
| T6 verifier utility | confidence threshold vs matched random/always/never escalation，固定 extra-call budget | 无 Pareto 改进则停止 verifier reliability claim |
| T7 lineage | truth/hash/split/overlap/provider training membership | 未闭合时停止 public/independent/generalization claim |
| T8 dynamic scope | delays + controlled failures；arrival/retire/version 另行冻结 | fixed-pool replay 不能外推自然 nonstationarity |
| T9 formal consistency | menu mutation、same-tick order、label、hazard、variance | 任何一项不能执行，先修规格，不进入新实验 |

## 5. 推荐推进顺序

1. 先统一正文名称：建议具体算法叫 **MIRA-Relay**，MIRA 只在需要表示框架时使用。
2. 先修 P0 的状态、顺序、标签和 hazard/verifier 合同，并写单测与 recovery 检查。
3. CPU 先跑 T1/T2/T4/T5 stop gates，再决定是否支付 T3 的完整比较成本；不要用 A800 追一个尚未闭合的机制。
4. 完成 baseline eligibility/fidelity 分层，GraphRouter 条件项不进 strict main table，Online/Replay SGD 拆行。
5. T6 单独形成 verifier utility 矩阵；不能拿 selector 结果代替 verification claim。
6. 完成 provider lineage、split 和 endpoint 审计后，一次性冻结 confirmation E6，再填主结果、摘要和结论。

## 6. 审查结论

论文的叙事方向已经对齐参考文档，当前真正的短板是“根因—机制—证据”还没有闭合，而不是标题或背景不够华丽。下一轮修改应优先让 MIRA-Relay 在动态菜单、延迟释放、失败语义和 verifier 接口上可执行，再用动作级消融证明每个部件确实改变了因果链；在此之前，不应把当前 TBD 草稿包装成效果论文。
