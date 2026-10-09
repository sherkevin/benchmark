# AAMAS 2027 叙事对齐与逐轮审查卡

- 版本：v1
- 日期：2026-10-04
- 适用稿件：`paper/aamas2027/main.tex`
- 依据：`research/2026_AI顶会最佳杰出论文叙事范式_六大范式_阅读整理.md`、`docs/决议/0008-论文叙事范式作为写作评审基准.md`
- 状态：当前生效的写作审查卡；不是 AAMAS 官方规则，也不是录用承诺。

## 1. 这轮选定的叙事组合

### 主范式：根因手术刀

论文的中心不是“在线模块比固定模块高多少”，而是先解释一个可重复的失败：在具体 provider 菜单中，动作发生在当前输出可见之前；正确性标签只属于被选 provider，而且可能延迟释放。静态赢家因此不能利用部署后的证据，普通无约束更新又可能把稀疏、延迟的观察误当成即时的全局证据。方法必须直接改变这条信息和反馈链，而不是只增加一个预测头。

主线句：

> 当工具调用的正确性只能以 selected-only、delayed outcome 的形式到达时，怎样让后续选择利用这些证据，同时不把未观察结果、运行失败和调用后的信号偷偷带入调用前决策？

### 支撑范式一：新基准暴露失效

公共 truth layer 提供答案，33-provider routing protocol 提供可审计的动作、菜单、延迟、失败和释放合同。该 routing layer 是本项目协议，不能冒充已有社区 benchmark。它的作用是把“工具能返回”与“工具在当前分子上正确”拆开，并暴露静态/普通在线方法在真实信息边界下的失效。

### 支撑范式二：反直觉重构

论文挑战的默认设定是：看到一次结果后，就可以把它当作普通在线标签，或把 post-call confidence 直接回填 selector。本文把 selector、verifier 和 delayed updater 的信息边界拆开，并把 fallback、selected-only release、runtime hazard 和 correctness ledger 放入同一事件合同。这个重构只有在动作级消融和恢复检查通过时才成立。

### 暂不作为主线的范式

- 理论照亮经验：当前可提供形式化合同和复杂度分析；尚未有足够定理支撑“理论保证有效”，不能以定理型故事书写。
- 极简统一美学：MIRA-Relay 仍包含多个职责模块，尚未证明组合本身带来不可替代的统一收益。
- 社会价值叙事：只作科学工具可靠性的动机，不承担技术创新主张。

## 2. 论文必须保持的因果链

每一章都要能回到同一条链：

`科学工具被调用` → `正常返回仍可能错误` → `选择前看不到未选输出，正确性又只对已选工具产生且延迟` → `静态赢家/无约束更新各自漏掉一个信息边界` → `把选择、验证和释放更新写成可审计事件合同` → `MIRA-Relay 作为 MIRA 框架的具体候选，针对这些边界提供状态、探测和风险账本` → `用匹配基线、动作级消融和独立恢复检查验证` → `明确在哪些菜单、端点、延迟和失败条件下成立`。

如果某段不能把读者交给链上的下一环，就应删除、移动或改写；增加背景和模块名不能修复断链。

## 3. 当前稿件的段落骨架

| 位置 | 必须完成的动作 | 当前稿件的合格条件 |
|---|---|---|
| Abstract | 现实场景、失败根因、方法改变的接口、比较合同、结果状态、边界 | 不把 TBD 或 development diagnostic 写成效果；一句话说清 selected-only + delay 是根因 |
| Intro P1 | 建立科学工具调用的现实 | 直接进入“具体 provider 选择”，不扩成泛 agent 宣传 |
| Intro P2 | 给出可复核失败 | 分子/端点/provider 菜单例子；正常返回与 correctness 分开 |
| Intro P3 | 解释旧方法的共同缺口 | fixed、static、unconstrained online、post-call confidence 各缺什么，不能只列名字 |
| Intro P4 | 给出唯一 insight | selector/verifier/updater 的信息边界和事件释放合同；这是主线转折点 |
| Intro P5 | 只介绍与根因一一对应的机制 | fallback、family/provider state、probe、hazard ledger 各自有失败模式，不写模块清单 |
| Intro P6 | 列出可证伪承诺 | 每条 claim 对应一个 RQ、一个主比较和一个失败条件 |
| Problem | 先接口后公式 | 每个符号先自然语言，再公式，再用 `z000282` 实例化；不新增无来源原始概念 |
| Method | 输出和信息边界先于实现 | 说明 selector 看什么、verifier 何时可看什么、updater 何时更新；复杂度和可恢复性随后 |
| Benchmark/Baselines | 先划清公共 truth 与项目 routing protocol | 不把自建回放层称为权威社区 benchmark；每个 baseline 有角色和公平合同 |
| Experiments | 按 RQ 而非执行时间写 | 先任务有效性和 headroom，再主比较，再机制/边界/成本；主表只放合法同层方法 |
| Discussion | 解释因果含义 | 说明哪些结果支持根因、哪些只支持协议/诊断；不重复表格、不扩大 scope |
| Conclusion | 留下一条持久教训 | 信息边界和 delayed feedback 是工具适应的核心条件；不引入新结果 |

## 4. Claim—evidence—falsifier 对照

| 主张 | 最近邻问题 | 必须出现的证据 | 证伪或收窄条件 |
|---|---|---|---|
| direct selection 是 input-dependent 且有 headroom | fit-only fixed/static selector | public truth、完整菜单、Best Fixed/Frozen/Oracle、scaffold 分层 | Oracle 与固定策略无 headroom，或 provider 输出不可追溯 |
| delayed selected-only feedback 是实质约束 | 普通 contextual bandit/online SGD | 可见历史、release queue、delay `{0,1,4}`、selected-only recovery | checker 发现偷看未选输出/未来标签，或 delay 不改变可见信息 |
| MIRA 的共享状态改变后续 exploitation | probe-only / provider-local update | 同 schedule 的 NoUpdate、IndependentResidual、family/provider permutation；action changes 与 rescued/harmed | 所有改变来自 probe，或 action-level paired effect 跨折为零/负 |
| verifier 与 selector 的分界有实际价值 | post-call confidence 被回填 selector | pre-call 与 post-call 日志分离、matched escalation、calls/latency/unknown/refusal | verifier 不能改善 utility，或 selector 使用了后验信息 |
| 方法可实时运行且可恢复 | 离线重训或 opaque router | p50/p95 update/selection、state bytes、snapshot + raw JSONL independent replay | 需重放全历史，或不能从事件前缀重建动作 |

开发诊断、smoke、受控事件和负候选只能解释机制边界，不能替代上表主证据。

## 5. 每轮审查必须给出的六个答案

1. 现有方法具体在哪里翻车？是否有真实 provider/menu/feedback 例子？
2. 根因是否是一个可证伪的机制，而不是“缺少适应性”的抽象判断？
3. 每个方法部件是否只服务一个根因，并能被去掉后单独检验？
4. 论文是否把 public truth、routing protocol、online effect 和 controlled diagnostic 分开？
5. 主表是否先对最强合法近邻，而不是先对 Random 或 evaluator-only Oracle？
6. 哪个结果会迫使我们修改故事？如果没有明确答案，当前段落不合格。

## 6. 写作禁区与修改优先级

- 不把用户提供的六范式文章中的论文、奖项或数字写成已核验事实；它是叙事参考。
- 不用“动态”“实时”“可靠”替代可测量的 release、latency、failure、unknown 和 correctness 定义。
- 不用模块数量、公式数量或标题修辞证明创新；创新要由根因—机制—消融—边界闭环支撑。
- 不用一个 pooled accuracy 同时证明选择、验证和在线更新三个 claim。
- 不按失败尝试的时间顺序写主文；候选停止和负结果放在机制边界/错误分析。

修改顺序固定为：先修 P2–P4 的根因和转折，再修贡献与实验映射，最后才润色句子和图注。每次修改保留审查报告、旧稿 hash、新稿 hash 和复核结论。

## 7. 当前状态

- 主叙事已选：根因手术刀。
- 基准叙事已选：新基准暴露失效，作为结构支撑。
- 当前论文是七页内部 pre-results draft；经验单元仍为 TBD。
- Graph Relay、HRA 等已核验负机制诊断不删除，放在内部证据链中，不冒充主方法效果。
- 下一步是独立 reviewer 逐段审查；审查结束后只做能提高三份评价标准分数的最小重写，并重新编译 PDF。
