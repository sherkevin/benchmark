# Round 0：Benchmark / 最近邻 / 创新性独立审查

审查对象：当前生效故事线、方法论、Benchmark+Baseline 文档和 Task49 验收审计。依据：`评价标准/03-Benchmark与Baseline评价标准.md`。本报告只做独立审查，不修改生效主线。

## POSITION

`challenge / revise`。当前材料足以支持“fixed-pool、selected-only、delayed scientific provider routing protocol”的候选研究对象，但不足以支持“动态 JEV”已经构成方法创新。临时状态：Gate A/B/C 均 HOLD；建议把动态 JEV 改成待验证的 Proposed，而不是现成结论。

## 1. 动态 JEV 是否超出已有工作

**TARGET**：selector → confidence verifier → selected-only updater 的整体是否构成 H4/B9 以上的方法贡献。

**EVIDENCE**：当前最终 state/update/exploration 尚未冻结；已有工作覆盖 contextual routing、selected-arm online update、cascade、delayed bandit、correlated-arm surrogate、同功能 provider routing、conformal gate 和 selective prediction，包括 [Adaptive LLM Routing](https://aclanthology.org/2025.findings-emnlp.1301/)、[Online Multi-LLM Selection](https://arxiv.org/abs/2506.17670)、[Correlation-Aware Contextual Bandits](https://arxiv.org/abs/2607.09015)、[LQM-ContextRoute](https://arxiv.org/abs/2605.14241)、[Delayed UCB](https://proceedings.neurips.cc/paper_files/paper/2019/hash/56cb94cb34617aeadff1e79b53f38354-Abstract.html)、[TREACLE](https://proceedings.neurips.cc/paper_files/paper/2024/hash/a6deba3b2408af45b3f9994c2152b862-Abstract-Conference.html) 和 [Conformal LLM Routing](https://aclanthology.org/2026.acl-srw.70/)。Best Fixed=`0.9155` 仍高于当前动态 selector。

**REASONING**：如果只改变组件顺序或名称，H4/S4、M5/M8 不成立。必须存在统一接口、不变量或 task-specific update，并由 integration ablation 和反例证明必要性。

**DECISION TEST**：冻结唯一 Proposed，在同菜单、特征、初始化、delay、预算和 seed 下运行 Frozen、Best Fixed、matched Online/Replay、Greedy/PILOT/BayesianRouter、verifier-only 和 full；另做去 shared-state、去 verifier、去 delay queue 的消融。若 full 不超过 Best Fixed 和最强 matched baseline，或 `Δ_int≤0`，停止“动态 JEV 方法优越性”主张。

**CONFIDENCE**：0.93。影响：故事 S4/S5/S6、方法 M5/M8/M13、Benchmark B9/B10。

## 2. JEV / dynamic 命名是否过强

**POSITION**：`challenge / revise`。当前 `dynamic` 最多能表示 `feedback-adaptive`。

**EVIDENCE**：当前是 fixed pool + selected-only delayed replay，没有自然 arrival、retire、version drift；Laya 目前只有静态 forward，没有 selected-only optimizer/writeback。Benchmark 标准 B7、故事 H9 都禁止把 static replay 写成 non-stationary adaptation。

**REASONING**：JEV 不是已核验的标准对象；“dynamic”若没有环境事件，容易被审稿人理解为重包装。调用后 verifier 也不能回填成调用前 selector。

**DECISION TEST**：做术语去名化盲审：若读者不能指出一个超出 routing/delayed bandit/cascade 的单一差异，正文暂用 `feedback-adaptive provider selection with post-call verification`；只有加入预登记 arrival/retire/version/shift 并报告 recovery，才恢复 environment-dynamic 表述。

**CONFIDENCE**：0.95。影响：故事 H9、方法 M10/M15、Benchmark B7/Gate C。

## 3. TDC truth + Task36 routing layer 是否足够作主 benchmark

**POSITION**：`challenge`，但可通过收窄解决。

**EVIDENCE**：[TDC](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/4c56ff4ce4aaf9573aa5dff913df997a-Abstract-round1.html) 是公共 truth/benchmark 生态；当前生效文档也承认 Task36 routing layer 是本项目协议，不是社区 benchmark。BBB 仍是 development；hERG/AMES、provider lineage、confirmation overlap 尚未闭合；旧 panel 与官方 test 存在 canonical/scaffold overlap；24 个 B3clf 变体不能当 24 个独立 family。

**REASONING**：公共 truth 的权威性不自动传递给自建 routing protocol。必须把 truth layer、provider layer、事件 schema、hash、模型资格和 untouched confirmation 分开写，不能把自建回放协议冒称公认 benchmark。

**DECISION TEST**：发布可复核 truth/provider/event manifest 和 checker；绑定 model-fresh 或 lineage-aware checkpoint；生成 molecule/scaffold-disjoint confirmation provider outputs；至少增加一个合格扩展端点，或明确把论文收窄为 BBB protocol case study。

**CONFIDENCE**：0.93。影响：B1–B5、B11、Gate A A2/A3/A4。

## 4. baseline 是否足够现代和完整

**POSITION**：`challenge`。

**EVIDENCE**：已有 Random、Best Fixed、Oracle、Frozen、Online/Replay SGD、Greedy、PILOT、BayesianRouter；GraphRouter/Laya-online 未闭合，缺 canonical delay comparator、same-function provider router 和 correlated surrogate。可补充 [CAMS](https://proceedings.neurips.cc/paper_files/paper/2024/file/5966e7c5c017d8449d215faeb1db4173-Abstract.html) 作为 privileged active-label control，[GraphRouter](https://arxiv.org/abs/2410.03834)、[RouteLLM](https://arxiv.org/abs/2406.18665)、[FrugalGPT](https://arxiv.org/abs/2305.05176) / TREACLE 作为静态/级联近邻，[LLMRouterBench](https://aclanthology.org/2026.findings-acl.1881/) 作为外部 sanity，不混入医药主表。

**REASONING**：没有至少一个 direct same-function provider baseline 和一个 canonical delay baseline，就无法判断“新机制”是否只是普通 router 在新数据上的适配。CAMS 的 full-label loss 只能做 privileged control，不能排入可部署排名。

**DECISION TEST**：补 Random receipt、Delayed UCB、canonical PILOT、LQM/Correlation-Aware 行为审计或同合同实现；按 deployable / privileged / diagnostic 分组；每个 baseline 固定版本、commit、适配差异、license、调参预算和 recovery。

**CONFIDENCE**：0.94。影响：B9/B10、Gate B。

## 5. provider family 相关性是否会虚增 headroom

**POSITION**：`challenge`。

**EVIDENCE**：24 个 B3clf 变体相关性高；provider pooling 可能把 family artifact 当成独立 arms。LLMRouterBench 也报告 ensemble 增大后收益递减。

**REASONING**：pooled 33-provider 的 Oracle gap 不能直接解释成可学习的 provider diversity。必须看 family-macro、leave-family-out、exposure 和 lineage 分层。

**DECISION TEST**：报告 all-provider、family-balanced menu、family-macro、leave-family-out 和 provider exposure；若收益只存在于 pooled accuracy，不保留 general provider adaptation 主张。

**CONFIDENCE**：0.96。影响：B11、A4、R4/C4。

## 6. dense cache replay 是否真的遵守 selected-only

**POSITION**：`challenge`。

**EVIDENCE**：合同要求 visible history、release time、action、state hash；full panel 只允许用于 fit-only warm start、Oracle 或 audit。缓存拥有完整 provider 输出，容易产生 post-hoc leakage 怀疑。

**REASONING**：如果 runner 在选择前读取了未选输出，所有 online 结果失去意义。不能只靠代码说明无泄漏，必须有独立 checker。

**DECISION TEST**：向 trace 注入 unselected output、future reward 和 full-panel 字段；checker 必须拒绝非法 trace，合法 trace 的 action/state 必须不变。否则结果降级为 offline diagnostic。

**CONFIDENCE**：0.95。影响：H5/H6、B6、R2。

## 7. post-call verifier 不能支撑 pre-call selection

**POSITION**：`challenge`。

**EVIDENCE**：confidence escalation 的 `0.923823 / 1.206 calls` 是 post-call 结果；它与 selector 的 future correctness 不是同一个 estimand。cascade 和 conformal routing 已覆盖“返回后继续调用/安全门”的一般形式。

**REASONING**：若 gain 只来自第二次调用，贡献应写 verifier/cascade；不能把 post-call confidence 喂回 pre-call selector，也不能用端到端 accuracy 隐藏额外调用成本。

**DECISION TEST**：分开 selector-only、verifier-only、selector+verifier、always/never/random matched escalation；统一 total call budget、coverage、accepted risk、拒答成本、feedback 和 latency。

**CONFIDENCE**：0.98。影响：H6、S2/S3/S6、M3/M11/M15。

## 8. 当前 Gate 临时判定

| 闸门 | 临时判定 | 原因 |
|---|---|---|
| Gate A | HOLD | truth 层可用，但 routing protocol、lineage、overlap、confirmation 仍未完全闭合 |
| Gate B | HOLD | development baseline 主表已形成，但 Random、强静态 router、delay/same-function 近邻和 Laya-online/fidelity 未闭合 |
| Gate C | HOLD | delay/failure/负结果有证据，但 E1–E5、dynamic event、paired uncertainty、成本表和 confirmation 不完整 |

## 9. Kill criteria

- `K1`：唯一 Proposed 在三个 delay 中至少两个不超过 Best Fixed 和最强 matched baseline，或 `Δ_int≤0`；停止方法优越性主张。
- `K2`：selector 读取 truth、未选输出、full panel、future reward 或 post-call confidence；直接 FAIL。
- `K3`：fixed-pool 中没有声明的 state change，或 d∈{0,1,4} 无法重建；改名为 feedback-adaptive 或停止。
- `K4`：endpoint、lineage、confirmation 未闭合；只能写 BBB development protocol。
- `K5`：family-macro/leave-family-out 无 gain；删除 general provider claim。
- `K6`：gain 只在额外 post-call 调用后出现；保留 verifier/cascade 结果，删除 selector claim。
- `K7`：最近邻无法运行或审计；不能写 state-of-the-art 比较。
- `K8`：raw event/hash/recovery 不一致；主结果无效。
- `K9`：扣除调用、延迟、内存和拒答成本后 gain 消失，或 paired CI 跨 0；写成本/负结果边界。
