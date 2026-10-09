# Task49：医药自建 benchmark 与 baseline 冻结审查

## 2026-10-08 扩展研究：条件第二次调用

当前生效研究口径仍见 [六份主线文档](../../docs/研究主线/README.md)；下方 9 月 26 日的计划和数字保留为历史，不能覆盖已完成的 four-provider fresh 实验或后续修正。

- 第二次调用 v1/v2 已在 722 个 development 分子上完成并独立复算；补充答案方向和拒答边界没有超过简单 cascade，不能据此提升在线复核或 agent 能力 claim。[v2 规范报告](conditional_second_call_v2/report.md) 保存全部条件、失败定位和原始证据入口。
- 旧 scaffold-block 重放改变逐分子到达机制；其百分位范围仅作重放敏感性，不视作原流的常规 95% CI。历史数字保留，解释已更正。
- 五篇部分反馈/延迟学习原文审查已落 [报告与基线合同](conditional_second_call_v1/partial_feedback_literature/report.md)。目前先按 [成本隔离实验卡](conditional_cost_isolation_v1/experiment_card.md) 核对确定成本与未知收益的训练边界，再决定提前状态机制。候选与标准原理不自动成为创新结论。
- 完整科学目标和 gate 仍未完成。主版 `paper/aamas2027/main.pdf` 的 8 页构建合规与这次扩展的科学效果分别验收。
- 成本隔离已完成并独立复算：原订单仅 +0.000194/题，128 种随机订单中 37 种胜原模型、7 种胜 cascade，未支持成本处理足以修复失败。[规范报告](conditional_cost_isolation_v1/report.md)。后续以已知成本外置为共同对照，再检验同模型即时/延迟动作变化标签的价值，不搜索更多静态基函数。
- [主稿统计口径审查](../../research/experiment_section_style_20261006/primary_uncertainty_scope_audit_20261008.md) 确认主表、机制消融及 post-call 均重采样固定结果，没有上述改变到达顺序的错误。但自适应策略的 scaffold 间共享学习历史，现有区间尚未证明新流上的总体覆盖率；下一步为现有强对照设计完整随机到达轨迹重放，补齐到达顺序的实际不确定性。它仍不是新 cohort 或未查看确认集。
- [到达顺序实验](primary_arrival_robustness_v1/report.md) 已完整运行并独立核验：128次共同均匀排列、全部当前对照、61,440条流、9,646,080次决策。预定受控MIRA−SGD均值+4.736pp，MC均值区间[+4.317,+5.156]，125/128正收益；自然MIRA−Frozen为−.287pp，区间[−.372,−.202]。只量化157个已查看分子的到达分布，不是新confirmation或自然漂移。原序复现、资源GO、首次分箱浮点失败及完整复算均留痕。
- [全分子错误分解](../../experiments/logs/task49_primary_arrival_robustness_20261008/error_diagnosis_v1/report.md)及独立复算解释自然pooled/等scaffold符号翻转：singleton合计净+2,034，多分子scaffold净−3,764；没有删除分子、换主指标或归因化学机制。全文已经8页编译、全9页视觉检查并提升固定main.pdf；完整科学gate继续HOLD。
- 条件调用另有[信息价值与终局规则一致性候选审查](conditional_second_call_v2/verification_value_review.md)：说明拟合后验不自动满足全期望关系，不能把信息价值与固定验证器的实际损失混为一谈。[独立审查](conditional_second_call_v2/verification_value_independent_review.md)确认推导成立，并指出A均匀mask与B的selector选择可能造成条件分布变化。实际根因尚未测定；不据此改变验证器。

## 原冻结审查（2026-09-26，历史正文）

日期：2026-09-26

状态：方向已冻结；数据发布版、最终确认集和 baseline 实现尚未完成。

## 2026-09-26 后续更正与实现

- 下文的“652 完整 33-output 面板”口径错误：652 是七家族投票完整且可评分；具体输出完整为 724，其中有参考标签 716。当前按 `data_contract_v0.md` 执行。
- 下文 0.8947 来自 `t1_bbb_characterization.py` 的 **七家族投票**，不是当前 33 个具体 provider。该旧脚本还按当题输出是否缺失屏蔽候选，best fixed 缺失时回退，使用了现任务选择前没有的可用性信息。因此它不能证明当前直接选择任务的线性可学习性高或低，只保留为历史背景。
- 三个 online kernel 及 selected-only 延迟反馈已实现、通过公式对照和 576 次真实缓存检查，见 [报告](online_adapter_v1/report.md)。完整强 baseline 尚待离线 fit 与正式实验。
- hERG 原始来源和 NCATS 数据已追溯到本地，见 [端点核对](endpoint_audit/report.md)。阈值及训练映射仍未完成，43 个现有连接仍属候选。

## 结论

主 benchmark 继续使用本项目已有的医药工具结果库，不再把 RouterBench、LLMRouterBench 或 ASlib 设为论文主任务。论文要回答的任务收窄为：给定一个分子、一个性质和一组候选预测模型，选择一个模型取得结果；终止后获得该次选择是否正确的二元反馈，并在后续分子上实时更新选择器。

这个决定与现有数据和方法目标一致，但当前 BBB 面板只能作为 development benchmark，不能单独承担最终 AAMAS 证据。722 个可评分分子是独立任务单位；33 个 provider 输出不是 33 倍独立样本，其中 24 个是同一 B3clf 家族的配置变体。完整 33-output 面板只有 652 个分子。后续任何题量、置信区间和显著性检验都按分子或预先定义的 scaffold group 计算，不能按 provider-output cell 伪增样本。

## 当前 BBB 面板是否有效

它有真实的选择空间。按七个来源家族统计，最佳固定 provider 为 ADMETlab，准确率为 0.9155；事后 oracle 为 0.9806，绝对 headroom 为 0.0651，47 个分子存在“最佳固定工具错误、其他工具正确”。在 652 个完整具体模型面板上，最佳模型错 45 个，其中 43 个可被其他模型事后救回，只有 2 个分子所有 33 个输出均错。

但“存在 oracle headroom”不等于“可以学会路由”。当前 Morgan fingerprint + per-provider logistic regression 的 5-fold scaffold CV 准确率为 0.8947，比训练折选出的 best fixed 低 0.0208；各 provider 正确性预测 AUC 约为 0.53–0.68。这个负结果说明当前任务不是靠一个普通线性 selector 就能轻易刷分，也说明 BBB 单端点的可学习信号可能不足。benchmark 有研究价值，但尚未证明足以支持最终方法主张。

审稿人还会追问两个更严重的问题。第一，BBB 真值和部分 provider 训练数据都可追溯到 BBBP/BBB_Martins，现有资格说明不能证明测试分子未被 provider 训练使用。第二，这 722 个分子已经反复用于方法开发，不再是未触碰确认集。因此当前面板适合设计、调试、消融和预注册前的诊断，不适合在论文中称为独立最终测试。

## 主 benchmark 的冻结形态

主 benchmark 是我们定义的医药在线选择协议，不是把缓存表换个名字当新数据集。最终发布至少包含以下三层。

| 层 | 用途 | 冻结要求 |
| --- | --- | --- |
| BBB-development | 方法开发、接口检查、消融 | 使用现有 722 个可评分分子；scaffold split；明确已被反复查看，不报告为最终确认 |
| Multi-endpoint evaluation | 主表、跨性质稳定性 | 从现有工具库接入至少 BBB、hERG、AMES 三个有实验真值的端点；每端点独立做真值清洗、provider 资格与 scaffold split；若 hERG/AMES 真值未接通，不得用工具共识代替 |
| Frozen confirmation | 最终确认 | 从未用于特征、超参、方法和停止规则选择；优先使用池外、时间更晚且可审计的分子；只在方法和 baseline 冻结后打开一次 |

每个 episode 的合法输入为分子结构、性质标识、候选 provider 的公开卡片和此前已释放的反馈。动作是选择一个 provider。环境只返回所选 provider 的缓存输出；该 episode 终止并经过判分后，学习器才收到 `correct ∈ {0,1}` 或明确的 missing/unscorable 状态。评估器持有完整输出矩阵和真值，选择器不能读取未调用 provider 的当题输出。

主实验报告累计错误、分段准确率、相对训练折 best-fixed 的 gain、oracle-gap closed、适应速度、更新延迟、单步选择时延和峰值内存。随机种子共享同一预定义到达顺序集合；所有方法使用同样的初始数据、反馈延迟、反馈数量、候选池和超参预算。准确率之外必须报告逐样本选择与更新日志。

动态性分三种，不能混称为真实漂移：随机顺序只检验 stationary online learning；按 scaffold/性质组成构造的顺序只检验 controlled shift；只有按工具版本或真实测量时间排序的数据才能称为 temporal/version shift。

## Baseline 冻结

主表需要回答两个问题：预训练判别表示是否改善冷启动，以及我们的实时更新是否超过已有在线方法。因此 baseline 分为四组。

### 必须运行的主 baseline

| Baseline | 作用 | 适配边界 |
| --- | --- | --- |
| Best fixed provider | 最强无上下文策略 | 只能用训练折选择，不能看测试折 |
| Full-information oracle | 任务上界 | 只作上界，不称为可部署方法 |
| Greedy LinUCB，AAAI 2026 Online Multi-LLM Selection | 无离线判别先验的现代在线方法 | 使用单步算法；删去原论文多轮 prompt evolution，保留同一 selected-only feedback |
| PILOT，Findings EMNLP 2025 | 离线 query/provider 先验 + selected-arm 在线 ridge 更新的最近邻 | 按论文公式复现；没有确认到官方代码，必须记录偏离项 |
| BayesianRouter，ICLR 2026 | 离线判别 router 形成先验 + 在线 Thompson posterior 的最近邻 | 使用同一 feature、初始数据和反馈流；不得使用额外测试统计 |

Random 和静态最佳仍保留作 sanity control，但不能作为证明先进性的主要对手。

### 静态强路由对照

RouterDC（NeurIPS 2024）和 GraphRouter（ICLR 2025）用于检验“一个强离线 router 是否已经足够”。两者使用完整训练分区的 provider outcome 监督，只在测试时冻结。RouterDC 有作者仓库但没有仓库级许可证，当前只按论文复现，不能直接复制其源码进入本仓；GraphRouter 为 MIT，可优先通过官方实现或 LLMRouter 适配。LLMRouter 仅作为集成框架，使用前需核对其实现与原论文公式、默认超参和数据权限是否一致。

### 同 backbone 机制对照

这组对照与 Ours 共享完全相同的 backbone、输入和初始化，隔离在线训练方法本身：

1. Frozen JEV：只用冷启动模型，不更新。
2. Online SGD：每次合法反馈后对同一小型头做一次普通梯度更新。
3. Replay SGD：固定大小 replay buffer + 相同更新预算。
4. Proposed online update：只替换为论文候选的实时更新方法。

若 Proposed 只优于 RouterDC/GraphRouter，却不优于 matched-backbone Online SGD 或 PILOT/BayesianRouter，不能声称实时学习方法本身成立。若 Frozen JEV 不优于无预训练初始化，不能保留“JEV 解决冷启动”的主张。

### 不进入当前主表

RouteLLM 主要面向 strong/weak 两模型路由，只在最终池确实缩到两个 provider 时加入。Cascade Routing 解决追加调用和停止问题，与当前单次直接选择任务不同，移到未来验证/复核扩展。ParetoBandit 是 2026 预印本，可在真实非平稳协议完成后作为 drift baseline；在当前 stationary BBB replay 上不作为必跑项。ASlib、RouterBench 和 LLMRouterBench 可用于附录 sanity 或接口迁移，不替代医药主 benchmark。

## 审稿通过条件

作为 AAMAS 审稿人，我会在以下条件同时满足时接受“自建 benchmark 有效”这一说法：任务来自真实工具输出和独立实验真值；样本单位与重复输出分清；训练、开发和最终确认严格隔离；工具训练数据重叠已审计或明确限制主张；至少三个端点展示相同机制；强静态 router、强在线 bandit、离线先验 + 在线更新近邻和 matched-backbone 更新均在同权限下比较；逐样本日志、版本、失败和评分代码可复现。

任一以下情况都会构成明显拒稿理由：把 33×730 当独立样本；只在反复开发过的 BBB 上报最终结果；用工具共识代替 hERG/AMES 实验真值；只和 random、best fixed 或几十年前的 bandit 比；给 Ours 更多离线标签、反馈或调参预算；把人为重排称为真实时间漂移；看到完整 provider 矩阵后再更新当前 episode；用 oracle headroom 代替可学习性证据。

## 执行顺序

1. 冻结 v0 数据合同：独立分子数、端点、provider 家族、可见字段、反馈时钟和评分。
2. 完成 hERG/AMES 真值连接与 provider 训练重叠审计；失败则把论文范围明确缩到 BBB development study，暂不提交完整方法主张。
3. 实现统一 adapter，先跑 Best fixed、Greedy LinUCB、matched-backbone Online SGD。
4. 复现 PILOT 和 BayesianRouter；RouterDC/GraphRouter 至少各保留一个静态强对照。
5. 全部 baseline 通过信息权限审计后冻结超参，再打开最终确认集一次。

## 证据

- 本仓 BBB 统计：`data/tool_results/divergence/t1_bbb_characterization.json`
- 具体模型粒度审计：`data/tool_results/divergence/pharma_tool_granularity_audit_v2.json`
- provider 资格边界：`task/task36-pharma-provider-replay/qualification_notes.json`
- Task36 信息隔离接口：`src/probe_headless/pharma_bbb_replay.py`、`src/probe_headless/pharma_bbb_judge.py`
- 外部论文、官方仓库与许可证见同目录 `sources.jsonl`。

## 2026-09-26 逐行审计后的更正与推进

上文完整面板的旧数字需更正：33 个具体输出完整的行是 724，其中有参考标签 716；652 是七家族投票齐全且可评分的行。64 个额外完整行被旧家族投票的平票规则遮掉，不能据此删除具体工具选择样本。旧家族准确率和 oracle 数字不自动迁移到 716 行面板。

数据合同及独立复核已完成：35 个来源哈希、2,190 个端点真值连接、29,200 个 provider cell 通过检查。hERG/AMES 严格参考连接为 43/129，尚未满足正式多端点资格；池外真值候选已列入合同，不能直接当作最终确认数据。证据见 `data_contract_v0.md` 和 `audit_validation/run_v1/summary.json`。

baseline 合同 v0 已写明 PILOT 的 cosine/UCB、BayesianRouter 默认脚本的先验/选臂偏离、GraphRouter 的监督边与平票问题。所有方法共享初始化权限与反馈规则，动作不同允许反馈记录不同。静态强 router 按 ADR-0021 至少一个执行；上文“至少各一个”不增加必做项。资格不足时回到范围决策，不自行取消多端点和 confirmation 要求。

方法卡给出了固定写入/可学读取构造的凸组合边界与反例，不把它升为主算法。下一步先验证固定表示是否真的限制学习，避免给已有 ridge/SGD 取新名字。

真实 tokenizer 审计发现 BBB 默认菜单中 31/33 工具 id 被截断，24 个 B3clf 变体文本相同；1024/512 配置对 730 个分子的输入全部完整。报告见 `laya_input_audit/report.md`。无标签表示提取 pilot、输入包和 A800 脚本已准备，输入合同 4 项测试通过；尚未执行模型推理或训练，OSS 访问仍待打通。

## 2026-09-26 指纹选择诊断已完成

33-provider 指纹诊断已经在 A800 上完成并通过独立恢复。结果没有达到继续条件：train-only Best Fixed 为 `0.9155`，MLP 为 `0.8961/0.8906`，BT+CLS 为 `0.8878/0.8947`，线性头为 `0.8726/0.8795`。因此当前不能把冻结指纹或普通离线 router 作为动态 JEV 的有效 backbone。

逐分子分解显示，fixed 错误中有 59/61 个存在可救 provider；MLP 每个 seed 只救回 20/24 个，却损害 34/42 个 fixed 正确选择。下一步先做保守的 calibrated switch 诊断，再接入 selected-only 在线流；若普通切换已能解释信号，不新增算法。完整核验和边界见 [fingerprint diagnostic report](fingerprint_diagnostic_v1/report.md)。

calibrated switch 已完成：MLP seed 0/1 的 outer accuracy 为 `0.8947/0.9100`，Best Fixed 为 `0.9155`；10 个 outer fold 没有一个超过 fixed。它不能解释为简单的 argmax 过于激进，后续转向真实 Laya 表示和 selected-only 在线比较。

## 2026-10-08 B-only 融合诊断

在既有条件第二调用开发证据之后，冻结并执行了 `conditional_second_call_v2/fusion_diagnostic_card_v1.md`。完整结果见 [`fusion_diagnostic_report_v3.md`](conditional_second_call_v2/fusion_diagnostic_report_v3.md)，原始回执与独立核验见 `experiments/logs/task49_conditional_fusion_diagnostic_20261008/real_v6/`。

分支2机械触发：固定 `K=(provider0, provider3, 首动作拒答)` 在 fold0/1/3 的 S 实际首选面板和 U evaluator 展示面板中，风险差与 Brier 差均为正，跨折去重 entity=63。这个结果只安排下一项单因素终局融合干预，不证明 posterior 不一致、不构成新方法效果，也不进入主稿。下一张候选卡为 `conditional_fusion_intervention_v1/experiment_card.md`，目前未执行。

## 2026-10-08 CGEF 固定采集终局融合干预

候选卡 `conditional_fusion_intervention_v1/experiment_card_v2.md` 已按修订后的固定合同执行。实验只改变终局融合规则：同一首工具、同一已取得的第二工具 pair、同一动作阈值、同一 post-paid 成本，比较 `ignore-second`、原 masked-logistic 和共享协方差 Gaussian marginal 的 CGEF。A fit 只读取四个 native probability 与 truth；B 只作评价；没有读取 C、confirmation、provider/API 或 GPU。

原始结果在 `experiments/logs/task49_conditional_fusion_intervention_20261008/cgef_v1/`，规范报告在 `conditional_fusion_intervention_v1/report.md`。1,518 条 B 行和 506 个 entity 的 pooled paired utility 差为：CGEF−原 masked-logistic `+0.000000`（数值误差约 `8.8e-19`），CGEF−ignore-second `−0.000659`。仅 fold 3 相对原规则超过卡内 `+0.01` 线；相对 ignore 没有 fold 通过，故候选停止，不做 CGEF 闭环或 Gaussian 参数搜索。

独立 verifier `src/probe_headless/verify_cgef_intervention_v1.py` 不导入 runner，重算五折 A 参数、1,518 条 B row、506 个 entity 和六行聚合；参数与逐行重算的最大绝对差均为 `0`。核验 JSON SHA 为 `e77d35e07a7dcd8197dcbbf60d383d44205530631c85a86bd17c5d9fe5957eb3`。负结果保留，不进入主版论文，也不改变生效方法。它只排除当前 Gaussian 终局融合假设，下一步回到同一 gain/value 模型下即时/延迟反馈和预算相同的闭环 acquisition 设计。

## 2026-10-08 投稿范围 baseline receipt join

依据缺口审查，已完成只读的 `baseline_scope_lock_and_receipt_join_v1`。它没有新增实验，而是把当前材料按投稿范围重新接起来：主矩阵固定为四个 checkpoint-bound fresh providers、157 个 scaffold-disjoint BBB 分子、一次调用、selected-only、delay `{0,1,4}`；33-provider/722 分子合同保留为 development scope。

机器矩阵和 59 个 raw/recovery 证据路径的 hash 检查在 `task/task49-pharma-benchmark-baseline-freeze/baseline_scope_lock_and_receipt_join_v1/`。主行包括 Random、Best Fixed、Frozen、Matched Online-SGD、Greedy LinUCB、BayesianRouter、MIRA-Relay、Retrospective Best Fixed 和 MIRA-Relative ablation；Replay-SGD、PILOT、Gaussian stratum、GraphRouter、Laya-online 分开列为 separate/audit-only。没有跨 likelihood raw ranking，也没有把 evaluator-only control 当成 deployable baseline。

状态收敛为：旧 `current_acceptance_audit_v2` 的 A/B/C=`11/16,11/16,10/16 HOLD` 仍代表广义研究范围；`stage_b_scientific_submission_gate_v3` 的 A/B/C=`15/16,13/16,14/16 PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE` 代表当前窄投稿切片。两者 scope 不同，不能互相覆盖。该 join 提高了 Benchmark/Baseline 的可审查性，但不关闭 hERG/AMES、自然 lifecycle、完整 33-provider model-fresh、post-call refusal 或 agent 闭环缺口；科学 gate 仍保持 HOLD。

## 2026-10-09 主稿 claim-scope 审计

为防止窄范围证据被写成 broad 结论，完成了只读 `paper_claim_scope_audit_v1`。审计覆盖摘要、引言、问题定义、方法、实验合同、结果、限制、结论和图表相关文字；对照已锁定的 `baseline_scope_lock_and_receipt_join_v1/matrix.json`。

- 所有必要范围锚点均存在：公开 TDC BBB、四个 checkpoint-bound fresh providers、157 个 scaffold-disjoint confirmation molecules、one-call、selected-only、$D\in\{0,1,4\}$，以及 natural/controlled correctness-drift 分离。
- 主稿明确把 33-provider pool 限为 development evidence，把不兼容的近期 router 限为 audit-only；没有把它们写成 model-fresh 或 broad leaderboard 结果。
- 自然结果明确是 null，不支持 generic online improvement；受控 label flip 明确是 evaluator-side，不建立 natural provider nonstationarity；arrival 结果明确不建立 new-scaffold/deployment generalization。
- post-call refusal、hERG、AMES、lifecycle 和 integrated multi-call agent evaluation 均被标记为 primary scope 之外的扩展或未测范围。

机器回执、源文件与矩阵 hash、独立核验位于 `task/task49-pharma-benchmark-baseline-freeze/paper_claim_scope_audit_v1/`；独立 verifier 状态为 `verified_independently`。没有发现需要修改主稿的越界正向 claim，因此 `paper/aamas2027/main.tex`、主版 PDF 和其 SHA 未改变。该审计不关闭 broad A/B/C gate；窄 scope 仍为 15/16、13/16、14/16，broad scope 仍为 11/16、11/16、10/16 HOLD。
