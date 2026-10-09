# 最近方法与统一比较矩阵

- 日期：2026-10-06
- 状态：研究审计文档；不改变当前 benchmark、baseline 角色或论文主文。
- 目的：把“最近”拆成任务、信息、反馈、目标和实现合同五个维度，避免把有相似名字的方法混成一张 raw-accuracy 排名表。
- 当前主协议：公开 TDC BBB truth、四个 checkpoint-bound fresh providers、157 个 scaffold-disjoint confirmation molecules、一次调用、selected-only delayed feedback、$D\in\{0,1,4\}$。
- 当前主协议的唯一生效入口：[Benchmark 与 Baseline](../../docs/研究主线/生效/03-Benchmark与Baseline.md)。

## 1. 读法和比较规则

“可比”不是“可以引用”。本矩阵把每行分成四种角色：

- **strict**：输入表示、fit-only warm start、菜单、反馈时钟和更新预算均落在共同合同内，可做成对比较。
- **adapted**：算法思想可以迁移，已有适配回放，但表示、似然、先验或更新语义至少有一项不同；只报告相对自身 Frozen 或同一适配 stratum 的结果。
- **audit-only**：论文或源码相关，但必要输入、反馈或实现保真尚未冻结；没有效果数字。
- **anchor**：任务下界、强固定策略或评估器上界；不作为可部署在线学习器排名。

以下字段使用 `MISSING` 表示原始收据没有该字段；不从代码参数、哈希或模型维度推算。该规则来自成本/预算公平审计和 baseline eligibility 审计：

- [成本/预算公平审计](../../task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_fairness_audit_v1/report.md)
- [baseline 资格矩阵](../../task/task49-pharma-benchmark-baseline-freeze/baseline_eligibility_matrix_v2/report.md)
- [baseline 合同 v3](../../task/task49-pharma-benchmark-baseline-freeze/baseline_contract_v3/report.md)

## 2. 矩阵总表

| 方法/工作 | 任务输入与表示 | 反馈时序与可见信息 | 训练/更新目标 | selected-only？ | 失败/拒答语义 | 与当前 MIRA-Relay 的可比性 | 需要补做的实验或证据 |
|---|---|---|---|---|---|---|---|
| **RouterBench**（基准面板，不是算法） | 公开 query、候选 LLM 输出、质量分数和成本；原生是多 LLM routing panel，不是 BBB 分子/provider 输入 | 面板提前生成全候选输出和质量；原生离线评估，未规定本项目的 release queue | 成本—质量曲线、router quality、AIQ/convex-hull 等；可训练/测试 router 而无需再次调用模型 | **否，原生 full-panel**；若改成在线实验，必须在 wrapper 中屏蔽未选分数 | 原生记录 quality/cost；没有本项目 runtime failure、unknown truth、refusal 的化学 ledger | **不是 baseline**。可作为外部 routing 协议参考；不能把 full-panel 分数当 selected-only 证据 | 明确数据版本/许可；实现 selected-only wrapper、延迟 $D$、菜单/arrival/drift 和 failure/unknown/refusal 字段；先做 leakage verifier，再决定是否做 transfer。当前生效文档把它放在外部迁移而非主 benchmark |
| **Frozen direct**（strict control） | 当前 query $x,e,A_t,H_t$；fresh confirmation 使用 Morgan-64 加截距，fit 区为每 provider 拟合 direct logistic correctness head | 决策只读 query、完整当前菜单和已释放历史；无学习更新；反馈即使释放也不改动作 | 固定 fit-only correctness score；evaluation 期间参数不变 | **是**：策略不读取未选结果，也不更新；选中结果只进入 ledger | runtime failure、chemical error、unknown、refusal 分开；unknown/refusal 不生成 chemical label | **strict 主控制**；是 MIRA 冷启动的可核对参照 | 作为每个 fresh/stress stream 的必有对照；统一报告 success、conditional correctness、failure、unknown/refusal、成本和状态字段。当前开发成本收据见 `baseline_cost_budget_v1/report.md` |
| **Online SGD**（matched logistic control） | 与 Frozen 相同 query、provider menu、65 维 direct prior 和 fit-only warm start | selected provider 的 outcome 在 `decision_tick+1+D` 释放后才可用；未选 provider 不产生标签 | 对已释放 selected reward 做一次 Bernoulli logistic/BCE 梯度更新；固定学习率，不重放历史 | **是** | failure 不写 chemical correctness；unknown/refusal 不更新 chemical head；状态链需逐事件复核 | **最强 strict logistic 近邻**；可与 Frozen、MIRA 做 paired comparison，不等于更现代 router 的 raw 总榜 | 继续保留 matched v2 fail-closed receipt；把 adapted 行缺的 `state_bytes`、`unknown_reason`、`refusal_action`、统一 timing 补入最终主表，不能从旧日志回填伪值。收据入口：`experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/` |
| **Greedy LinUCB**（modern adapted Gaussian） | 同一 BBB menu/hidden-64 可用，但当前适配使用 fit-only reward-ridge/Gaussian prior；另有 adapted direct-logit 历史行 | selected reward 按共同延迟队列释放；每次 rank-one posterior 更新；只读已释放 selected feedback | Gaussian reward posterior + UCB；`alpha`、prior precision、noise 在实验卡固定 | **是（适配 wrapper）**；原论文/实现的任务语义仍需与药物 contract 区分 | runtime failure 记 attempts 分母；unknown/refusal 字段未在全部旧 receipt 闭合；不能默认为 0 有能力 | **adapted modern comparator**。协议时钟一致，但 likelihood、prior 和成本状态不与 strict logistic 相同；只做 paired online−Frozen 或 Gaussian-stratum 比较 | 已有 60-stream CPU development replay 和 fresh controlled stress 行；若要进入完整主表，补齐统一 state/refusal/unknown 成本 schema，并固定 Gaussian 与 logistic 不跨层排名。证据：`greedy_linucb_development_v2/report.md`、`provider_fresh_drift_stress_v2_bayesian/report.md`（同 stress 表） |
| **PILOT-adapted**（budget-aware modern router） | 原工作使用 query/context 与 provider embedding 的投影表示；本项目适配使用冻结 encoder/query projection 和 provider vectors，非原生药物输入 | 适配版在线只读取 selected provider delayed reward；离线 preference prior 先由 fit 区训练 | 两阶段 preference prior；在线 selected-arm ridge，cosine mean 加 UCB；不能把普通 cosine/UCB 改称 MIRA | **是（本项目适配版）**；原 RouterBench 训练/评估协议本身通常有全臂离线 panel | runtime failure 与 chemical correctness 分开；原工作没有当前 unknown/refusal ledger，需适配时显式记录 | **adapted、非 strict**。已有统一回放和负结果，但 projected representation、prior 和 likelihood 与 MIRA/Frozen 不同 | 若升级为 paper 主表：在四-provider fresh confirmation 上冻结 projection/prior，补 unified state bytes、failure/unknown/refusal、budget 和 independent replay；否则保留为 development/audit modern comparator。证据：`pilot_adapted_development_v1/{preregistration.md,report.md}`、`baseline_adapters_v0.md` |
| **BayesianRouter**（Bayesian adapted router） | 原论文是 query/response-model routing；审计过的实现以离线 Bradley–Terry/分类表示生成 provider-specific Gaussian prior；本项目使用 fresh/BBB query 与 provider menu | selected reward 在 release tick 后做 posterior update/Thompson；当前确认 stress 只读取已选、已释放事件 | 离线 BT/CLS prior + Gaussian posterior/Thompson selected-arm update；原仓库存在默认路径与论文设置不一致，不能把坏默认当复现 | **是（独立适配实现）**；full-panel 只用于 fit-only prior，evaluation 禁止读未选结果 | runtime failure 单独保留；当前 adapted raw receipt 的 unknown/refusal 覆盖不完整，保持 `MISSING` | **adapted modern comparator**。controlled stress 有可核对同协议结果，但不是 native 代码完全复现，也不与 logistic strict 行无条件合并 | 主表需同时给 prior/likelihood/feature lineage、状态 bytes、failure/unknown/refusal 和成本；保留 native-BT receipt 与 direct-prior audit 分离。当前 stress 证据：`provider_fresh_drift_stress_v2_bayesian/report.md`；源码审计：`baseline_audit/bayesianrouter/` |
| **GraphRouter**（静态 graph router） | task/query/provider(LLM) 异构图；节点含 query、task 和 provider 描述 embedding，边含历史 performance/cost；BBB 单端点会使 task 节点退化 | 原论文主要从训练/验证 interaction edges 学习，测试时冻结 edge predictor；没有 selected-only delayed online queue | 异构 GNN 边预测 performance/cost，再 argmax 选择；静态 inductive generalization to new providers 是重点 | **否**：原生是 fit/full interaction edge 训练后静态推断，不是 selected-only learner | 原实现按 performance/cost 处理；本项目 runtime failure、unknown truth、refusal、chemical correctness 的分账尚未定义 | **audit-only / blocked**。图输入语义、provider descriptor、edge 权限、tie-break、failure ledger、成本和独立 verifier 未闭合；不能写成已运行 baseline | 若要运行：冻结 provider descriptors；只用 outer-fold fit/calibration edges；登记 native/tie-aware tie-break；补 BBB failure/unknown/refusal/成本合同、independent recovery 和静态 confirmation。证据：`graphrouter_static_adapter_v1/blocking_report.md`、`baseline_audit/graphrouter/SOURCE.json` |
| **RouterDC**（静态 dual-contrastive router） | query 表示与候选 LLM/provider 表示；通过 dual-contrastive 学 query–model compatibility，原生是多 LLM 选择 | 离线完整训练/偏好或任务分数，测试时静态选择；没有本项目的 selected-only release clock | 双对比学习的静态路由目标；不含已选反馈后的在线后验更新 | **否，按论文协议**；若改成 selected-only wrapper，已不是无修改的原方法 | 原论文路由分数/任务质量语义不能直接替代 runtime failure、unknown truth、refusal；适配必须新增 ledger | **audit-only / optional adapted**。适合回答“强离线 router 是否已足够”，不回答动态 delayed adaptation；官方仓库未声明许可证，不能复制代码 | 资源允许时按论文独立重实现；冻结 provider/query feature、fit-only labels、tie/failure/refusal semantics、independent verifier，再与 Frozen/Best Fixed 做静态对照。来源和许可审计：`task/.../sources.jsonl`（`routerdc-paper`、`routerdc-code`）、`baseline_adapters_v0.md` |
| **MIRA-Relay**（proposed） | 与 Frozen 相同输入和 fresh provider menu；frozen provider surface + provider-local fit-rate-centered residual ledger | selected-only correctness 只在 release tick 进入状态；延迟 $D\in\{0,1,4\}$，当前 one-call contract | 冷启动严格退化为 Frozen；release 后仅更新被选 provider 的 residual counters；runtime hazard 与 chemical correctness 分开 | **是** | runtime failure 不写化学 correctness；unknown/refusal 不生成 chemical label；审计状态含 `fit_rates/successes/counts/prior_count` | **candidate/proposed**；与 Frozen、Matched Online-SGD 成对比较，与 adapted modern rows 分层报告 | E3 需继续补 mechanism/independent-residual/family 轴；主结果必须同时报告 natural fresh null 与 controlled evaluator-side drift，不把后者写成自然 lifecycle/general online gain。当前 receipts 在 `experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/` 和 `task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1/` |

## 3. Anchor 行：必须存在，但不参与在线方法总排名

| Anchor | 输入/反馈权限 | 目标 | 失败/拒答 | 允许的解释 |
|---|---|---|---|---|
| Random | 同菜单、同 episode；不使用 provider outcome | 检查任务是否有可学信号 | 按同一 attempts/success 分母记账；拒答能力不由该行推断 | 任务下界和协议 sanity |
| Best Fixed | 只用 fit 区完整 panel 选一个 provider；evaluation 固定 | 强静态 incumbent | 无 online update；failure 按 selected call 记账 | static headroom；不得写成 online learner |
| Retrospective Best Fixed | evaluator 读取 confirmation full panel/truth 选一个固定 provider | 事后不可部署上界/参照 | 不进入决策状态 | 上界和 headroom；不是逐分子 Oracle，也不参与可部署排序 |
| Oracle（若单独报告） | evaluator 读取每题完整 panel + truth | 逐题机会空间 | privileged，不可部署 | 只能作 evaluator-side upper bound，不能作为 baseline superiority |

这些 anchor 的角色来自 [baseline role roster](../../task/task49-pharma-benchmark-baseline-freeze/baseline_role_roster_v1/report.md) 和当前生效 benchmark 文档；它们回答“任务有无 headroom”，不回答“延迟反馈学习器是否有效”。

## 4. 统一可比性结论

### 4.1 当前可以放进同一主结果语境的行

- **strict core**：Frozen direct、Matched Online-SGD；MIRA-Relay 作为 candidate，与二者共享 common query、fit-only prior、menu、selected-only release 和 delay。
- **same protocol but adapted**：Greedy LinUCB、BayesianRouter（当前 controlled stress 已有结果）。它们可放在“modern adapted controls”小节，但必须同时显示 representation、likelihood 和 prior 差异。
- **anchors**：Random、Best Fixed、Retrospective Best Fixed/Oracle 单列。

这不是“只挑对我们有利的方法”：它是防止把不同 statistical stratum 的输入和先验差异误读成算法收益。该分层在 `baseline_cost_budget_v1/report.md` 和 `baseline_eligibility_matrix_v2/report.md` 中已有机器表与审计结论。

### 4.2 不能直接写成已完成效果的行

- **PILOT**：有 adapted development receipt 和 selected-only wrapper，但当前投稿 fresh 主表未冻结其 full confirmation 证据；若保留，只标 adapted/audit。
- **GraphRouter**：provider-description/edge semantics、selected-only update 和 failure/refusal contract 尚未闭合，当前只审计。
- **RouterDC**：论文近邻和源码快照已登记，但没有本任务的独立重实现/效果收据；许可证也未在仓库元数据中声明。
- **RouterBench**：是静态/离线公开 routing panel，不是动态 selected-only baseline；外部迁移要另建 wrapper 和 leakage audit。

## 5. 对实验矩阵的直接要求

若要让主实验矩阵满足三份验收标准，至少保留以下问题—对照—证据映射：

| RQ | 必须回答 | 最小方法集 | 不能省略的字段 |
|---|---|---|---|
| RQ1 任务 headroom | provider 是否有 context-dependent 差异？ | Random、Best Fixed、Retrospective Best Fixed/Oracle、Frozen | fit/test 分隔、full-panel 权限、失败分母、scaffold/family 分层 |
| RQ2 strict online | selected-only 延迟反馈是否优于不更新？ | Frozen、Matched Online-SGD、MIRA-Relay | release tick、delay 0/1/4、paired delta/CI、state hash、unknown/refusal |
| RQ3 modern transfer | 近期 router 思路迁移后表现如何？ | Greedy LinUCB、BayesianRouter；PILOT 若无 fresh receipt 单独列 | feature/prior/likelihood、adapted 标签、同菜单/预算、selection/update cost |
| RQ4 mechanism | 收益来自 provider-local release residual，而非普通更新或 backbone？ | MIRA full、Frozen/no-update、Matched SGD、必要的 provider-local/family/independent ablation | action changes、rescued/harmed、failure 与 chemical error 分开、反例与 negative result |
| RQ5 boundary/cost | 何时不工作、代价是什么？ | 全部可部署行 + anchors | natural stationary vs controlled drift 分开、runtime failure、unknown/refusal、p50/p95、memory/state |

## 6. 目前尚未闭合的缺口

1. **统一 adapted 成本和状态**：PILOT/BayesianRouter/Greedy 的旧 receipt 有 selection/update timing，但部分没有 `state_bytes` 或逐事件 `unknown_reason/refusal_action`；保留 `MISSING`，不能用模型大小代填。
2. **RouterDC/GraphRouter 的静态强对照**：如果正文声称“覆盖近期静态 router”，必须完成至少一个 paper-faithful independent adapter；否则正文把它们写成 related-work/audit-only，而不是已运行 baseline。
3. **RouterBench 外部迁移**：full-panel panel 与当前 selected-only contract 不等价；必须先做 wrapper 和 leakage verifier，再决定是否投入实验。
4. **失败/拒答覆盖**：当前 one-call confirmation 的 unknown/refusal 不是能力结果，观察为 0 不能写成“方法学会拒答”。
5. **E3 机制充分性**：现有受控 stress 证明 declared drift 下可恢复，但还没有用完整 independent-residual/family/interface 轴证明收益只能由 provider-local residual 解释；E3 审计资料不能升级成已闭合主张。

## 7. 可引用的证据索引

- 主 benchmark/实验矩阵：[docs/研究主线/生效/03-Benchmark与Baseline.md](../../docs/研究主线/生效/03-Benchmark与Baseline.md)
- 适配合同与开源来源：[baseline_adapters_v0.md](../../task/task49-pharma-benchmark-baseline-freeze/baseline_adapters_v0.md)
- 角色和比较权限：[baseline_role_roster_v1/report.md](../../task/task49-pharma-benchmark-baseline-freeze/baseline_role_roster_v1/report.md)
- 输入/预算/成本审计：[baseline_cost_budget_fairness_audit_v1/report.md](../../task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_fairness_audit_v1/report.md)、[baseline_cost_budget_v1/report.md](../../task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_v1/report.md)
- 资格和硬阻塞：[baseline_eligibility_matrix_v2/report.md](../../task/task49-pharma-benchmark-baseline-freeze/baseline_eligibility_matrix_v2/report.md)
- RouterBench/GraphRouter 一手论文文本：`research/paper_sources/aamas_related/text/routerbench.txt`、`research/paper_sources/aamas_related/text/graphrouter.txt`
- RouterDC、PILOT、BayesianRouter 来源登记：`task/task49-pharma-benchmark-baseline-freeze/sources.jsonl`、`baseline_audit/`、`pilot_adapted_development_v1/`、`bayesianrouter_native_bt_raw_v1/`
- 当前 controlled BayesianRouter 收据：[provider_fresh_drift_stress_v2_bayesian/report.md](../../task/task49-pharma-benchmark-baseline-freeze/provider_fresh_drift_stress_v2_bayesian/report.md)

## 8. 本文档的决策边界

本矩阵不把任何未完成的适配写成结果，也不改变三份生效文档。它的作用是让写作和实验设计在同一张表上回答：

1. 这个方法看到了什么？
2. 它什么时候能得到标签？
3. 它优化什么？
4. 它是不是只用 selected feedback？
5. 它如何记 runtime failure、unknown 和 refusal？
6. 它能和 MIRA-Relay 做哪一种比较？

若某行无法填写其中任一项，默认归入 audit-only，直到新合同和证据把缺口关掉。
