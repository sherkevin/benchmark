# ADR 索引

当前研究故事线、方法论、Benchmark、Baseline 和三份评价标准以 [研究主线文档](../研究主线/README.md) 为唯一入口。这里的 ADR 保留过去的技术裁定和实验闸门；只有在适用范围明确且没有被新的主线文档替代时，才约束具体执行。新的小范围双方共识统一写入 [决议目录](../决议/README.md)。

| ID | 摘要 |
| --- | --- |
| [0022](0022-continuous-aamas-research-loop.md) | Accepted，2026-09-26。持续推进已批准计划，每个小任务后核对故事、方法与证据；实验用星云 A800 单任务执行，开始官方 LaTeX 题目摘要草稿 |
| [0023](0023-morgan-diagnostic-gates-next-method.md) | Accepted，2026-09-26。Morgan 静态诊断与 calibrated switch 均未超过 Best Fixed；停止在该表示上调参，先测 Laya 表示与 selected-only 在线 baseline |
| [0024](0024-laya-static-diagnostic-gate.md) | Accepted，2026-09-27。Laya 静态 candidate-marker 未通过预登记闸门；不再调静态 head，保留为在线反馈实验的可选输入 |
| [0025](0025-bayesianrouter-online-gate-failed.md) | Accepted，2026-09-27。selected-only BayesianRouter 在线更新在 3 种延迟上均未超过 frozen BT+CLS；保留为负 baseline，下一步先做 matched SGD/replay 对照 |
| [0026](0026-matched-sgd-controls.md) | Accepted，2026-09-27。先用同一 BT checkpoint 的 Frozen/Online-SGD/Replay-SGD 隔离普通 head 更新收益；标准更新不作为创新 |
| [0027](0027-aamas-long-term-gate-target.md) | Accepted，2026-09-27。长期目标是 AAMAS 2027 前通过数据、隔离、真实运行、独立恢复、冻结确认五道闸门 |
| [0001](0001-tool-identity-version-lineages.md) | 工具同一性以权重+训练数据+端点定义为准，重训版本算两个工具 |
| [0002](0002-tool-table-schema.md) | Step1 工具表 schema：一行一工具、输出性质类型必填、可用性门槛 |
| [0003](0003-step1-verification-protocol.md) | 核验分 L3/W/V 三档，记录格式为值列+状态列+原因列三件套，禁用哨兵值；文末修订节把 L3 准入从「逐位一致」松绑为「标签跨轮稳定」 |
| [0004](0004-verification-label-level.md) | 核验准入判标签级，概率漂移只记账不拦截；重复轮次从 2 提到 8。取代 ADR-0003 §一 L3 准入第 3 条的逐位一致 |
| [0005](0005-endpoint-level-smoke.md) | 冒烟判据改端点级，正式阶段一次性不重试，判据改版后整批重跑。含「当前代码状态」一节，逐行登记落地后的四处改动与变异验证 |
| [0006](0006-training-goal-decision-usability.md) | 训练目标裁定：可训练主任务为「判断这组预测能不能用」（α_i 驱动），选工具偏心降为评测目标。维持 ADR-0028 D1，判 `Proposal.md` §3.7 为未吸收的回退。GPU 闸门保持关闭 |
| [0007](0007-benchmark-task-requirements.md) | benchmark 题集构造要求（要求先于现状）：R1 现象 / R2 取舍 / R3 量级（题量 ≥5000）/ R4 可训练 / R5 防伪（易分子占比 ≤30%）五组不变量，每条带「为什么必须 + 验收判据」。owner 给出两个量化口径（5000、30%），2026-09-15 续裁文末三个数（难分子下限 ≥500、伪共识种子 50/100/200、选族优先级 BBB+hERG+AMES）。连带把 USER_TODO U-8 从「要不要废 BBB」重框为「按什么配比出题」；可行性证明 5000 @ 易≤30% 只在「分子 × 工具子集」单位下可达（BBB 单族 ≈13k 名义题） |
| [0008](0008-recovered-tmp-evidence-ingest.md) | 2026-09-16 环境清空后 /tmp 幸存数据的消毒纳入与防复发口径（owner：「搜索本机、尽量恢复、纳入 git 避免重复」）。四类确定性边界纳入 `data/recovered_tmp_evidence_20260916/`（冻结资产按 sha256 锚定 / run 现场减 `.cfg` / 顶层小文件 / 小型目录），378.5MB·10660 文件；密钥五道消毒（源头 chmod 600、`.cfg` 整排、多模式扫 0 命中、删 figshare AKIA 文件、第三方 git 魔法文件去毒）；生成器 `tools/build_recovered_manifest.py` 连跑 md5 一致。**客观证死 U-9 选项 (c)**：纳入 run 经判定 `p_y1=False`（是上一版 QED runs，非 BBB runs_naked），task23 逐字节重放仍不可行、处置收敛为 (a)/(b)。未来 run 现场入库属停机条件第 4 条交 owner |
| [0009](0009-fixed类题目形态分层口径.md) | **Proposed（草案，待 owner 批）**。fixed 类题目形态分层口径（owner 09-17 裁定①「分层叠加、立 ADR 钉死」的执行产物）：(a) 题集层归 ADR-0007/0006，单题层七道闸逐闸钉适用粒度——闸4 判别关离散零超参、闸5 ①②自检、闸6 baselines()+preflight **逐题**；闸0/闸1/闸3 生成器判分器/§4.1 免能力天花板**题型级一次**；闸2 实跑普查**族级一次**。(b) 防背主路线 = 真实分子 + R5.2 难度配比（易≤30% ⇒ 裸答投影 ~0.55），**取代** DECISIONS 08-26「自造从未存在的新分子」（:147①⑥）、08-27 乙变体（:149④）、C 链隐藏规律注入前提（:151，与 R1.2 冲突）——逐行列出处置，批准后按 §5 规则 4 移入 §4 不静默删除；08-23「答案不能是性质值」（:140）与 B 套**不冲突**（交付物是可用性判断不是性质值，:143 输出形式自由、:153 最高优先行背书）。(c) 路线图 §7.0「唯一在推 S1」与 §3 表「🔒 封存」矛盾、BENCH_SPEC B-4「没有一道题有合格判别关」两处挂账的订正文本（批后才落；B-4 更新以 task25 演示批闸4/5/6 实测为条件）。(d) 跨族难度口径待裁三案（归一化比例/族内分位/分族阈值表——hERG 仅 3 族 tw≥4 恒空集、AMES 4 族 tw=4 与伪共识塌缩）+ BBB 族内四种算法实测 56/59/73/12（真因 = truth_unresolved 排除开关，**非** tied_families_no_vote）+ 跨族去重推荐「计一次 + 逐族标注」（同 owner 09-15「子集重复放大」注记逻辑），并挑明 730 池内 ≥500 结构不可达、达标压力转移到扩池 |
| [0010](0010-nebula-a800-remote-execution.md) | Accepted，2026-09-21。星云平台远程执行形态：跑批走训练任务（`run mdl`）而非 Notebook（3 天页面不访问即回收，与无人值守长流程冲突）；代码一律走 git 模式（`--git_path` 内网 GitLab + `--commit_id` 钉 sha），**禁用默认的本地目录打包上传**——CLI 源码 `detectCodeSourceMode()` 兜底会把 cwd 打 zip，本仓根目录 4.9 GB，1 GB 闸门的 `--force` 会真推上去；数据走 OSS 挂载 `ai4sci-develop-storage` 的 `jingwu/` 前缀；所有提交经 `tools/nebula/neb` 统一入口，默认 dry-run 须 `--yes` 才提交（由来：2026-09-21 一次误提交实测到传完即提交、无二次确认）；每卡 15 核/240 GB 与 `pytorch280` 取自团队 9 个在跑 Notebook 的实测配比。遗留 6 条，其中「10 个工具源码在 github、生产网禁 clone」是跑批真卡点，「尚无一次成功真实提交」为当前状态 |
| [0012](0012-three-layer-dynamic-jev-agent-loop.md) | Accepted，2026-09-24。主线改为动态工具适应模块＋实时学习的动态 Jev＋Agent 可靠调用能力三层闭环；选择只是模块动作，复核/验证/拒答是主 Agent 核心能力 |
| [0011](0011-pretrained-discriminative-online-decision.md) | Accepted（研究方向），2026-09-23。用预训练判别式决策能力帮助冷启动，以真实反馈持续修正工具选择；Jev为功能参照、Laya为候选，Bandit不锁定唯一算法。具体更新方案、冷启动收益和原创性待验证；既有实验权限不变 |
| [0013](0013-open-source-first-composition.md) | Accepted，2026-09-24。先冻结功能合同与 benchmark，再按功能拼装已核许可证的开源组件；本项目只写 provider 适配、延迟反馈、轨迹日志和医药 evaluator 胶水 |
| [0014](0014-benchmark-provenance-and-external-role.md) | Accepted，2026-09-24。PTRB-v0 是基于 Task36 真实结果与回放环境拟构建的领域 benchmark 草案；tau²、Inspect、JevBench、OBP 和通用 routing benchmark 用于外部迁移/评估，不冒充主 benchmark |
| [0015](0015-public-benchmark-first-for-method-paper.md) | Superseded by 0021。曾采用 RouterBench 主 panel；后续由 Owner 收窄为医药直接工具选择主任务 |
| [0017](0017-deterministic-overlap-bound-for-online-gain-gate.md) | Accepted，在线增益门使用完整 mixture propensity、确定性 overlap Freedman 半径和 look/provider alpha spending；Nebula 只作环境 smoke |
| [0018](0018-context-conditioned-cell-jev-mvp.md) | Superseded by 0019；原方案是 4-cell cell-provider HT/Freedman 状态 |
| [0019](0019-exploration-only-context-jev-estimator.md) | Proposed，下一版只用随机探索样本更新 context-provider bounded mean，利用结果不污染估计，保留 exact propensity 审计 |
| [0020](0020-batch-frozen-exploration-gate-contract.md) | Proposed，修正 $[-1,1]$ gain 的 Hoeffding 半径、按 batch 冻结 gate，并把置信目标限定为固定 calibration 流 |
| [0021](0021-pharma-first-direct-selection-benchmark.md) | Accepted，2026-09-26。医药自建 benchmark 为主，当前任务收窄为单次直接 provider 选择；BBB 为 development，多端点 evaluation + frozen confirmation 后才形成最终证据；冻结现代强 baseline 与 matched-backbone 对照 |

0004 与 0005 已于 2026-09-13 补立，`docs/计划_全工具远程跑批与结果库.md` 的 Step 10 的文档部分由此完成，ADR-0003 文末修订节已改为指向 ADR-0004。**ADR-0005 的代码部分已于 2026-09-13 落地**：两个跑批控制层共四处旧分子级判据改为端点级（ADR 表格点名三处，落地核出 `execute()` 盖章闸这第四处），共享判据函数 `smoke_endpoints_without_values()` 落在 `drug_tool_adapters.py`，补两项 ADR 要求的测试与数值层对等测试并做变异验证，全仓 173 passed 1 skipped 无回归，两个封存正式 campaign 与四个第三批 campaign 的 verify 退出码全部不变，真实批次（CypReact 蔗糖）复现完成。逐行细节见 ADR-0005 的「当前代码状态」一节。CypReact 是否新建 campaign 重开仍归 owner。

本目录的编号只覆盖本项目，从 0001 起。上一版项目封存期另有 28 份 ADR，编号自成一套，与本目录不通，留在 `archive/governance/adr/`，git 未跟踪。

其中 0028 是个例外：它裁的是本项目的训练管线，不是上一版项目的。状态 Accepted，2026-09-02，未作废，把可训练主任务从「选哪个工具」改为「这组预测能不能拿去做决策」。它原先只存在于 git 未跟踪的 `archive/` 里，远程 clone 看不到，2026-09-13 已逐字复制进 [../inherited/](../inherited/README.md)，连同它引用的提案 v2、四份评审原文、方向7 调研与三条前置检验讨论。它与 `docs/Proposal.md` §3.7 及 `docs/目标.md` 的六步路线冲突，**已于 2026-09-13 由 [ADR-0006](0006-training-goal-decision-usability.md) 裁定**：维持 ADR-0028 D1，可训练主任务为「判断这组预测能不能用」，选工具偏心降为评测目标，`Proposal.md` §3.7 判为未吸收的回退（补吸收，不 Superseded）。冲突的完整登记与四件待裁事项见 `docs/目标.md` 的「一处待裁的目标冲突」。
| [0028](0028-matched-sgd-development-result.md) | Accepted，2026-09-27。普通 selected-only Online/Replay-SGD 只改善 Frozen warm start，未超过 Best Fixed，不作为动态 JEV 创新 |
| [0029](0029-provider-transfer-as-next-method-candidate.md) | Accepted，2026-09-27。provider 残差转移在五折 held-out 信息筛选中有稳定信号，作为下一方法候选；order-only drift 不作为前提 |
| [0030](0030-provider-transfer-v1-rejected-v2-retained.md) | Accepted，2026-09-27。低秩 provider-transfer replay 低于 Frozen；scalar transfer 有信号但未超过独立更新，只暂留一次安全门版本 |
| [0031](0031-provider-transfer-line-stopped.md) | Accepted，2026-09-27。gated provider-transfer 仍未超过 independent selected-only，停止该主线，转做有界 context residual memory |
| [0032](0032-context-memory-candidate-survives-screen.md) | Superseded by 0033。原始未校准 context residual memory 仅保留为历史候选筛选记录 |
| [0033](0033-context-memory-calibration-invalidates-v1.md) | Accepted，2026-09-27。BT 排序分数不能直接当 correctness 概率；校准后的 context memory 三个 delay 均未超过 independent，停止该主线 |
| [0034](0034-ag-jev-support-gate-stops-candidate.md) | Accepted，2026-09-27。AG-JEV 的随机审计置信门在 722×33 的现有样本上先验上无法打开，停止该候选 |
| [0035](0035-calibrated-provider-transfer-stopped.md) | Accepted，2026-09-27。校准 provider-transfer 优于置换关系但低于 Frozen，停止该线 |
| [0036](0036-round-snapshots-are-repeatability-evidence.md) | Accepted，2026-09-27。Task36 的 round1/round2 是重复性/执行漂移证据，不足以证明工具版本随时间更新 |
| [0037](0037-uniform-propensity-residual-jev-stopped.md) | Accepted，2026-09-27。统一随机探索的 propensity residual JEV 三种延迟均低于同探索对照，停止 |
| [0038](0038-margin-propensity-residual-stopped.md) | Accepted，2026-09-27。只在低 margin 题探索的 propensity residual JEV 仍低于同探索对照，停止 |
| [0039](0039-confidence-bin-residual-no-signal.md) | Accepted，2026-09-27。冻结 correctness score 的 provider×confidence-bin fit-only 校准没有信息优势，不做在线版本 |
| [0040](0040-keep-33-provider-menu-primary.md) | Accepted，2026-09-27。保留 33 个具体 provider 为主菜单，7-provider 家族代表菜单只作敏感性附录 |
| [0041](0041-pilot-adapted-online-baseline-negative.md) | Accepted，2026-09-27。PILOT-adapted 离线先验加 selected-only cosine/UCB/ridge 未形成稳定收益，保留为现代负 baseline |
| [0042](0042-postcall-confidence-is-verification-signal.md) | Accepted，2026-09-27。provider 返回后的 numeric confidence 对 correctness 有 held-out 信号，转入 fit-only verifier/refusal 候选 |
| [0043](0043-postcall-verifier-threshold-passes-cpu-gate.md) | Accepted，2026-09-27。fit-only 固定置信度阈值复核在五折 CPU 回放中超过随机拒答，进入 selected-only delayed online calibration 候选 |
| [0044](0044-confidence-gated-second-tool-verification.md) | Accepted，2026-09-28。confidence 门控的第二工具复核在相同调用成本对照下提高 pooled accuracy，进入动态验证流程候选 |
| [0045](0045-direct-dynamic-jev-information-boundary.md) | 部分被决议 0006 替代，2026-09-29。保留 selected-only 信息边界和负结果；MIRA 目标 claim 不再因该 ADR 降级 |
| [0047](0047-guide-v08-closure-target-contract.md) | Superseded by 0048；v08 目标和审查现场保留。 |
| [0048](0048-guide-v09-closure-contract-correction.md) | Accepted，2026-10-08。Guide v09 把 E1 headroom、E2/E3 主估计量、E4 逐 delay、多重比较、最终 sealed exposure、modern nearest 和候选 artifact checker 写成关闭硬条件。 |
| [0046](0046-dynamic-verifier-risk-gate-stopped.md) | Accepted，2026-09-28。延迟动态 verifier 风险门在 CPU 回放中准确率和调用成本均未过闸门，不上 A800、不调参 |
# ADR（历史与技术裁定）

当前研究故事线、方法论、Benchmark、Baseline 和三份评价标准以 [docs/研究主线/README.md](../研究主线/README.md) 为唯一入口。这里的 ADR 保留过去的技术裁定和实验闸门；它们只有在适用范围明确且没有被新的主线文档替代时，才约束具体执行。新的小范围双方共识统一写入 [docs/决议/](../决议/README.md)。
