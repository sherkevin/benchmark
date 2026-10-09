### 2026-09-22 10:23 CST — 启动 task31 Benchmark/Baseline Freeze Review
- 状态：⚡️进行中。
- 用户目标：deep research 调研合适 benchmark 与 baseline，不停留在讨论层。
- 开工前状态：已读取 TODO_LOG 与 task30 report。task30 已完成主要 nearest baseline closure：ACTS、RFF-UCB、P-LinUCB/P-TS、ParetoBandit-style 已覆盖主要方法族。
- 本轮工作：创建 task31；建立 benchmark selection review 与 baseline selection review 文档。
- 关键结论：当前需要冻结 evaluation protocol，而不是无限扩展实验。候选 benchmark 初步收敛为 LLMRouterBench + ASlib SAT12-ALL + BBB，Synthetic 作为 mechanism validation appendix。
- 结果文档：`task/task31-AAMAS2027-benchmark-baseline-freeze/{task,report}.md`；`research/AAMAS2027_Benchmark_Selection_Review.md`；`research/AAMAS2027_Baseline_Selection_Review.md`。
- 未完成：需要继续补充外部文献核验，并最终确定 main/appendix benchmark 与 baseline split。
- 下一步：完成 freeze review 后，再基于冻结 gap 设计 Ours。


### 2026-09-22 — task31 external verification continuation
- 状态：⚡️进行中。
- 用户目标：继续使用 Remote Desktop Commander 进行真实项目推进，不依赖聊天上下文。
- 本轮工作：重新连接 macOS benchmark 环境；读取 TODO_LOG、task31 report、benchmark/baseline review；完成第一轮外部文献核验。
- 关键结论：当前 benchmark/baseline 冻结方向保持不变。LLMRouterBench 用于现代 routing 场景，ASlib SAT12-ALL 用于经典 algorithm selection 场景，BBB 保留作为 challenger/tool routing stress test；当前仍不进入 Ours 设计。
- 结果文档：`task/task31-AAMAS2027-benchmark-baseline-freeze/report.md`。
- 数据/代码：无新增实验代码。
- 未完成：精确冻结 split、metric、main/appendix baseline。
- 下一步：完成 protocol freeze 文档后，再进入 failure mode 总结。

### 2026-09-22 10:xx CST — 继续 task31 Evaluation Protocol Freeze
- 状态：⚡️进行中。
- 用户目标：恢复 Remote Desktop Commander 后继续真实开发，不停留在聊天讨论。
- 本轮工作：重新连接 U-CGPCVWR0-0024.local；读取项目交接文档；检查 task31 当前状态；创建 evaluation protocol 草案。
- 关键结论：在设计 Ours 前，需要将不同 benchmark 统一映射到 selective challenger onboarding 数学定义：context x、incumbent/challenger、reward r(x,a)、advantage Δ(x)。
- 结果文档：`research/AAMAS2027_Evaluation_Protocol.md`。
- 数据/代码：无新增实验代码。
- 未完成：需要继续冻结 benchmark split、baseline main/appendix、metric protocol，并补充 failure mode analysis。
- 下一步：完成 baseline freeze 后进入 Ours 问题定义。


### 2026-09-22 — task31 protocol freeze continuation
- 状态：⚡️进行中。
- 本轮工作：继续完善 `research/AAMAS2027_Evaluation_Protocol.md`。
- 完成内容：补充 split protocol、challenger arrival simulation、baseline freeze、failure mode analysis。
- 关键决策：评价重点从静态 benchmark 排名转向 selective challenger onboarding，强调局部优势发现能力。
- 未完成：需要继续核验 benchmark 数据实际可获得性，并形成 final freeze report。
- 下一步：整理 task31 final freeze report，随后进入 Ours 问题定义。

### 2026-09-22 — task31 final freeze report draft
- 状态：⚡️进行中。
- 用户目标：继续真实开发，完成 benchmark/baseline freeze，不进入 Ours 设计。
- 本轮工作：读取 task31 report 与 baseline review；创建 `task/task31-AAMAS2027-benchmark-baseline-freeze/final_freeze_report.md`。
- 关键结论：evaluation protocol 已统一为 selective challenger onboarding；benchmark 与 baseline 候选已基本冻结。
- 结果文档：`task/task31-AAMAS2027-benchmark-baseline-freeze/final_freeze_report.md`。
- 数据/代码：无新增实验代码。
- 未完成：需要进一步完成参数/split freeze，并开展 frozen baseline failure mode analysis。
- 下一步：基于 frozen baseline 的失败模式定义 Ours 问题边界。

### 2026-09-22 — task31 failure mode analysis start
- 状态：⚡️进行中。
- 用户目标：继续使用 Remote Desktop Commander 持续推进，不停留在聊天讨论。
- 本轮工作：读取 final freeze report；创建 `task/task31-AAMAS2027-benchmark-baseline-freeze/failure_mode_analysis.md`。
- 关键结论：冻结后的核心 gap 不是 challenger advantage 定义本身，而是有限反馈下 selective positive-region discovery 与传统 reward maximization 的目标差异。
- 结果文档：`task/task31-AAMAS2027-benchmark-baseline-freeze/failure_mode_analysis.md`。
- 未完成：继续从 failure mode 推导 Ours 的严格问题定义。
- 下一步：分析已有方法假设边界，形成 Ours formulation。

### 2026-09-22 — task31 transition from failure analysis to Ours formulation
- 状态：⚡️进行中。
- 用户目标：继续真实开发，不停留在聊天汇报。
- 本轮工作：读取 failure_mode_analysis；基于 frozen baseline gap 创建 `research/AAMAS2027_Ours_Problem_Formulation.md`。
- 关键结论：当前研究问题初步定义为 selective challenger region discovery，而非传统 cumulative reward optimization。
- 结果文档：`research/AAMAS2027_Ours_Problem_Formulation.md`。
- 未完成：需要继续比较 contextual bandit、treatment effect estimation、selective classification 等邻近领域，确定最小创新假设。
- 下一步：完成 Ours formulation refinement 后再设计算法。

### 2026-09-22 — Task32/34 间接历史泄漏修复与独立重放
- 状态：本轮隔离修复、固定配置诊断、定向回归和结果落盘完成；Task32/34 整体研究任务仍进行中，未进入 Ours。
- 用户目标：持续使用 RDC 在 macOS benchmark 仓库真实开发，不停留在聊天进展汇报。
- 本轮工作：核对磁盘、代码和结果；发现真实 challenger 经历史 pseudo-task incumbent 间接进入 H1/H2 与 auxiliary calibration；独立实现双重排除、重跑两条诊断、补 poisoning/阳性对照/成本记账测试；更正 estimand 与方法族否定的过强叙事。
- 关键结论：原 r1 结论不能继续直接引用，使用 Task32 r2 serial 和 Task34 isolated；指定构造无稳定正增益不等于整个研究方向不成立，BBB 小预算正例保留。15 条定向测试通过，两条诊断分别独立重放逐字节相同。
- 结果文档：`task/task32-AAMAS2027-problem-formulation/{report,history_isolation_audit,history_isolation_results,estimand_audit}.md`；`task/task34-T3-modern-safe-calibration-closure/{report,isolated_results}.md`；相关 research 与 Task31 failure analysis 已追加更正。
- 数据/代码：`src/probe_headless/t32_strict_history_{pack,audit}.py`、`t32_verify_history_isolation.py`、`t34_meta_calibrated_gate_isolated.py`、`t34_verify_isolated_calibration.py`；`data/tool_results/divergence/t32_history_isolation_r2_serial*.json`、`t34_meta_calibrated_gate_trainonly_isolated*.json` 及两份 verification.json；原文件与首次复现失败证据保留。
- 未完成：可学习结构与 baseline estimation error 尚未闭合；Task34 当前 pooled-quantile 仅启发式，不能冒充正式 conformal fidelity/风险保证。未运行全仓测试，未提交或推送，未改动用户大量历史删除现场。
- 下一步：以最新 report 为入口核清可学习相对均值、校准的 in-sample/out-of-sample 分布差；任何 cross-fitting/新实验协议先单独登记，不改 Task31，不覆盖失败结果找赢家。
- 收尾验签：`data/tool_results/divergence/t32_t34_isolation_completion.json` 为 PASS；7 个新 Python 文件编译、原始文件 seals、源代码 hash、两条独立重放均通过，scoped `git diff --check` 通过。
- 数值复核：Task34 ASlib 三格约 +3.6e-08～+3.8e-08 的 post-gate 正值已原样保留并提升表格精度；对应 total IOU 仍负，未把舍入零当作非正结论。

### 2026-09-22 — 首轮故事线讨论与星云只读体检

- 状态：证据复核与候选比较完成，研究主张待 owner 拍板；本轮未启动新实验。
- 用户授权：按故事线、创新点、方法、小批真实数据验证、综合结果的顺序持续推进；关键问题先多角度比较，由 owner 决定。星云使用遵循用户指定的 nebula-support：只读检查可执行，提交命令仅准备给用户执行。
- 已核证据：直接复算 Task32 串行修复版及重复版逐字节一致、五个来源哈希匹配；核对 Task34 isolated 结果，三个环境各十二个校准配置的总接入收益均负。两项完成的是隔离修复和固定构造诊断，不是新方法成立。
- 主推荐（未裁定）：论文先围绕有限试用下提高新工具接入净收益，不预设全程不退化的安全保证。先诊断旧策略估值误差是否为主要瓶颈，试用信息价值暂不独立立为贡献；历史误差的交叉拟合用于消除校准偏差，不能单独当创新。
- 星云现场：tools/nebula/neb doctor 检出 CLI 0.1.3、登录正常、pytorch280 在允许列表、当前 HEAD 已推送；因 NEBULA_OSS_ACCESS_ID/NEBULA_OSS_ACCESS_KEY 缺失返回 1。queues 当次返回 A800 空闲 84、Backlog 0，仅为查询时快照。未读取或打印凭据值，未改配置，未提交训练。
- 交付边界：抽查 t32_strict_history_audit.py、t32_strict_history_pack.py 与 tools/nebula/neb 均尚未跟踪；HEAD 已推送不意味着这些新文件已可被远端取得。
- 证据入口：task/task32-AAMAS2027-problem-formulation/history_isolation_results.md、estimand_audit.md；task/task34-T3-modern-safe-calibration-closure/report.md；data/tool_results/divergence/t34_isolated_calibration_verification.json。
- 下一步：请 owner 裁定净收益主线与严格损失约束主线的取舍；再登记首批诊断的唯一假设、对照、判据与否定条件。平台凭据、实验代码版本和提交预算在真实提交前解决，不阻塞当前讨论。

### 2026-09-22 — Owner 选择净收益主线，登记 Task35 首批诊断

- 裁定：Owner 通过方案选择明确选择「净收益优先（推荐）」。试用损失计入总收益，同报伤害，不预设严格风险保证；已同步 research/AAMAS2027_Ours_Problem_Formulation.md 文末，不改 Task31 或旧 Accepted ADR。
- 事前登记：task/task35-net-gain-diagnostic/experiment_card.json。三套既有嵌套训练区、全部挑战工具、B=4/32、stream seeds=0/1/2、Ridge alpha=1。比较少反馈预测与充分历史参照，并分别替换旧/新工具估值为已实现回报做额外信息诊断。
- 信息边界：只有 fewshot_estimated 为选中反馈可用的试用策略；充分 challenger 历史和未选动作真实回报都只进具名诊断参照，不能冒充零历史方法或随机环境均值 oracle。LLM 汇总同时留 dataset-macro 与 pooled；试用成本全部计入。
- 实现：新增 src/probe_headless/t35_net_gain_diagnostic.py；独立测试与运行前可解释性审查进行中。环境使用 .srcprobe/beir_venv（numpy 1.26.4、sklearn 1.6.1），未安装依赖。
- 运行边界：单批本地 CPU，不调用模型 API、不运行科学预测工具、不提交星云任务、不扫参数；通过审查与单测后才运行。输出独立命名，不覆盖旧结果；真实结果出来后先讨论再决定下一批。

### 2026-09-22 — Task35 首批完成并逐样本复核

- 状态：该批诊断与复现验收完成，不等于方法成立；未启动下一批。
- 运行前验收：独立科学审查 Go，仅限探索性诊断；补齐 LLM 的宏平均试用/后续分解和十五数据集完整性校验。Task35 新测试加 Task32/34 隔离回归合计 64 passed，范围不是全仓测试。
- 真实运行：本地 CPU、B=4/32、三种顺序，LLM 120、ASlib 186、BBB 42 个 episode，合计 348 个 episode、2436 条策略记录。外部预测调用及星云提交均为零。
- 产物：data/tool_results/divergence/t35_net_gain_diagnostic_v1.json 及 _repeat.json；两个进程输出 sha256 均为 0a4d27c5177f3da1302c54b8aa21da521d8d10eece2043f907b71c11b130ba8f。task/task35-net-gain-diagnostic/verification.json 为 PASS：来源与实验卡哈希匹配，独立逐样本记账/宏平均复算最大绝对差 5.55e-17。
- 核心结果：B=4 的 fewshot_estimated 净收益分别为 LLM -0.029561、ASlib -0.016276、BBB -0.025445；给足 challenger 历史的当前 Ridge1 参照仍未转正。旧回报实值替换和新回报实值替换均能改善，不支持把旧策略估值误差认作唯一瓶颈。
- 成本边界：ASlib 固定前 32 次强制试用平均贡献 -0.085667，之后使用已实现回报逐样本最佳选择仍只贡献 +0.009474，总净收益 -0.076193。此结论只约束当前内层输入流与固定试用规则，不约束可提前停止或选择试用时机的方法。
- 结果解释已同步 research/AAMAS2027_Ours_Problem_Formulation.md 的 Task35 节；Oracle 与充分历史参照明确标为额外信息诊断，不作零历史、随机均值或正式确认性主张。
- 下一步候选（待 owner 拍板）：优先检验何时继续试用、何时停止投入，估值/表示改进保留作必要对照；现成方法能够解释全部收益时不另起 Ours。尚无新方法贡献，不申请 GPU。所有改动未提交、未推送。

### 2026-09-22 — Owner 纠回医药双层 Agent 主线

- 当前任务：Owner 明确聚焦医药同功能工具选择，要求先分析本地真实工具错误，再训练主 Agent 与工具管理模块；指定 `/Users/jingwu/Desktop/科学工具可靠性_Agent阶段汇报_v7_20260918.md` 为意图依据，已全文阅读。
- 两层分工：主 LLM 真实 SFT/RL 学请求功能、复核、作答和弃答；同功能 Bandit 学当前输入选择哪个具体工具。新工具冷启动只是泛化支线。不能把 RL 降为可选附录，不能用 ASlib 固定试用损失推动主线。
- 已纠正文档：research/AAMAS2027_Ours_Problem_Formulation.md 与 docs/AAMAS2027_论文主线与执行路线_DynamicToolRouting.md 顶部已加范围更正；旧研究正文和 Task35 原结果保留，不自动改写 Accepted ADR。此前「净收益优先」不代表 Owner 认可助手擅自加入的通用新工具接入场景。
- 本轮仅做医药缓存审计与方法比较，未启动训练或新预测。

### 2026-09-22 — BBB 工具粒度审计揭示被家族投票掩盖的正确输出

- 同一批 652 个完整样本上，7 家族投票的最佳来源答对 607 个；45 个错误中，31 个有其他家族票正确，14 个所有家族票都错。
- 按性质配对表展开为 33 个具体模型/版本输出后，原 14 个家族全错样本中 12 个存在正确模型输出，只剩 z012532、z013208 在本次 33 个输出中全部错误。最佳固定模型仍答对 607 个，其 45 个错例中 43 个存在其他模型正确输出。该上界使用事后真值，不是已学到的策略效果。
- 例子：z002905 的真值为 1，DeepPurpose CNN/MPNN 都输出 0，Morgan 输出 1；家族多数票仍为 0。DeepPurpose-Morgan 总体答对 584/652，但在 ADMETlab 的 45 个错例中答对 34 个，说明总体排名和错误互补性不能混用。
- 产物：src/probe_headless/pharma_tool_granularity_audit.py；data/tool_results/divergence/pharma_tool_granularity_audit_v2.json、_v2_repeat.json、pharma_tool_granularity_audit_verification.json。两进程逐字节一致，sha256=ad23ff70a117a551e575a13377b7c9a9012c96147ef1eee247fec038f55245fe；652 行家族票全部由原始输出重建一致。
- 修正边界：v1 曾按列名大小写拼 B3clf ID，错误低估注册表匹配数；v2 改读 --clf/--sampling 参数映射，原始标签逐项不变，31 个模型/版本匹配 BBB 注册条目。登记不等于运行时仍可用，另两项有缓存而不在该 BBB 注册表；33 个输出也不是 33 个独立证据来源。
- 训练接口实况：task25 的 tools.json 嵌入家族票，不是逐 provider 回放；smoke_harness_wrapper.py 仍固定材料题，bbb_harness.py 在报告中是待建入口。可复用预算、账本与视图思路，但尚不能称已接成 BBB 的 function-key→Bandit→provider 训练环境。预测缓存、真值与 Agent 可见目录必须分离。
- 多角度候选：基础双层系统必须作对照；复核时把已调用工具/输出纳入 Bandit 是一个待验机制，不能把不同意见自动当正确意见；分支式 SFT 轨迹构造是另一候选，本轮均未宣称新颖性成立。
- 下一步：先对齐 Bandit 动作是具体模型/版本还是家族集成，并检查资格与训练数据污染；不把原 14 个样本直接标成具体模型池的必弃答样本。随后落实医药缓存回放与基础训练对照，方法由 Owner 拍板。未修改桌面汇报、旧真值或预测表，未提交/推送，未运行 GPU。

### 2026-09-22 — Owner 选定具体模型粒度，Task36 回放接口验收

- 裁定：Owner 选择「具体模型/版本（推荐）」；家族保留作来源关系标记，不先合成多数票。33 个现存输出是候选，不因此全部取得正式训练资格。
- 本轮结果：新增 src/probe_headless/pharma_bbb_replay.py 与 pharma_bbb_judge.py。前者从原始 CSV 返回所选 provider 的原文值与按既有阈值/词汇表得到的标签，不读实验真值；后者只在整题 answer/abstain 后给已调用工具和最终答案的正确性反馈。支持功能请求、选择器接口、明确预算/候选池/快照轮次、失败记账与终止。可选标量奖励要求显式传入全部系数，不预设奖励表、不双扣调用成本。
- 验收：103 条定向测试通过，其中本回放 88 条、Task32/34 相邻隔离回归 15 条，非全仓。独立验证逐项走实际 call 接口比对 47,450 个快照格，包含 288 个失败格；两个进程输出逐字节一致，sha256=23da42eec5c9ca69e0068a20bb7ac2c746717151ec2647fc2c995ad6d65ecd5e，输入及实现哈希匹配。
- 审计发现：32/33 模型有两轮快照；B3clf 16 个模型在两轮间共翻转 38 个模型×分子标签，涉及 27 分子。24 个 B3clf 各缺同样 6 个分子，每轮共 144 空值；不补值、不平均、不挑正确轮次。ADMETlab 只有一轮缓存。
- 资格边界：DeepPurpose 的文档称自训练，但worker实际加载已锁定预训练权重，所查材料未给训练成员；不能据同源/高分/no_split 推断确切训练重叠。graphB3/LGBM版本锁也不能证明训练无重叠；LGBM有既有来源等级限制。正式入训清单仍待按证据冻结。
- 产物：task/task36-pharma-provider-replay/{task.json,report.json,qualification_notes.json,verify_replay.py,verification.json,verification_repeat.json}；verification.json 内 cache_audit 是最终代码下的缓存审计，早期 cache_audit.json 为优化前快照，不覆盖。
- 接线例：人为固定调用 z002905 的 DeepPurpose CNN（0.0030845485161989927，标签0）和 Morgan（0.977143406867981，标签1），提交后才给CNN错/Morgan对反馈。此例证明接口，不是训练效果，也不是靠较高概率挑正确工具的算法。
- 未完成：模型、SFT/RL配方、最终工具池、数据切分、奖励及两层更新顺序未冻结；桌面汇报提GRPO、旧ADR-0028曾否决GRPO，实际算法需显式澄清。当前API隔离不是OS沙箱，未来LLM进程不能获得缓存/真值/仓库文件权限。
- 下一步：围绕已对齐的医药双层方法收敛首批训练方案，先固定环境与基础选择器，再比较主Agent训练前后；不能拿已实现回放冒充RL完成。无新科学预测、API调用或星云提交，未提交/推送。

### 2026-09-22 — Task37 深度汇总与当前文档收敛

- 用户要求：停止随提示附和和主线漂移，深刻核对当前进度、待做/未做任务、论文故事与创新，并及时更新文档。本轮按该请求执行，不启动新训练。
- 当前稳定目标：医药同功能具体模型选择，Contextual Bandit为核心学习问题；主Agent可靠性/参数训练是另一层。PPO/GRPO不再作为工具Bandit前置，新工具冷启动和通用接入保留支线身份。
- 核心结论：已有数据、旧七家族学习实验及当前具体模型回放，不是完全未研究；但没有当前33模型设定下完整的训练—验证—测试主表，也没有已证的新算法或主Agent医药训练成果。Task24真值扩展和全49工具验收均未闭合。
- 科学纠正：43个可救错例只是事后互补，不证明能从特征识别；Morgan救回34个同时会换错57个。方差分解的大共同难度项不能推出条件选择不可学习；新综合文档给了说明性反例，不改ADR原文或把反例当新理论贡献。
- 文档：新增 docs/AAMAS2027_论文主线与进度.md，完整说明对象、进度、三块拟贡献、候选机制、证据边界与章节结构；docs/任务清单.md 新增P01–P11状态、依赖、判据和负责人角色。README、CONTEXT、目标、文档地图重建当前入口，原内容折叠为历史并保留；旧Proposal/计划/通用接入路线与矩阵加适用范围指针。
- 独立核验：三路只读审计（数据、实现、科学）及一次无上下文读者检查，归纳在 task/task37-project-reconciliation/review_notes.md。读者发现的失败分母和基础未见分子测试位置已修正。不是完整论文审稿通过。
- 验收与结果：task/task37-project-reconciliation/{task.md,report.md,input_manifest.json,verify_documents.py,verification.json}。验出旧五份入口正文完整保留，桌面汇报和关键实验产物哈希不变，本地当前链接存在，Task36来源和实现哈希对应。未重跑模型、未测全仓、未更新平台状态，未提交/推送。
- 下一步：P01具体模型资格、P02切分与反馈权限、P03最近邻及机制审查并行准备；再由Owner确认小批P04。当前方法候选是利用相关错误结构改善条件能力估计，未获原创性或有效性证明，不预先包装为Ours。

### 2026-09-22 — Task38 / P01 首批模型与快照清单

- 开始逐项处理当前任务，先做P01，不重复展开全项目讨论、不启动训练。
- 已完成：复用Task36已核缓存，生成33具体模型run1候选清单；730分子排除原8个矛盾真值得722，保留缺值分子及失败状态。核对23,826个模型×分子寻址，其中23,682有效预测、144缺失。每模型有CSV列、来源哈希、标签规则、跨轮状态与来源限制。
- 验签：两进程候选清单逐字节相同，sha256=d49ff3cdbd32436581d4b64e427b239cf62c640bbff1ed9cd6b04028c94bf100；来源/实现哈希和集合计数检查通过。不读取当前正确率决定模型纳入，不覆盖原结果。
- 产物：task/task38-p01-provider-pool/{task.md,build_manifest.py,candidate_pool.json,candidate_pool_repeat.json,verification.json}。P01状态同步为候选清单已核、诊断范围待裁。
- 推荐：允许先用全部固定缓存做有来源限制的方法诊断；正式无污染外部泛化资格仍需单独补证。若Owner要求首批即给外部科学结论，则先补训练成员与外部真值，不借缓存回放替代。
- 未做：没有分训练/测试集（属P02）、没有训练或新增预测、没有远端提交。待诊断范围确认后冻结该清单，再推进P02。

### 2026-09-22 — 最新进展复核：P01外部证据优先，非待选方案

- 用户要求：重新查看项目最新推进。本轮核读当前入口、Task36–38及最新磁盘产物，不新增训练方案或科研裁定。
- 状态更新：Task38 `owner_decision.json` 已记录Owner选择“先补外部测试证据”，`docs/任务清单.md` P01也已更新。上条日志“诊断范围待裁”是较早快照；现在首批训练暂停，33模型722分子清单仍是候选，不再重复询问已作决定。
- 新证据：Task38 `reference_membership.json` 已将722候选分子全部匹配到归档BBB_Martins来源。该结果不是具体权重训练成员证明，也不授予外部测试资格。本轮核对来源TSV哈希、唯一ID和集合覆盖，未独立重跑分子规范化。
- 本轮核验：Task36和Task38各20项来源/实现哈希一致，两组既有重复件分别逐字节一致；从粒度审计652条明细复算607固定正确、43可救错例、2全错以及Morgan的34救回/57换错；候选逐provider合计23,682有效与144缺失。重新执行103项定向回归，103 passed in 4.28s，非全仓。
- 结果：`task/task37-project-reconciliation/进展复核_20260922_2044.md`及同名JSON保存本轮状态、核验输出和边界。新方法、当前具体模型完整主实验和医药Agent SFT/RL仍未完成。
- 下一步：继续P01具体权重训练/调参成员及外部测试证据；P02协议与P03近邻审查并行准备。未启动Bandit/LLM训练、未新预测、未安装、未操作星云或Git提交推送；未修改Task38原有文件。

### 2026-09-22 — Task39：Owner五步科研迭代规范落地
- 状态：规范与首轮候选卡完成；具体方法/最近邻审查与学习实验尚未完成。
- 用户目标：以提高投稿成功可能性为目标，故事线→候选创新→连贯方法→审查→小批真实验证；实验用于巩固或修正主张。
- 本轮工作：重读当前仓库入口及Task38裁定，核读AAMAS官方评价/审稿指引；新增唯一完整迭代规范，更新目标、AGENTS、README、文档地图、综合说明和P04/P05依赖。候选方法草案先于实验，不再等待跑出赢家才提出创新。
- 关键结论：研究进展以主张得到何种支持/反驳计，不以跑批或工程数量计；小批好结果只是初步支持，标准方法已足够则撤回新增机制主张，负例不自动否定整个方法族。数据资格并行服务实验，不阻塞故事/方法准备。
- 结果文档：`docs/研究迭代规范.md`；`task/task39-story-method-validation-loop/{task,report,round01}.md`。
- 数据/代码：无新实验与算法修改；Task39 `input_manifest.json`和`verification.json`保存修改前来源及文档验收。精确编辑、四份历史折叠正文、六个保护文件、引用及scoped diff检查为PASS。
- 未完成：当前相关错误结构候选的已发表最近邻比较、完整估计/更新规则、实验卡具体参数；Task38外部证据与P02协议仍未闭合。没有把规范落地当作方法成立。
- 下一步：从Task39 round01推进故事—最近邻差异—具体机制—反证实验卡，P01/P02并行补齐；审查与授权条件满足后执行唯一小批。未撤销Task38“先外部证据”裁定，未训练、未新预测、未操作星云或Git提交推送。

### 2026-09-23 09:03 CST — RDC恢复并补齐Task40/41中断交接
- 状态：Mac已返回pong，文件读取、终端单行exec及写入后读回均成功。两次多行REPL输入曾报失败，后续单行exec正常；不能将此误报为权限缺失。
- 用户目标：重试连接，接续真实工作，不重复已完成任务。
- 补录上轮：Task40最近邻与数学/信息合同审查收口，泛化固定核共享机制归为已有参考，不命名Ours；Task41新增 `src/probe_headless/pharma_kmtl_reference.py` 及单调用终止反馈适配、45项新测试，与88项回放回归合计133通过。属于方程参考与人工fixture，不是医药效果或官方动作轨迹fidelity。
- 本轮核验：Task40/41既有重复件分别逐字节一致，三项/五项来源哈希匹配；Task41结果SHA256仍为b4d881240bd52b2e87dd41f77f12e5669df6f81daebff288acb460e669fcc6b7。没有重复训练或冒称本轮重跑133测试。
- 结果文档：`task/task40-pharma-nearest-method-audit/{report,method_audit,small_batch_card}.md`；`task/task41-kmtl-reference-contract/{report,fidelity_notes}.md`；本轮重连证据 `task/task41-kmtl-reference-contract/rdc_recovery_20260923.json` 已写并读回确认。
- 并行现场：Task38外部来源采集资料已落盘；观察到另一Qoder执行进程正在生成 `external_evidence_audit_repeat.json`。本轮不重启其下载/审计、不改其脚本和结果，避免将同事新成果据为本轮或重复跑。所读快照仍标external_qualification_granted=false。
- 未完成：当前具体新颖机制、正式P02及P01外部资格仍未自动闭合；Task38裁定未撤销。
- 下一步：在已有Task40审查和Task41方程参考上准备合法小批，不再重新实现固定核候选；外部审计由正在执行该项的工程师继续收口，结果先按模型权重与分子身份解释，再决定是否具备申请实验的依据。无训练、新预测、安装、星云操作或Git提交推送。

### 2026-09-23 — Task38外部证据部分核查收口，P01未放行

- 执行范围：承接Owner“先补外部测试证据”和“继续”。三路旧子调查没有返回结论，本轮由主线程读取本地资产、官方GitHub/Dataverse/Zenodo记录及ADMET论文，不将失败调查计为独立评审。
- 身份证据：DeepPurpose3模型、B3clf24模型、ADMET-AI2版本共29个候选动作对应37个权重文件，均已与实际发布物核对。DeepPurpose ZIP匹配官方MD5；B3clf24与ADMET10个文件匹配固定提交Git对象；ADMET 1.4.0的fold0还与论文存档逐字节一致。其他4个候选本轮未查，不默认通过。
- ADMET发现：1.4.0实际权重记录多任务数据、随机80/10/10划分、种子0–4，每个训练28,619行；不是BBB分子数，也不是排行榜骨架划分。已取得的fold0/1测试名单各3,578行，与722候选分别匹配89/90个，共同13个；13不是合格外部池，尚缺其他成员及完整数据。2.0.1对应重新训练版本，不能沿用旧名单。
- B3clf发现：已验签标准化器与common KNN的拟合规模均为7,407，KNN特征矩阵7,407×475，但没有分子ID映射。仅反序列化上述两份已验签本地joblib读取元数据，未执行fit或predict；不能把规模当成员证明。
- 真值发现：固定B3DB主表7,807行与候选池精确匹配609分子，其中27个与BBB_Martins标签不一致；external表175行匹配10个，其中3个标签不一致。两类分歧可能重叠，不相加计不同分子。external表171正/4负、150条只注明DrugBank、全部logBB为空；文件名不证明外部独立性。已保存逐源行、引文和待核状态，未覆盖标签或自动扩展排除集。
- 下载边界：Zenodo按范围成功取回一个权重、两份测试名单及args.json，随后返回403，明确原因为网络异常流量。停止请求，不绕限制；完整训练CSV、余下划分和日志待取。仅验证已提取ZIP成员CRC，未声称核过整包MD5。
- 产物：`task/task38-p01-provider-pool/report.md`、`external_qualification.json`、`cross_source_label_disagreements.json`、`external_evidence_audit.json`及重复件、`external_evidence_verification_final.json`；公开原文与元数据在`evidence/`，采集及核验脚本同目录。资格为29项UNKNOWN、4项本轮未审，没有新放行。
- 验收：37份公开快照哈希核验；审计双进程逐字节一致，SHA256=f01bd0da3d1c0a5091c047026789d6a052ff686e417cb2c5a5a23069e24b1c55。独立代码复算交集/标签分歧，5项已知同一/不同/无效分子及内容变化控制通过，报告本地链接、脚本语法和格式检查通过。Task36预测与实现哈希、Task38候选和裁定未变；非全仓测试、非实验标签裁决。
- 未完成与下一步：P01及训练暂停保持；继续补权重绑定的实际成员、核读已定位分歧的原始实验文献，再确定所有拟纳入模型共同的合格外部集。外部作者联系、新预测或训练均未执行。P03/P05可并行准备，本轮不认领Task40/41的同事成果，不重做其参考实现；未提交/推送或操作星云。

### 2026-09-23 09:42 CST — Task42：P02评价合同与首批具体提案
- 状态：提案及人工合同代码验收完成，P02未批准冻结、真实学习批次仍BLOCKED。
- 用户目标：继续真实推进五步科研循环；本轮不重复Task38外部调查、不重做Task41共享参考。
- 科学进展：把有效返回正确性与全任务系统成功分开，证明丢失败分母可逆转策略排名；冻结泛化与预序列在线适应采用不同时钟/区间口径；明确二值题后标签可恢复，直接预测器要匹配真实标签预算。这些是比较合同，不包装为创新。
- 本轮工作：新增 `src/probe_headless/pharma_evaluation_contract.py`、47项人工测试，接实际Task36类验证终止反馈与失败None；卡中提出单调用、4次/provider初始调用、128目标/256上限和明确参数，但批准、合格池、划分、H0/特征版本仍未填实。未生成真实划分或训练。
- 验收：47新增+45 KMTL参考+88回放回归共180 passed in 1.46s；两进程合同产物逐字节一致，SHA256=daede22e7949d0a3f60706a1bebb67c24e9df5151ec16b5a7bb5b09d0310387f。不是180个医药任务或全仓全绿。
- 结果文档：`task/task42-p02-evaluation-contract/{task,report,protocol_proposal}.md`、`experiment_card.proposed.json`、`verification.json`；一手依据在 `research/AAMAS2027_医药评价与反馈协议审查.md`。
- 数据/代码：`data/tool_results/divergence/t42_evaluation_contract_v1{,_repeat}.json`保存四个人工反例及Task38只读readiness。七项保护文件未变、当前链接/编译/scoped diff检查通过；任务清单P02/P03/P05和综合说明已同步准确边界。
- 未完成：readiness列明29模型UNKNOWN、4本轮未审，未授外部资格；当前多动作system_success基线适配仍待验、非平凡机制与C1真实效果未证。学习版本检查依赖可信运行器，非OS隔离保证。
- 下一步：补具名模型训练成员和分歧真值依据，同时完成P02关键建议审查及当前基线适配；获合格数据与批次批准后按唯一小批执行，不再以更多人工合同测试代替科研实证。无新预测、真实fit/update、LLM训练、Zenodo绕限、星云或Git提交推送。

### 2026-09-23 — 目标与科学进展复核：按五步论文闭环汇报
- 状态：本次进度复核完成；最新科研/工程交付仍为Task42，没有新增学习结果。
- 用户目标：汇报真实进展并确认是否理解最终目标；核心是医药具体工具选择及主Agent可靠使用，服务有竞争力的AAMAS论文，不是无限完善结果库。
- 本轮工作：重新连接指定macOS，读取TODO、当前目标/入口/任务清单、Task40与Task42报告和Task38资格表，检查最新任务目录及git status/log；按故事、创新、方法、审查、真实验证五步对账。
- 关键结论：近期有价值的科学排错和工程准备，但当前具体模型设定的完整小批学习仍未做；非平凡机制缺口与外部资格缺口独立存在，不能说只差权限，也不能把180项人工/接口测试当科研效果。最近推进偏重准备，需要把重心移回可审的具体机制与反证实验。
- 结果文档：`task/task37-project-reconciliation/目标与科学进展复核_20260923.md`，含来源、五步状态与执行自评；不替代当前目标和各task事实源。
- 数据/代码：未修改；本轮未重跑测试、未验签全部历史数值、未训练/新预测/提交推送。
- 未完成：具体候选机制与最近邻差异；P01仍29 UNKNOWN、4本轮未审；P02真实划分/历史/批准；P04与P08当前医药学习/主Agent训练未完成。
- 下一步：优先交付可执行且可反驳的候选方法审查，数据证据并行服务该批次；保留既有训练限制，不再用无关人工合同测试代替实证进展。

### 2026-09-23 10:26 CST — 最新进展只读核对
- 状态：应用户“获取最新进展”核读当前入口、任务清单、Task38/40/42报告及上一轮目标复核；最新实质性交付仍为Task42，未发现更新的医药学习结果。
- 科学进展：具体模型错误互补性与回放已具备，固定核共享候选经Task40审查归为既有参考；当前仍缺可审的非平凡机制和具体模型设定下的真实小批学习结果。Task42的180项通过是人工合同/回归验收，不是医药性能。
- 执行边界：Task38核实29个动作的发布身份，但外部资格29项UNKNOWN、4项未审，训练仍未获授权；P02提案未批准冻结。Owner“先补外部测试证据”的裁定不变。
- 仓库现场：只读Git核查最新HEAD仍为21b39e90（2026-09-18），工作树有大量未提交变化；本轮未清理或提交。来源为 `task/task38-p01-provider-pool/report.md`、`task/task40-pharma-nearest-method-audit/report.md`、`task/task42-p02-evaluation-contract/{report.md,verification.json}` 和 `task/task37-project-reconciliation/目标与科学进展复核_20260923.md`。
- 未完成与下一步：优先形成有最近邻差异、更新规则和反证条件的候选机制；并行补权重训练成员与实验真值、裁P02协议，满足资格与批准后执行唯一真实小批。本轮无测试、训练、新预测、远端写入或Git提交推送。

### 2026-09-23 11:22 CST — 目标、故事与方法复述前的缺口整理
- 用户要求先复述目标、当前故事线和方法论以便对齐，同时列出不足、记录TODO并逐项解决。本轮复核 `docs/目标.md`、Task39候选卡、Task40方法审查、Task42协议提案和当前P01–P11；另做一次只读独立缺口审计。
- 已把7项有关闭判据的缺口写入 `docs/任务清单.md` 的“当前不足与逐项关闭顺序”：新机制审查、P01外部资格、P02协议、基线适配、P04真实小批、独立确认与Agent/论文交付。该表标为待本轮复述对齐，不以清单改写Owner决议或授予训练权限。
- 当前理解：Owner已定医药同功能具体模型选择与主Agent可靠使用两层目标；C1上下文可学、C2新机制有额外价值、C3系统收益仍是待验主张。Task40已将固定核共享归为已发表KMTL类参考，不得冒充Ours。Task42系统成功目标和划分等仍是提案，非冻结协议。
- 未完成与下一步：等待Owner对本轮复述的纠正或确认；若有实质新共识，按项目ADR纪律逐项落盘，再依第1项机制卡及并行P01/P02推进。本轮仅文档修改，未执行实验、测试、训练、新预测或Git提交推送。

### 2026-09-23 13:37 CST — 纠正上一版故事与TODO排序
- Owner指出上一版复述仍未理解透彻。本轮重读其指定桌面阶段汇报全文，核对当前目标、综合说明、Task39/40/42；两路独立只读审查分别对照原始意图和论文因果链。
- 纠错结论：上一版将固定Agent单调用的Bandit诊断当作整篇论文方法，将新选择算法预设为核心贡献，并把Agent主动复核/弃答及真实SFT/RL排到最后，偏离阶段汇报。总问题是正常返回的科学证据仍会错甚至共同错，Agent如何决定取证、复核、作答或停止；Bandit在功能请求下管理具体provider。功能key隔开可迁移的Agent行为和随工具池变化的在线工具能力，是两层方法的连接。
- 科学边界：当前目标文档又明确具体模型条件选择是首要研究能力，两份材料的主次张力需Owner澄清；不以此退回旧ADR的“选择只作评测”。七家族14个共同错与33具体输出2个全错不能混用；单调用反馈不能无假设辨识任意联合错误。现成固定核共享已被Task40归为KMTL参考。
- 文档：已撤回 `docs/任务清单.md` 上轮强行排序的7项表，改为5类未排序、可验收的故事/Agent/Bandit/数据/实验缺口，标注待对齐和现有训练限制。旧版误读在本日志保留，不静默覆盖执行历史。
- 未完成与下一步：向Owner复述完整因果故事和两层方法，确认9月22日“选择首要”与指定阶段汇报的论文总目标如何承接；随后据共识写ADR并按主张修订执行顺序。本轮无实验、测试、训练、新预测、外部写入或Git提交推送。

### 2026-09-23 13:46 CST — 当前提案内部审稿判决
- 用户追问若以审稿人视角是否会通过当前提案。本轮按paper-reviewer快速预审口径，核对AAMAS 2027官方主轨范围/评价标准、Task40最近邻、Task42信息合同和当前数据资格；两路独立只读审稿意见均为Reject（若作为内部提案则Major Revision）。
- 决定性问题：缺单一可反驳的贡献命题；“识别正常返回的错误证据”尚无合法可辨识信号与强直接预测对照；七家族14个全错与具体模型2个全错不可混用，现有外部资格及真实Agent/两层实验不足以支撑宏大叙事。固定核共享与序列复核的已有近邻必须正面比较。
- 可复用审稿报告：`task/task37-project-reconciliation/提案内部预审_20260923.md`，含判决、证据和最小改变判决的门槛。它是作者侧预审，不代表AAMAS正式评审，也没有把一种候选方向预设成Owner决议。
- 下一步：先选并写清一个中心问题、信息合同、相对最近邻的确切增量及反证条件，再让P01/P02证据与最小真实实验服务该命题。本轮无实验、测试、训练、新预测、外部写入或Git提交推送。

### 2026-09-23 14:30 CST — Task43：多源调研后的提案修复候选
- 用户要求多调研并给具体可行修法。本轮重读指定阶段汇报、当前目标与Task38/40/42，核读15份外部一手论文/官方规则，来源、证据、主张台账及8份公开PDF原文保存于 `task/task43-repair-proposal/`。`agent-reach check-update`显示v1.5.0已是最新版。
- 决定性审查：Gao–Koller 2011已做按已见输出选下一分类器的价值计算；Kelly 2025、Cascade Routing 2025、ECED、相关融合、付费观测和早弃答进一步覆盖旧叙事。固定核KMTL亦是已发表参考。当前不能把“相关错误+序列复核+弃答”或简单成对探测称为新算法。
- 修复候选：保留“针对输入选更准确的具体provider”为首道独立判据；在其显示信号后，至多二调用的BBB试点检验第二份证据对最终错误的实际增益。主Agent在主实验只见真实工具结果，自己决定复核/作答/弃答；提供风险摘要另作消融。成对探测仅在稀疏真实观察条件下用于识别第二次调用价值，合法全缓存监督对照不被人为遮蔽。二次日志无法识别第三次价值，故首批不做三调用主张。正式主读出候选设90%覆盖、平均不超过1.5次调用，以非平凡调用预算检验复核取舍；尚待协议裁定。
- 数据与投稿：现有33模型722分子仍29项UNKNOWN、4项未审，Task38未授权训练/新预测；Task42的128题仅探索。按旧发生率，50个七家族共同错约需2329个独立分子，500个旧难分子约需6447个，仅作规划投影。AAMAS 2027摘要10月1日、全文10月8日，作者OpenReview账号已由Owner确认齐备；现版主轨论文缺合格外部数据、强基线增量和真实Agent结果，不建议直接提交。9月29日作者侧Go/No-go闸门已写入报告，非Owner决议。
- 产物：`task/task43-repair-proposal/{task.md,report.md,report.html,report.pdf,sources.jsonl,evidence.jsonl,claims.jsonl,run_manifest.json,ai_assistance_record.md}` 与 `evidence/`；`docs/任务清单.md`增Task43五项关闭判据。报告结构验证PASS，15条外部引文URL验证15/15，台账20来源/20证据/10主张关联校验PASS，8份原文SHA校验PASS，PDF四页视觉核查无截列，`git diff --check` PASS。两位独立只读审阅的核心反馈已吸收：恢复具体模型选择优先级、移除主实验风险摘要、二调用上限、全信息对照。方法原创性仍未被证明。
- 未完成与下一步：Owner尚未确认候选贡献与旧题量承接；P01外部资格/P02多调用协议及实验授权未具备。接下来先定具体模型条件选择和强近邻的同信息实现卡，并行完成外部权重成员与真值审查；合格后才申请一次真实128题小批。没有运行医药学习、LLM训练、新科学预测或星云写入，也没有Git提交/推送。

### 2026-09-23 14:59 CST — Task44：把外挂在线 Bandit 写成方法候选
- 用户追问是否只是找到了真问题而没有解法，并强调阶段汇报中的外挂模块须实时学习。本轮复核阶段汇报 §2.6、当前 `docs/目标.md`、Task40/42 的反馈时钟与权限；核对 LinUCB、Gao–Koller、Cascade、Kelly 和延迟在线反馈一手来源。
- 纠正：Task43 虽给出条件准确率 `q` 与第二次调用价值 `Δ`，仍缺一条明确在线更新链。Task44 把一题定义为 episode：Agent 决定请求/复核/停机，管理器选择具名模型；首次选模学习输入条件的有效正确率，第二次请求学习在首次真实返回条件下的参考决策减错增益；同题不提前给真值，整题终止且可验证标签到达后才更新。
- 最小额外证据：单调用不能识别同题共错结构；第二次调用价值需要具名成对探测、正选择概率和逐次日志。失效与化学答错分开，未调用工具无奖励。静态缓存的逐题揭示只能证明 prequential 回放，不是现实世界标签即时返回或概念漂移适应。
- 结果：新增 `task/task44-online-provider-manager/{task.md,report.md}`，`docs/任务清单.md` 链接该候选。明确 LinUCB/价值信息/延迟 Bandit 均有近邻，在线机制尚无原创性或医药性能证明；保留强基线淘汰条件和 Task38 暂停。
- 未完成与下一步：P01 合格具体模型与外部真值、P02 成对探测合同、P03/P05 同信息强近邻适配；取得具名批次授权后才执行真实小批，并分开报告冻结泛化与先决策后更新。无学习、新科学预测、远端写入或Git提交推送。

### 2026-09-23 15:26 CST — Task45：动态判别式决策模型方向与Laya源码核查
- Owner提出预训练Jev类决策能力帮助冷启动、真实反馈持续学习，并给出Laya链接。本轮将该方向记为ADR-0011；Bandit只是候选建模，具体实现与“够一篇论文”不作已批准或已证结论。目标文档及任务清单已承接。
- 源码结论：Laya是独立Jev类开源实现，当前为离线微调、eval/no-grad推理，无逐题在线更新接口。Choice概率及熵置信度不能当科学工具正确率或Bandit后验；其微调使用完整目标分布，不能把未调用工具补为负例。候选marker表征需新增导出接口，且依赖整份菜单。
- 方法产物：`task/task45-dynamic-decision-model/{task,report,implementation_card}.md`给出冻结预训练表示、两个在线logistic头、有限权重先验、显式探索及实际采样概率、有效率/化学正确率分离、题后延迟反馈与去重更新。参数只是待审建议，无训练或效果结果；追加复核承接Task44，不从单调用伪造联合反馈。
- 最近邻：PILOT（EMNLP Findings 2025）与BayesianRouter（ICLR 2026）已覆盖离线判别先验＋在线部分反馈，不能把组合本身称原创。LLM-PriorCB作者列ICML workshop；匿名NeurIPS稿仅索引可读，直取403，未保存或全文核读，不声称已录用。
- 可复用资产：`reference/laya-8465935/`固定Git提交8465935的32份源码/说明/测试/许可证与微调notebook；`reference/laya-hf/`固定revision的模型卡与三份配置，无权重；`reference/papers/`两篇公开正式PDF。12条来源在`sources.jsonl`，逐文件哈希在各SOURCE.json。
- 核验：38份清单资产哈希一致，8个新文档本地链接存在，来源ID与本地路径校验通过；定向文档格式检查见`verification.json`。两路独立只读审查补清源码接口、收益语义及强近邻。未做模型安装/推理、医药学习、LLM训练、新科学预测、星云写入或Git提交推送。
- 未完成与下一步：先审冷启动标签条件与同信息实现卡，验证先验是否有能力区分同功能具体模型；补齐P01/P02后，获具名批次授权才做真实小批，依次检验预训练信息、在线净收益和错误先验恢复。现有33模型/722分子的外部资格与训练暂停不变。

### 2026-09-24 11:13 CST — 主线重定：可信证据闭环与动态Jev三层架构
- Owner纠正主次关系：研究重点是主Agent对正常返回但可能错误的工具证据进行复核、验证、作答或拒答；provider选择只是动态工具适应模块中的一个动作。最终交付明确为：面向工具调用的动态适应模块、实时学习的动态Jev方法、以及主Agent综合工具调用能力提升。
- 形成并落盘 [ADR-0012](docs/adr/0012-three-layer-dynamic-jev-agent-loop.md)：动态工具适应模块维护任务/工具/反馈/预算并估计验证价值；动态Jev用typed decisions统一处理选择、证据风险、验证/复核和终止动作；主Agent读取结构化状态并学习完整调用轨迹。主Agent最终决定是否复核、作答或拒答，不能用provider命中率代替端到端能力。
- 动态Jev设计补充题内与题间两个时间尺度：题内看到工具输出后更新证据状态，题间等真实/终止反馈到达后更新轻量head、adapter或后验。反馈只更新实际观察动作；未调用和未知真值保持缺失；日志同时记录Jev建议、Agent实际动作及覆盖关系。统一episode效用含错误、拒答、调用、验证和延迟项。
- 文档同步：`README.md`、`docs/目标.md`、`docs/AAMAS2027_论文主线与进度.md`、`docs/任务清单.md`、`research/AAMAS2027_Ours_Problem_Formulation.md`、Task44/45已改为三层主线。Task45新增 `dynamic_jev_design.md`；Task44的两次调用保留为首轮pilot边界，不定义总架构。
- 科学边界：动态Jev方案仍是候选，未训练、未生成新预测、未获得医药模型池授权；Laya Choice/熵集中度仍不能直接作工具正确率或证据真值。P01/P02、强近邻和端到端Agent实验仍未完成。
- 核验：本轮修改文件已通过定向 `git diff --check`；未执行模型、训练、远端写入或Git提交推送。
- 下一步：先把动态Jev的typed-decision接口、延迟反馈和Agent轨迹合同变成可审实现卡，再按P01/P02资格与协议做最小三层实验：Jev适应、模块动作、Agent端到端，分别设定可推翻条件。

### 2026-09-24 11:37 CST — AAMAS审稿规则：动态Jev与可信工具调用
- 用户要求从AAMAS审稿人视角判断本故事的难点、重点和好论文标准，并写成规则文档。本轮使用 `paper-reviewer` 与 `academic-writing` 规范，核对AAMAS 2027官方 Reviewer Guidelines、Submission Instructions 与 Findings说明。
- 审稿判断：故事具备AAMAS的Agent learning/adaptation相关性；最大风险是被看成“Jev分类器＋在线Bandit＋Agent wrapper”的拼接。好论文必须把正常返回≠可靠证据、动态Jev在线更新、工具适应、主Agent复核/验证/拒答和端到端结果串成一个可识别因果链。
- 新文档：`docs/论文质量规则_动态Jev可信工具调用.md`。内容包含研究难点、AAMAS门槛、审稿问题、MUST/SHOULD/FAIL规则、反模式、主轨/Findings定位、6维内部评分和硬闸门、投稿前自检卡。硬闸门为真实在线更新、合法反馈权限、Agent端点指标、强近邻基线、无信息泄漏。
- 规则中的关键审查点：延迟且选择性的反馈；valid/correct/utility语义分离；复核价值的条件效应；题内状态更新与题间参数更新；Jev建议与Agent实际动作的归因；冷启动、漂移、新provider和错误先验恢复；真实医药模型资格、真值独立性和划分。
- 官方依据：AAMAS主轨要求原创性、意义、soundness、可复现性、清晰度、相关性和state of art；正文最多8页，核心内容不能只放补充材料；Findings仍要求科学可靠、贡献可识别、证据支持，只是整体范围/一般性/验证广度/影响可能较窄。官方链接已写入文档。
- 验收：新规则文档共9个主体章节，定向 `git diff --check` PASS；`docs/README.md`已新增入口并更新三层主线表述。未运行模型、训练、新预测或远端写入。
- 未完成：三层实验、P01/P02资格与协议、强基线适配、主Agent行为训练仍未完成；当前规则结论是投稿前准入标准，不是论文结果。

### 2026-09-24 13:40 CST — Task46：开源复用、benchmark 与 baseline 组合路线
- 用户明确执行策略：先把故事线和方法论记入文档，确定 benchmark/baseline，再按功能寻找开源项目二次改造，避免从零实现。本轮使用 `agent-reach` 完成 GitHub 官方仓库、README、LICENSE 和接口检索；`agent-reach check-update` 显示 v1.5.0 已是最新。
- 形成开源优先决议 [ADR-0013](docs/adr/0013-open-source-first-composition.md)：Laya 做冻结冷启动/静态基线，AnyJev 的 `observe`/closed-form head 做动态 Jev 候选骨架，bayesianbandits 作为 P1 selected-arm 在线后验主实现候选，River/contextualbandits/MABWiser 做交叉基线，tau2-bench/Inspect AI 做交互运行骨架，jev-benchmarks/OBP 做概率/OPE 评估；jev-ultrafast 只借可靠调用状态机模式。
- 新增主蓝图 `docs/开源复用与集成路线_动态Jev.md`：冻结 PTRB-v0 episode/schema、T0–T3 任务、B0–B8 baseline、信息泄漏和 propensity 规则、组件组合图、P0–P4 实施闸门及“不从零建设”清单。
- 新增 `task/task46-open-source-composition/{task.md,report.md,sources.jsonl,verification.json}`。官方候选快照保存在 `reference/`，包含 AnyJev、Laya既有快照、SemIf-OpenJev、jev-benchmarks、jevcal、bayesianbandits、contextualbandits、MABWiser、River、Vowpal Wabbit、OBP、tau2-bench、Inspect AI、jev-ultrafast、AgentDojo、ToolSandbox 与 Online_Multi_LLM。
- 许可证处置：Apache/MIT/BSD 候选可进入依赖审查；Vowpal Wabbit GitHub SPDX 为 NOASSERTION、ToolSandbox 为自定义/Other、openJev-verdict 权重和许可证仍待人工核验，只作 reference。开源项目自报 teacher agreement、默认 DB reward 或 LLM judge 不能作为医药真值。
- 核验：GitHub API 读取和源码摘录落盘；AnyJev `observe` 源码已核到 30/60/120 周期重拟合；文档链接和 `verification.json` 检查通过；`git diff --check` 待本轮末执行。没有训练、新预测、真实 fit/update、远端写入或 Git 提交推送。Task38 外部资格限制和训练暂停保持。
- 下一步：写 P1 provider adapter、pending feedback、trace/evaluator 最小接口和合同测试；待 Task38/P02 放行后再运行最小真实批次，分别检验 Jev 适应、模块动作和 Agent 端到端收益。
- 追加验收：`sources.jsonl` 17 行 JSONL、133 份参考快照哈希、文档路径和 `git diff --check` 均 PASS；本轮仍无训练、新预测、真实 fit/update、远端写入或 Git 提交推送。
- 工单：`.scratch/task46-open-source-composition/` 已拆成 adapter contract、dynamic Jev wrapper、Agent evaluation 三张可关闭 ticket。
- 研究材料补强：31 个候选仓库快照的 `repo.json` 已规范化并补充可读到的 head commit；133 份 reference 文件的 SHA-256 manifest 已刷新并复核通过。

### 2026-09-24 14:05 CST — PTRB-v0 来源边界澄清
- Owner追问“基于Task36的主 benchmark”是否是自己做的。本轮明确：是，本项目正在构建的领域 benchmark 草案；Task36 是真实结果与回放环境资产，不是现成社区 benchmark。
- 新增 [ADR-0014](docs/adr/0014-benchmark-provenance-and-external-role.md)：PTRB-v0 负责医药工具可靠性核心问题；tau²/Inspect/JevBench/OBP/ASlib/LLMRouterBench 负责交互运行、概率/OPE 或迁移 sanity check，不能合并成一个排行榜或替代药理真值。
- 同步更新 `docs/开源复用与集成路线_动态Jev.md`、Task46报告、任务清单和README，避免把内部协议包装成已有标准。文档路径和 `git diff --check` 待本轮末复核；未训练、未新预测、未远端写入。

### 2026-09-24 14:20 CST — 文档与Task46进展推到远程
- 为避开工作区已有的大量历史删除、实验结果和未跟踪代码，创建分支 `codex/progress-20260924`，只提交本轮研究文档、ADR-0011—0014、Task46开源快照与三张工单；没有把旧数据/结果或删除动作带入提交。
- 提交 `6547dfa4`（`docs: persist dynamic jev benchmark and open-source plan`）已推送到 `origin/codex/progress-20260924`。远端已生成 Code Review 入口：`https://code.alibaba-inc.com/jingwu.jw/benchmark/codereview/new?from=master&to=codex/progress-20260924`。
- 推送前核验：JSON/JSONL解析通过，Task46 133 份参考快照 SHA-256 一致，自有文档 `git diff --check` 通过，暂存路径策略检查通过；上游README快照保留其原始尾随空格以维持证据快照哈希。
- 当前仍未训练、未生成新预测、未做真实 fit/update；工作树中的历史删除、Task26—45及数据结果仍未推送。下一步从三个工单开始写 adapter contract、dynamic Jev wrapper 和 Agent evaluation 的最小实现，先做合同测试再申请合格批次。

### 2026-09-24 16:10 CST — Task47：Agent benchmark 与 baseline 发表范式调研
- 用户要求调研 Agent 论文如何用 benchmark 和 baseline 支撑发表，并确认我们是否已有自己的 benchmark/baseline。本轮按 `agent-reach` 检索并核读论文原文、官方代码和项目页，覆盖 AgentBench、τ-bench、τ²-bench、ToolSandbox、AgentDojo、AppWorld、TravelPlanner、OSWorld、MedAgentBench、MINT、AgentBoard、LLMRouterBench、ToolBench-X、ToolLLM、Reflexion、Voyager、PILOT 和 BaRP。
- 关键新增邻域是 ToolBench-X（2026 arXiv 预印本）：1,106 个可执行任务，5 类可恢复工具 hazard，直接评估诊断与恢复。由此不能把“正常工具返回可能不可靠”写成首次贡献；主线应收紧到真实医药 provider、延迟 selected-only feedback、动态Jev在线更新和主Agent复核/拒答。
- 新增研究报告 `research/Agent_Benchmark_Baseline_Publication_Survey_20260924.md` 和 `task/task47-agent-benchmark-baseline-survey/`。报告明确：Task36 是真实回放资产，PTRB-v0 是 project-defined benchmark 草案；B0–B8 是候选矩阵，尚未冻结运行结果。建议四个 slice：S1工具选择、S2结果验证、S3拒答/升级、S4在线漂移适应。
- `docs/README.md` 和 `docs/任务清单.md` 已增加 Task47 入口及关闭判据。来源清单22行 JSONL，研究报告和 Task47 文档定向 `git diff --check` 通过；尚未运行训练、新预测、真实 fit/update 或外部科学批次。
- 下一步：把 Task47-01 至 Task47-06 转成合同/runner/泄漏审计工单，先跑静态 fixture；满足 Task38/P02 后再做最小在线实验。ToolBench-X 数据许可和外部复用条件在正式使用前单独核验。

### 2026-09-24 16:35 CST — Task47研究进展提交并推送
- 研究报告、Task47报告/来源/验证记录、文档地图、任务清单和本次执行日志已提交为 `36278e36`（`research: audit agent benchmark and baseline publication norms`）。
- 已推送到 `origin/codex/progress-20260924`；Code Review：`https://code.alibaba-inc.com/jingwu.jw/benchmark/codereview/new?from=master&to=codex/progress-20260924`。
- 推送范围严格限定为本轮8个研究/文档路径，工作区已有历史删除、Task26—46、数据结果和未跟踪代码未带入。来源JSONL 22行唯一、关键外部链接可访问、定向 `git diff --check` 通过。
- 本轮仍未训练、未生成新预测、未做真实 fit/update，也未解除Task38/P02资格限制。下一步按Task47-01至Task47-06先冻结PTRB-v0合同、split/泄漏审计和同合同baseline runner，再在获批后做最小真实科学批次。

### 2026-09-24 17:05 CST — Task48：纠正为方法论文公开 benchmark 优先路线
- Owner纠正上一轮问题：当前论文是方法论论文，调研重点应是别人实际复用的公开 benchmark、公开性边界和可直接采用的 benchmark 组合，而不是先把 PTRB-v0 做成主 benchmark。
- 一手先例已核对：PILOT、BaRP、Cascade Routing 直接使用 RouterBench；RouteLLM 使用公开 MMLU/MT-Bench/GSM8K 和 Arena 数据；FrugalGPT 使用公开任务加作者采集的 API 输出；PAK-UCB、NeMoS 使用公开任务/evaluator 加模型输出 replay 和 arrival 协议；全新环境主要属于 benchmark 论文。
- 新增 `research/Method_Paper_Public_Benchmark_Survey_20260924.md`、`task/task48-method-public-benchmark-selection/` 和 [ADR-0015](docs/adr/0015-public-benchmark-first-for-method-paper.md)。主组合定为 RouterBench 主 routing panel；LLMRouterBench 固定子集作现代外部复核；BigCodeBench/HumanEval-X/Sudoku 做 arrival/drift 泛化；ToolSandbox 或 tau2-bench 二选一做 Agent sanity；BBB/Task36 做真实医药 application case。
- 修正旧调研报告入口，更新 README、docs 地图、任务清单和 ADR 索引。18 条来源唯一且链接校验通过，定向 `git diff --check` 通过。
- 提交 `97736939`（`research: prioritize public benchmarks for method paper`）已推送到 `origin/codex/progress-20260924`。Code Review：`https://code.alibaba-inc.com/jingwu.jw/benchmark/codereview/new?from=master&to=codex/progress-20260924`。
- 本轮没有训练、新预测、真实 fit/update 或科学批次。下一步先实现 RouterBench adapter 和 selected-only delayed replay 静态 fixture，再按授权进行方法对照。

### 2026-09-24 18:10 CST — Task47：开源 Jev 源码下载与动态更新适配评估
- 按用户要求下载并核查三个真实开源 Jev 类实现：AnyJev（main，commit `a59a69eaa91e62dc9b42924f8491f76d1b1972ea`，Apache-2.0）、Laya（main，commit `23a17522aa4942da6cce53a995a275760320b691`，Apache-2.0）和 SemIf-OpenJev（master，commit `23cf1f39fc9534fe81437200959b6dfc7106e45a`，MIT）。源码快照在 `task/task47-jev-source-evaluation/reference/`，没有下载模型权重。
- 选择 AnyJev 作为动态 JEV 实现骨架：唯一同时具备 `Question.noul`、冻结模型上的 L2 hidden-state head、`observe` 标签入口、30/60/120 阈值重拟合、artifact 导出/恢复。Laya 适合作静态强基线，SemIf 适合作直接 logits/离线温度基线。
- 用 AnyJev FakeBackend 做了选中工具 0/1 反馈的闭环冒烟：固定 `noul` 问题，正确映射为 Yes index 0、错误映射为 No index 1；16 条 selected-only 反馈在8条首次拟合、16条再次拟合，导出/恢复后预测一致；上游 `test_decider_fake.py` 24/24通过。SemIf 核心/校准测试12/12通过。Laya纯训练检查通过61项，但测试模块有设计性的SystemExit；模型测试在当前Python3.9/Transformers-huggingface-hub版本不兼容环境下未收集。
- 关键边界：AnyJev 自带 `observe` 是累积历史后完整重拟合，不是我们要声称的每条反馈常数时间实时更新；它没有延迟反馈队列、selected-arm propensity、动态菜单/版本冷启动、遗忘与漂移协议。这些必须由我们的 `DynamicToolSelector` wrapper 和在线 residual/RLS 后验更新实现。
- 产物：`task/task47-jev-source-evaluation/report.md`、`raw/source_manifest.json`、`raw/binary_observe_smoke.json`。`agent-reach check-update` 显示 v1.5.0 已是最新。没有医药训练、新科学预测、真实大批 fit/update 或远端写入。
- 下一步：固定 AnyJev 状态 schema（分子/性质/工具版本/输出）、selected-only delayed feedback contract、候选菜单 hash、propensity 和版本化快照；先实现 wrapper 的合同测试，再和 AnyJev observe、Laya、SemIf 及标准 contextual bandit 做同信息回放。

### 2026-09-24 17:25 CST — Task48补充：Agent验证公开先例
- 继续核对后补充 CRITIC（AmbigNQ/TriviaQA/HotpotQA，工具交互式验证和修正）与 ToolRL（BFCL/API-Bank/Bamboogle，工具调用奖励/评测）作为 Agent 层公开方法先例。RouterBench 只能证明动态选择，不能单独证明主 Agent 会复核、修正或拒答。
- 当前三段公开证据链收紧为：RouterBench 主 routing panel；CRITIC 公开 QA 任务做复核/拒答主 sanity；ToolSandbox 或 tau2-bench 做 stateful interaction 补充；BBB/Task36 做真实医药 application case。BFCL/API-Bank 只作工具名/参数/调用链 sanity。
- Task48来源增至20条，唯一性、链接和定向 `git diff --check` 通过。提交 `3adb2293`（`research: add public verification benchmark precedents`）已推送到 `origin/codex/progress-20260924`。
- 本轮仍未训练、新预测、真实 fit/update 或科学批次。下一步先实现 RouterBench adapter 和 CRITIC-style verification task adapter，再评估 ToolSandbox/τ²-bench 依赖。

### 2026-09-24 19:00 CST — 本周周报落盘
- 参考桌面 `周报汇总/0917/` 的格式和语气，整理 09-18 至 09-24 的工作：Task24/25 演示批与泄漏阻塞、Task32/34隔离修复、Task35通用净收益诊断、BBB具体模型粒度与Task36回放、三层动态Jev主线、AAMAS审稿规则、公开benchmark角色和AnyJev/Laya/SemIf源码核查。
- 周报明确区分已完成的接口/审计与未完成的训练、真实fit/update和科学结果，保留正式批被真值泄漏拦停、Task38资格未放行、P02合同未冻结等困难。
- 成稿：`/Users/jingwu/Desktop/周报汇总/0924/周报_2026-09-18_09-24.md`。未创建模型实验、未运行新预测、未推送远端。

### 2026-09-24 19:20 CST — 周报按口语风格重写
- 根据反馈，删掉上一版过长的研究总结和过密数字，改成更接近平时沟通的周报：只保留 BBB 泄漏、通用 Bandit 收线、具体模型互补、动态 JEV 和当前卡点。
- 成稿已覆盖桌面原文件：`/Users/jingwu/Desktop/周报汇总/0924/周报_2026-09-18_09-24.md`。

### 2026-09-25 10:00 CST — 动态 JEV 数学抽象候选稿
- 按“先数学对象、后 backbone/训练方法”的要求，把任务写成有限时域、带延迟 selected-only 反馈的 POMDP / partial monitoring 问题；provider 选择只是 delayed contextual bandit 子问题。
- 统一动作集合为 CALL / VERIFY / ANSWER / ABSTAIN，分别定义 q_exec、q_corr、q_ans 和验证价值 VOI；明确未调用工具不补负标签、反馈进入 pending、更新必须 exactly-once。
- 给出最小可证伪切片：冻结 Laya/AnyJev 表示 + q_exec/q_corr 在线 head + 显式探索，之后再接 VOI 和主 Agent。候选稿：`docs/候选方法_动态JEV数学抽象.md`。
- 当前仍是候选抽象，不代表最终 backbone、损失或算法已裁定；未运行训练或新预测。

### 2026-09-25 10:30 CST — 收窄动态 JEV 数学定义
- 上一版抽象混合了完整 POMDP、JEV 输出和主 Agent，定义不够清楚。本轮改成最小问题：一个分子/性质、多模型逐次调用、最终选择一个已调用结果或拒答、真值延迟返回。
- 明确定义了样本对象、episode、可见历史、动作、provider 正确标签、损失函数、selected-only 延迟反馈和逐条更新；第一版不允许自由融合多个模型结果。
- 明确第一版问题名为 delayed-feedback selective provider selection；Contextual Bandit 只对应单步选择，完整复核/拒答扩展再进入 POMDP。
- 更新候选稿：`docs/候选方法_动态JEV数学抽象.md`。仍是候选形式化，未裁定最终 backbone 或训练方法。

### 2026-09-25 12:00 CST — 动态 JEV 数学建模 v2 落盘
- 将任务重新提升为“带延迟反馈的预算约束主动信息获取 POMDP”，用潜在 provider 返回的联合分布表示工具相关错误，用 belief state 表示 episode 内可用信息。
- 明确定义 observation 与 delayed feedback、CALL/ANSWER/ABSTAIN 动作、Bayes-optimal Bellman 方程、严格 VOI、跨 episode 在线更新、coverage-risk 约束和共享真值反馈组。
- 明确 provider 独立正确率只能作为第一阶段近似，不能直接代表 VOI；JEV/backbone 是对 belief 或 Q-value 的近似，不是数学问题本身。
- 新文档：`docs/动态JEV数学建模_v2.md`，并已加入 `docs/README.md` 文档地图。未运行训练或新预测。

### 2026-09-25 12:30 CST — 数学建模 v2 公式校验
- 将旧的 q_corr/VOI 近似提升为完整参考问题：联合潜在 provider 输出与共享真值、belief state、有限预算 Bellman 方程、严格 VOI、跨 episode 延迟更新和 coverage-risk 约束。
- 明确“最优”仅指给定信息权限/成本/反馈规则下的 Bayes-optimal reference，不声称当前 backbone 或训练器已经最优；补充未知环境参数时的联合后验说明。
- LaTeX 分隔符配对检查通过（display 37/37，inline 55/55）；旧草稿已指向 v2。

### 2026-09-25 13:00 CST — 数学模型清晰度修订
- 明确“每个 provider 单独建模”与“需要 provider 间关系”的边界：单步选择只需边际正确率；多步复核/VOI 必须知道新工具相对已有证据的增量信息，完整参考问题保留联合潜在输出，但第一版实现不要求逐对 provider 建复杂模型。
- 将数学文档全部公式改为 Markdown 可渲染的 `$...$` / `$$...$$`，移除 `\\[\\]` 和 `\\(\\)`。
- 在文档中分开 Bayes-optimal reference 与可实现近似，并把 coverage-risk 作为主评价约束；修复重复章节编号。

### 2026-09-25 14:00 CST — 数学建模 v3：最小符号与真实 case
- 根据反馈重写数学建模：原始对象收窄为 x、候选 provider 集合、动作、即时返回、实验真值、调用预算和反馈延迟；history、正确性、反馈、策略、价值函数全部标为派生量。
- 每个公式下补 z002905 的真实实例化：SMILES、CNN 0.0030845485161989927、Morgan 0.977143406867981、真值 1、对应标签和反馈。
- 明确单步选择只需 provider 边际模型；多步复核才需要条件返回关系。v3 文档：`docs/动态JEV数学建模_v3_最小符号案例.md`。
- v2 已指向 v3；未运行训练或新预测。

### 2026-09-25 真实性审查：v3 最小数学模型 × Task36 replay
- 针对 `docs/动态JEV数学建模_v3_最小符号案例.md` 逐项核对 `src/probe_headless/pharma_bbb_replay.py`、`pharma_bbb_judge.py` 和 Task36 合同测试；未运行训练、新预测或真实 fit/update。
- `python3 -m pytest -q src/probe_headless/tests/test_pharma_bbb_replay.py`：88 passed，0.94s。
- `python3 task/task36-pharma-provider-replay/verify_replay.py --output /tmp/task36_verify_20260925.json`：PASS，47,450 snapshot cells，288 failure cells。
- 真实 z002905 脚本轨迹跑通：B=2，CNN raw `0.0030845485161989927`/label 0，Morgan raw `0.977143406867981`/label 1，终止后 selected feedback 为 0/1，answer_correct=1。
- 已确认 v3 仍有接口缺口：ANSWER 只能提交整数 label 不能提交 raw；失败返回 feedback.correct=None 不能由 `label(o)-y` 记为 0；8 个 unresolved 分子不产生可学习真值；预算耗尽时 selector_input 仍列候选但 CALL 被 API 拒绝；没有真实反馈时间戳/Δ、成本、随机策略/propensity、θ 更新器和 `P(o|h,CALL p)` 估计器。后续需增加 runner、pending queue、成本配置、selected-only learner 和日志版本。
- 可实现性审查版已落盘：`docs/动态JEV数学建模_v4_可实现版.md`。它把环境、理论参考策略和动态算法分开，明确 M0–M2 的最小实验顺序，以及实时性、成本和准确率不能提前声称的边界。
- 验证过程日志：`experiments/logs/math_model_v3_replay_validation_20260925.jsonl`。当前没有训练、新预测或真实 fit/update 结果。

### 2026-09-25 16:30 CST — 动态 JEV 创新性与线性状态递推审查
- 调研 linear attention / fast-weight 的原始推导、PILOT、CAMS、AnyJev 和 delayed-feedback 先例。结论：$S/Z$ 递推、在线线性头、延迟队列和工具路由单独都不是新颖性来源。
- 将候选方法收窄为：selected-only delayed feedback + 随机复核 probe 识别 conditional verification gain + 固定大小线性状态 + risk gate。M0 学 provider correctness，M1 学 verification gain，M2 才让 gain gate 控制追加调用。
- 新增 `docs/动态JEV数学建模_v5_线性状态递推与创新边界.md`，给出递推展开、核加权等价性、$O(D)$ 更新/查询复杂度、propensity/IPW、有效样本量和可声称/不可声称的理论边界。
- 当前“动态 JEV”口径限定为冻结 Laya/AnyJev 表示上的在线状态头；尚未定义或声称更新 backbone 权重。
- 当前仍未训练或运行动态 JEV。下一步先实现 M0 的 selected-only runner 和状态更新合同，再用真实 Task36 回放验证递推与强 baseline。

### 2026-09-25 17:15 CST — 从任务痛点倒推动态 JEV 创新候选
- 将主问题从“做一个实时更新的 JEV”收窄为“在已经看到当前工具证据后，追加某个 provider 是否值得”。静态 provider accuracy、普通 routing bandit 和 JEV calibration 都不能直接回答这个条件收益问题。
- 新增候选方法文档 `docs/候选方法_动态JEV_在线验证增益.md`：把追加调用定义成 intervention，目标是 conditional verification gain；用随机复核 probe + inverse propensity 解决 selected-only 反馈下的可识别性，再用线性状态和 risk gate 做在线决策。
- 当前判断：这条线比单纯“JEV + RLS + 延迟队列”更有方法潜力，但创新性仍需 causal bandit / active model selection 近邻检索和 M0–M3 实验确认。
- 补充检索确认：propensity/IPW、因果 bandit 和 delayed causal inference 也已有先例。因此候选创新必须落在 typed tool verification 的条件收益、反馈合同和风险门控组合上，不能把 IPW 本身包装成新方法。

### 2026-09-25 18:00 CST — M0 selected-only runner 首次 smoke
- 新增 `src/probe_headless/m0_dynamic_jev.py` 和 `src/probe_headless/tests/test_dynamic_jev_m0.py`。runner 只在 Task36 回放外包一层：即时 `q_exec`、终止后延迟 `q_corr`、pending queue、exactly-once、失败/`unscorable` 不进入 correctness 标签、精确 epsilon-greedy propensity。
- 真实 Task36 小 smoke 已跑通：3 个分子、CNN/Morgan 两个具体 provider、B=1、delay=1、epsilon=0.2、seed=0。3 次选择、3 条反馈释放、队列清空；z002905 的 CNN 反馈为 0，后续 Morgan 反馈为 1/0，均只在 release 后更新。
- 定向测试：`95 passed`（新增 M0 合同测试 + 原 Task36 replay 测试）。没有新预测；所有返回均来自已冻结缓存。
- 产物：`experiments/logs/m0_dynamic_jev_smoke_20260925_config.json`、`..._raw.json`、`..._events.jsonl`、`..._summary.json`。
- 当前限制：这是 provider-marginal Beta baseline，不是线性状态 JEV，也不是 M1 verification gain。下一步接入冻结 feature encoder 和随机二次 probe；在此之前不报告 accuracy gain 或方法结果。

### 2026-09-25 18:25 CST — M0 runner contract cleanup and real smoke recheck
- 修正 `src/probe_headless/m0_dynamic_jev.py` 的 episode 观察逻辑：预算分支只读取一次 history，CALL 后直接按本次返回终止，避免重复 `observe()` 和重复 `last` 取值；pending episode 查询改为 `FeedbackQueue.pending_for_episode()`，不再从 runner 读取私有队列。
- 定向合同测试：`python3 -m pytest -q src/probe_headless/tests/test_dynamic_jev_m0.py` → 7 passed；语法检查 `python3 -m py_compile src/probe_headless/m0_dynamic_jev.py` 通过；Task36 原合同回归 `python3 -m pytest -q src/probe_headless/tests/test_pharma_bbb_replay.py` → 88 passed。
- 真实小 smoke：`python3 src/probe_headless/m0_dynamic_jev.py --entity z002905 --entity z000282 --provider deeppurpose_bbb_molnet_cnn --provider deeppurpose_bbb_molnet_morgan --max-calls 1 --round 1 --delay 1 --epsilon 0.2 --seed 7 --output /tmp/m0_task36_smoke_20260925_b.json`。2 个 episode、2 次 selection、2 条 feedback release，`pending_after_flush=[]`；propensity 为 0.9/0.1，release event ID 唯一；第二次 selection 的 learner version 为 2，说明首条 delayed q_corr 在下一 episode 选择前才生效。
- 本次 smoke 的完整可复现记录已写入 `experiments/logs/m0_dynamic_jev_contract_recheck_20260925_{config,raw,events,summary}.{json,jsonl}`。
- 该 smoke 仍是 provider-marginal Beta M0；M0 现在显式拒绝 `max_calls>1`，把多步 VERIFY 留给后续 M1，避免把一次 CALL 误报成多步策略。
- 同步更新 `docs/README.md` 与 `docs/动态JEV数学建模_v4_可实现版.md`：标明 M0 合同实现入口与日志，保留动态 JEV/M1/M2 未实现边界。

### 2026-09-25 19:00 CST — Vault loop attention / KV cache 辅助阅读
- 阅读 `/Users/jingwu/work/vault/loop attention.md`、`kv cache.md`、`Linear Attention.md`、`01-Methods/inference-KV-Cache.md` 和 `01-Methods/inference-kv-cache-management.md`。
- 对主线最有用的不是把 loop/KV cache 直接搬进 JEV，而是四个状态约束：冻结 encoder、当前 query/history 与跨 episode 统计分开、在线状态用 append-only event + exactly-once release、encoder/菜单/标签合同变化时旧状态必须隔离或重建。
- 明确不能过度声称：线性递推、冻结 KV/递归 Q、前缀复用都不是动态 JEV 的算法创新；它们只帮助定义状态生命周期、复用条件和失效边界。
- 新增辅助研究笔记：`research/vault_loop_attention_kv_cache_inspiration_20260925.md`。
- 主线未改变，下一步仍是 M1：在 M0 runner 上加入随机二次 probe、propensity 日志和 `G/Z^g` verification-gain 更新。

### 2026-09-25 19:45 CST — M1 随机二次 probe 与 verification gain 最小闭环
- 新增 `src/probe_headless/m1_dynamic_jev.py`：固定首 provider，在剩余 provider 上按显式 probe probability 随机选择第二 provider；记录 exact propensity；终局由 trainer-side TruthStore 计算 `baseline_loss`、`after_loss` 和 `gain = baseline_loss - after_loss`。
- 新增 `VerificationFeedbackQueue` 与 `VerificationGainState`：延迟反馈 exactly once release；固定 bias + probe-provider one-hot 非负特征；按 IPW 更新 `G += phi * gain / propensity`、`Z += phi / propensity`，查询为 `phi^T G / (phi^T Z + eps)`。
- 新增 `src/probe_headless/tests/test_dynamic_jev_m1.py`，M1 + M0 + Task36 replay 定向回归共 `105 passed`。真实 Task36 frozen replay smoke v2：3 个 episode、2 个 provider、3 个二次 probe、3 条 scored feedback，gain `[1, 0, -1]`，队列清空；propensity 恒为 `1.0`，只验证二次调用与延迟更新。
- 真实 Task36 frozen replay smoke v3：6 个 episode、3 个 provider、probe probability `0.5`，2 个 probe、4 个不 probe，probe propensity `0.25`，2 条 scored feedback exactly once release，gain `[0, 1]`，队列清空；用于检查非平凡 propensity/IPW 合同。
- 补充 v4：4 个 episode、3 个 provider、probe probability `0.8`，4 个 probe，propensity `0.4`，4 条 release exactly once，gain `[1, 0, -1, 0]`；config 记录 claim/falsification condition。
- 日志：`experiments/logs/m1_dynamic_jev_smoke_20260925_v{2,3,4}_{config,raw,events,summary}.{json,jsonl}`；说明：`research/m1_dynamic_jev_runner_20260925.md`。
- 当前边界：固定首 provider、固定 one-hot 特征、没有 gain/risk gate、没有 backbone fine-tune；这轮只验证 M1 反馈和状态合同，没有新工具调用。

### 2026-09-25 20:30 CST — M2 gain gate 合同 runner 与 STOP propensity smoke
- 新增 `src/probe_headless/m2_dynamic_jev.py` 和 `src/probe_headless/tests/test_dynamic_jev_m2.py`：复用 M1 delayed gain 状态；探索分支按显式概率均匀 probe，利用分支仅在当前估计 gain 高于 `call_cost` 时调用，否则 STOP；同时记录 probe/STOP exact propensity。
- 定向 M0/M1/M2/Task36 replay 回归：`109 passed`；`py_compile` 通过。
- 真实 Task36 frozen replay v2：6 个 episode、3 个 provider、探索概率 `0.8`、delay=1、seed=0；4 次 probe、2 次 STOP，probe propensity `0.4`、STOP propensity `0.2`，4 条 scored feedback exactly once release，gain `[-1, 0, 0, 0]`，队列清空；没有新工具/API 调用。
- 真实 Task36 frozen replay v3：6 个 episode、探索概率 `0.5`、delay=1；3 次 probe、3 次 STOP，其中第 6 次选择在 learner version `2` 读取已释放 gain 后进入利用分支，gate open，propensity `0.75`；队列清空。
- 日志：`experiments/logs/m2_dynamic_jev_smoke_20260925_v{2,3}_{config,raw,events,summary}.{json,jsonl}`；说明：`research/m2_dynamic_jev_gain_gate_20260925.md`。
- 当前边界：只有 gain gate，没有 risk gate、历史内容 encoder、VOI 校准或 backbone fine-tune；该 smoke 只验证合同，不报告效果提升。
- 下一步：在同一 frozen replay 上做 always/never/random/M2 的预注册对照，先看净效用、错误率和调用数，再决定 risk gate。

### 2026-09-25 21:00 CST — M2 与基本策略的匹配 frozen replay 对照
- 在同一批 30 个 Task36 frozen 分子、CNN 固定首 provider、Morgan/MPNN 候选上比较 never-verify、always-verify、M1 random probe 和 M2 gain gate；4 个策略均无 unresolved，propensity 全正，release id 唯一，队列清空。
- 结果：never accuracy `0.700`/平均调用 `1.000`；always `0.900`/`2.000`；random `0.833`/`1.433`；gain gate `0.933`/`1.900`。这些是 30-case 管线数字，不是最终方法效果结论；gain gate 使用 one-hot 特征和 zero call cost。
- 产物：`research/m2_baseline_comparison_20260925.md`、`experiments/logs/m2_baseline_comparison_20260925_{config,raw,events,summary}.{json,jsonl}`。
- 下一步：扩大到预冻结 evaluation split，加入多 seed、调用成本和 risk 约束，再判断 gate 是否真的有净收益。

### 2026-09-25 21:30 CST — M1/M2 理论闸门审查
- 审查当前 M1/M2 的 verification-gain 定义、IPW propensity、STOP action 和 delayed feedback。结论：现有 `gain` 在 failure/abstain 时不等于终止损失；`bias + provider one-hot` 不是条件 gain 且会让未观测 provider 被共享 bias 误判为已知；M2 点估计 gate 没有 LCB/risk 保证；`epsilon=0/1` 会破坏 overlap。
- 新增 `research/m1_m2_theory_gate_audit_20260925.md`，给出正确的潜在终止损失定义、IPW 识别式、本地 synthetic oracle/IPW 测试、BBB held-out 回放和单条 Nebula 队列的进入条件。
- 结论：当前 M2 不直接提交 Nebula；先修 loss 合同、冷启动支持、正 propensity 和置信门，再做本地多 seed 验证。

### 2026-09-25 22:00 CST — M1/M2 理论合同修正
- 去掉 `VerificationGainState` 的共享 bias；state 只保留 provider one-hot 的 `G/Z/W2`，未观测 provider 的 estimate 为 `None`。
- 统一 M1/M2 的 task loss：成功且正确为 0，成功但错误为 1，失败/拒答为显式 `abstain_loss∈[0,1]`；gain 保证在 `[-1,1]`，调用成本只进入 gate。
- 新增 provider-specific Kish effective sample size 和保守 Hoeffding radius；M2 利用分支使用 `estimate - radius > call_cost`，日志记录 confidence delta、radius 和 LCB。
- 版本升为 M1 v2 / M2 v2；更新合同测试与研究说明 `research/m1_m2_theory_gate_fix_20260925.md`。
- 本地 M0/M1/M2/Task36/evaluation 回归：`157 passed`。没有连接 Nebula，没有新预测或远程实验。
- 下一步：先做合成潜在损失上的 IPW 收敛/LCB 测试，再决定是否提交单条 Nebula 队列。

### 2026-09-25 23:15 CST — 否定 Kish 半径，改为 HT 置信门
- 合成验证发现旧的 self-normalized IPW + Kish 半径不具备可接受的覆盖：100 seed、n=2000 时覆盖率为 0.90/0.81，旧日志保留在 `experiments/logs/m1_m2_theory_synthetic_20260925_v1/`。
- 将 M1/M2 gate 改为固定分析集 HT 估计：`HT/N`，记录每个候选的 inclusion propensity 和 `sum b_i^2`，使用 martingale Hoeffding 半径；重复在线查看使用 `delta_t = delta*6/(pi^2*t^2)` alpha spending。旧 `G/Z` 只作诊断，不再开 gate。
- 新增脚本 `src/probe_headless/run_m1_m2_theory_synthetic.py`，1000 seeds 的 v5 日志显示 n=100/500/2000 的覆盖率为 0.945–0.970，MAE 随 n 下降；结果报告：`research/m1_m2_ht_gate_20260925.md`。
- M1/M2 版本升级为 v3-ht；M0/M1/M2/Task36/evaluation 回归仍为 `157 passed`。尚未提交 Nebula。
- 下一步：在冻结 BBB 本地确认流检查全量可评分、正 inclusion propensity、HT 与 counterfactual oracle 对齐；通过后再做一条 Nebula dry-run，并等待用户确认后提交。

### 2026-09-26 00:00 CST — 722-case BBB HT 确认
- 扩展到缓存中的 730 个分子，固定排除 8 个 unresolved reference，得到 722 个可评分分析样本；脚本记录排除清单，不把 unresolved 当成数字标签。
- 722-case、10 seeds：探索概率 0.5 时 Morgan/MPNN HT MAE 为 `0.047/0.043`；探索概率 0.8 时为 `0.016/0.023`。队列清空、propensity 全正、全部分析样本可评分。
- 严格 alpha-spent gate 在两组配置都 `0 / 7220` 次打开，说明当前半径在这个规模上过于保守，不能声称已经节省调用；这是一条需要解决的真实方法问题。
- 研究记录：`research/m2_bbb_ht_confirmation_20260925.md`；日志：`experiments/logs/m2_bbb_ht_confirmation_20260925_v{2,4,5}/`。
- 下一步：设计并理论验证更高效的 time-uniform Freedman/固定 horizon gate，再决定是否值得做 Nebula。当前不提交远程性能任务。

### 2026-09-26 01:00 CST — Freedman + 批次 gate 本地确认
- 将 HT 半径改为 Freedman：`V=sum(1/pi-1)`、`B=max(1,1/pi-1)`，半径为 `(sqrt(2 V log(2/delta)) + B log(2/delta)/3)/N`；gate 每 64 个 episode 重算一次，批次内冻结决策，`delta_epoch=delta*6/(pi^2*epoch^2)`。
- 合成 1000 seeds 的覆盖率为 0.951–0.976，MAE 随样本量下降；日志 `experiments/logs/m1_m2_theory_synthetic_20260925_v6/`。
- 722 个可评分 BBB、10 seeds、epsilon=0.8、batch=64：Morgan/MPNN HT MAE `0.0553/0.0225`，gate 平均打开 `376.4/722`，平均调用 `1.904/episode`，终止准确率 `0.8083`；队列、评分、propensity 均通过。静态参考为 never `0.6039`、always Morgan `0.8781`、always MPNN `0.7770`。
- 研究报告：`research/m1_m2_freedman_epoch_gate_20260925.md`；代码版本为 M1 v4-freedman / M2 v4-freedman。
- 结论：现在有一条可以尝试的可运行方案，但它仍是 provider-marginal 状态，不是最终 context-conditioned JEV。下一步只准备一条 Nebula 规模化复现 dry-run；不在远程调参、不建多条线。

### 2026-09-26 01:20 CST — Nebula 单任务 dry-run
- `tools/nebula/neb doctor`：通过；A800 可用 21、backlog 0；HEAD `f06aca9476e6ab46b5e0f8718863dae5d95c330c` 已推远端。
- 已生成一条 dry-run：`jingwu_dynamic_jev_freedman_preflight_001`，1 worker × 1 A800，15 core/240 GB，入口 `src/probe_headless/nebula_m2_preflight.py`，参数为 seed 0、epsilon 0.8、batch 64、confidence delta 0.05、call cost 0。
- 当前没有 `--yes`，没有创建任务、没有占用队列；等待用户确认后才提交这一条。

### 2026-09-25 16:45 CST — v5 安全 overlap 修正与本地全量复核
- 复核发现上一版不能直接提交：M2 利用最优 provider 时，探索命中该 provider 记录了错误的分支 propensity；Freedman 反解少了平方根内的线性项；运行中观察到的 `V/B` 不能直接当确定界；半径裁到 1 也不安全。上一版 coverage、MAE、gate 数字降级为 exploratory，不再作为方法确认。
- 修正 `m1_dynamic_jev.py` / `m2_dynamic_jev.py`：记录完整 mixture propensity；使用策略声明的确定性 `pi_min` 计算保守 Freedman 半径，最多裁到 2；M2 的 alpha spending 按 look 和 provider 数量分配；禁止 runner 使用零 probe/exploration；STOP 的 scored 事件也更新状态版本。
- 新增 ADR `docs/adr/0017-deterministic-overlap-bound-for-online-gain-gate.md`，把证明范围固定为预先排除 unresolved 后的 scored 分析集；Nebula 只做冻结回放代码/环境 smoke，不作方法效果或 coverage 结论。
- 定向动态 JEV/M0/M1/M2/BBB/evaluation 回归：`158 passed`。本地 722-case frozen replay：`n=722`、queue empty、propensity 全正、release id 唯一、gate open `402`、probe `649`、calls `1371`、terminal accuracy `0.796398891966759`；这些数字仅是 smoke 记录。
- 本次本地实验配置、逐 episode 原始 JSONL 和摘要：`experiments/logs/nebula_m2_preflight_v5_local_20260925/`。
- 下一步：提交包含该修正的单条 Nebula smoke 任务；任务完成后只核对运行环境、队列和逐 episode 日志，不进行远程调参。

### 2026-09-25 16:59 CST — Nebula 单任务 smoke 完成
- 第一次提交 `01a0d7c2828e02aad63204b645e196aa` 在平台拉代码前失败，原因是提交参数误写了不存在的 commit SHA；状态为 `FAILED`、0s、无容器、无占卡。wrapper 的绝对路径兼容问题也在此过程中修正并推送。
- 用实际远程 SHA `71af7bb92c6620c7fdf218e451babeadf415f5d0` 重提单条任务 `jingwu_dynamic_jev_freedman_preflight_002`，task `01a0d7c34982fe179179100e4845a8f0`，1 worker × 1 A800，15 core/240 GB，运行状态 `SUCCESS`，worker 退出码 0。
- 远程逐 episode stdout、stderr、最终 status 和配置已保存到 `experiments/logs/nebula_m2_preflight_v5_remote_01a0d7c34982fe179179100e4845a8f0/`。摘要与本地完全对齐：722 scored、queue empty、positive propensity、gate open 402、probe 649、calls 1371、terminal accuracy `0.796398891966759`。
- 这次远程任务只证明冻结回放入口、依赖环境、延迟队列和日志能在 Nebula 跑通；没有训练 backbone、没有调用外部 API，也不支持方法效果或 coverage 结论。

### 2026-09-25 17:21 CST — post-freeze BBB 正式对照回放
- 固定协议并提交代码：`6d11df7d6f78263fecca3245c54c2dbd1425189b`；脚本为 `src/probe_headless/evaluate_bbb_formal.py`，协议为 `research/AAMAS2027_BBB_Formal_Evaluation_Protocol_v1.md`。
- 运行 `20 seeds × 4 call costs × 5 policies`，按 Bemis–Murcko scaffold 切分：616 calibration、106 held-out、429 个 scaffold，80 个 held-out scaffold。
- 原始日志 `experiments/logs/bbb_formal_postfreeze_retrospective_v1_20260925/raw.jsonl` 共 116,792 条；配置和汇总在同目录 `config.json`、`summary.json`。无重复 row，held-out 全部 scored，propensity 最小值 0.2。
- 主成本 `call_cost=0.1`：always Morgan accuracy `0.8962`、net utility `0.3038`；random probe 和 dynamic gate 都是 accuracy `0.7745`、calls `1.7948`、net utility `0.4050`。dynamic gate 在该成本下 gate open 为 0，完全退化成随机探索。
- `call_cost=0.05` 时 dynamic gate 相比 random probe 的 paired net utility 差为 `-0.0136`（95% seed t 区间 `[-0.0252,-0.0019]`），但仍明显劣于 always Morgan；`call_cost=0` 时准确率 `0.8269`，平均调用 `1.9788`，没有形成调用节省。
- 结论：当前 provider-marginal one-hot M2 没有通过主效用标准，不能声称动态 JEV 已有效。问题定位为上下文缺失和保守置信门退化，下一步做 context-conditioned 状态和 no-update/新鲜 scaffold 对照，不在这批 held-out 上调阈值。
- 完整结果说明：`research/AAMAS2027_BBB_Formal_Evaluation_Report_v1.md`。

### 2026-09-25 17:32 CST — v2 no-update 对照与 fresh split 审计
- 升级正式对照脚本和协议：新增 `no_update_gate`，使用与 M2 相同的探索/门逻辑，但将反馈延迟到完整 stream 结束，隔离在线更新本身的收益。代码提交 `3794bd4a0c52d00ffb73fc71483fa95e559474cd`。
- v2 运行 `20 seeds × 4 call costs × 6 policies`，raw JSONL `174,552` 行；无重复 row，held-out 全部 scored，propensity 最小值 `0.2`。结果目录：`experiments/logs/bbb_formal_postfreeze_retrospective_v2_20260925/`。
- 主成本 `0.1` 下 dynamic gate、random probe、no-update gate 完全相同；`0.05` 下 dynamic 相对 no-update 的 utility 差为 `-0.0136`，95% seed t 区间 `[-0.0339, 0.0067]`，不能说明在线更新有稳定收益。always Morgan 仍为最强固定策略。
- fresh split 审计：仓库没有可证明 provider 未见过的真正 fresh BBB 分子池。预注册 GroupShuffleSplit 的 111 test scaffold 可作为 selector structural holdout，但与 BBBP 训练数据 overlap `107/111`，不能称 model-fresh；审计记录为 `research/BBB_fresh_split_audit_20260925.md`。
- 写下下一版最小方法规格：4-cell（TPSA × aromatic ring）context-conditioned JEV，cell-provider 独立 HT/Freedman 状态，`O>=4` 才允许认证门，global prior 只影响探索。文档：`research/AAMAS2027_ContextConditioned_JEV_MVP_Spec_v1.md`。
- 当前不继续调 gate threshold；下一步先按该规格写单测和 synthetic coverage，再做当前回放 pilot，真正 fresh confirmation 需要新增分子、provider snapshot 和独立 truth。

### 2026-09-25 17:45 CST — context-conditioned JEV MVP 状态合同
- 新增 `src/probe_headless/context_dynamic_jev.py`：固定 4-cell `(TPSA>60, aromatic_rings>0)`，每个 cell-provider 独立维护 `N/H/O`、HT/Freedman 半径和 LCB 门；未知 cell 不开门，global prior 不进入 certified 状态。
- 新增 `src/probe_headless/tests/test_context_dynamic_jev.py`：验证 context 不读真值、STOP/未选中仍计 eligible `N`、exact propensity、重复 event 拒绝、未知 cell 不开门和半径随 support 下降。
- 相关回归：动态 JEV/M0/M1/M2/BBB/evaluation + context 共 `162 passed`，`py_compile` 通过。
- ADR-0018（Proposed）和规格文档已写入：`docs/adr/0018-context-conditioned-cell-jev-mvp.md`、`research/AAMAS2027_ContextConditioned_JEV_MVP_Spec_v1.md`。当前模块只完成统计状态合同，尚未接入 M2 runner，也没有冒充效果实验。

### 2026-09-25 17:55 CST — context JEV synthetic coverage 负结果
- 运行 `500 seeds × 1000 episodes` 的固定 potential-gain synthetic coverage，4000 个有足够 support 的 cell-provider 检查全部覆盖，coverage `1.0`。
- 但 `call_cost=0.1` 下 gate 平均打开 `0` 次；平均 STOP `200.218`，Morgan/MPNN action `399.816/399.966`。确定性 overlap Freedman 半径仍过宽，context 分层没有直接解决门退化。
- 第一版把 episode index 当 look index，已保留失败日志 `experiments/logs/context_jev_synthetic_coverage_v1_20260925/`；按 64 episode 一个 look 修正后保留 v2/v3 日志，正式 v3 汇总在 `experiments/logs/context_jev_synthetic_coverage_v3_20260925/summary.json`。
- 结论：统计合同通过，但实时门还不能工作；下一步先研究有覆盖依据的更紧 confidence sequence 或明确更长 calibration horizon，不把 context pilot 接入 BBB 后宣称有效。
- 报告：`research/AAMAS2027_Context_JEV_Synthetic_Coverage_Report_v1.md`。

### 2026-09-25 18:05 CST — exploration-only context JEV pilot
- 新增 `src/probe_headless/m3_context_jev.py`：只用随机探索分支的 gain 更新 4-cell bounded-mean 状态，exploitation 结果只进终局日志；延迟反馈 exactly-once、propensity 和 gate 单测已覆盖。
- 当前回放 pilot/full retrospective：`20 seeds × 4 call costs × 106 heldout`，raw `8,480` 行，日志 `experiments/logs/bbb_context_exploration_only_retrospective_v1_20260925/`。
- 结果：net utility 相对 random/no-update 在 cost `0/0.05/0.1` 分别改善 `0.0528/0.0293/0.0105`（paired 95% seed 区间均低于 0）；cost `0.2` 门关闭并与 random 相同。主成本 `0.1` accuracy `0.7892`、calls `1.8358`、utility `0.3944`，优于 random 的 `0.7745/1.7948/0.4050`，但仍不如 always Morgan `0.8962/2.0/0.3038`。
- synthetic exploration-only coverage：500 seeds × 1000 episodes，coverage `1.0`，cost `0.1` 平均 gate open `96.6`；说明该 alternative 能在保证覆盖的同时打开门，但 BBB 结果仍是 retrospective。
- ADR-0018 已被 ADR-0019 取代；新规格为 `research/AAMAS2027_ContextConditioned_JEV_MVP_Spec_v2_ExplorationOnly.md`。当前候选方法更清楚，但 formal fresh test 仍需新分子/provider snapshot/truth。
- 回归：M0/M1/M2/BBB/evaluation/context/M3 共 `164 passed`，相关 `py_compile` 通过。

### 2026-09-25 18:18 CST — exploration-only 统计审查与修正版回放
- 独立审查发现两个硬问题：gain 实际在 `[-1,1]`，旧 Hoeffding 半径少了范围因子 2；按 batch 分配的 alpha 在 batch 内逐 episode 重查 gate，不能覆盖重复检查。另确认 calibration 均值不自动覆盖 scaffold-heldout 均值。
- 修正 `src/probe_headless/context_dynamic_jev.py`：半径改为 `min(2, 2*sqrt(log(2/delta_look)/(2n)))`，LCB 下截到 `-1`；新增 `freeze()`，M3 每 64 episode 在 batch 起点计算 gate 并冻结到下一个 batch。相关单测与 M0/M1/M2/BBB/evaluation 共 `166 passed`。
- 数学合同与审查记录：`research/AAMAS2027_ExplorationOnly_JEV_Statistical_Contract_v1.md`、`research/AAMAS2027_ExplorationOnly_JEV_Statistical_Audit_20260925.md`；ADR-0020 记录目标和 batch-frozen 决议。
- 500 seeds × 1024 episode 的固定有限总体前缀审计：63,706/63,706 检查覆盖，pointwise/family coverage 均为 `1.0`，日志在 `experiments/logs/exploration_only_finite_population_prefix_audit_v2_20260925/`。
- 修正后的 BBB 回放（20 seeds × 4 cost × 106 heldout）在所有 cost 点 gate `0/2120`，与 random/no-update 完全相同；结果记录为负结果，不再使用旧 v1 的 gate/utility 结论。报告：`research/AAMAS2027_BBB_Context_Exploration_Only_Evaluation_Report_v2.md`。
- 下一步：若要恢复可用 gate，只能研究有覆盖依据的 pooling/层次状态或固定 horizon 评估；不能手工缩小半径。fresh confirmation 仍需要新分子池、provider snapshot 和独立 truth。

### 2026-09-25 18:25 CST — 修正版 commit 复现完成
- 在 `2f42546c` 上重新生成三份可复现日志：固定有限总体前缀审计 `experiments/logs/exploration_only_finite_population_prefix_audit_v3_20260925/`；synthetic `experiments/logs/context_jev_exploration_only_synthetic_v3_20260925/`；BBB 回放 `experiments/logs/bbb_context_exploration_only_retrospective_v3_20260925/`。
- 三份任务均成功。前缀审计仍为 `63,706/63,706` 覆盖；synthetic 为 `4,000/4,000` 覆盖且平均 gate open `0`；BBB 四个成本点 gate 均 `0/2,120`，与 no-update 相同。
### 2026-09-26 — Task49 医药 benchmark 与 baseline 冻结审查
- Owner 确认使用本项目医药结果库自建主 benchmark，并将主任务收窄为单次直接 provider 选择：分子/性质/候选进入，选择一个工具，题后释放 0/1 正确性反馈，后续 episode 实时更新。
- 核对现有 BBB：722 个可评分独立分子，7 个来源家族；最佳固定准确率 0.9155，oracle 0.9806，headroom 0.0651。33 个具体输出中 24 个属于 B3clf 变体，完整 33-output 面板只有 652 个分子，不能把 provider-output cell 当独立样本。
- 现有 Morgan + logistic scaffold-CV selector 为 0.8947，低于 best fixed 0.0208；说明有 oracle 空间但可学习性尚未成立。现有 BBB 已反复用于开发，只定为 development benchmark。
- 冻结 benchmark 三层：BBB-development；至少 BBB/hERG/AMES 的 multi-endpoint evaluation；方法与 baseline 冻结后一次性打开的 frozen confirmation。hERG/AMES 未有独立实验真值前不得用工具共识替代。
- 冻结 baseline：Best Fixed/Oracle；AAAI 2026 Greedy LinUCB；PILOT；BayesianRouter；RouterDC/GraphRouter 静态强对照；相同 backbone 下 Frozen/Online-SGD/Replay-SGD/Proposed。RouterDC 官方仓库未声明许可证，只按论文复现；GraphRouter、Online Multi-LLM Selection、LLMRouter 为 MIT。
- 新增 `task/task49-pharma-benchmark-baseline-freeze/report.md` 与 `sources.jsonl`；新增 `docs/adr/0021-pharma-first-direct-selection-benchmark.md`，将 ADR-0015 标为 Superseded；更新 ADR 索引和任务清单。
- 本次没有训练、预测、API 实验、Nebula 任务或方法效果结果。下一步先完成 v0 数据合同与 hERG/AMES 真值可行性审计，再实现统一 baseline adapter；不打开最终确认集。

### 2026-09-26 — 直接工具选择主线下一步计划
- 核对 ADR-0021、Task49、Task42 反馈协议及 Task45 适配卡；独立只读审查补出方法推导缺项、baseline 适配忠实性、冷启动权限和两类训练曝光问题。
- 新增 `task/task49-pharma-benchmark-baseline-freeze/next_plan.md`，任务清单增加入口。计划为数据审计、baseline 适配、方法推导并行准备，协议审查后小批真实缓存验证，再做多端点与最终确认。没有新增已接受的算法决议。
- 近期产物明确为数据资格表、baseline 适配表、实时更新方法卡；强调随机流与受控变化、可恢复的二值真值、准确率与计算成本、分子统计单位及在线依赖。静态强 router 按 ADR-0021 至少一个执行，优先 GraphRouter。
- 本轮只读核对与计划文档更新，没有训练、预测、外部 API 调用、Nebula 提交或新效果数字。数据发布版、backbone 和最终更新算法仍未定。
- 下一步：从现有 hERG/AMES 真值候选核对端点定义和分子交集，同时补齐 PILOT/BayesianRouter 适配表与单次在线更新推导。

### 2026-09-26 — 持续目标、官方论文草稿与 Task49 三份合同
- Owner 已批准按 Task49 计划持续研究，单条 Nebula A800 任务、逐小任务检查故事/方法/证据；持续 goal 已建立并保持 active，决议 ADR-0022 已落盘。README、CONTEXT、目标、文档地图、研究规范和当前任务入口已对齐 ADR-0021 的直接选择主线，旧复核门/RouterBench 主线保留历史。
- AAMAS 2027 官方模板和主轨/投稿/审稿原页已归档；题目为 Learning to Select Scientific Tools from Outcome Feedback，192 词摘要及一页 PDF 在 `paper/aamas2027/`。构建成功、目视检查完成，明确 internal draft/method and results pending。记录 hyperxmp/字体依赖及官方 sample 同样出现的 ifx 警告；未投稿。
- `data_contract_v0.md` 完成来源/计数口径：BBB 722 带参考标签、724 完整输出行、716 两者兼具；652 属七家族投票面板。hERG/AMES 严格连接 43/129。独立复核 35 来源、2,190 joins、29,200 cells 全通过，记录在 `task/task49-pharma-benchmark-baseline-freeze/audit_validation/run_v1/`。训练曝光、hERG cutoff、再分发权限与最终确认仍未完成。
- `baseline_adapters_v0.md` 记录 PILOT cosine/UCB 与两阶段先验、BayesianRouter 默认脚本偏离、GraphRouter 边权限和平票处理；共同 warm start、零目标标签 cold start 分开，动作不同允许反馈记录不同。在线区间不得对固定日志逐行 bootstrap。算法实现与正式效果仍未完成。
- `method_card_v0.md` 给出固定写入/可学读取的凸组合边界、梯度与反例，不升为主方法；修正 ridge 先验、offset 与 rank-one 更新的目标一致性。下一科学问题是固定表示是否存在瓶颈，而非给标准在线头命名。
- 三个恢复代理均因服务端 403 insufficient balance 停止，主执行者完成后续工作；不把它写作新的独立代理评审。完整复盘见 Task50 report。

### 2026-09-26 — 真实 Laya 输入审计与 A800 表示提取准备
- 按预登记运行真实 tokenizer 和未修改原始序列函数。730 分子 × 3 端点菜单 × 3 配置 = 6,570 序列；BBB 默认 512/192 每条 31/33 个 id 被截断，24 个 B3clf 变体均只剩 b3clf。1024/512 在全部输入上完整，选为下一轮最小通过配置；不宣称表示/准确率已改善。
- Transformers 4.57.1 和 checkpoint 对应的 5.0.0 各运行一遍，逐条 token/marker 等全部结果字段一致。配置、逐条 raw、摘要、实例和版本比较在 `experiments/logs/task49_laya_input_audit_20260926/`；解释在 Task49 `laya_input_audit/report.md`。
- 固定 revision 的 Laya 842,609,210-byte 权重已下载，SHA-256 匹配 `891102d3…c8d86c`；位于 `.bench_runs/task49-assets/laya-1c5edc17/`，不放 git。下载直连失败与镜像成功均记录在 `experiments/logs/task49_laya_asset_transfer_20260926/events.jsonl`；清单在 Task49 `laya_representation_pilot/asset_manifest.json`。
- 新增 `laya_representation_pilot.py` 与 Nebula entry：固定 730 个无标签请求、33 候选、1024/512 输入、candidate-marker hook、原 scorer 重建校验、逐请求数组哈希、延迟/显存记录及失败停止。输入预检成功，4 项信息合同测试通过，日志在 `experiments/logs/task49_laya_representation_prepare_20260926/`。尚未运行模型 forward 或训练。
- A800 阻塞已具体化：`neb doctor --mount_oss` 缺项目 OSS key；本机现有默认 Aliyun profile 的只读访问在连接阶段超时，未验证其项目权限；OSS 公网/internal 入口也超时。未改全局配置，未提交任务、未占卡。实验卡与可执行命令在 Task49 `laya_representation_pilot/run_plan.md`。
- 下一步：恢复项目 OSS 访问或取得已挂载且 hash 匹配的模型目录后，固定已推送 commit，只提交一条无标签表示提取任务；同时推进统一 baseline adapter。随后另行登记指纹/Laya 线性与非线性 head 的开发诊断，保留正式确认集。
- 本轮代码、输入、原始审计日志、三份合同及官方模板源码已通过 `c3849c4e5535ebc12567f9282448afce51ff9c3b` 推送到 `origin/codex/progress-20260924`。只提交本轮相关文件，未纳入工作区其他历史修改/删除。模型权重仍在本地忽略目录，PDF 在本地 build；A800 任务未提交，持续目标保持 active。

### 2026-09-26 — 在线 baseline 核心实现与现任务诊断准备
- 新增 `pharma_online_baselines.py` / `pharma_selection_stream.py`：Greedy LinUCB H=1、PILOT cosine online kernel、BayesianRouter Gaussian TS kernel；后两者尚无离线 fit，不称完整复现。标准递推不作为 Proposed。
- 20 项数值/时序检查通过；64 个真实 BBB development 分子 × 3 kernel × delay 0/1/4，共 576 次选择/终局/到期反馈；实际失败保留。另 6 条固定 B3clf 失败控制通过。最终参数与独立批量解最大均值差约 2.17e-15，证据见 `experiments/logs/task49_online_adapter_20260926/`、Task49 `online_adapter_v1/`。
- hERG 追溯确认 FP-ADMET cardiotoxicity 与 liability 是两套模型；当前 endpoint 33 对应 liability。作者 NCATS 原始 train/validation 数据已下载并固定 commit/hash，当前只作来源材料。cutoff 与 exact training mapping 仍不完整，见 Task49 `endpoint_audit/report.md`；不扩张正式可评分结论。
- 发现旧 0.8947 来自七家族投票、预知缺失掩码与 fixed 回退，不能承担现 33-provider 主线可学习性证据，已在 Task49 顶部追加更正。
- 新建 Task49 `fingerprint_diagnostic_v1/`：722 分子、33 具体 provider，所有失败保留；按 scaffold 分五折，内部 validation 选 checkpoint。对照 linear/MLP/BayesianRouter offline-loss adaptation/direct-property，2 seeds，固定预算。代码/input/实验卡已备齐，24 项定向检查通过，日志 `experiments/logs/task49_fingerprint_diagnostic_20260926/preflight_v1/`。尚未训练；准备推送固定代码后单条 git-only A800，无需 OSS。
- Laya 存储入口仍缺，已请 Owner 配置项目 `.env` 或提供已有挂载模型目录；未请求重复实验许可。独立 baseline 审查代理始终 pending_init，已中断等待，无独立审稿结果。来源代理后续句柄缺失，主执行者接续材料。
- 下一步：doctor/dry-run 通过后依 ADR-0022 提交一条指纹学习诊断，下载完整逐样本日志与 checkpoint，按实验卡决定表示路线；不中断长期目标，不把本轮 smoke 当成方法效果。

### 2026-09-26 — 指纹诊断 A800 结果恢复、独立核验与 calibrated switch 复核
- Task `01a0de1cf322cd9ee1ead48f41569649`（`jingwu_task49_fingerprint_diagnostic_002`）已在 A800-SXM4-80GB 成功结束，worker 退出码 0；提交 commit `a50d0164cb8d598817fb2483581120cb76ebf95c`，训练日志记录 wall time 18.26s，平台任务运行约 3m47s。
- 通过 Nebula all-logs 下载完整 worker/scheduler stdout/stderr；没有用受大小限制的 tail 结果替代。远程事件计数为 config 1、split 5、epoch 8000、fit_complete 40、checkpoint_manifest 40、checkpoint_chunk 1970、evaluation 5776、complete 1。
- 恢复脚本核对 40 个 checkpoint 的压缩/原始 hash、state_dict、独立 NumPy score、provider/status/reward/fixed/oracle 记录和 validation-best epoch；首次 `recovered_v1` 仅在 JSON 导出时因 NumPy int64 失败，保留现场，修正后 `recovered_v2/summary.json` status=verified。提交 git tree 与执行源码 18 项 hash 复核通过。
- 结果：train-only Best Fixed `661/722=0.915512`；具体 provider Oracle `720/722=0.997230`，fixed 错误 61 个中 59 个存在其它可救 provider、2 个不可救。Linear BCE `0.872576/0.879501`，MLP BCE `0.896122/0.890582`，BT+CLS `0.887812/0.894737`（两个数字依 seed），直接性质 MLP `0.759003/0.760388`；均未超过 fixed，预登记继续条件不满足。
- 逐分子配对显示 MLP 两个 seed 分别救回 20/24 个 fixed 错误，同时损害 34/42 个 fixed 正确选择；说明当前指纹选择器不能安全识别何时切换。
- 按独立分析卡完成 validation-selected calibrated switch（不训练、不调用新 provider、不打开确认集）：MLP seed 0/1 outer `0.894737/0.909972`，switch rate `0.6994/0.3366`，10 个 outer fold 均未超过 fixed。结论：问题不只是 raw argmax 过于激进，停止在 Morgan 指纹上继续调 gate。
- 新增报告与分析入口：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md`、`selective_switch_card.md`、`src/probe_headless/analyze_pharma_fingerprint_diagnostic.py`、`src/probe_headless/analyze_pharma_selective_switch.py`；Task49/Task50/任务清单/next_plan 已追加边界和更正。
- 当前回头检查：benchmark 的真实选择空间得到具体 provider Oracle 支撑，但静态指纹可学习性未成立；没有把负结果包装成动态 JEV 效果。下一步是恢复 Laya 模型目录后做一次无标签表示提取，并在同一反馈合同下补齐 PILOT/BayesianRouter 离线 fit 与 Frozen/Online-SGD/Replay-SGD 对照；最终确认集继续关闭。

### 2026-09-26 — Laya 模型接入 OSS 与在线 BayesianRouter 诊断入口校验
- Owner 提供项目 OSS 凭据后，未把 AK/SK 写入 git、命令或日志；仅写入根目录忽略文件 `.env`。选用 `ai4sci-develop-storage`，endpoint `oss-cn-zhangjiakou.aliyuncs.com`，对象前缀 `jingwu/benchmark/task49/laya-1c5edc17/`；`ai4sci-develop-fast` 暂未使用。
- 官方 ossutil 2.4.0 dry-run 显示 5 个文件、846,195,574 bytes；正式上传返回 exit 0。远端逐对象 size 与本地一致，五个文件 OSS CRC64 全部一致；模型对象为 multipart。核对证据在 `experiments/logs/task49_laya_asset_transfer_20260926/ossutil_upload_v2/`，`neb doctor --mount_oss` 已通过。
- 直连桶虚拟主机的 TLS 在本机被提前断开，path-style 被 OSS 拒绝；上传仍由 ossutil 完成，只通过本机临时 Host 保留转发访问根 endpoint。这个绕行不进入 Nebula 任务配置。
- 新增在线诊断入口 `nebula_fingerprint_online_diagnostic.py` 与预登记卡：同一 BT 离线先验下比较 frozen 与 selected-only BayesianRouter，delay 0/1/4；修正反馈计数、冻结策略无 posterior update、随机种子和 provider-shape 检查。相关 28 项定向测试通过。
- 下一步：用已推送 `5c5035531112b72706cfdfe24b9c12874fc646d2`，单条提交 Laya 无标签表示提取任务；先保存 dry-run，再观察真实 A800 任务并下载全量日志。Laya 结果出来前不做选择器效果结论。

### 2026-09-27 — Laya A800 真实运行失败定位与 OSS FUSE 导出修复
- 修正版 Laya 任务 `01a0de6f07a6e2892d3250d34029961a` 已真实进入 A800，输入合同、模型权重、GPU 型号和 scorer reconstruction 都通过；第一次 shard 导出在 `np.savez_compressed(/data/oss_bucket_0/...)` 收尾时失败，报 `OSError: [Errno 95] Operation not supported`。原因是 OSS FUSE 支持顺序写但不支持 ZIP 收尾所需的 seek；这不是模型或 CUDA 失败。worker exit 138，未得到表示结果，完整日志和 hash 在 `experiments/logs/task49_laya_representation_20260926/remote_v2/`。
- 已修 `laya_representation_pilot.py`：先在本地临时目录生成 npz，再以顺序流复制到 OSS，逐文件检查大小和 SHA-256；新增回归测试，Laya 输入/在线诊断相关 33 项测试通过。
- 下一步：提交该修复后的新 commit，改用新的输出前缀 `laya_features_v3`，只重提一条 Laya A800 任务；保留 v2 失败前缀和日志，不覆盖。若 v3 成功，才进入 Laya 表示的冻结 head/online baseline 诊断。

### 2026-09-27 — Laya 表示提取 v3 成功并完成远端核验
- 任务 `01a0de81ce4269ebebc6000c40ce9ae2` 在一张 A800-SXM4-80GB 上成功完成，源码固定为 `e9d394377862a2c873735f4cd29d5d72670d9d0a`。模型 hash、输入合同和运行时版本已记录；worker 事件为 config 1、runtime 1、batch_complete 92、complete 1。
- 730 个 BBB 开发请求、33 个 provider 导出为 1024 维 candidate-marker 表示；输出是 92 个 shard（91×8 + 1×2），另有 `events.jsonl`、`raw.jsonl`、`summary.json`。运行 45.07 秒，峰值 allocated 2.60 GB。
- OSS 前缀 `ai4sci-develop-storage/jingwu/benchmark/task49/laya_features_v3/` 列出 96 个对象（含目录标记），95 个内容对象的 CRC64 已逐项读取；完整 Nebula 日志已下载并脱敏。证据入口：`experiments/logs/task49_laya_representation_20260926/remote_v3/`，结果说明：`task/task49-pharma-benchmark-baseline-freeze/laya_representation_pilot/report_v3.md`。
- 这一步只证明固定 Laya checkpoint 的表示导出可运行，不包含正确率、baseline 比较或在线学习结论。下一步：在相同 scaffold 划分和反馈合同下做 Laya 冻结 head 及 online baseline 的开发诊断；确认集仍关闭。远端结果未用本地慢速 relay 全量复制，后续任务直接从同一 OSS 前缀读取。

### 2026-09-27 — Laya 静态诊断预提交缺陷被拦截
- 静态 Laya 诊断任务 `01a0dea10f70b9d5e93818e34e37b229` 完成 doctor/dry-run 并进入 QUEUEING 后主动停止；没有创建容器、占用 GPU 或产生训练/评价行。检查发现入口在导入 torch 前没有设置 `CUBLAS_WORKSPACE_CONFIG`，先修复后再提交。
- Nebula 的平台状态/调度日志会自动回显运行时环境中的平台凭据。已对当前 Laya v1/v2/v3 与静态诊断日志做脱敏，新增 `tools/nebula/sanitize_logs.py`，包装器输出也统一过滤；脱敏后未检出 OSS 或平台凭据明文。历史已推送 commit 仍可能保留平台运行时值，需在平台侧轮换/失效相关凭据，未改写 git 历史。
- 脱敏回归与静态诊断测试共 22 项通过。下一步：提交本次确定性与日志保护修复，使用新 commit 只重提一条静态 Laya 诊断；确认集继续关闭。

### 2026-09-27 — 静态 Laya 诊断输入合同修正
- 重提任务 `01a0dead94a3e03814da01dc4635a67f` 成功进入 A800，但在训练前的父清单校验失败：代码错误地要求 provider 菜单按字典序排列，而冻结的父清单保留历史菜单顺序。任务没有 `input_validated`、epoch、evaluation 或 complete 事件，不能算实验结果。
- 修正为保留父清单顺序，并把已核验的 Laya 排序菜单显式重排到父菜单；增加父清单顺序和重排回归测试。定向测试现为 24 项通过。失败证据和事件计数在 `experiments/logs/task49_laya_static_diagnostic_20260927/retry_v2/failure_summary.json`。
- 下一步：推送这次合同修复，更新运行计划中的实际 commit，只提交一条最终静态诊断任务；仍不打开确认集。

### 2026-09-27 — 静态 Laya 诊断 v3 完成恢复和独立核验
- 最终任务 `01a0debb7da647dcee0b42564fcc8ced`（`jingwu_task49_laya_static_003`）在 A800-SXM4-80GB 成功结束，使用 commit `e453c0647f58b533bcd075245b7ec1d721920f1e`。完整 Nebula 日志已下载，不使用受大小限制的 tail 代替；worker 事件为 config 1、split 5、epoch 8,000、checkpoint_manifest 40、checkpoint_chunk 1,644、evaluation 5,776、fit_complete 40、complete 1。
- 新增 `recover_pharma_laya_static_diagnostic.py`，恢复 40 个 checkpoint 并核对压缩/原始 hash、state dict、validation-only 最佳 epoch、provider/status/reward/fixed/oracle 记录和 OOF 分母。95 个 OSS 内容对象重新下载到恢复目录（92 个 shard + 3 个 JSON，92,857,171 bytes），用独立 NumPy 前向重算全部 5,776 条分数；最大绝对差 `1.64032e-4`，在预先记录的 `5e-4` CUDA/CPU float32 容差内。汇总在 `experiments/logs/task49_laya_static_diagnostic_20260927/retry_v3/recovered_v5/summary.json`，人读报告在 `task/task49-pharma-benchmark-baseline-freeze/laya_static_diagnostic_v1/report.md`。
- 具体 provider 的 Best Fixed 为 `0.915512`，Oracle 为 `0.997230`。Laya 的 Linear BCE 为 `0.871191/0.879501`，MLP 为 `0.885042/0.858726`，BT+CLS 为 `0.875346/0.868421`，Direct property 为 `0.713296/0.713296`（两个数依次为 seed 0/1）；所有方法均低于 fixed，预登记的 `+0.02 且 4/5 正 fold` 闸门未通过。
- 裁定：不继续在 Laya 静态 head 上调参，也不把这个结果写成动态 JEV 效果。保留 Laya 表示作为在线适应的可选输入；下一小任务回到同一反馈合同下补齐 PILOT、BayesianRouter、Frozen/Online-SGD/Replay-SGD 的离线/在线对照。最终 confirmation 集继续关闭。
- 恢复器、报告、ADR 和小型恢复摘要已提交本地 commit `c289d236`；推送时 GitLab SSH 直连超时，现有本机 `127.0.0.1:7890` 通道也被关闭，当前远端仍停在上一 commit。下次网络通道可用时只需推送这一条 commit，不要重新跑实验。

### 2026-09-27 — baseline 下一步独立审查
- 完成 `task/task49-pharma-benchmark-baseline-freeze/baseline_next_audit_20260927.md`：Greedy LinUCB 在线递推和真实缓存冒烟可运行；PILOT 仍只有 online kernel，随机 prior 不构成正式 baseline；BayesianRouter 有 Morgan BT+CLS + selected-only 诊断入口但尚无结果；GraphRouter/RouterDC 仅审计，matched Frozen/Online-SGD/Replay-SGD 尚未统一实现。
- 核对信息边界：在线 policy 只收到冻结 query 与已释放的所选 reward，延迟队列和失败记账通过；当前 runner 没有把每题候选菜单传入决策，若最终菜单不固定需先修统一 runner。23 项在线相关测试通过，未提交 Nebula、未改数据或实验结果。
- 建议下一小任务先跑已有 BayesianRouter frozen-vs-online Morgan 诊断，再补 PILOT 离线 fit、统一候选菜单/事件合同和 matched SGD 对照；不把标准 rank-one、LinUCB、cosine/UCB 或 Thompson 更新包装成创新。

### 2026-09-27 — BayesianRouter 在线诊断首次提交失败（不计科学结果）
- 预登记的 frozen BT prior vs selected-only BayesianRouter 任务 01a0defa9e9e1a811a3811764244928a 已完整取回日志。worker 在执行任何应用事件前因旧 commit 的 package-relative import 报 ImportError: attempted relative import with no known parent package；Nebula 随后以 138/retryable 包装退出并因 backoff=0 结束。没有训练、选择、反馈、评价或 complete 事件，不把它当成 eviction-only 结果，也不把它计入效果。
- 完整日志已脱敏保存在 experiments/logs/task49_fingerprint_online_20260927/full_download_sanitized_v2/；原始下载包删除，避免保留平台环境中的运行时凭据。失败报告和结构化摘要在 task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/online_bayesianrouter_effect_report.md 与 online_bayesianrouter_effect_failure.json。
- 后续 commit 已增加文件入口 fallback 与确定性 CUDA 初始化；小规模 A800 import/integration smoke 成功（2 streams、1 fold、1 seed、delay 0、2 epochs），但只证明入口/反馈链路可运行，不是方法效果。
- 下一步：使用已推送的修复 commit，按原实验卡只重提这一条完整诊断；先恢复全量日志并核验事件、权重和 per-sample 记录，再决定是否继续该 baseline。确认集保持关闭。

### 2026-09-27 — BayesianRouter 在线诊断完成：标准 selected-only 更新未通过闸门
- 修复入口后的任务 `01a0e0c65614a23dee31880441509c29` 在 A800 正常完成，提交 commit `ffc5fd2f43113da83497b57cdc07a13b82f07428`。完整事件为 config 1、epoch 2,000、stream_complete 60、decision 8,664、feedback 8,664、complete 1；独立恢复核对奖励、状态、延迟释放、frozen/online 版本和三份源 hash 全部通过。
- 10 个 paired fold-seed stream 的均值：delay 0 frozen/online `0.89136/0.88859`（Δ `-0.00277`），delay 1 `0.89136/0.88927`（Δ `-0.00208`），delay 4 `0.89136/0.89068`（Δ `-0.00068`）。每个 delay 只有 4/10 paired stream 为正；预登记要求至少 2/3 delay 的均值为正，实际 0/3，闸门失败。
- 在线 kernel 平均 stream p50 选择约 256μs，frozen 约 22μs；只作描述性成本记录。结论不是“动态学习有效”，而是标准 selected-only Bayesian update 在当前 Morgan BT 表示上不足，保留为强负 baseline，不调参、不打开 confirmation。
- 证据：`experiments/logs/task49_fingerprint_online_20260927/recovered_v2/summary.json`、`events.jsonl`、`full_download_sanitized_v3/`；结果报告 `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/online_bayesianrouter_effect_result.md`；决议 ADR-0025。
- 下一步：先补齐同一反馈合同下的 Frozen/Online-SGD/Replay-SGD matched controls，明确表示更新与 delayed selected-only feedback 的接口，再决定是否提出新方法；confirmation 继续关闭。

### 2026-09-27 — 接入 Nebula 实验交互 skill 的运行契约
- 阅读并应用 `/Users/jingwu/.codex/skills/nebula-experiment-pipeline/SKILL.md` 及其 run contract / Nebula operations 说明。它解决的是实验流程和证据管理：提交前冻结配置、输入清单、Git commit、任务/应用 ID、状态阶段、完整日志下载、脱敏收集和失败分类；不替代本项目的科学恢复器、结果复核或方法设计。
- 将已完成的 `task49_fingerprint_online_20260927` 补齐标准契约：`config.json`（修正为真实 job 名）、`input_manifest.json`、渲染后的脱敏 `submission.json`、`remote.json`、规范化 `events.jsonl`、`results.json`、`summary.json`、`collection.json`。结果全部由 `recovered_v2/summary.json` 派生，没有新增实验或伪造指标。
- 复核结论不变：第二次任务成功，第一条是 startup/import 基础设施失败；selected-only BayesianRouter 在线更新在 3 个 delay 上均未通过预登记闸门。完整日志仍通过已验证的 `nebula-cli logs download --all` 获取并脱敏保存；渲染器本身只生成计划，不负责实际提交，真实提交仍由 `tools/nebula/neb` 完成。
- 下一步：把同一契约用于 Frozen/Online-SGD/Replay-SGD 对照实现；实验仍一次只提交一条，confirmation 集保持关闭。

### 2026-09-27 — 匹配的 Online-SGD / Replay-SGD baseline 适配器
- 按 BayesianRouter 负结果后的下一小任务审查，先不提出新 JEV；固定每个 fold/seed 的 BT checkpoint、64 维 hidden 和 provider score head，只隔离是否更新、是否回放。
- 新增 `src/probe_headless/pharma_matched_sgd.py`：Online-SGD 每条已释放 selected reward 做一次 BCE-with-logits 行更新；Replay-SGD 使用 FIFO 容量 64、batch 上限 8、seed 20260927，从已释放样本回放并做一次平均梯度更新。两者都只保存决策时复制的 selected feature，未选 provider 与未释放反馈不进入状态。
- 新增 5 项针对性测试，连同已有 online baseline 测试共 `26 passed`。测试覆盖初始权重、梯度数值、延迟 feature copy、FIFO 上限、随机种子、wrong-arm 拒绝。
- 用真实 BBB cache 做本地 adapter smoke：6 条流（Online/Replay × delay 0/1/4）、384 次真实缓存选择，所有流 decision/terminal/feedback 守恒，64 次反馈更新，真实失败保留；结果明确标记为 `new_scientific_predictions=0`，不作效果结论。证据在 `experiments/logs/task49_matched_sgd_smoke_20260927_v2/`。
- 写入适配合同与 ADR-0026。当前仍未占用 A800；下一步先把该 adapter 接入与现有 BT 训练/checkpoint 同步的 Nebula 小 smoke，再决定是否跑完整 development 诊断。Replay 的额外计算成本必须单独报告，不能包装成创新。

### 2026-09-27 — matched SGD Nebula A800 smoke 完成并恢复
- 按预登记 `matched_sgd_smoke_v1`，用 commit `a83e7d7c511b0c8c533bc51ed2d87c446da8c838` 在 Nebula A800 提交一条任务：`01a0e0fe66bb9ce3a704a3ac4b2d9801`，application `xdl-6ab8930bdb5c`，1 fold × 1 seed × 1 delay × 2 epochs；worker 正常退出，未打开 confirmation。
- 应用事件完整：config 1、epoch 2、decision 435、feedback 435、stream_complete 3、complete 1。三条轨迹各 145 个 evaluation 决策；Frozen 更新数 0，Online/Replay 更新数各 145，状态均有限，源 hash 和输入 hash 独立恢复通过。结果只记为 adapter smoke，`scientific_result_available=false`，不能解释为效果。
- 新 skill 的完整日志接口已实际验证：`nebula-cli logs download --all` 成功取回 664,232 bytes。第一次本地脱敏误把短环境值替换进 JSON 数字，已作废该派生文件；同一任务重新下载后用只处理长项目密钥和整行环境变量的规则脱敏，最终恢复使用 `full_download_sanitized_v2/`，原始归档未保留。
- 契约文件已补齐：`remote.json`、`events.jsonl`、`results.json`、`summary.json`、`collection.json`；独立恢复器为 `recover_pharma_matched_sgd_diagnostic.py`，证据入口在 `experiments/logs/task49_matched_sgd_smoke_20260927/`。
- 下一步：先审查 smoke 的输入/事件边界，再按同一契约准备 5-fold × 2-seed × delay 0/1/4 的完整 development 对照；Replay 的更高 batch 计算成本单独计时，不把它包装成创新。

### 2026-09-27 — 长期目标落盘
- 将持续目标写成 ADR-0027：AAMAS 2027 前完成可复现的医药 direct-selection benchmark 与动态 JEV，并依次通过数据资格、方法/信息隔离、真实 Nebula、独立恢复、冻结后 confirmation 五道闸门。
- 当前落点是匹配 SGD 的 A800 smoke 已通过；下一道闸门是审查 smoke 后再跑完整 development，对 confirmation 不做提前承诺。

### 2026-09-27 — replayable evidence smoke 通过
- 为 matched SGD 入口增加每个 fold/seed 的压缩 prior/features snapshot，并在恢复器里加入独立 NumPy 的 Frozen/Online/Replay 选择与更新重放；本地 27 项测试和状态对照通过。
- 用 commit `532570b81f62a481af2d671b4d47444515844d18` 提交 A800 smoke `01a0e114ee465bc61c841e5b4dd0af8c`，worker exit 0。完整日志下载、脱敏和恢复通过：config 1、epoch 2、fit_complete 1、decision/feedback 435/435、stream_complete 3、complete 1；确认集关闭。
- 第一次脱敏派生曾把短环境值替换进 JSON，已废弃并重新下载；最终证据在 `experiments/logs/task49_matched_sgd_smoke_20260927/replayable_v4/`，不把 smoke 数值当效果。

### 2026-09-27 — matched SGD full development 完成并通过独立恢复
- 按预登记卡提交唯一完整任务 `01a0e12060b770fc6bf2c6f34e418328`，commit `2017967daf80e5f58fa8c51ac2f2bb4e09f53c5d`，5 folds × 2 seeds × delays 0/1/4，worker exit 0。
- 完整事件：config 1、epoch 2,000、fit_complete 10、decision/feedback 12,996/12,996、stream_complete 90、complete 1。完整日志已用 `nebula-cli logs download --all` 取回并脱敏；恢复器第一次因残留 smoke 的 3-stream 检查失败，修复后使用同一日志恢复 `recovered_v2/summary.json`，独立 NumPy 重放全部选择和更新通过。
- development pooled accuracy：Frozen `0.8913`，Online-SGD `0.8991`，Replay-SGD `0.8964`；Best Fixed `0.9155`。Online/Replay 虽改善 Frozen warm start，但仍分别低于 Best Fixed `0.0164/0.0192`，不支持把普通 selected-only 更新写成动态 JEV 创新。ADR-0028 已记录裁定，confirmation 继续关闭。
- 报告与分析：`experiments/logs/task49_matched_sgd_development_20260927/report.md`、`analysis_v1.json`、`results.json`、`recovered_v2/`；可复现分析入口 `src/probe_headless/analyze_pharma_matched_sgd_development.py`。下一步回到表示/信息优势方法设计，不在这组普通 head 上调参。

### 2026-09-27 — stream drift 噪声审计与下一方法筛选
- 新增 `src/probe_headless/audit_pharma_stream_drift.py`，在冻结 BBB development 输入上比较 block size 16/32/64 与 500 次 within-fold permutation null。winner changes 与 null 接近：16 为 7.20 vs 7.48，32 为 4.00 vs 3.63，64 为 1.60 vs 1.73；margin 也没有超过 null。结论：不以 order-only change point 作为动态方法来源。证据 `experiments/logs/task49_stream_drift_audit_20260927/analysis_v2.json`，报告 `task/task49-pharma-benchmark-baseline-freeze/stream_drift_audit_v1/report.md`。
- provider/local-memory 信息筛选已完成且只读冻结面板：fit-positive provider transfer AUC 五折均值 `0.7366`，fit/eval correlation 均值 `0.8060`；Morgan top-5 memory AUC `0.6248`，随机邻居 `0.4691`。证据 `experiments/logs/task49_information_screen_20260927/`，报告 `information_screen_v1/report.md`。这只证明有信息，不证明在线效果。
- provider-transfer replay 用已恢复 matched-SGD 的真实冻结 BT prior/64d hidden features 重跑。低秩 `W h` v1 为 `0.8893/0.8893/0.8886`，低于 Frozen `0.8914`；scalar transfer v2 为 `0.8955/0.8941/0.8927`，低于 independent bias `0.8983/0.8948/0.8989`；surprise+margin gated v3 为 `0.8941/0.8948/0.8934`，三个 delay 中两个低于 independent。已按 ADR-0031 停止 provider-transfer 主线，保留负证据。
- context residual memory 零机时 replay：每 provider FIFO64、top-5 nonnegative cosine、只写入 selected released frozen residual。真实 memory 为 `0.8927/0.8969/0.8976`（delay0/1/4），independent bias `0.8948/0.8948/0.8927`，random memory `0.8920/0.8948/0.8907`。通过候选筛选但不是正式效果；下一步先写 replayable smoke，验证 memory snapshot/延迟恢复，再跑完整 development。证据 `experiments/logs/task49_context_memory_replay_20260927/`，报告 `context_memory_replay_v1/report.md`，方法卡 `context_memory_method_card_v1.md`，ADR-0032。
- 本轮没有 Nebula、新训练、新 provider 调用或 confirmation；所有回放只使用已恢复的真实冻结先验和 selected-only evaluator reward。新增 ADR-0029 至 ADR-0032，并更新 Task49 `next_plan.md`。
- 下一步：实现 context-memory adapter 的事件/状态快照与独立 NumPy recovery，做单条 A800 replayable smoke；smoke 失败则先修，不读取 confirmation，不直接跑完整 development。

### 2026-09-27 — context memory adapter 本地 smoke
- 新增 `src/probe_headless/pharma_context_memory.py`：Frozen provider logit + 每 provider FIFO residual memory，top-5 nonnegative cosine 读取；只在 feedback release 时写入 selected provider，保存 decision feature snapshot，支持 candidate menu、版本和 exactly-once 校验。
- 新增 `src/probe_headless/run_pharma_context_memory_smoke.py` 和 4 项定向测试。真实 BBB cache 64 分子 × frozen/online × delay 0/1/4，共 384 次决策；本地事件回放和独立重放全部通过，online memory 每流 64 条写入，frozen 为 0。证据 `experiments/logs/task49_context_memory_smoke_local_20260927/run_v1/`，定向测试总数 31 passed。
- 该 smoke 只证明 adapter/延迟/状态恢复合同可运行，没有新 provider 调用或科学效果结论。下一步将固定 commit，在 Nebula A800 只提交一条同样的 replayable smoke；平台日志完整下载和脱敏后再决定是否进入完整 development。

### 2026-09-27 — context memory 校准审查与 v2 回放
- 在提交 A800 smoke 前做独立方法审查，发现 v1 将 BT 排序分数当 correctness 概率，并把 probability residual 直接加回 raw ranking score；同时 smoke 的 Morgan64/随机 prior 与正式 hidden snapshot 不一致，原 replay 也复用了同一个 adapter，不能称独立验证。审查记录：`task/task49-pharma-benchmark-baseline-freeze/context_memory_replay_v1/independent_audit_20260927.md`。
- 未提交原计划的 context-memory A800 smoke，避免为错误定义消耗机时。新增可回放日志/独立 NumPy recovery 修复，但只在本地验证代码合同；没有新 provider/API 调用。
- 按新协议把每个 provider 的 BT score 在 outer-fold fit 区做固定 affine logistic calibration，再在 correctness probability 空间做 selected-only residual memory。已用已恢复的 5-fold × 2-seed × delay 0/1/4 真实 development evidence 回放：Frozen calibrated `0.8831`，independent `0.8769/0.8769/0.8713`，context memory `0.8755/0.8727/0.8700`，random memory `0.8727/0.8769/0.8699`（依次为 delay 0/1/4）。context memory 三个 delay 均未超过 independent，delay 1 低于 random，按预登记闸门停止。结果 `experiments/logs/task49_context_memory_calibrated_replay_20260927/`，报告与 ADR-0033 已写入。
- 下一步：不重跑旧 context memory；先设计一个有明确统计语义、能在校准 correctness score 和 selected-only delayed feedback 下被 CPU 证伪的新候选。候选通过后再做一次 A800 硬件 smoke；confirmation 继续关闭。

### 2026-09-27 — AG-JEV 先验支持闸门
- 审查下一候选 Audit-Gated Calibrated JEV。它用随机审计解决 selected-only 选择偏差，统计语义比前几条干净，但当前 722 条 BBB development 题不支持它的置信门：即使审计概率 1、context 合并为 1 格，每 provider 仅约 21.9 条样本，末次 Hoeffding 半径约 1.05，已大于 residual 最大值 1；四格 context 更只约 5.5 条/格-provider，半径为 2。
- 因此没有跑 CPU 效果或 A800；候选文档已标记停止，ADR-0034 记录计算。下一步仍是方法设计，不通过缩小半径或扩大 context 追分；先找在现有样本支持下可识别的校准 selected-only 状态压缩机制。

### 2026-09-27 — 校准 provider-transfer v2 CPU 回放
- 为排除“旧 transfer 只是 score 语义错误”的解释，先做 per-provider affine logistic calibration，再用 fit-only provider residual covariance 预测未选 provider 的 probability correction；同一 feedback、delay 和 5-fold×2-seed 回放比较 Frozen、independent、真实 transfer 和置换 transfer。
- 结果：Frozen `0.8831/0.8831/0.8831`；independent `0.8762/0.8769/0.8630`；transfer `0.8755/0.8783/0.8817`；permuted `0.8706/0.8720/0.8755`（delay 0/1/4）。真实关系优于置换，但三个 delay 均低于 Frozen，delay0 低于 independent，按闸门停止；不跑 A800、不调参。报告/ADR-0035 与结构化证据已写入 `task/.../calibrated_provider_transfer_v2/` 和 `experiments/logs/task49_calibrated_provider_transfer_20260927/`。

### 2026-09-27 — Task36 round1/round2 provenance audit
- 审计入口：`src/probe_headless/audit_pharma_snapshot_rounds.py`；机器结果：`experiments/logs/task49_round_snapshot_audit_20260927/results_v1.json`；报告：`task/task49-pharma-benchmark-baseline-freeze/round_snapshot_audit_v1/report.md`。
- 730 个分子、33 个 provider 中 32 个有 round2；24 个 provider 有 raw value 变化，16 个有 label 变化，0 个有 status 变化。变化集中在 B3clf。
- `data/tool_results/manifest.json` 明确说明 B3clf 两遍是为记录 PaDEL 概率抖动，ADMET-AI 两遍逐字节一致；没有 checkpoint、部署版本或更新时间证据。
- 权威 census replay1/replay2 的四个模型都有完整 receipt：输入、checkpoint/config/source 和 output hash 均相同，概率漂移为 0，两轮只相隔约 40 秒。
- 裁定 ADR-0036：现有 round1/round2 只能作为 repeatability / execution-drift 压力测试，不能写成工具随时间更新；动态主线从 delayed selected-only outcome feedback 建模。没有新 provider 调用、训练或 Nebula 任务。
- 下一步：基于这个边界提出一个统计语义自洽的新在线更新候选，先做 CPU 级可证伪回放；通过后才考虑一条 A800 smoke，并保持 confirmation 关闭。

### 2026-09-27 — propensity residual JEV 候选连续否证
- `propensity_residual_jev_v1` 预登记了固定 epsilon=0.20 的均匀探索、已校准 Frozen correctness residual、HT propensity 权重和 ridge=20。CPU 回放 5 folds × 2 seeds × delay 0/1/4：Frozen calibrated `0.8831`，Audit-Frozen `0.8748`，PCR-JEV `0.8493`；相对同探索对照三个 delay 均为负。证据 `experiments/logs/task49_propensity_residual_jev_20260927/results_v1.json`，报告/ADR-0037，确定性重跑 cmp 通过。
- 为减少探索成本，预登记 `margin_propensity_jev_v1`：只在 fit 中位 top-two margin 以下的题探索（epsilon=0.5）和更新。结果 Frozen `0.8831`，Margin-Audit-Frozen `0.8658`，Margin-PCR-JEV `0.8616`；三个 delay 相对同探索对照均 `-0.00416`。证据 `experiments/logs/task49_margin_propensity_jev_20260927/results_v1.json`，报告/ADR-0038，确定性重跑 cmp 通过。
- 两条线都停止，不调探索率、ridge 或 margin 门限，不申请 A800。HT/propensity 作为实现部件保留；当前问题转向“冻结 score 本身的置信区间/校准残差是否有可识别的在线信号”，先做 fit-only 信息筛选。

### 2026-09-27 — confidence-bin residual CPU 筛选正式复核
- 将 confidence-bin residual 筛选从临时 inline 计算固化为 `src/probe_headless/audit_pharma_confidence_bins.py`，输入为冻结 fingerprint diagnostic 的 722 条 development 题和已恢复的 5-fold × 2-seed BT snapshots；不读取 confirmation，不产生新 provider 调用。
- 正式结果在 `experiments/logs/task49_confidence_bin_screen_20260927/results_v1.json`：Frozen calibrated `0.8830986590`；区间数 2/3/4/5/8 的 corrected accuracy 分别为 `0.8796551724/0.8817193487/0.8824042146/0.8761925287/0.8796312261`，平均 delta 分别为 `-0.0034434866/-0.0013793103/-0.0006944444/-0.0069061303/-0.0034674329`。严格按 `delta > 0` 统计，正流数最多 `4/10`，没有达到预登记的稳定正信号闸门。
- 同一输入重复运行后与结果文件逐字节 `cmp` 通过；输入 SHA `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`，evidence SHA `4f7bed1d53aabd093943ac18e525b5cf62ffe56250237ecb0b74f42d2d1c6b9e`。
- 裁定 ADR-0039：不实现 score-bin residual 在线版本，不申请 A800。原报告中正流数已按正式脚本更正；这里的负结果只否定当前简单区间校准，不否定其它表示或主 Agent 的验证/拒答路线。
- 下一步：收束当前 score-residual 候选族，先做一次主方法/信息边界审查，决定是转向有独立可观测反馈的 verification/refusal 扩展，还是在没有新信息来源前暂停动态 JEV 主线；confirmation 继续关闭。

### 2026-09-27 — 33-provider 菜单粒度敏感性审计
- 由于 33 个 provider 中有 24 个是 B3clf 配置变体，先预登记固定的 7-provider 家族代表菜单：`admet_ai_1_4_0`、`deeppurpose_bbb_molnet_morgan`、`graphb3`、`lgbm_bbb`、`b3clf_dtree_common`、`fpadmet`、`admetlab_3_0`。代表清单不看标签、不看 evaluation 结果，仍对应具体可调用 provider。
- 在同一 5-fold scaffold split 和 Morgan 1024-bit 上，33-provider 菜单的 Best Fixed / fit-only 线性 head / Oracle 为 `0.91557/0.89618/0.99723`；7-provider 代表菜单为 `0.91557/0.90308/0.99030`。7-provider 线性 head 虽高约 `0.00691`，但五个 outer fold 全部低于 Best Fixed，oracle headroom 反而更小。
- 机器证据：`experiments/logs/task49_menu_sensitivity_20260927/results_v1.json`；脚本 `src/probe_headless/audit_pharma_menu_sensitivity.py`；重复运行逐字节 `cmp` 通过。无新 provider/API 调用、无 Nebula、confirmation 关闭。
- 裁定 ADR-0040：33 个具体 provider 继续作为主 benchmark；7-provider 代表菜单只作菜单粒度敏感性附录。该审计排除了“线性 head 负结果主要由 B3clf 变体造成”的简单解释，但没有产生动态 JEV 效果。
- 下一步：基于目前几条动态候选均未超过强固定策略的证据，做一次主线收束审查，优先判断是否转到有独立可观测反馈的 verification/refusal 扩展；在新信息来源未确定前不申请 A800。

### 2026-09-27 — PILOT-adapted 离线先验与真实缓存接口 smoke
- 补齐了此前缺失的 PILOT offline fit：`src/probe_headless/pilot_offline_fit.py` 按预登记协议先做 fit-only provider-conditional query triplet projection，再冻结 projection 学 provider preference vectors；online 仍复用已有 cosine/UCB/ridge kernel。明确标记为医药任务适配版，不称官方逐行复现。
- 新增 `run_pilot_offline_fit_smoke.py`、`recover_pilot_prior_adapter_smoke.py` 和 4 项定向测试。fold0 fit 区 444 分子、33 provider、100 epochs 得到 12,550 triplets、33,234 correct-wrong pairs，fit pair accuracy `0.852531`；同 seed 先验文件逐字节重跑一致。定向测试与已有 online baseline 共 `24 passed`。
- 接入真实 BBB cache 的 fold0 evaluation 前 64 题，delay `0/1/4` 三条流各有 64 decision/terminal/feedback，最终版本 64；独立恢复重新核对 prior hash、cohort、provider binding、delay 和版本通过。带源码 hash 的最终证据：`experiments/logs/task49_pilot_prior_adapter_20260927/run_v2/`，离线先验结果：`experiments/logs/task49_pilot_offline_fit_20260927/run_v2/`，报告：`task/task49-pharma-benchmark-baseline-freeze/pilot_offline_fit_v1/report.md`。
- 这是 baseline 适配 smoke，不是效果实验：没有新 provider/API 调用、没有 Nebula、没有 evaluation 效果结论，confirmation 关闭。
- 下一步：把 fit-only PILOT prior 接入统一 development runner，先做 CPU 小回放和信息权限审查，再决定是否占用一条 A800 做完整 baseline 诊断。

### 2026-09-27 — PILOT-adapted development replay 完成
- 将 fit-only PILOT-adapted prior 接入 5-fold scaffold 流，2 个 fit seed、delay `0/1/4`，Frozen 与 Online 共 60 条真实 BBB development stream；逐事件日志、input/source hash、prior 文件和独立恢复均通过。
- pooled accuracy：PILOT Frozen `0.882964`；PILOT Online delay0/1/4 为 `0.882271/0.880194/0.885042`；Online−Frozen 为 `-0.000704/-0.002773/+0.002074`，每个 delay 只有 `4/10` paired stream 为正；Best Fixed `0.915512`。
- 预登记要求至少两个 delay 均值为正，未通过。PILOT online 在 delay4 的小幅改善仍低于 Best Fixed 约 `0.03047`，不申请 A800、不调 alpha/epoch/precision，不打开 confirmation。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/pilot_adapted_development_v1/report.md`；证据：`experiments/logs/task49_pilot_adapted_development_20260927/run_v1/`；ADR-0041 已记录裁定。
- 这条结果进一步排除了“只要换成现代离线 preference prior，普通在线更新就能解决当前 direct selection”的解释。下一步回到主方法信息边界，优先审查 verification/refusal 扩展或新的可观测反馈来源。

### 2026-09-27 — post-call confidence 信息筛选留下正信号
- 新增 `audit_pharma_postcall_confidence.py`，只读取已缓存的 provider 原生 numeric probability 和独立 system-success；confidence 定义为 `2*abs(p-0.5)`。在 5 个 scaffold-heldout evaluation folds 上，160 个 provider-fold AUC 的 median/mean 为 `0.760306/0.758333`。
- 固定 `admetlab_3_0` 的全量 accuracy 为 `0.915575`；按 evaluation confidence 排名前 50%/80%/90% 的 accepted accuracy 为 `0.961111/0.956747/0.950590`，5/5 folds 都高于全量。结果：`experiments/logs/task49_postcall_confidence_screen_20260927/results_v1.json`，重复运行逐字节 cmp 通过。
- 边界明确：confidence 在 provider 调用之后才可见，不能用于 direct pre-call tool selection；evaluation 内排序只是信息筛选，不是部署阈值。ADR-0042 将它登记为下一条 fit-only verification/refusal 候选，不把筛选数字称为在线效果。
- 下一步：先用 fit 区固定 confidence quantile/风险阈值，在 evaluation 流中运行合法的 accepted/refused 回放，再考虑 selected-only delayed online calibration；confirmation 继续关闭。

### 2026-09-27 — fit-only post-call verifier threshold 通过 CPU 闸门
- 新增 `src/probe_headless/replay_pharma_postcall_verifier_threshold.py` 和独立恢复器 `recover_pharma_postcall_verifier_threshold.py`。每个 outer fold 只用 fit 区选择 Best Fixed provider，并用 fit 区 confidence `c=2*abs(raw_probability-0.5)` 的 0.8 coverage 分位拟合阈值；evaluation 只做接受/拒答，标签只用于计分；随机拒答为同目标 coverage 对照。
- 五折结果：verifier mean accepted accuracy `0.960185`，random `0.915127`；pooled `0.961606` vs `0.915078`，gain `+0.046528`，五折全部高于随机，满足预登记的 `>=4/5` 与 `>=0.02` 闸门。
- 证据在 `experiments/logs/task49_postcall_verifier_threshold_20260927/run_v1/`，独立恢复 `recovered.json` 通过。没有新 provider 调用、没有 Nebula、confirmation 仍关闭。报告与裁定为 `task/task49-pharma-benchmark-baseline-freeze/postcall_verifier_threshold_v1/report.md` 和 ADR-0043。
- 为排除 confidence ties 带来的实际 coverage 差异，恢复器又按 verifier 每折实际接受数匹配随机对照，pooled accepted accuracy `0.916230`，仍低于 verifier `0.961606`。
- 边界：这是 post-call verification/refusal 的静态证据，不是 direct pre-call selection，也不是动态 JEV 效果。下一步先做 selected-only delayed feedback 下的在线 calibration/risk-state CPU 候选；CPU 不过就不申请 A800。

### 2026-09-28 — confidence 门控第二工具复核通过开发闸门
- 新增 `src/probe_headless/replay_pharma_confidence_escalation.py` 与独立恢复器，固定第一/第二 provider 为 fit-only Best Fixed 排序，沿用已冻结的 fit-only 0.8 confidence 阈值；第一工具低置信度时追加第二工具，随机追加使用相同 fallback 数量和 seed `20260928`。
- 五折 pooled accuracy：Best Fixed `0.915512`，confidence escalation `0.923823`（`+0.008310`），随机追加 `0.904432`；平均调用次数 `1.206`，fallback rate `0.206`，4/5 folds 不低于 Best Fixed、4/5 folds 高于随机。两工具 oracle `0.966759`，第二 provider 单独使用 `0.889197`。
- 固定数量随机 null 的 500 个 seed 均值 `0.911191`、标准差 `0.005553`，只有 `2.2%` 不低于 confidence escalation。证据 `experiments/logs/task49_confidence_escalation_20260928/run_v1/`，独立恢复与确定性重跑通过。
- 边界：这是调用后验证流程，不是 selected-only 在线更新；正式协议沿用前一项 0.8 阈值。此前 `.5/.8/.9` 快速屏幕已单独标记为 exploratory，不把它当确认性结果。报告与 ADR-0044 已写入。
- 下一步：先做 selected-only 延迟反馈下的动态阈值/第二 provider 可靠性状态 CPU 回放，并与静态 escalation 共用调用预算；不通过就停止，不申请 A800。

### 2026-09-28 — direct 动态 JEV 信息边界收束
- 第一性原理审查写入 `task/task49-pharma-benchmark-baseline-freeze/fresh_method_review_20260928.md`：在当前 pre-call selected-only 信息合同和无真实版本漂移证据下，没有可以诚实称为新动态 JEV 主方法的候选；普通 head、Bayesian/PILOT、transfer、memory、propensity 和 Laya 负结果已经覆盖主要解释。
- 持久决议 ADR-0045：不再继续换在线 head；最多做一次固定 DG-SafeJEV CPU 控制，若不过就停止 direct 动态主线。研究主线转为 direct-selection benchmark 加调用后 confidence verifier/escalation，动态风险状态仍需 CPU 证伪。

### 2026-09-28 — AAMAS 题目与摘要按当前证据更新
- 题目改为 `Selecting and Verifying Scientific Tools with Delayed Outcome Feedback`，摘要明确区分 direct pre-call selection 的负结果与 post-call confidence escalation 的开发结果，不再承诺尚未成立的新动态 JEV。
- 摘要中的数字来自已恢复结果：Best Fixed `0.9155`，confidence escalation `0.9238`，平均调用次数 `1.206`，随机 escalation `0.9044`。官方模板 `make draft` 编译成功并目视检查一页 PDF；内置编译器因工作区 class 文件在 `reference/template` 下未自动搜索而报告 `aamas.cls not found`，不改模板文件。
- 源码在 `paper/aamas2027/main.tex`、`abstract.tex` 和 `README.md`，已推送到远程。

### 2026-09-28 — 延迟动态 verifier 风险门 CPU 闸门失败
- 新增预注册 `task/task49-pharma-benchmark-baseline-freeze/dynamic_verifier_v1/preregistration.md`、回放 `src/probe_headless/replay_pharma_dynamic_verifier.py` 和独立恢复 `src/probe_headless/recover_pharma_dynamic_verifier.py`。只使用冻结的 722 条 BBB development、33 provider、五折 scaffold split 和已有缓存；没有新 provider/API 调用、没有 Nebula、confirmation 关闭。
- 真实失败语义补齐：第一工具没有 numeric confidence 时强制追加第二工具；第二工具同样失败时最终 reward 为 0，失败不伪造 confidence-bin 状态。原始首跑因此停下，修正后完整回放成功。
- 三个 delay 的 dynamic final accuracy 均为 `0.900277`，静态 q=.8 escalation 为 `0.923823`；平均调用次数 `1.490305`，风险门全程保守回退，成本上限 `1.25` 和准确率闸门均失败。每个 delay 的 5 折安全覆盖条件为 `4/5`，但不足以通过总闸门。
- 证据 `experiments/logs/task49_dynamic_verifier_20260928/run_v2/`，独立恢复逐事件通过，run_v2/run_v3 结果和恢复文件逐字节一致。报告 `task/task49-pharma-benchmark-baseline-freeze/dynamic_verifier_v1/report.md`，ADR-0046 已写入并更新索引。
- 裁定：不调 alpha/delta/n0/batch/分位数，不上 A800；dynamic verifier 作为负结果保留，主线继续静态 post-call verifier/escalation。只有新的独立可观测反馈出现后才重新设计动态状态。

### 2026-09-28 — 新建独立决议文件夹与第一份统一评价原则（待双方确认）
- 新建 `docs/决议/`，包含管理规则、编号索引和 `0001-研究故事线与评价标准.md`。
- 第一份决议把当前必须遵守的原则写清：故事线与方法论要充分、合理、完备、自洽且有明确创新切口；主 benchmark 要公认且有权威性；baseline 要能完成同一任务并具备时效性、新颖度或经典性；创新点不能因小问题被单方面推翻，任何修改必须先讨论并经用户与 Codex 双重确认。
- 文件当前状态为“待双方确认”，不会自动覆盖 `docs/adr/` 中已有决议；用户确认后再改为“生效”。

### 2026-09-28 — 公共 benchmark 权威性审计完成
- 使用 agent-reach/Exa 和公开原始文件核验 TDC ADMET、B3DB、PharmaBench。当前冻结 722 条 BBB development 真值与 TDC `BBB_Martins` 逐条对齐：完整分子重叠 695，最大碎片重叠 `722/722`，标签一致率 `100%`；TDC 原始 2,039 行、1,975 unique 与 ADMET benchmark 规模一致。
- B3DB 重叠 609 条、一致率 `95.57%`；PharmaBench BBB 重叠 559 条、一致率 `90.16%`。不同公开集存在系统性标签差异，不能直接混成一个真值。
- 产物：`research/benchmark_authority_audit_20260928/report.md`、`source_manifest.json`、本地只读源文件、`src/probe_headless/audit_public_benchmark_authority.py` 和结构化结果 `experiments/logs/task49_benchmark_authority_audit_20260928/`；run_v1/run_v2 逐字节一致。
- 当前判断：BBB 可以写成“公共 TDC 真值锚点 + 我们新增的 provider routing 评测层”，但 hERG/AMES、公用 tool-use benchmark 和 provider 训练泄漏仍需单独审计。该证据补充决议 0001 草案，不自动把它改为生效。

### 2026-09-28 — AMES 公共真值链补审完成
- 用冻结的 TDC AMES 源 `task/task49-pharma-benchmark-baseline-freeze/data_audit/sources/ames.tab` 与 PharmaBench AMES 做 canonical 逐分子审计；TDC 7,278 行/7,255 unique，PharmaBench 9,139 行，重叠 7,255 条。7,222 条一致，33 条全部是 `TDC=0 -> PharmaBench=1`，没有反向差异。
- 这支持 TDC AMES 作为公共真值锚点，但不支持把 PharmaBench AMES 与 TDC 逐条合并。BBB 也补充确认 503 条 `1->1`、55 条 `0->1`、1 条 `0->0`。
- 产物：`src/probe_headless/audit_public_ames_authority.py`、`experiments/logs/task49_ames_authority_audit_20260928/run_v1/` 与 `run_v2/`、更新后的 `research/benchmark_authority_audit_20260928/report.md`。脚本运行前写入 config，两次结果指标一致。
- 当前判断：公共 truth 层可以候选采用 TDC BBB + TDC AMES；Task36 仍是 provider routing/replay 层。hERG 端点仍需独立处理，provider training-membership audit 仍未完成。
- 下一步：审计 TDC hERG 的端点语义与当前 43 条可对齐样本，再写 benchmark 两层协议草案；不把本次审计当成用户已确认的决议。
- hERG 覆盖与 lineage 审计已完成：TDC hERG 655 行/648 unique，当前 730 池严格覆盖 43、最大碎片覆盖 44；与 NCATS/Siramshetty 文件有 128 条训练集和 5 条验证集 canonical overlap，但不能据此宣称 assay 等价或 FP-ADMET 精确训练成员。证据 `experiments/logs/task49_herg_authority_audit_20260928/`，两次 substantive fields 一致。
- 已先写候选草案 `task/task49-pharma-benchmark-baseline-freeze/public_benchmark_contract_candidate_v1.md`：TDC truth layer、provider-output/replay layer、信息边界、baseline 合同和发布停止条件均分开；状态仍是候选，不改 ADR-0021 或 `docs/决议/0001`。

### 2026-09-29 — 官方 TDC Group split 冻结审计与旧开发集重叠发现
- 完成 TDC ADMET Group archive 的只读下载、哈希核对和重复审计；archive SHA-256=`bd0005246cb6f1672333c28ad202112a8bf071ab65d847848715030308faf1f1`。冻结文件在 `research/benchmark_authority_audit_20260928/sources/tdc_admet_group_split_v2/`：BBB train_val/test 为 1,624/406 行、1,581/394 unique；AMES train_val/test 为 5,821/1,457 行、5,802/1,453 unique。两个 endpoint 都无 invalid SMILES、无 canonical overlap、无非手性 Bemis--Murcko scaffold overlap；文件内部重复行分别为 BBB 55、AMES 23，后续必须显式固定去重规则。
- 新增 `src/probe_headless/audit_tdc_admet_group_split.py` 的 panel hash 记录，并完成 `bbb_v1/v2/v3`、`ames_v1/v2` 审计。第三次 BBB 审计发现旧 722 条 fingerprint development panel 与官方 BBB test 严格 canonical 重叠 147 条、最大碎片重叠 152 条；因此官方 test 不能直接作为旧开发工作的 untouched confirmation。该发现已写入 `research/benchmark_authority_audit_20260928/report.md` 和 `task/task49-pharma-benchmark-baseline-freeze/public_benchmark_contract_candidate_v1.md`。
- 完成 33-provider training-membership 只读审计：当前 panel 的独立资格为 0；GraphB3 为 G3（本地 train/test 表可核对但 checkpoint 绑定仍缺），ADMET-AI、DeepPurpose、LGBM-BBB、24 个 B3clf 为 G2，FP-ADMET/ADMETlab 为 G1。报告：`research/provider_training_membership_audit_20260929.md`。24 个 B3clf 仍按一个高度相关的 B3DB family 计数。
- 科学裁定：本轮只推进 benchmark 资格和证据，不提交 A800，不打开 confirmation；候选合同和 `docs/决议/0001-研究故事线与评价标准.md` 仍未获得新的双方确认，不改其决议状态。
- 验收：split audit 脚本 `py_compile` 通过；重复结果 substantive fields 一致；canonical/scaffold disjoint 和 147/152 overlap assertions 通过；未调用 provider/API、未生成新科学预测。
- 下一步：提交并推送本轮定向变更；随后设计“新方法冻结后未读取 confirmation + provider 输出/训练成员绑定”的最小可行重建计划，先补资格证据，再决定下一条 Nebula 任务。

### 2026-09-29 — fingerprint diagnostic 完整日志二次取回与独立恢复
- 只读查询任务 `01a0de1cf322cd9ee1ead48f41569649` 得到 `SUCCESS`，worker 退出码 0，application `xdl-6ab7d642730a`；未重启、未补提任务。
- 通过 `env -u ... nebula-cli logs download TASK --all` 重新下载完整 archive。四个容器日志的 bytes/hash 与既有 `remote_v2/download_manifest.json` 一致；没有发现 `LTAI`、`accessKey`、`secret` 模式。二次收据：`experiments/logs/task49_fingerprint_diagnostic_20260926/remote_v2/download_all_02_receipt.json`。
- 用 `recover_pharma_fingerprint_diagnostic.py` 对二次下载的 worker stdout 独立恢复，得到 `recovered_v2_full/summary.json`；1 config、5 split、8,000 epoch、40 fit、40 checkpoint manifest、1,970 checkpoint chunks、5,776 evaluation、1 complete 全部通过。摘要 hash=`45ffa7131a35a8c4407d233445b4911c9a293058ad3194d97536c47892574906`，与既有正式 `recovered_v2/summary.json` 相同，8 个方法/seed 的 accuracy 与 failure 计数一致。
- 更新 `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md`，明确 full-log 证据、重复恢复和当前负结果边界；没有把静态诊断改写成动态 JEV 效果。
- 下一步：只保留一次正式恢复摘要作为论文证据；回到 benchmark/provider 资格闸门，制定新的未读取 confirmation 与 provider checkpoint 绑定方案。当前不追加 A800 训练。

### 2026-09-29 — TDC BBB split 内部标签冲突审计
- 对官方 Group split 做了重复 canonical 的标签一致性检查。BBB `train_val`：1,624 行、1,581 unique、40 个 duplicate groups、8 个 `0/1` conflict groups；BBB `test`：406 行、394 unique、12 个 duplicate groups、2 个 `0/1` conflict groups。AMES train/test 分别只有 19/4 个 duplicate groups，均无标签冲突。
- 冲突分子逐组保存于 `experiments/logs/task49_tdc_admet_split_audit_20260929/bbb_v4/conflicting_groups.json`；审计脚本 `audit_tdc_admet_group_split.py` 现在同时记录 duplicate group、extra row 和 conflicting label group。
- 这改变了 benchmark 下一步：不能只决定“是否去重”，还必须决定 BBB 冲突组是排除还是按重复测量建模；禁止按第一行、多数票或模型表现静默合并。候选 confirmation 闸门已写入 `task/task49-pharma-benchmark-baseline-freeze/fresh_confirmation_plan_v1.md`，状态仍为草案，不改变决议。
- 更新了公共 benchmark 审计和候选合同；尚未训练 provider、尚未提交 A800。下一步先由 Gate 0 固定冲突处理，再选择能绑定 checkpoint 的最小 provider menu。

### 2026-09-29 — confirmation 候选的 molecule/scaffold 两种隔离口径
- 在 BBB 官方 test 清理 2 个标签冲突 canonical group 后，392 个 unique 中有 147 个与旧 development panel 严格 canonical 重叠；排除后剩 245 个 molecule-disjoint candidate。
- 旧 panel 的非手性 Bemis--Murcko scaffold 还覆盖 235 个 clean test 分子，因此真正 scaffold-disjoint candidate 只有 157 个。该计数不是效果结果，而是 confirmation eligibility 审计；收据在 `experiments/logs/task49_tdc_admet_split_audit_20260929/bbb_v4/fresh_confirmation_overlap.json`。
- 已把两种口径写入 `fresh_confirmation_plan_v1.md` 与 benchmark authority report：论文不能把 245 个分子直接叫结构独立；如采用强口径，应以 157 个为候选并重新生成 provider outputs。
- 下一步：完成 Gate 0 的冲突处理决策草案和 provider checkpoint 绑定清单；在此之前不打开 test correctness、不提交新的 A800 训练。

### 2026-09-29 — TDC BBB 冲突语义外部来源核对
- 查询 TDC Dataverse file metadata 和 B3DB 原始论文后，仍没有证据能把 BBB 的 10 个 `0/1` canonical 冲突组解释成重复实验，或判断哪个标签优先。TDC metadata 只确认公开 archive/file 的来源、版本和更新时间；B3DB 论文只说明更广泛来源存在重复 InChI 与清洗流程。
- 记录 `research/benchmark_authority_audit_20260928/tdc_conflict_semantics_note.md`：在原始实验来源补齐前，Gate 0 只能选择排除冲突组，或找到来源后按重复测量建模；禁止第一行、多数票或模型驱动合并。
- 这一步没有替换任何标签、没有训练和没有远程任务。下一步是整理冲突组的原始来源追踪，若追不到则采用排除方案并重新生成 provider training manifest。

### 2026-09-29 — 冲突组回溯到 TDC Drug_ID
- 将 10 个 BBB conflict group 回溯到完整 `tdc_bbb_martins.tsv`。每组都对应两个不同 `Drug_ID`（例如 `acetylsalicylate/aspirin`、`levodopa` 两条记录），说明冲突来自 TDC 的不同记录/别名进入同一 canonical 分子；但 TDC 文件没有每条记录的原始实验出处，仍不能据此决定标签优先级。
- 已把 raw TDC entries 和 source hash 加入 `bbb_v4/conflicting_groups.json`，并更新 `tdc_conflict_semantics_note.md`。当前最干净的候选仍是排除冲突组，除非找到原始实验来源。

### 2026-09-28 — 决议 0001 经双方确认正式生效
- 用户明确同意将第一份决议落为统一标杆；`docs/决议/0001-研究故事线与评价标准.md` 状态改为“生效”，`docs/决议/README.md` 索引同步更新。
- 从现在起，故事线、方法论、benchmark、baseline 和创新点的重大修改都必须先经过讨论和双方确认；实验结果只能提供证据，不能单方面推翻生效决议。

### 2026-09-28 — 建立三主线文档、三评价标准和唯一生效版本结构
- 按双方确认的治理要求新建 `docs/研究主线/`：`生效/` 下固定三份研究主文档和三份对应评价标准，`版本/` 下保存 v001 历史副本。六类各只有一个生效入口；方法、benchmark 和创新仍明确标注候选/未决，不把文档生效误写成算法已证。
- 新建 `docs/研究主线/README.md`，说明主文档、task/research/experiments 证据层和决议层的分工。更新根 README、CONTEXT 和 `docs/README.md`，旧目标/综合进度/数学候选/质量规则保留但不再与六份主文档并列定口径。
- 新建并生效 `docs/决议/0002-研究文档分层与唯一生效版本.md`；`0001-研究故事线与评价标准.md` 标记为被替代，历史内容拆入六份主文档和评价标准，不静默删除。
- 结构检查：生效研究文档 3 份、评价标准 3 份；版本目录没有“生效”状态；新增文档 `git diff --check` 待提交前复核。
- 未完成：将这套结构接到后续论文写作、实验卡和任务清单；方法最终冻结和 confirmation 仍未完成。
- 下一步：检查链接/公式/状态一致性，定向提交并推送；后续所有主线修改只从六份生效文档进入。

### 2026-09-28 — 验收标准来源分级审计
- 核对 AAMAS 2027 主轨征稿、reviewer guidelines 和 submission instructions：官方明确评价 originality、significance、soundness、reproducibility、clarity、relevance、presentation、state of the art，并要求说明应用语境、agent 作用、benefit/feasibility/impact；核心内容不能依赖补充材料。
- 复核既有 `research/AAMAS_2023-2026_论文调性与DynamicToolRouting选题调研.md`：已读 2023–2026 的代表性 AAMAS 论文，但文件明确不是全会计量或完整“优秀论文统计”。因此没有把现有三份验收标准包装成官方评分表。
- 新增 `research/AAMAS2027_验收标准来源审计_20260928.md`，把三份验收标准逐条分成：官方硬要求、代表性论文规律、项目特定防守线和治理原则；在三份生效评价标准中加入来源等级与官方链接。
- 结论边界：公共 benchmark、现代 baseline、selected-only 信息隔离、provider lineage、恢复脚本等是本项目加严条件，不是 AAMAS 对所有论文的明文要求；“必须公认 benchmark”应写成项目策略。
- 下一步：若要把“历届优秀论文规律”做成更强证据，按 2023–2026 每年 5–10 篇近邻主轨论文建立编码表，再决定是否升级评价标准措辞。

### 2026-09-28 — 三份验收标准升级为严格投稿前闸门 v002
- 根据 AAMAS 2027 官方硬要求、AAMAS 2023/2024/2025 代表性获奖/提名论文和本项目的泄漏/真值风险，重写三份评价标准。每份现在都有：硬性淘汰条件、0/1/2 证据评分、关键项最低分、总分线、Reviewer 追问、失败后的处理和双人独立评分规则。
- 新增 `research/AAMAS2027_验收标准逐条溯源矩阵_20260928.md`：逐条区分 O（官方要求）、P（代表性论文规律）、C（项目加严）、G（治理）。矩阵明确 AAMAS 没有公开固定的 baseline 数量或录用分数，三份文档是内部 go/no-go 闸门，不是官方判定器。
- 修正来源边界：application context/benefit/feasibility/adoption/impact 是 IASI 应用方向的明确要求，对 LEARN/GAAI 不能冒充统一硬条款；本项目把它作为加严审查项。
- 生效评价标准升为 v002，v001 保留为历史版本；三份研究主文档仍为 v001。严格标准不保证录用，只把明显不够锋利、不够新、不够可复现或实验不公平的稿件挡在投稿前。
- 下一步：按 v002 的评分表逐项审查当前故事线、方法和 benchmark；任何一项拿不到原文/代码/日志证据，先补证据再写论文。

### 2026-09-28 — 用 v002 严格标准反查当前项目
- 新增 `research/AAMAS2027_验收标准自检_20260928.md`。当前项目不通过投稿前闸门：故事线 S3/S5/S7 未到 2；方法 M4/M5/M7 未到 2；benchmark 触发 BBB 冲突、provider checkpoint 资格和 confirmation 三个硬闸门。
- 该自检证明 v002 不是装饰性清单：它阻止我们把动态 JEV 候选、development replay 和 provider 未完成资格写成最终贡献或主结果。
- 下一步按 Gate 0–5 补齐冲突语义、provider 绑定、方法冻结和独立 confirmation，再重新双人评分；不通过不构建投稿稿。

### 2026-09-28 13:24 CST — 相关论文原文级阅读补证
- 状态：本轮阅读证据与交叉约束已落盘，未修改三份生效评价标准。
- 本轮完成：下载并核验 8 篇与在线选择、LLM/tool routing、冷启动和有限反馈最接近的论文；保存来源 URL、PDF SHA-256、逐页文本；阅读摘要、引言、问题定义、方法、理论、实验和限制，并记录页码。
- 关键结论：contextual bandit/tool routing 本身不是创新；最接近的 P04/P06 已覆盖 query-dependent online selection。P06 明确把 delayed/noisy feedback 和 dynamic model pool 留作 future work；P04 已有 LLM provider selection + contextual MAB。我们的切口必须把 selected-only delayed correctness feedback、动态 provider/version 事件和可恢复的实时 JEV 更新写进形式模型与实验。
- 结果文档：`research/AAMAS2027_相关论文逐篇阅读记录_20260928.md`；本地阅读源目录 `research/paper_sources/aamas_related/`。
- 诚信边界：这仍不是 2023–2026 每年 5–10 篇论文的系统综述；当前是 8 篇最接近论文的原文级深读。三份 v002 评价标准保持生效，暂不静默升级。
- 下一步：继续补齐各年度 LEARN/GAAI/online decision 样本，并把 P04/P06/P07/P08 转成公平 baseline contract 后，再提交三份评价标准 v003 候选修订。

### 2026-09-28 — 2023–2024 AAMAS 原文级补读（subagent）
- 定向下载并逐页核对 AAMAS 2023 Trust Region Bounds for Decentralized PPO、AAMAS 2024 Learning to Schedule Online Tasks with Bandit Feedback、AAMAS 2024 Potential-Based Reward Shaping for Intrinsic Motivation；记录官方/原文 URL、PDF SHA、页码、问题、方法、理论、baseline、实验和限制。
- 结果写入 `research/paper_sources/reading_agent_2023_2024.md`；明确三篇是定向近邻/质量参照，不是全会统计，不修改三份生效评价标准。
- 下一步：root 决定是否将该文件纳入本轮提交，并在证据充分后提出三份标准 v003 候选；当前没有新的实验和没有新的科学结论。

### 2026-09-29 — 2025–2026 近邻论文原文复读补证（subagent）
- 独立复读 AAMAS 2025 的 Truthful Mechanisms for Linear Bandit Games with Private Contexts、Soft Condorcet Optimization，以及 AAMAS 2026 的 Truthful Reverse Auctions for Adaptive Selection via Contextual MAB；记录正文页码、公式、理论、baseline、实验协议、限制和对三份验收标准的映射。
- 新增 `research/paper_sources/reading_agent_2025_2026.md`。官方 PDF/arXiv 阅读件及 SHA-256 记录在文档中；未修改三份生效评价标准。
- 直接结论：contextual provider selection 已被 A26-1 覆盖；SCO 的单事件在线更新可作为“可计算在线目标”的启发，但不能直接当作动态 JEV；我们的新增约束必须落到 selected-only、delayed binary correctness、provider/version lifecycle 和可恢复更新。

### 2026-09-29 — baseline 合同 v1 与 AAMAS 2023–2026 补读收口
- 状态：近邻论文证据和 baseline 角色映射已落盘；没有启动新效果实验或 A800 任务。
- 论文补读：独立记录覆盖 AAMAS 2023 Trust Region Bounds、AAMAS 2024 Online Scheduling/PBIM、AAMAS 2025 Truthful Mechanisms/Soft Condorcet、AAMAS 2026 Truthful Reverse Auctions；补充文档分别在 `research/paper_sources/reading_agent_2023_2024.md` 与 `research/paper_sources/reading_agent_2025_2026.md`，哈希清单在 `research/paper_sources/aamas_related/manifest_supplemental.json`。这些是定向样本，不冒充全会统计。
- 新增 `task/task49-pharma-benchmark-baseline-freeze/baseline_contract_v1.md`：把 C0–C8 主实验控制、GraphRouter/RouterBench 静态对照、P04/P01/C2MAB-V 扩展 baseline、动态 provider 事件、信息边界、指标和停止条件逐项写成合同。特别记录：A26-1 已覆盖 contextual provider selection；没有动态事件和 delayed selected-only feedback，不能宣称动态 JEV 已被证明。
- 更新 `research/AAMAS2027_相关论文逐篇阅读记录_20260928.md`，链接两组补读材料；未修改三份生效研究主线和三份生效评价标准。
- 未完成：统一 runner、C6–C8 正式同任务效果、GraphRouter checkpoint/训练边绑定、provider lineage、BBB 冲突处理和独立 confirmation。下一步先做 baseline schema 静态检查和最小本地适配正确性回放，不打开 confirmation，不提交新的效果型 A800 任务。

### 2026-09-29 — baseline 合同机器可读静态检查
- 完成：新增 `task/task49-pharma-benchmark-baseline-freeze/baseline_contract_v1.json` 与 `src/probe_headless/validate_baseline_contract_v1.py`，把 C0–C8 roster、selected-only/延迟/事件约束、日志字段和可部署/特权角色写成可检查协议。
- 验收：运行 `python3 -B src/probe_headless/validate_baseline_contract_v1.py`，结果 `PASS`；收据 `task/task49-pharma-benchmark-baseline-freeze/baseline_contract_v1_verification.json`，记录合同与检查器 SHA-256。该检查没有读取 provider 输出、没有训练、没有效果结论。
- 下一步：基于该 roster 做统一 runner 的接口适配正确性回放；仍先处理 BBB 冲突与 provider checkpoint/训练成员绑定，不打开 confirmation，不提交新的效果型 A800 任务。

### 2026-09-29 — 故事线文档加入论文行文与段落行为规则
- 用户要求把“被录用论文的内在行为逻辑、段落构造、章节转接和行文风格”纳入故事线与创新点文档。本轮先阅读 AAMAS 2024/2025/2026 近邻论文原文级记录，并核对 `earning-roles` 的 `best_paper_structural_audit`、`theoretical_depth_audit`、AAMAS 预结果稿、claim–evidence matrix、研究要求和治理决议；没有把这些规律包装成录用保证。
- 新增证据底稿：`research/AAMAS2027_论文行文结构与段落逻辑审计_20260929.md`。核心规律是“具体现实问题 → 可复现失败 → 失效原因 → 针对性 insight → 可执行机制 → 匹配证据 → 边界”，并将段落拆为 topic sentence / 支持句 / 桥接句。
- 故事线与创新点升级为 v002 并生效：`docs/研究主线/生效/01-故事线与创新.md`；历史 v001 保留，v002 副本在 `docs/研究主线/版本/故事线与创新/v002-故事线与创新.md`；`docs/研究主线/README.md` 已更新当前版本。active 与 version SHA-256 一致。
- 新增内容包括：一句话主张轴、六段引言漏斗、章节行为顺序、RQ-driven 实验结构、topic-sentence/claim–evidence/reviewer 五问检查，以及“这不是 AAMAS 官方模板、也不保证录用”的边界说明。没有改变已确认的研究对象或 benchmark/baseline 合同。
- 下一步：按 v002 对当前 LaTeX 草稿做第一次结构审查，先补引言/方法/实验的骨架和 claim–evidence 位置，不填尚未完成的实验数字。

### 2026-09-29 — 三份审查标准升级为 v003 严格可复核评分
- 根据 AAMAS 2027 官方要求、AAMAS 2023–2026 代表性论文原文级阅读、earning-roles 结构/claim-evidence 审计和本项目的信息边界，把三份生效评价标准升级为 v003。
- 故事线标准：12 项、权重 100、每项 0–4 分；硬闸门 H1–H10；新增 Abstract、六段引言漏斗、Related Work/Method/Experiments/Discussion/Conclusion 的章节行为、逐段 topic sentence → support → bridge、术语/case、五问盲读和 STORY READY / SUBMISSION REVIEW READY 分级。
- 方法论标准：M1–M15、每项 0–4 分、满分 60；新增时间队列、信息隔离、失败语义、机制必要性、公式/代码一致性、动态事件可识别性、实时 p50/p95、消融、恢复和 claim scope；总分线 49/60，核心项不得低于 3。
- Benchmark/Baseline 标准：B1–B15、每项 0–4 分、满分 60；明确公共 truth layer 与自建 provider routing layer 的边界；新增 truth/label/split/provider lineage、selected-only 事件、延迟/lifecycle、family correlation、baseline fidelity、统计、raw-log recovery 和 confirmation freeze；总分线 49/60，B1–B8 不得低于 3。
- 三份标准都保留 O/P/C 来源分层，明确内部 go/no-go 线不是 AAMAS 官方评分表，不保证录用；作者与独立复核者取较低分，硬门槛先于总分。
- 生效文件：`docs/研究主线/生效/评价标准/` 三份 v003；历史副本：`docs/研究主线/版本/评价标准/v003-*`；三份 active/version SHA-256 已逐一核对一致；`docs/研究主线/README.md` 已更新为 v003。
- 来源审计追加 v003 评分机制说明：`research/AAMAS2027_验收标准来源审计_20260928.md`。
- 验收：定向 `git diff --check` 通过；未运行实验、未修改 benchmark truth、未新增科学效果结论。
- 下一步：用 v003 对当前故事线、方法和 benchmark/baseline 做第一轮真实打分，逐项生成 scorecard；不满足硬门槛的项先补证据或缩小主张，再进入下一步实验/论文写作。

### 2026-09-29 — v003 评价标准的组合贡献与闸门逻辑只读审查（subagent）
- 完成：只读审查三份 `docs/研究主线/生效/评价标准/` v003，记录写入 `research/task49_composition_20260928/rubric_review.md`，未修改 active/版本主文档。
- 发现：三份硬门槛正向问句与“是即失败”规则反转；组合/backbone 防守语句可能被执行成组合一票否决；dynamic 混合反馈自适应与 provider drift；confirmation 开放前置与通过条件存在阶段循环；内部审查语句与论文正向措辞缺分层。
- 建议：v004 统一 PASS/FAIL/UNKNOWN，按整体组合的问题契合、机制必要性、交互收益/成本/覆盖优势评分；加入 `dynamic_scope`；拆 `ELIGIBLE_FOR_CONFIRMATION` 与 `CONFIRMED` 两阶段；明确内部评语不复制进正文。保持严格证据、硬闸门、双人取低分和负结果要求。
- 下一步：由 root 决定是否将上述最小修改落实为 v004 候选；本审查不推翻故事线、动态 JEV 候选、benchmark 或 baseline。

### 2026-09-28 — 组合式动态 JEV 数学候选独立审查
- 新增 `task/task49-pharma-benchmark-baseline-freeze/composition_20260928/math_review.md`，未修改生效主文档、未训练、未读取 confirmation。
- 优先审查候选 A：valid selected binary output `z` 与 delayed selected correctness `r` 精确恢复本次性质标签 `y~=z`（r=1）或 `1-z`（r=0）；共享属性头 `q` 与 provider 条件正确性头 `h_a` 通过全概率分数 `s_a` 连接到下一次工具选择。
- 报告写明现有可观测字段、失败/unknown/冲突/漂移/探索反例、A0–A8 因果消融、与普通 block-sparse multi-task SGD 的等价条件、实时计算与独立恢复闸门；未宣称新颖性已证。
- 候选 B 为 margin-gated 稀疏更新，候选 C 为 provider/version residual 修复，均暂不进入主线；下一步由 root 审查是否形成方法卡并与用户讨论。

### 2026-09-28 — 组合贡献先验审查（subagent）
- 只读审查完成并写入 `research/task49_composition_20260928/prior_review.md`：组合不被一票否决，但必须有同一问题缺口、强制输入输出关系、可验证不变量和 matched ablation。
- 复核 AAMAS 2024 DOL-RM、AAMAS 2025 SCO/PBIM、AAMAS 2026 truthful reverse auctions，以及 P06/P07/P08、Laya/AnyJev 原文与本地快照；记录 URL、页码和边界。
- Task49 负结果与可救空间已写清：普通静态/selected-only 拼接未超过强 fixed，Oracle 与 fixed 的差距仍存在。提出待验证的 residual→shared provider/context state→exact linear-adapter statistic transport→conservative gate 整体候选；未称为已验证方法。
- 未完成：CPU transport 等价、held-out transfer/permutation 与动态事件回放；未申请 A800、未打开 confirmation。

### 2026-09-29 — 组合贡献规则修正与候选方法卡收口
- 用户明确指出：不能把已有组件组合直接说成“不是创新”；内部应审查完整组合是否形成针对问题的统一机制，正文用正向语言描述，准确交代来源，不主动自我贬低。本轮按该要求执行，没有改写成“组件不是创新”的论文结论。
- 生效决议：`docs/决议/0003-组合方法的整体贡献判定.md`。它规定完整机制、统一接口/不变量、integration gain、matched ablation 和 falsification condition 的判定方式，并区分 `feedback-adaptive`、`environment-nonstationary` 与 `both`；确认分 Stage A eligibility / Stage B frozen confirmation。
- 三份验收标准升级为 v004，active 与 version 副本一致：修正 PASS/FAIL 闸门方向，允许组合方法通过整体机制审查，加入 dynamic_scope 与两阶段 confirmation；内部防守标签不复制到论文正文。来源审计追加在 `research/AAMAS2027_验收标准来源审计_20260928.md`。
- 新增候选方法卡 `research/task49_composition_20260928/proposal.md`：selected binary output $z$ 与 delayed correctness $r$ 恢复 $\\tilde y$，共享性质头 $q$ 与 provider 条件正确性头 $h_a$ 通过全概率分数进入下一次选择；写明 selected-only 信息合同、失败边界、复杂度、匹配对照、CPU 证伪卡和 kill criteria。它仍是候选，不是已冻结方法或 confirmation 结果。
- 既有事件字段只读审计：10,776 条 terminal events 中 10,621 条二元字段可验证，恢复 mismatch 0；155 条字段缺失/非二元记录排除且未补标签。收据：`experiments/logs/task49_composition_20260928/label_recovery_audit_v1/result.json`。这不是效果实验。
- 修复候选方法卡中的 LaTeX 控制字符，检查 proposal 无 tab/control character；未提交 A800、未打开 confirmation、没有新增科学效果结论。
- 下一步：运行候选 A 的 CPU 预筛（A0/A1/A2/A3/A5，5 scaffold folds × 2 seeds × delay 0/1/4），先验证 selected-only 事件接口、恢复一致性、整体增益和必要消融；通过后再决定是否进入 A800 与方法冻结。

### 2026-09-29 — A800 指纹诊断完整日志恢复与负结果确认
- Nebula 任务 `01a0de1cf322cd9ee1ead48f41569649` 已正常完成，worker 退出码 0；没有重跑。
- 通过 all-logs 接口下载完整压缩包，保留下载日志、zip 和解包 stdout 的 hash。独立恢复器 `recovered_v3` 验证 1 config、5 splits、8,000 epochs、40 fits、40 checkpoints、1,970 checkpoint chunks、5,776 evaluations、1 complete event，状态 `verified`。
- 结果：Best Fixed `0.915512`，Oracle `0.997230`；Linear BCE `0.872576/0.879501`；MLP BCE `0.896122/0.890582`；BT+CLS `0.887812/0.894737`；直接性质 MLP `0.759003/0.760388`（两列均为 seed 0/1）。所有学习式静态 selector 均低于 fixed，MLP 也没有达到预登记 go-signal；因此当前普通静态指纹选择不进入动态 JEV 主方法。
- 该负结果已经写入 `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md`。它否定的是具体弱假设，不否定 provider routing、Laya 表示或 selected-only 动态协议；不打开 confirmation。
- 下一步：按相同信息合同先完成 Laya 表示的真实可运行性/静态能力审计；之后做 Frozen、Matched Online SGD、Replay SGD、PILOT、BayesianRouter 的公平在线对照，再决定是否值得实现表示更新或 provider/version 事件扩展。

### 2026-09-29 — Truth-Recovered Structured Kernel CPU 接口原型
- 新增 `src/probe_headless/truth_recovered_structured_kernel.py`：固定表示上的共享性质头 q、selected provider 条件正确性头 h，以及全概率选择分数；接口分成 `decide → bind_selected_output → observe`，把输出绑定和 feedback release 明确分开。
- scored 成功事件按 $z/r$ 恢复 $\\tilde y$，只更新共享头和 selected provider 头；failure/unknown 只消费，不造标签、不更新；错 provider、未绑定、重复消费和非二元字段直接拒绝。该原型不读取 evaluator full panel，未改动现有 Gaussian kernels。
- 新增 `src/probe_headless/tests/test_truth_recovered_structured_kernel.py` 与接口说明 `task/task49-pharma-benchmark-baseline-freeze/composition_20260928/truth_recovered_kernel_v0.md`。新测试 11 项通过，与现有 online baseline 测试合计 31 项通过；py_compile 和 diff 检查通过。
- 当前边界：尚未接入 `TerminalFeedbackQueue`，因为现有 queue 的 observe 只传 reward。下一步必须在 pending event 保存 selected output status/label，并在 release 时传给该原型；不能从 evaluator full panel 回填。此原型只是可运行性和信息合同证据，不是效果实验或创新结论。

### 2026-09-29 — selected-only delayed replay adapter
- 新增 `src/probe_headless/truth_recovered_selection_stream.py`：不改现有 `pharma_selection_stream`，单独保存 selected output 和 delayed correctness；`complete` 只绑定输出并把 correctness 放入 pending queue，`release` 到 due tick 才调用 structured kernel 的 `observe`。
- `due_tick=decision_tick+1+delay`，delay 0 也在下一 tick 更新；fixture 每次只读取被选 provider 的 response，failure/unknown 守恒释放但不产生恢复标签。新增测试覆盖 delay 0/1/4、释放前不更新、feedback conservation、selected-only、失败/unknown 和 provider 生命周期校验。
- 同时补上 kernel/adapter 对 `provider_id` 的绑定校验，避免错误 provider 的输出进入 pending event。三组测试目前共 40 项通过，未跑 A800、未产生效果结论。
- 下一步：把该 adapter 接到真实 BBB replay 的最小开发流，先记录每条 selected-only 事件和 state hash；仍需先通过 CPU 证伪卡，再考虑 A800。

### 2026-09-29 — 修正 delayed adapter 的未来反馈隔离
- 复核 adapter 后发现：response 中的 `chemical_correct` 和 `feedback_status` 属于 release 之后的信息，不能在 `complete/bind` 阶段随 mapping 传入 learner。
- 已修复 `truth_recovered_selection_stream.py`：绑定前只传 `provider_id/status/label`，correctness 元数据留在 queue，直到 due tick 才传给 `observe`；新增 RecordingKernel 测试验证 kernel 在 release 前看不到未来字段。
- 三组测试共 41 项通过，py_compile 和 diff 检查通过。仍未跑 A800，也没有效果结论。

### 2026-09-29 — 真实 BBB replay 合约回放接通
- 新增 `src/probe_headless/truth_recovered_bbb_runner.py`，复用真实 `PredictionCache/ReplayEpisode/TruthStore`，只把 selected provider 的 `status/label` 绑定到 learner；correctness 先私存，到了 due tick 才进入 kernel。失败和 unknown 保留为无更新事件。
- 新增 `src/probe_headless/tests/test_truth_recovered_bbb_runner.py`；runner、adapter、kernel 和现有 online baseline 定向测试共 45 项通过，py_compile 与 diff 检查通过。
- 在真实冻结 BBB replay 上跑了 8 个样本的 delay=1 合约 smoke：8 decision、8 terminal、8 feedback，全部守恒；terminal/decision 没有 `chemical_correct`、`recovered_label`、truth 或完整 provider panel；未读取未选 provider。结果不是效果结论。
- 证据：`experiments/logs/task49_composition_20260929/bbb_replay_contract_v1/`；说明：`task/task49-pharma-benchmark-baseline-freeze/composition_20260929/bbb_replay_contract_v1/report.md`。
- 当前卡点：候选方法还没有在相同 scaffold splits、delay 0/1/4 和 matched baselines 下做 CPU 效果预筛；confirmation 和新的效果型 A800 任务仍关闭。
- 下一步：完成 `structured_cpu_effect_v1` 预注册，固定 A0 Frozen/Best Fixed、A1 Matched Online SGD、A2 Truth-only、A3 Correctness-only、A5 No-integration，先跑 CPU 证伪卡，再按 go/kill 条件决定是否冻结方法。

### 2026-09-29 — structured CPU effect 预注册与 zero-prior coverage 闸门
- `task/task49-pharma-benchmark-baseline-freeze/structured_cpu_effect_v1/preregistration.md` 已写入，固定 TRS、A0-Frozen/A0-BestFixed、A1 direct selected-only SGD、A2 Truth-only、A3 Correctness-only、A5 No-integration；固定 5 scaffold folds × 2 seeds × delay 0/1/4、fit-only warm start、主指标、聚类 bootstrap、日志恢复和 kill criteria。confirmation 关闭，尚未运行效果实验。
- 在预注册的主实验前做了 zero-prior coverage check，不读取工具输出、TruthStore 或 evaluator：20 次确定性贪心决策全部选择菜单首个 `admet_ai_1_4_0`，最长连续 20 次。该风险真实存在，不能把零先验结果包装成动态学习。
- 证据：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/zero_prior_coverage_check/`。主运行只能使用预登记的 fit-only warm start；不能临时加入 UCB/Thompson/随机探索或手工 provider 偏置。
- 下一步：审查 runner 是否能从已验证的 fold/seed BT+CLS checkpoint 提取固定 64 维表示，拟合 q/h warm start，并严格按预注册的 selected-only release 流运行；若 warm-start 无法独立恢复，则停止效果实验。

### 2026-09-28 — Truth-Recovered Structured Kernel 冷启动审查（subagent）
- 新增 `research/task49_composition_20260928/cold_start_review.md`：确认全零 q/h prior + deterministic greedy 会产生菜单首位偏置；首位持续 failure/unknown 时存在严格永久锁定反例，未选 provider 无法获得反馈。
- 建议效果主配置必须使用 fit-only warm-start checkpoint；若承担 provider coverage/在线识别主张，所有匹配方法统一固定 epsilon exploration，并记录 propensity、RNG、menu hash，增加 Exploration-Frozen 对照。
- 记录 warm-start 数据权限、短流 coverage 计算、availability 与 selected-only 边界；未跑实验、未修改生效文档、未打开 confirmation。

### 2026-09-29 — 冷启动风险独立审查
- `research/task49_composition_20260928/cold_start_review.md` 完成只读审查：全零 q/provider prior 加 deterministic argmax 会产生菜单首位锁定；共享 q 不能给未选 provider 提供可识别的在线信号。若声称 provider coverage/在线识别，必须统一、预登记探索并记录 propensity；本轮主配置不隐式加入探索，只使用可恢复 fit-only warm start。
- 这与 zero-prior coverage check 的 20/20 首位选择相互印证。该结论是实现闸门，不改变候选方法的整体故事线。

### 2026-09-29 — structured CPU effect runner timing与工程 smoke_v4
- 完成：runner 已固定使用已恢复的 `bt_cls` 64 维 encoder；q/h/direct warm start 仍是 fit-only、200 epoch AdamW、validation-best。A1 失败按 `system_success=0` 更新，结构化方法失败/unknown 守恒但不造恢复标签。
- 补齐：每个 decision 和 release/update 记录纳秒级运行时间，stream 汇总 `decision_runtime` 与 `update_runtime` 的 p50/p95；独立 verifier 检查 `decision_ns`、`update_ns` 非负且存在。配置保留输入、checkpoint 和源文件 hash。
- 验收：定向 55 项测试通过；smoke_v4 运行 1 fold × 1 seed × 1 evaluation molecule × 6 methods × delay0，共 6 decisions/terminals/feedback、18 条事件；独立 verifier `verified`，每条流 selected panel read=1 且 selected-only。该 smoke 不是效果结果。
- 证据：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v4/`；报告：`task/task49-pharma-benchmark-baseline-freeze/structured_cpu_effect_v1/smoke_v3_report.md`（报告中的 v3 是同一最小合约，v4 另存计时证据）。
- 下一步：提交这一组 runner/预注册/证据文件后，按预注册跑 180 条 CPU 动态流；先独立复核和聚合，未通过 go/kill 条件不打开 confirmation 或提交新的 A800 效果任务。

### 2026-09-29 — structured CPU effect v1 全量预筛完成：候选未过闸门
- 运行：5 scaffold folds × 2 seeds × 6 dynamic methods × delay `{0,1,4}`，共 180 streams、25,992 decisions，使用固定 bt_cls 64D encoder 和 fit-only warm start。没有读 confirmation，没有用 GPU。
- 证据：77,976 条事件，10 个 fit manifests；独立 verifier `verified`。原始日志与 hash 在 `experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/`。
- 聚合：独立脚本 `src/probe_headless/aggregate_structured_cpu_effect_v1.py` 连接 terminal identity 与 delayed feedback；primary accuracy 将 failure/unknown 计为 0，记录 valid-return accuracy、失败数和 decision/update runtime。结果在 `full_v1/aggregate.json` 与 `aggregate.md`。
- 结果：Frozen structured 总体约 0.8982；Matched Online SGD delay0/1/4 为 0.8913/0.8885/0.8968；Structured 候选为 0.8857/0.8927/0.8961。Structured−Matched 的全样本点差 `-0.00069`，scaffold bootstrap 95% CI `[-0.00633, 0.00518]`，P(delta>0)=0.388；没有 delay 达到预注册 `+0.01`，正 fold 数最高 3/5，因此 CPU go 条件失败。
- 解释边界：这是固定表示、固定 warm start、一步在线更新、全概率积分的具体候选负结果；不把它泛化成动态 JEV 问题已被否定。confirmation 和新的效果型 A800 任务继续关闭。
- 下一步：独立诊断漂移、积分结构和 provider coverage；提出并预注册一个数学上有理由的保守/先验锚定更新候选，再跑同一 CPU gate。若无增益或明确成本/恢复优势，停止该候选。

### 2026-09-28 — structured CPU 结果审计修正与 affine 中止收据
- 发现并修正旧报告的三处过度表述：`+0.01/4-of-5` 是开发资源判据，不是 preregistration 数值门槛；旧 scaffold bootstrap 存在 seed/delay 方法复制造成的伪重复，区间暂不作正式不确定性证据；原 verifier 是契约检查器，不是逐事件参数 replay。
- 修正文件：`task/task49-pharma-benchmark-baseline-freeze/structured_cpu_effect_v1/report.md`、`research/task49_composition_20260928/negative_diagnosis.md`；审计说明：`task/task49-pharma-benchmark-baseline-freeze/structured_cpu_effect_v1/audit_correction_20260928.md`。
- affine 诊断 retry_v2 进程已退出，只留下 fold0 部分事件（292 行，无 summary、无完整状态快照），已记为 `incomplete_abort`，收据：`task/task49-pharma-benchmark-baseline-freeze/affine_structured_diagnostic_v1/attempt_audit.md`。不从部分事件推断截距效果。
- 当前仍不能进入 confirmation 或提交新的 A800 效果任务。下一步：完成独立逐事件 replay 与按分子/scaffold 的配对统计；保留负结果，之后再决定是否值得预注册 PAR-JEV CPU 证伪。

### 2026-09-28 — affine 诊断独立中止审计补充
- 独立审计确认 retry_v2 无残余进程，只完成 2 个 fit_complete（fold0 两个 seed）和 290 条 evaluation，预期 10/1,444；无 stdout、退出码、traceback、summary 或验证结果。
- 收据：`research/task49_composition_20260928/affine_attempt_audit.md`、`experiments/logs/task49_composition_20260929/affine_structured_diagnostic_v1/retry_v2/abort_receipt.json`。原因未知，不从部分事件推断截距效果。

### 2026-09-28 — 原始事件计数校正
- 复核发现 full_v1 `events.jsonl` 实际为 78,166 行：25,992 decision、25,992 terminal、25,992 feedback、180 `stream_complete`、10 `fit_complete`。此前的 77,976 仅是三类 sample events；报告已明确区分，不能把它写成完整日志行数。

### 2026-09-28 — selected-only contract regression check
- 运行 `pytest`：structured kernel、selection stream、BBB runner、online baselines 定向测试共 45 项，全部通过（0.45s）。这是工程回归证据，不是效果结果。

### 2026-09-28 — full_v1 独立逐事件 replay 与保守统计完成
- 新增 `recover_structured_cpu_effect_v1.py`：不导入 runner，检查 180 条流的 tick 连续性、decision→terminal→feedback 顺序、delay queue、release 时刻、provider/status/label 绑定、版本契约、summary 守恒、fit manifest hash，以及 `provider_id == argmax(scores)`。
- full_v1 replay 通过：180 streams、25,992 sample records；events SHA `349223a10e741ad4e2e389932a7049f83038be38ab121c16d24428d5331fadc9`。首个 feedback 的 `release_tick+1` 变异被拒绝；收据在 `recovered_v2/tamper_rejection.json`。
- 新增 `aggregate_structured_cpu_effect_v2.py`：全局 382 个 scaffold cluster 一次重采样并保持 seed/delay 配对。delay0/1/4 点差 `-0.005540/+0.004155/-0.000693`；all-delays CI `[-0.006294, 0.005377]`，P(delta>0)=`0.3868`。这是开发期复核，不改历史 aggregate，不打开 confirmation。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/structured_cpu_effect_v1/stat_replay_audit_v1.md`；输出：`experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/full_v1/recovered_v2/`。

### 2026-09-28 — 审计修正已推远程
- 相关提交已推送到 `origin/codex/progress-20260924`：`6478c1d0`（报告/中止收据）、`3557450a`（独立 replay + conservative aggregation）、`a2123620`/`ff0291e3`/`f9b66e79`（统计、报告与 replay 强化）。PAR-JEV 仍是候选，等待数学边界审查，不进入 A800。

### 2026-09-28 — PAR 因子化候选被 availability 审计否证，转 joint-success residual
- 新增并运行 `src/probe_headless/audit_par_jev_availability.py`，只读 5 个 fit fold 和 33 个 provider；结果：provider 平均 `|u(y=1)-u(y=0)|` 约 `0.1774–0.2057`，最大值 `0.4039–0.4586`。结果文件：`experiments/logs/task49_composition_20260929/par_jev_availability_audit_v0/result.json`。
- 因此停止 `par_jev_factorized_v0` 作为主方法候选。这个结果只否证 `V ⟂ Y | v` 的默认因子化，不否定动态 JEV 或低维在线校准的一般方向；因子化 kernel 与合同测试保留，不跑其效果实验、不申请 A800。
- 新增候选 `joint_success_residual_jev.py`：直接建模有效且正确的端到端成功事件，冻结 provider query-specific logit，只维护每个 provider 的 scalar residual/curvature；不需要 availability 与真值独立假设。候选说明：`research/task49_composition_20260928/joint_success_residual_jev_candidate.md`。
- 候选合同测试：PAR + joint-success 共 `19 passed`；这是实现证据，不是效果结果。下一步是为 joint-success residual 写 CPU gate，固定同一 5-fold × 2-seed × delay `{0,1,4}`，并与 Frozen/direct SGD/fixed-step 做 matched 对照；confirmation 与新的 A800 效果任务继续关闭。
- 回归补充：PAR、joint-success、structured kernel、selected-only adapter、BBB runner 和既有 online baselines 共 `64 passed`（0.45s）。
- 新增 `research/task49_composition_20260928/joint_success_cpu_gate_v0.md`：固定 joint-success residual 的输入、matched controls、信息边界、G1–G5 和停止条件。当前只完成合同测试与 availability 否证，不运行全量效果、不打开 confirmation。
- 完成 joint-success residual 的真实冻结 BBB CPU integration smoke：fold0/seed0、32 个 evaluation、delay `{0,1,4}`，两方法共 192 条事件；每个 delay 64 decision/terminal/feedback，pending=0，terminal 无 truth/correctness/recovered label/panel 泄漏。residual 的 smoke 结果为 `29/32, 27/32, 28/32`，Frozen 为 `28/32, 28/32, 28/32`；只作合同接通证据，不作效果结论。证据：`experiments/logs/task49_composition_20260929/joint_success_residual_smoke_v0/`。
- 下一步：为 smoke 写独立 JSONL verifier，再决定是否运行完整 CPU gate；confirmation 和 A800 效果任务继续关闭。
- 独立 verifier `verify_joint_success_residual_smoke.py` 已验证三个 delay 的 smoke：每个方法 32 decision/terminal/feedback，延迟合同、身份守恒、terminal 信息边界和最终 pending=0 均通过；把首条 feedback 提前一个 tick 的篡改样本被拒绝。验证输出和 `tamper_rejection.json` 已保存在同一 smoke 目录。
- joint-success residual 全量 CPU 预筛完成：5 fold × 2 seed × 3 delay × 4 methods，共 120 streams、51,984 events；独立 verifier `verified`，篡改 release tick 被拒绝。保守 paired fold bootstrap：residual 相比 Frozen 的点差为 delay0 `-0.004861`、delay1 `-0.005527`、delay4 `+0.003434`，区间均跨 0；相比 matched direct SGD 为 `+0.005498/+0.007620/+0.008300`，区间也均跨 0。结果不能打开 confirmation 或 A800，报告：`task/task49-pharma-benchmark-baseline-freeze/joint_success_cpu_effect_v0/report.md`。
- 解释边界：joint-success residual 比因子化 PAR 数学上更干净且已可运行，但当前 frozen representation/warm start/静态顺序没有显示超过强 Frozen；不能把相对弱 direct SGD 的正点差写成方法效果。下一步转向 baseline fidelity、独立 endpoint/provider-version 事件和 benchmark 资格，不继续调参追分。
- 盘点 baseline readiness：`task/task49-pharma-benchmark-baseline-freeze/baseline_readiness_v1/report.md`。Best Fixed/Oracle/Frozen/matched SGD/PILOT 已有可解释证据；Replay/Greedy LinUCB/BayesianRouter/GraphRouter/Laya 仍分别受统一 runner、entrypoint、实现或资产阻塞。名单仍合理，但不能把 adapter smoke 写成完整 baseline 效果。

### 2026-09-28 — PAR-JEV 数学边界审查完成
- 独立审查文件：`research/task49_composition_20260928/par_jev_math_review.md`（137 行，SHA-256 `848d8358acc6f3a44f8ad3a7b591fa6994d80be1c490fbae7239e40360e9f7d6`）。
- 结论：PAR-JEV 是可实现的候选，但尚未成为生效方法。`u[qh_1+(1-q)h_0]` 只有在 `V ⟂ Y | v` 时成立；一般需用 `Σ_y q_y u_{a,y} h_{a,y}`，或明确限制条件独立假设。`h` 必须定义为有效返回条件下的正确率。
- 标量递推只能称为曲率锚定的一步 online-Newton/ONS 风格更新，不能称精确 logistic 后验、MLE 或 Fisher posterior。selected-only 反馈只能支持当前 policy 下有足够支持的 provider/condition，不能推出未选择 provider 的总体无偏结论。
- 必须先通过 G1 合同、G2 标量数学、G3 冷启动等价、G4 support、G5 matched counterfactual、G6 动态性检查；未通过前不改生效方法文档、不打开 confirmation、不提交 A800 效果任务。
- 下一步：只在候选目录实现最小 PAR-JEV kernel 与 synthetic replay，写精确 preregistration；先做 CPU gate。
### 2026-09-28 — 三份评价标准与当前项目状态核对
- 核对三份生效评价标准 v004 与当前证据。标准文本已经具备硬门槛、逐项 0–4 分、证据路径、通过线和停止动作；但三份文件标注的生效日期为 2026-09-29，早于本次核对日期 2026-09-28，治理上需后续确认日期是否调整。
- 独立保守暂评分（严格口径）：故事线与创新 47.5/100，方法论 37/60，Benchmark+Baseline 37/60；三份均未过线，不进入 STORY READY、方法最终冻结或 confirmation。只看主线文档的宽松分为故事 55/100、方法 39/60。
- 复核 `paper/aamas2027/main.tex` 后补充严格口径：正文目前只有摘要，故正式投稿闸门的故事线分为 47.5/100；55/100 仅是只看主线文档的宽松分。
- 主要缺口：nearest-work 与 claim–evidence–falsification 表未闭合；最终机制没有 integration gain；truth 冲突/duplicate/overlap/provider lineage 未冻结；现代 baseline 未统一跑完；完整论文段落审计未做。
- 另发现生效方法文档含控制字符导致若干 LaTeX 公式损坏；严格方法分已按 M1/M6 扣分，方法定稿前必须修复并加入控制字符 lint。
- 新增状态审计：`research/AAMAS2027_current_evaluation_scorecard_20260928.md`。该记录不修改三份生效主文档，不把暂评分伪装成双人最终评分。
- 下一步：先冻结 truth 与 provider 资格，再统一 baseline runner 和候选方法 CPU 证伪卡；通过后才考虑 A800/confirmation。
### 2026-09-29 — 实验与数据负结果根因分析
- 对 structured CPU、joint-success residual、静态 fingerprint 诊断和冻结输入做只读重算。新增脚本 `src/probe_headless/analyze_effect_root_causes.py`，输出 `experiments/logs/task49_composition_20260929/root_cause_analysis_v1b/analysis.json` 和配置/hash。
- 直接根因：Frozen structured 约 0.8982，低于 fit-only Best Fixed 0.9155；provider correctness-only 与 structured 都让约 78–83% 的选择偏离 Frozen，而 truth-only 只偏离约 3–4%。structured changed decisions 上 Frozen 对→structured 错多于 Frozen 错→structured 对（delay0: 46 对 28；delay1: 34 对 26；delay4: 25 对 22）。
- 机制解释：高维 65 参数 provider head 在每条约 145 个 evaluation 的 selected-only、零探索流上被常数步长更新；provider score L2 相对 Frozen 的平均漂移约 0.20–0.25，而 Frozen top-1/top-2 margin 中位数仅约 0.0016。delay 越大漂移和损失越小，符合过快更新破坏排序的解释。
- 数据/任务解释：evaluation 中每个分子平均 27.83/33 个 provider 正确，中位数 32；490/722 个分子至少 30 个 provider 正确；provider correctness 相关系数均值约 0.447、最大约 0.969。当前 provider、菜单和分子分布静态，只有反馈时钟变化，没有真实 drift/version/retirement 事件，因此没有明确动态收益目标。
- 不能据此否定动态 JEV 总方向；下一步只验证三件事：低维先验锚定 residual 是否恢复 Frozen、真实 provider/version/分布事件是否产生可利用动态信号、统一探索与 propensity 是否解决 selected-only 支持不足。暂不调参、不换 backbone、不提交 A800 效果任务。
- 完整分析：`research/task49_composition_20260928/experiment_data_root_cause_analysis_v1.md`。
- 独立补查 `stream_drift_audit_v1`：block size 16/32/64 的原始 winner switching（7.20/4.00/1.60）与 permutation null（7.48/3.63/1.73）接近，margin 也没有系统高于 null。因此 evaluation 顺序不是本轮负结果的主要原因，不引入 order-only change point；根因分析文档已修正。
- 完成 `order_sensitivity_diagnostic_v1`：固定 fold0/seed0、同一 fit-only warm start，原始顺序+19 个 permutation，3 方法×2 delay，共120 streams/52,200 events；独立重算和事件核验通过。delay0 原始 Structured−Frozen 为 -0.0414、随机均值 -0.0109，Correctness-only 也同步变差；delay4 没有同样低尾异常。结论是顺序会局部放大 selected-only provider-head 更新，但不是跨 delay 的统一根因。报告：`task/task49-pharma-benchmark-baseline-freeze/order_sensitivity_diagnostic_v1/report.md`；原始日志：`experiments/logs/task49_composition_20260929/order_sensitivity_diagnostic_v1/run_v1/`。
- Nebula `01a0de1cf322cd9ee1ead48f41569649` 已结束为 `SUCCESS`，worker 退出码 0，真实 GPU 为 A800-SXM4-80GB。使用 all-logs 完整下载，不重跑；恢复 v4 核对 1 config、5 split、8000 epoch、40 fit、40 checkpoint manifest、1970 chunk、5776 evaluation、1 complete，summary hash 与历史 v3/v2 相同。当前正式证据目录：`experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v4/`；fingerprint report 已统一更新路径。
- 完成 `fit_curvature_joint_residual_v1`：在相同 BBB 输入、5 folds×2 seeds×3 delays 下，将 joint residual 的初始精度从常数 `1.5` 改为 fit-only Fisher `1+Σp(1-p)`，并重跑常数精度、fixed-step、matched direct 对照；150 streams/64,980 events，独立 verifier 通过。fit-curvature−Frozen 为 `-0.00002/-0.00277/-0.00209`（delay 0/1/4），没有稳定超过 Frozen；相对 matched direct 为 `+0.01034/+0.01037/+0.00277`。结论：先验强度会放大漂移，但不是动态收益缺失的主因。报告：`task/task49-pharma-benchmark-baseline-freeze/fit_curvature_joint_residual_v1/report.md`。
- 完成 `controlled_version_event_v1` 正控：在真实冻结 BBB 输出上，按 fold fit-only Best Fixed provider 注入中点后的 provider degradation，方法只能看到 selected delayed failure。90 streams/38,988 events，独立核验通过；fit-curvature residual 事件后相对 Frozen accuracy 为 `+0.0357/+0.0302/+0.0371`（delay 0/1/4），目标 provider selection rate 下降约 `-0.0398/-0.0385/-0.0398`。这证明有明确 version event 时低维状态能学习；不把正控当主 benchmark 或自然动态收益。报告：`task/task49-pharma-benchmark-baseline-freeze/controlled_version_event_v1/report.md`。
### 2026-09-29 — Benchmark / Baseline 三门验收审计
- 触发原因：按 owner 最新要求暂停盲目扩展实验，先检查 benchmark 选型、baseline 选型、实验矩阵与结果是否达到验收标准。
- 完成：只读审计当前生效 Benchmark v001、baseline contract/readiness、TDC truth/lineage 审计和已有 CPU/A800 结果。确认现有 v004 标准已分散覆盖这些内容，但没有顶层独立 gate。
- 标准更新：新增生效 `docs/研究主线/版本/评价标准/v005-03-Benchmark与Baseline评价标准.md` 并同步到 `docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md`。新增 Gate A（benchmark 选型）、Gate B（baseline 选型）、Gate C（实验矩阵与结果），三门各 16 分，均需 ≥12 且关键子项不得低于 3；新增 R1--R8 结果合理性硬检查。旧 v004 保留在版本目录。
- 当前分数：Gate A `8/16`、Gate B `9/16`、Gate C `10/16`，全部 HOLD。总分不能抵消任一门未通过。
- 关键发现：TDC truth 有公共来源，但 Task36 routing layer 是自建协议；BBB 冲突、旧 panel/test overlap、provider model-fresh lineage 尚未闭合。Baseline 名单方向合理，但 Replay/LinUCB/BayesianRouter/GraphRouter/Laya 等未在同一 runner 完成正式效果。已有静态/在线 CPU 结果没有稳定超过 Frozen；controlled version event 只是人为正控，不能支撑自然动态 claim。
- 发现的口径漂移：`research/AAMAS2027_Benchmark_Selection_Review.md` 仍把 LLMRouterBench + ASlib + BBB 写作 main，与当前 TDC truth + Task36 routing protocol 方案冲突。未做 scope 决定前，不把两套都称 main。
- 证据：`research/AAMAS2027_benchmark_baseline_acceptance_audit_20260929.md`、`research/AAMAS2027_current_evaluation_scorecard_20260929.md`。
- 下一步：先和 owner 确认旧 benchmark 方案的处理方式；随后按 Gate A → Gate B → Gate C 修复缺口。Gate 全过前不提交新的效果型 A800 任务，不打开 confirmation。
### 2026-09-29 — 主 benchmark scope 决议生效
- 共同确认：主 benchmark 收敛为“公共 TDC truth + Task36 provider routing protocol”；主开发端点为 TDC BBB，Task36 33-provider panel 为主 routing panel。
- LLMRouterBench 与 ASlib SAT12-ALL 从 main 移出，旧方案保留为历史候选；若未来需要，只能另设 external transfer/appendix 实验。
- 决议：`docs/决议/0004-主Benchmark收敛为TDC与Task36医药工具路由.md`；主线新版本：`docs/研究主线/版本/Benchmark与Baseline/v002-Benchmark与Baseline.md`，已同步到生效文档。
- 边界：这只解决 benchmark scope，不代表 Gate A 已通过；BBB 冲突、development/test overlap、provider lineage 和 confirmation 仍未闭合。Gate A 当前更新为 `9/16`，整体仍 HOLD。
- 下一步：先收口 Gate A，再统一 baseline runner；不把外部迁移实验提前加入主线。
### 2026-09-29 — Gate A BBB confirmation 候选分母收口
- 目的：把 BBB 标签冲突、旧 development panel overlap 和未来 confirmation 候选分母从描述变成固定可复核输入；本轮不调用 provider、不训练 selector。
- 实现：新增 `src/probe_headless/build_bbb_confirmation_candidate_manifest.py`。规则固定为：排除 10 个 TDC canonical 冲突组；排除旧 panel 的 strict canonical overlap；排除旧 panel 的非手性 Bemis--Murcko scaffold overlap；按 isomeric canonical 去重。
- 结果：TDC test 406 行 / 394 canonical，生成 157 条 scaffold-disjoint candidate，标签 0/1=`36/121`，manifest SHA-256=`c0af602bbc4ec49df448b916fb1e54dfce183926965d61c184cb736e68402805`。独立检查通过，重复生成字节一致。
- 证据：`experiments/logs/task49_gate_a_20260929/bbb_confirmation_candidate_manifest_v1/{manifest.csv,result.json,config.json}`；报告：`task/task49-pharma-benchmark-baseline-freeze/gate_a_confirmation_manifest_v1/report.md`。
- 边界：157 条只是候选 confirmation 清单，不是已打开的 confirmation；provider lineage、方法/菜单/指标冻结和新输出生成仍未完成。Gate A 仍 HOLD。
- 下一步：继续收口 provider lineage 和 baseline 合同；不把这 157 条分子直接拿来调参或提交效果型 A800 任务。
### 2026-09-29 — provider lineage 评分校准
- 复核发现：provider training membership 未证明 model-fresh，不等于固定已部署菜单上的 routing benchmark 无效。主任务的合法主张是 lineage-aware operational routing，不能外推为 model-fresh 泛化。
- 新增：`task/task49-pharma-benchmark-baseline-freeze/provider_lineage_claim_matrix_v1/report.md`，明确 G1/G2/G3 provider 的可用主张、禁止主张、family 聚合和报告约束。
- 按 v005 评分锚点校准：Gate A A4 从 `1/4` 调整为 `3/4`，因为 lineage grade、family 相关性和 claim boundary 已有完整审计；Gate A 总分从 `9/16` 调整为 `11/16`，仍因 BBB 冲突/overlap 未闭合而 HOLD。
- 这次调整是证据评分修正，不是放宽独立性要求；model-fresh confirmation 仍需要 checkpoint-to-member 绑定。
- 下一步：继续处理 BBB conflict/overlap 合同，再进入 baseline runner 收口。
### 2026-09-29 — Gate A BBB 冲突政策候选与 benchmark 口径复核
- 新增候选报告：`task/task49-pharma-benchmark-baseline-freeze/gate_a_conflict_policy_v1/report.md`。对 10 个 canonical 冲突组（train/validation 8、test 2；20 行）完成计数和政策比较。
- 结论：在取得逐行 assay provenance 前，majority、first-row、model-based 修标签都没有可审计依据；P-C1（整体排除冲突组、保留原始收据、报告清洗前后分母）是当前唯一可直接执行的候选。它仍需双方确认，暂未写入生效 benchmark 或新 ADR。
- 已将 ADR-0021 标记为“benchmark 来源条款被 ADR-0004 部分 supersede，其 episode、baseline 及 BBB/hERG/AMES 最终多端点要求继续生效”；active/version Benchmark v002 明确 BBB 为当前开发 truth，AMES/hERG 通过资格 Gate 后再进入最终多端点证据。
- 当前 Gate A 仍为 11/16 HOLD；冲突政策候选和 157 条 scaffold-disjoint confirmation 候选已经具备，但 confirmation 尚未打开。
- 下一步：等待 P-C1 共同确认；确认后生成冲突排除后的统一 manifest，再处理 Gate B baseline runner，不启动新的效果型 A800。
### 2026-09-29 — C4–C8 baseline contract-only unified runner
- 按 Gate B 最小闭环要求，把 C4 `OnlineSGD`、C5 `ReplaySGD`、C6 `GreedyLinUCB`、C7 `PILOT-adapted`、C8 `BayesianRouter-adapted` 接入同一个真实缓存 runner；固定 64 条 BBB cohort、33 provider、Morgan-64、delay `{0,1,4}`。
- v1 在写 source hash 配置前失败（相对路径直接对绝对根求 `relative_to`），失败现场保留在 `experiments/logs/task49_baseline_contract_smoke_20260929/run_v1/`；没有开始 replay。修复后 v2 通过：15 streams、960 decision/terminal/feedback；fixture prior 明确只用于合同冒烟，不是 fit 或效果结果。
- 新增 `src/probe_headless/run_pharma_baseline_contract_smoke.py`、`src/probe_headless/verify_pharma_baseline_contract_smoke.py`；独立 verifier 不导入 baseline kernel，逐事件核验 cohort、selected provider、terminal/cache 绑定、selected-only 字段、delay/version 和反馈守恒。
- 证据：`experiments/logs/task49_baseline_contract_smoke_20260929/run_v2/verification.json`，events SHA-256=`5d10e3699e251475613edbbe3bb765fb4fdd26485a3b77fb3439b1c2e5609e90`；报告：`task/task49-pharma-benchmark-baseline-freeze/baseline_contract_smoke_v1/report.md`。
- 边界：只补齐统一接口与信息合同，未产生 accuracy/ranking，不打开 confirmation，不提交 A800；Gate B 的正式效果资格仍未通过。
- 下一步：由主线审查该合同证据是否足以将 Gate B 的 fidelity/统一 runner 子项提分；继续补齐 fit-only prior、GraphRouter/Laya 和正式效果矩阵。
### 2026-09-29 — baseline contract smoke provenance repair
- 为避免运行时 HEAD 与源码不一致，已用提交 `d20a503fe1085eb2e6109b7d8e4b16519838c3d4` 重新运行同一 contract-only smoke，正式证据目录改为 `experiments/logs/task49_baseline_contract_smoke_20260929/run_v3/`。
- v3 仍为 15 streams、960 decision/terminal/feedback；独立 verifier 通过，events SHA-256=`90efecb5b22372aa3f57f40fdf38e9517e680437b9c862899fbd5947d0084864`，config 中 HEAD 与提交匹配。
- `tamper_rejection.json` 记录把 feedback 注入 `system_success` 后 verifier 非零拒绝；原始事件未修改。v1 失败和 v2 首次修复证据均保留。
- 这只补齐 C4–C8 的统一接口/信息边界与 provenance，不提升为效果结果；Gate B 仍需 fit-only prior、GraphRouter/Laya 和正式主矩阵。
### 2026-09-29 — BayesianRouter baseline readiness 口径修正
- 复核 `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/online_bayesianrouter_effect_result.md`：已有完整 5 folds × 2 seeds × delay `{0,1,4}` 的真实 BBB selected-only development replay，BayesianRouter-adapted 在三个 delay 均未超过 Frozen，属于可计入开发表的正式负结果。
- readiness、scorecard、Gate B 审计已从“不可计效果/无正式结果”改为“可用负结果”，同时保留 adapted 边界和早期失败收据；不改变 Gate B 总分 `9/16`，因为 Greedy/Replay/GraphRouter/Laya 与全 roster 的统一正式主表仍未闭合。
- 下一步：继续收口剩余主 baseline 的正式效果和统一 fit 权限，不重复提交 BayesianRouter 任务，不把负结果包装成收益。

### 2026-09-29 — baseline contract required fields and provenance finalization
- 发现 v3 只验证了事件语义，尚未把 baseline 合同要求的 `state_hash_before/after`、`update_ms`、`method_id` 等逐事件字段完整落盘；未把 v3 当最终证据。
- 新增字段后先保留 v4，再在提交 `728216c200b06059ca1b60a6e5c44d403adcca83` 上重跑 v5；15 streams、960 decision/terminal/feedback，独立 verifier 和 tamper rejection 均通过。
- 正式证据：`experiments/logs/task49_baseline_contract_smoke_20260929/run_v5/`，events SHA-256=`eed9e036dc7ff97c4ddbf4b5adefc2341c22de61ed80c2f5fb4cb43ffee215f5`。这仍是接口/信息边界 smoke，不是效果结果。

### 2026-09-29 — Greedy LinUCB baseline 适配候选收口（未跑效果）
- 完成 C6 候选预登记、CPU runner 和独立 NumPy verifier：`task/task49-pharma-benchmark-baseline-freeze/greedy_linucb_development_v1/{preregistration.md,blocking_report.md}`、`src/probe_headless/{run_greedy_linucb_development.py,verify_greedy_linucb_development.py}`。
- 适配明确为 fit-only ridge provider mean + online $P=I$，不是原论文 zero-prior；seed 0/1 是确定性重复槽位，不按独立随机重复统计；沿用现有 evaluation order。
- 静态验收：py_compile 通过，`test_pharma_online_baselines.py` 21 passed；fit-only prior 预检为 5 folds × 33 providers × 1024 features，输入 SHA 为 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 未运行 60 条效果流、未生成 accuracy、未申请 Nebula/A800；原因是 BBB conflict policy、统一 warm-start/seed 合同和 provider lineage 仍未冻结。该候选不能进入主表或 confirmation。
- 本轮下一步：等现有 baseline 证据盘点，决定是只保留候选适配卡，还是在冲突政策与公平合同冻结后运行一次 CPU development replay；不以更多 runner 冒充科学进展。

### 2026-09-29 — GraphRouter 静态 baseline 适配可行性审查
- 只读核对上游 MIT commit `0edfb8758d45acd93c8029354867ffb49bb8a97f` 与本机依赖：torch/sklearn/pandas/yaml 可用，`torch_geometric` 不可用。
- 发现不能直接把上游 GraphRouter 接到 BBB：原实现需要 query/provider description embedding、multi-task/scenario edge 语义；BBB 只有单性质，provider descriptor 和图边权限尚未冻结。若直接 NumPy/PyTorch 重写，会变成不同模型，不能冒充 fidelity。
- 写入 `task/task49-pharma-benchmark-baseline-freeze/graphrouter_static_adapter_v1/blocking_report.md`，给出合法适配合同和停止条件。未安装依赖、未跑回放、未申请 A800。
- 结论：GraphRouter 保留为静态强 router 候选，不是在线 JEV；当前阻塞已从“还没做”收紧为可审计的输入/依赖/fidelity 问题。

### 2026-09-29 — Laya/JEV 在线适配边界审查
- 只读核对本地 Laya runtime：`Agent.system_one` 使用 `@torch.no_grad()` 和 `model.eval()`，仓库没有 optimizer、loss、update 或在线 checkpoint 写回接口；内置 Router 只按语言/workflow dispatch checkpoint，不负责 provider 选择。
- 因此 Laya forward/静态诊断不能直接称“动态 JEV”。合法候选只有冻结 Laya 表示+外接在线 head、只更新 scorer/adapter、或重新做 LoRA/encoder fine-tune，三者预算和方法都不同。
- 写入 `task/task49-pharma-benchmark-baseline-freeze/laya_online_adapter_v1/blocking_report.md`，并在 baseline readiness/acceptance audit 中补上边界。未改 Laya 权重、未跑在线效果、未申请 A800。
- 结论：当前保留 Laya 静态负诊断和表示来源；在线 Laya/JEV 只有在外接 head、参数预算、延迟反馈和状态 hash 合同冻结后才值得实验。

### 2026-09-29 — baseline 合同文档与实际证据重新对齐
- 修正 `baseline_adapters_v0.md` 和 `baseline_contract_v1.md` 的过时状态：明确 matched Online/Replay、PILOT-adapted、BayesianRouter-adapted 已有 development evidence；Laya 只有静态负诊断；Greedy/GraphRouter/在线 Laya 仍未闭合。
- 没有改动 baseline roster、实验结果或生效决议；只是把证据边界同步到合同文档，避免后续重复判断。

### 2026-09-29 — 验收顺序重新收敛
- 独立审查确认 Gate A/B/C 仍为 11/9/10，新增证据主要修正可复核性，不自动提分。
- 最小高收益顺序固定为：P-C1 冲突政策共同确认 → authoritative manifest/ADR → Greedy LinUCB CPU development replay → 再决定 GraphRouter/Laya-online 是否进入主表。
- 补充关键边界：当前只有 fixed-pool delayed selected-only feedback，没有自然 arrival/retirement/version drift。若不另冻结动态事件，论文 claim 必须写成 fixed-pool delayed adaptation，不能把在线增益直接叫 environment-dynamic JEV。

### 2026-09-29 — AAMAS 题目与摘要工作稿重写
- 依据 AAMAS 2025 官方目录及 GraphRouter/RouteLLM/MixLLM 一手标题，完成题目结构审查并保存：`research/AAMAS2027_title_abstract_audit_20260929.md`。
- 旧题目把 selection、verification、scientific tools、delayed feedback 堆在一起，像系统说明；当前工作题目改为 **After the Decision: Learning to Adapt from Delayed Outcomes**，把“决策之后如何根据结果调整”提到主轴，工具增强 agent 放入摘要明确范围。
- `paper/aamas2027/abstract.tex` 按现实问题→部分延迟反馈→协议→开发证据→信息瓶颈→适用边界重写。保留真实开发结论：selected-only online update 尚未超过 train-only fixed selector；confidence-gated escalation 只作为独立验证控制，不写成动态 selector。
- 项目 `make draft` 成功生成一页 PDF，`pdftotext` 核对到新标题和摘要；内置独立 LaTeX 编译器因不加载仓内 `reference/template/aamas.cls` 报缺失，已记录为编译器输入限制，不作为源码失败。
- 当前仍是 Internal Research Draft；方法、主实验和 model-fresh confirmation 未冻结，题目和摘要需在方法/结果收口后再做一次投稿版替换。
- 下一步：继续按 Gate A→B→C 收口 benchmark/baseline，方法冻结后只更新一次摘要 headline result；不要用这次文字修改替代方法和证据。

### 2026-09-29 — Oral 摘要风格审查与第二版摘要
- 使用 `agent-reach` 检索并核对有明确 Oral 标记的 ICML/NeurIPS 官方摘要，重点阅读 Pausing Policy Learning、Hierarchies of Reward Machines、APT、Twisted SMC、Debating with More Persuasive LLMs，以及延迟反馈相关的 Online Learning of Delayed Choices。
- 归纳出稳定结构：5–7 句、约 130–200 词；现实冲突→单一缺口/问题→一个机制动作→协议/理论→带 comparator 和成本的 headline result→机制性结论。优秀摘要用主动动词和单一主张轴，不把 benchmark 字段、baseline 名单和内部验收语言堆在一起。
- 新增审查记录：`research/AAMAS2027_oral_abstract_style_audit_20260929.md`。当前摘要原稿的问题是像内部实验报告：机制不够具体、协议字段过密、`does not establish` 等边界句过多、结尾偏 benchmark 宣传。
- `paper/aamas2027/abstract.tex` 重写为 160 词、8 句的 Oral 风格工作稿：首句直接进入 provider 选择冲突；明确 selected-only delayed feedback；把 update timing 作为唯一机制动作；保留真实开发 headline（0.9155→0.9238，1.206 calls/request）；把负结果解释为 counterfactual information bottleneck。
- `make draft` 成功，PDF 1 页、340049 bytes；`pdftotext` 核对到新摘要。当前仍是 Internal Research Draft，开发结果不能当作最终 confirmation 或新 JEV 方法效果。
- 下一步：方法冻结后把摘要压成最终 6–7 句版本，只替换 comparator、model-fresh confirmation headline 和适用范围；不再增加协议字段清单。

### 2026-09-29 — 摘要单主线修正
- Oral 样本复核发现上一版摘要同时把 selected-only online adaptation 的负结果和 confidence-gated verification 的正结果作为 headline，形成两条故事线；这与 direct selection 主线不一致。
- 修正：`paper/aamas2027/abstract.tex` 移除 confidence gate 的 headline 数字，摘要只保留“selected-only delayed feedback 是否足以改善后续选择”这一条主线；verification/escalation 只作为协议中的分离动作，结果留在正文辅助实验。
- 当前摘要约 141 词、8 句：现实冲突→信息缺口→明确问题→更新时序机制→benchmark 实例→开发负结果→信息瓶颈→可证伪目标。结论限定在 BBB development stream，不写普遍 impossibility。
- `make draft` 成功生成一页 PDF（338901 bytes），日志只保留官方模板已有的 `ifx` warning；`pdftotext` 核对到新摘要。
- 这次修改进一步确认：如果最终论文主线仍是动态 JEV/直接选择，摘要必须等待方法和 confirmation 结果后再写正向 headline，不能用 verification 的正结果替代 selection 的方法证据。

### 2026-09-29 — 摘要切换为投稿候选口径
- Owner 指出上一版摘要过于保守，像阶段性诊断，不能承载最终方法的优越性。复核生效方法/故事文档后，将摘要从“报告当前负结果”改为“明确目标机制与后续 headline result 的承载框架”。
- 当前投稿候选摘要明确三个方法约束：calibrated frozen selector 作为 cold-start prior；只在 selected feedback release 后更新；新证据未通过 uncertainty gate 时不切换。它直接解释为什么同时覆盖 frozen router 的适应不足和 unconstrained online head 的冷启动漂移风险。
- 摘要保留 public TDC truth + 33-provider panel、scaffold-disjoint split、failure、delay 和 controlled provider-version stress event 的实验范围；未编造最终 accuracy、recovery 或 confirmation 数字。
- `paper/aamas2027/abstract.tex` 204 词，`make draft` 成功，PDF 1 页（339011 bytes）。`paper/aamas2027/README.md` 同步说明：后续方法冻结后只替换结果段和适用范围，不再改主故事。
- Oral 风格审查补充写入 `research/AAMAS2027_oral_abstract_style_audit_20260929.md`：投稿候选版的优越性必须由可执行机制和匹配预算承载；若最终实验不支持，必须收窄 claim，不能靠摘要措辞掩盖。

### 2026-09-29 — 回归主线：Benchmark / Baseline / 方法执行链审查
- 按 owner 指令停止继续打磨题目和摘要，重新检查 Gate A/B/C 与当前方法承诺是否一致。
- 新增主线回归记录：`task/task49-pharma-benchmark-baseline-freeze/mainline_reentry_audit_v1/report.md`。它固定当前闸门快照（Gate A/B/C=`11/9/10`，均 HOLD）、P-C1 冲突政策待共同确认、baseline 主表未闭合，以及摘要三个机制承诺与可执行证据的对应关系。
- 结论：下一步高收益顺序仍是 P-C1 共同确认 → authoritative BBB manifest → Greedy LinUCB CPU development replay → 再决定是否开 DG-SafeJEV 或转向 post-call dynamic verifier；不提交新的效果型 A800，不把 contract smoke 或 controlled positive control 当主结果。
- Greedy 只读准备审查补充了三个硬缺口：当前 Morgan-1024 与 hidden-64 主证据不能直接混排；现 runner 尚未输出 baseline contract v5 所需的完整 provenance/state hash 字段；当前预登记是 fit-prior adaptation，不等于原论文 zero-prior Greedy，主表需分名或补 canonical 对照。

### 2026-09-29 — P-C1 冲突政策落地
- 按 owner 授权的技术细节自治范围，采用 P-C1：BBB canonical conflict group 整体排除，保留原始收据，主结果使用显式清洗后的 molecule-level 分母；不再等待逐项确认。
- 新增生效决议：`docs/决议/0005-BBB冲突样本采用P-C1整体排除.md`，并更新 `docs/决议/README.md`。
- 新增构建器：`src/probe_headless/build_bbb_authoritative_development_manifest.py`；生成 `experiments/logs/task49_gate_a_20260929/authoritative_bbb_manifest_v1/inputs.json`，10 个 conflict groups、722 行输入、0 行新增删除，manifest SHA=`94580e3ffb8a793674265c93e6d297239106d15965f6989f316f7f01fbff0f60`。0 行新增删除是因为历史 unresolved 过滤已经移除了当前 cache 中的冲突行，不能解读为原始 TDC 无冲突。
- 详细报告：`task/task49-pharma-benchmark-baseline-freeze/authoritative_bbb_manifest_v1/report.md`。下一步转入 Greedy baseline 的 v5 provenance、common feature strata 和初始化合同修正。

### 2026-09-29 — Greedy LinUCB v2 合同 smoke
- 将 C6 runner 改为支持 P-C1 authoritative manifest、BT/CLS hidden-64 common representation 和 baseline contract v5 provenance；不再把 Morgan-1024 fit-prior adaptation 与 hidden-64 主证据混排。
- 真实 smoke：`fold0 × seed0 × delay0`，online/Frozen 两条流，145 个 evaluation molecule，290 个 decision/terminal/feedback 事件；独立 verifier 通过。收据：`experiments/logs/task49_greedy_linucb_development_20260929/smoke_v2_fix/`。
- 篡改第一条 online feedback 的 `state_hash_after` 后 verifier 正确拒绝，现场：`.../smoke_v2_tamper/`。第一次相对 checkpoint 路径 hash 失败保留在 `.../smoke_v2/`，没有进入回放。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/greedy_linucb_development_v2/report.md`。这只是合同 smoke，不是效果结果；下一步在提交代码后跑完整 CPU development matrix。

### 2026-09-29 — Greedy LinUCB 全量 CPU development replay 收口
- 修复 `TerminalFeedbackQueue` 最终 drain 的事件日志顺序：新增 `drain_each`，保证每条 delayed feedback 在 policy update 后立即落盘；补充延迟 drain 状态版本测试，`test_pharma_online_baselines.py` 为 22 passed。
- 先前 `full_v2/` 的 verifier 失败被确认是日志协议问题，不是模型结果；目录保留，未纳入分析。修复后新跑 `experiments/logs/task49_greedy_linucb_development_20260929/full_v3_fix/`，5 folds × 2 deterministic slots × 3 delays × online/Frozen，60 streams、8,664 events，独立 verifier `verified`。
- 开发结果：online accuracy delay 0/1/4=`0.8511/0.8449/0.8629`，Frozen=`0.8566`，fit-only Best Fixed=`0.9155`；online−Frozen=`-0.0055/-0.0118/+0.0062`。没有达到预登记的 `>=0.02` 提升与多数 folds 为正的 go 信号，Greedy 保留为现代在线基线负结果，不进入主方法或 confirmation。
- 收据：events SHA=`7d96d466f5ee5ae6098e19154a343077110860f1882e8e7d5551cb967f55ce1c`；summary SHA=`abb76658c7c6dfb0306c2b11b5e0e428269afd46bc6c2c6ad9ef33dd45694fb9`；报告=`task/task49-pharma-benchmark-baseline-freeze/greedy_linucb_development_v2/report.md`。
- 解释边界：该负结果只约束 fixed-pool、selected-only delayed feedback、BT/CLS hidden-64、fit-only prior 的普通 LinUCB，不推出所有在线更新均无效。主线下一步回到 matched OnlineSGD/ReplaySGD、BayesianRouter 与 Gate B/C，不申请新的 GPU 任务。

### 2026-09-29 — Development baseline master table 收口
- 新增 `task/task49-pharma-benchmark-baseline-freeze/development_master_table_v1/report.md`，把 Best Fixed/Oracle、Matched Online/Replay SGD、PILOT-adapted、BayesianRouter-adapted、Greedy LinUCB 放在同一 development 证据表，并明确各自 Frozen control 不能混成一个算法排名。
- 统一观察：Best Fixed=`0.9155`，Oracle=`0.9972`；Matched Online-SGD 比自身 Frozen 约 `+0.78pp`，但仍低于 Best Fixed；Replay 约 `+0.50pp`；PILOT、BayesianRouter、Greedy 大多数 delay 未超过自身 Frozen，全部未超过 Best Fixed。
- 研究含义：机会空间存在，但当前瓶颈是强 incumbent 下何时安全离开、如何处理 selected-only delayed feedback 与动态 provider event，不是再换一个普通 online ridge/SGD/posterior。
- 下一步固定为：用这张表做方法候选审查；只保留一个有明确信息优势和 falsification condition 的安全动态候选，未通过 CPU development gate 前不申请新的 A800。

### 2026-09-29 — 方法搜索边界收紧
- 读取并核对 provider-transfer、context-memory、AG-JEV、DG-SafeJEV 和 delayed dynamic verifier 的既有报告：所有候选均已有明确停止条件和负结果；dynamic verifier 的 pooled accuracy=`0.900277`、静态 escalation=`0.923823`、平均调用=`1.490305`，因准确率与成本双失败。
- 更新 `task/task49-pharma-benchmark-baseline-freeze/next_plan.md`：direct pre-call 不再堆普通 selector/head/graph/memory/risk-threshold 变体；除非出现新的独立可观测反馈，否则主线保留 fixed-pool selected-only 的负证据，资源转向验收文档、post-call verifier/escalation 证据链、端点资格和动态 scope 边界。
- 当前无需提交新的 A800；所有新增方法实验必须先有新的信息来源和可证伪判据。

### 2026-09-29 — 三份生效主线文档同步
- 修正 `docs/研究主线/生效/03-Benchmark与Baseline.md`：P-C1 冲突整体排除从“待确认候选”更新为生效口径；补 authoritative manifest、Greedy 全量 replay 和 development master table 收据。
- 修正 `docs/研究主线/生效/02-方法论.md`：明确普通 selected-only head、provider transfer、context memory、动态 risk gate 均未形成可写成 Proposed 的正向证据。
- 修正 `docs/研究主线/生效/01-故事线与创新.md`：development master table 已补齐；Best Fixed=`0.9155` 仍高于当前动态 selector，不能把“动态 JEV 已有效”写进摘要或结论。
- 远程提交：`84d321bc`。当前三份唯一生效主线文档与真实 development 证据一致；confirmation 仍关闭。

### 2026-09-29 — 当前验收审计 v2
- 新增 `task/task49-pharma-benchmark-baseline-freeze/current_acceptance_audit_v2/report.md`，按 Gate A/B/C 区分“已有 development 证据”和“仍缺的硬条件”。
- 当前三门均保持 HOLD：Gate A 缺 hERG/AMES 与 provider lineage/confirmation 收口；Gate B 缺 Random 统一收据、静态强 router fidelity、Laya-online 或正式降级；Gate C 缺 E1–E5 全矩阵、动态 event stress、正式 paired uncertainty 和 confirmation。
- 明确当前论文可写事实与禁止越界主张；在硬条件完成前不提交新的 A800，不继续调已有负候选超参。

### 2026-09-29 — Paper pipeline / Orca / multi-agent debate 调研
- 完成本地 paper/research skill 盘点和外部一手来源核对。主流程采用 `paper-research-pipeline` + `paper-writer` + `paper-reviewer`，文献用 `agent-reach`/`scholar-search`/`med-paper-search`，末端用 `check-citations`；没有找到官方的 paper+Orca 一体化 skill。
- 外部证据确认：debate 需要 single/no-debate/majority-vote 反事实才能归因，必须保留 minority objection、动态停止和 claim-level evidence；PaperOrchestra 适合成稿阶段，STORM/VIRSCI/Tree-of-Debate/AI Scientist 只借流程结构，不能替代 benchmark、baseline、真实日志或人工 gate。
- 现有历史四方评审 `docs/inherited/0028-calibration管线v1四方评审裁决.md` 已核对，可作为本项目多 agent 独立审查的内部先例。
- 新增调研记录：`research/paper_pipeline_orca_debate_audit_20260929.md`；来源台账：`research/paper_pipeline_sources_20260929.jsonl`。
- 新增统一过程标准：`docs/论文研究与写作流程.md`，覆盖 P0–P11、candidate card、三轮以内 structured debate、Orca DAG、实验前后闸门和投稿红队；`docs/研究主线/README.md` 已加入入口。
- Orca runtime 已存在，但当前 `/usr/local/bin/orca` 是不可解析的 root symlink，直接调用报 `Unable to determine Orca.app path from symlink`；启动 app 返回已有实例锁。未把普通 collaboration subagent 结果冒称为 Orca Run，后续恢复 CLI 后按同一 artifact schema 绑定 Run。
- 下一步：按新流程从当前 Gate A/B/C HOLD 状态继续，先做 P3/P4 的 benchmark/baseline/方法候选审查，不开启新的效果型 A800。

### 2026-09-29 — Round 0 故事线/方法独立审查
- 完成：独立读取生效故事线、方法论、Benchmark/Baseline、current_acceptance_audit_v2 及生效故事线评价标准 v004；核对 contextual routing、selected feedback、delayed update、JEV、cascading、局部能力选择等最近邻审查记录。
- 结论：当前故事有可证伪问题和清晰信息边界，但动态 JEV 仍未冻结，方法候选未超过 Best Fixed，pre-call selector 与 post-call verifier 的 estimand/预算尚未分解；按 v004 临时评分 62/100，H4/H5/H8 为 UNKNOWN，HOLD/Major Revision。
- 产物：`research/debate_story_method_20260929/round0/story_aamas_reviewer.md`。
- 未完成：无；后续由主 agent 汇总其他 Round 0 报告并决定是否收窄方法主张。

### 2026-09-29 — Round 1 故事线交叉回应
- 完成：读取 Round 0 manifest、method_theory_auditor、benchmark_novelty_auditor；保留 selected-only 不可识别性、estimand 混淆和 fixed-pool scope 约束，同时挑战“固定 epsilon 唯一必要”“所有异常场景立即成为 H4/H5 门槛”“所有近期 baseline 必须运行”“平均 calls=1.25 通用 kill”四个过强要求。
- 产物：`research/debate_story_method_20260929/round1/story_aamas_reviewer.md`。
- 结论：H4/H5/H8 与 S3/S4/S6 仍未达标；允许按 contribution type 分支保留 protocol/interface 或 risk-cost 贡献，但必须有独立 utility、强公平对照和范围校准。

### 2026-09-29 — Round 2 冲突裁定
- A 最终立场：投稿主故事现在收窄为 protocol/negative diagnosis；保留单一、隔离、可证伪 method candidate，未通过同合同 CPU gate 前不写 dynamic JEV 主方法。
- B 最终立场：不立即拆成两篇；先拆 selector/verifier 的 estimand、RQ、成本和结果块，以 2×2 factorial 与 interaction utility 决定统一稿或后续分稿。
- 产物：`research/debate_story_method_20260929/round1/story_aamas_reviewer_round2.md`。

### 2026-09-29 — 故事线/方法论多 agent 辩论综合
- 完成 Round 0 独立审查、Round 1 交叉挑战和 Round 2 最终裁定；三名审查者分别覆盖 AAMAS 故事、方法理论、benchmark/baseline，coordinator 保留中间报告和少数意见，没有按多数票替代实验。
- 归档：`research/debate_story_method_20260929/README.md`、`round1/*_round2.md`、`synthesis/manager_synthesis.md`。
- 共识：当前不把 dynamic JEV 写成已验证主方法；selector/verifier/updater 必须拆 estimand；selected-only provider adaptation 需要显式 exploration/propensity、fit-only prior 或结构共享条件；fixed-pool 只支持 `feedback-adaptive` scope；自建 routing layer 只有完成 truth/lineage/contract/checker/overlap/confirmation 后才可作为领域化主 benchmark；baseline 按因果角色最小完备，不无限堆方法。
- 当前状态仍为 `HOLD / REWORK`，Gate A/B/C 全部 HOLD；主线文档未自动改写。本轮下一步固定为 Gate A/B/C 收口 → P/Q synthetic test → 唯一 Proposed 同合同 CPU matrix → selector/verifier 2×2 → 独立 confirmation，未过 CPU gate 不开新的效果型 A800。

### 2026-09-29 — 多 agent 下一步优先级审查（建议，待 Owner 确认）
- 依据故事、方法和 benchmark 角色分别复核，推荐顺序为：Gate 0 scope/claim 与唯一 method card 冻结 → Gate A benchmark eligibility → Gate B baseline role/fidelity → Gate C E1–E5 → Gate E6 一次性 confirmation。
- 可并行：baseline adapter CPU/synthetic smoke、hERG/AMES lineage 审计、checker/recovery 文档；不可提前：主表效果跑批、调参、把 controlled event 当自然 drift、confirmation。
- Gate A 停止线：truth/label/overlap/lineage/checker 仍不闭合则只做 protocol/data audit；Gate B 停止线：没有 source-fidelity strong nearest 或主方法合同不一致则停止 Proposed 比较；Gate C 停止线：E1 无 headroom、E2 不超过 strongest matched、E3 增益来自额外调用/标签或 E5 无法恢复则收窄或停止相应主张。
- 这是一条管理建议，不改变三份生效主线文档，也不等同于新的共同决议。

### 2026-09-29 — Owner 纠正：目标 claim 不降，方法改用新名称
- Owner 明确：不能把当前负结果直接转成 protocol/negative diagnosis 主线；目标仍是完成动态工具适应模块、延迟 outcome feedback 实时学习方法和 Agent 可靠调用能力。当前结果用于定位 bug、改方法和继续迭代，不用于撤销目标。
- 论文不直接使用 JEV 作为方法名。当前工作名定为 **MIRA（Menu-aware Incremental Reliability Adaptation for Scientific Tool Calls）**；JEV 只保留为历史功能参照。投稿前仍做名称冲突审查。
- 新增生效决议：`docs/决议/0006-目标主张保持与方法新命名.md`；更新三份生效主线文档为 v003/v002/v003、`docs/adr/0045` 状态和 `paper/aamas2027/README.md`。目标 claim 与当前证据状态分层记录：目标不降，已验证结果仍需 Gate/实验支持。
- 后续路线不变但定位改变：Gate 0 scope/method card → Gate A benchmark → Gate B baseline → P/Q 与唯一 MIRA 候选 CPU 闸门 → E1–E5 → Nebula A800 迭代 → 一次 confirmation；有充足算力时继续试错，但每轮必须写根因、改动、最近邻、预期改善和 kill criterion。

### 2026-09-29 — Owner 确认题目保持不变
- 当前题目继续使用 `After the Decision: Learning to Adapt from Delayed Outcomes`；方法名改为 MIRA，不把 MIRA 强行放进题目。
- 新增生效决议：`docs/决议/0007-论文题目保持不变.md`。后续只更新摘要、方法和实验证据，不因方法迭代顺手改题目。

### 2026-09-29 — MIRA 下一版机制审计与 method card
- 完成：按代码 bug、selected-only 信息边界、表示/目标错配、统计不足四类审查 development master table 及现有 JEV/router/verifier/version-event 候选。
- 产物：`research/debate_story_method_20260929/round1/method_theory_auditor_mira_method_card.md`。
- 结论：下一候选收窄为 MIRA-SafeSwitch（fit-only calibrated prior + selected-only delayed risk ledger + one-sided incumbent veto）；不估计未选 provider 当前质量，不把 Beta/Kalman 本身写成创新。最小验证为 no-event 对照 + controlled-version-event replay + 独立 NumPy replayer，预注册 P1–P6 预测与停止条件。
- 未完成：尚未运行 MIRA-SafeSwitch CPU replay；等待 Owner 按 Gate A/B/C 状态决定是否冻结该候选。

### 2026-09-29 — C0 Random baseline 合同收据
- 完成：在 `pharma_online_baselines.py` 增加固定 seed 的 `RandomPolicy`；反馈释放推进 protocol version，但不修改模型状态，propensity 按当前菜单记录为 `1/|A_t|`。
- 真实缓存 smoke：C0 + C4–C8、64 个 BBB development molecules、33 个 provider、delay `{0,1,4}`，共 18 streams；decision/terminal/feedback 各 1,152。GPU/API 新预测均为 0，`effectiveness_estimate=null`，confirmation 关闭。
- 独立 verifier 通过，篡改第一条 decision 的 propensity 后以退出码 1 拒绝。最终收据目录：`experiments/logs/task49_baseline_contract_smoke_20260929/random_v2/`；报告：`task/task49-pharma-benchmark-baseline-freeze/random_contract_smoke_v1/report.md`。
- Gate 影响：补齐 C0 合同证据，可支持 Gate B 的 B9/B10；Gate B 仍 HOLD，C1–C3 统一主表、静态强 router fidelity、GraphRouter/Laya 资格处理仍未完成。
- 下一步：先做 MIRA-SafeSwitch 的 no-event + controlled provider-version-event CPU replay；未过 CPU gate 前不开新的效果型 A800。

### 2026-09-29 — MIRA-SafeSwitch CPU replay v1
- 完成：新增 `mira_safeswitch.py`、CPU runner、独立 NumPy/scipy replayer 和分析器；候选只读取 fit-only direct prior 与 selected delayed feedback。
- 真实 development replay：5 folds × 2 protocol slots × 2 场景（no-event / controlled provider-version degradation）× 3 方法（Frozen / MIRA / matched Online-SGD）× 3 delays，共 180 streams；独立 replayer 逐事件核验通过，60 条 MIRA streams 重算 masked scores、Beta state、block reason、version。
- 结果：controlled-event 的 MIRA 事件后 accuracy 相对 Frozen 为 `+4.81/+3.99/+3.03pp`（delay 0/1/4），第一次合法 target failure release 后 target selection rate 为 0；no-event 相对 Frozen 为 `-0.42/-0.55/-0.55pp`。证据支持“观测到明确 version degradation 后安全离开 incumbent”，但暴露单次瞬时 failure 立即阻断带来的静态损失。
- 收据：`experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v1/`；报告：`task/task49-pharma-benchmark-baseline-freeze/mira_safeswitch_cpu_v1/report.md`。未使用 GPU/API，confirmation 关闭。
- 下一步：把瞬时 failure 与持续 degradation 分开，先实现连续两次 selected runtime failure 才阻断的 v2；在同一 180-stream 合同上预注册 no-event ROPE、event suffix 和切换阈值，若不能回收静态损失则停止该变体。

### 2026-09-29 — MIRA-SafeSwitch CPU replay v2 与 P/Q 信息边界检查
- 完成 v2：只把同一 provider 的 runtime failure 阻断改为连续两次；其余 prior、输入、fold、slot、delay、对照和日志合同保持不变。先前 `run_v2/` 的版本字段误标为 v1，已保留现场并修正生成器；权威收据为 `experiments/logs/task49_mira_safeswitch_cpu_20260929/run_v2_fixed/`。该收据的独立 replayer 通过 180/180 事件守恒和 180/180 MIRA 逐事件重放，terminal 篡改回放被拒绝。
- v2 结果：no-event 相对 Frozen 为 `-0.28/-0.42/-0.42pp`，均在预注册 -1.0pp ROPE 内；controlled-event suffix 相对 Frozen 为 `+3.70/+3.29/+2.74pp`，三个 delay 均超过 +1.0pp；第一次 target failure release 后 target selection rate 为 `1.64%/1.51%/1.18%`，均低于 2%。这只支持受控 provider-version event 下的机制信号，不是自然 drift、显著性或最终主结果。
- 新增 P/Q 隐藏未选标签不变性收据：`task/task49-pharma-benchmark-baseline-freeze/mira_pq_invariance_v1/report.md`，运行目录 `experiments/logs/task49_mira_pq_invariance_20260929/run_v1/`。5 个事件只反转未选 provider 标签，P/Q 的 action、scores、version 和 feedback state 完全一致；独立 verifier 通过，篡改 hidden labels 后退出码 1 拒绝。
- 相关代码：`mira_safeswitch.py` 的 streak 逻辑、CPU runner/verifier 的运行配置与事件合同、P/Q runner/verifier 和测试；MIRA 测试 `8 passed`。长程状态已更新到 `docs/长期任务与推进原则.md`。
- 当前判断：v2 通过 development 闸门，但仍未超过 Best Fixed，也未闭合 Gate B/C。下一步将 P/Q 证据并入方法闸门，收口强 baseline fidelity，随后运行唯一 Proposed MIRA CPU matrix；未过 CPU gate 前不开新的效果型 A800。

### 2026-09-30 — Random 全量 development baseline 收据
- 为补 Gate B 的统一下界，新增 `run_random_development_v1.py` 与独立 `verify_random_development_v1.py`。在同一 722 分子、33 provider、5 scaffold folds、2 protocol slots、delay `{0,1,4}` 合同上完成 30 条 stream（4,332 decisions/terminals/feedback）。
- 独立 verifier 重新核对 frozen cache 输出、truth/judge、均匀 propensity、selected-only visibility、due/release tick、版本时钟和 feedback conservation；30/30 streams `verified`。篡改第一条 propensity 后 verifier 退出码 1 拒绝。
- Random accuracy 三个 delay 都是 `0.844971`，这是延迟不影响随机动作的预期结果；明显低于 Best Fixed `0.9155`，仅作统一下界，不支持方法优越性 claim。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/random_development_v1/report.md`；收据：`experiments/logs/task49_random_development_20260930/run_v1/`。development master table 和当前 Gate B 审计已补上 Random，Gate B 仍因 GraphRouter/Laya fidelity 等缺口保持 HOLD。
- 下一步：核对静态强 router 的 source fidelity 和统一 raw-run contract；不因 Random 收据已补齐而提前申请 A800。

### 2026-09-30 — 静态 router 角色审查
- 复核 Gate B 的“强静态对照”缺口：`fingerprint_diagnostic_v1` 已有同任务 MLP BCE 与 BT+CLS 的 fit-only full-panel static router，5 个 scaffold folds、2 个 seed、40 fits、5,776 evaluations，checkpoint/NumPy recovery 完整；MLP 为 `0.8961/0.8906`，BT+CLS 为 `0.8878/0.8947`，均低于 Best Fixed `0.9155`。
- 新增范围审查：`task/task49-pharma-benchmark-baseline-freeze/static_router_sufficiency_audit_v1/report.md`。结论是 MLP/BT+CLS 可承担 task-native static-router 角色；GraphRouter 继续保留论文/源码审计，但因 provider description、edge 语义和 `torch_geometric` 依赖未冻结，不运行改写版、不冒称原方法。
- 更新 development master table 与当前 Gate B 审计：静态 router 证据已列入，Gate B 仍 HOLD，剩余是统一 raw-run contract、provider lineage/model-fresh 资格和 Laya/JEV 的正式范围裁定。

### 2026-09-30 — 验收状态快照 v3
- 新增 `research/AAMAS2027_current_evaluation_scorecard_20260930.md` 和 `research/AAMAS2027_benchmark_baseline_acceptance_audit_20260930.md`，把 Random 全量收据和 static-router 角色审查纳入当前状态。
- 临时重评：Gate A=`11/16` HOLD，Gate B=`11/16` HOLD，Gate C=`10/16` HOLD；Benchmark+Baseline 总分临时 `41/60`。这不是最终双人评分，也不改变目标 claim。
- 当前最近的高收益缺口已经从“有没有 Random/静态 router”转为：统一 raw-run/recovery schema、provider training membership/model-fresh lineage、hERG endpoint equivalence、E1–E5 与 dynamic event/成本/paired uncertainty、一次冻结 confirmation。

### 2026-09-30 — 主 baseline raw-run schema 只读审计
- 完成六类主 baseline 原始 JSONL 的横向字段审计：Matched Online-SGD、PILOT、BayesianRouter、Greedy LinUCB、MIRA SafeSwitch v2、Random。新增 `src/probe_headless/audit_baseline_raw_schema_v1.py`，只读读取已有日志，记录 event counts、语义字段别名和缺失项，不改写旧证据。
- 关键发现：Greedy 是当前唯一同时记录 decision-read/released feedback、terminal、feedback 和逐事件 state hash 的主 baseline。Matched/BayesianRouter 原始日志缺 native terminal 与 decision event ID/menu/read-set，但旧 recovery 可从 sealed inputs 派生验证 selected status、reward、delay/release 和更新计数；PILOT/Random 缺信息边界或状态 hash；MIRA 缺 menu/read-set/state hash，terminal correctness 也未单独落盘。BayesianRouter action replay 已补做：60/60 streams、8,664/8,664 actions、0 mismatch，正式收据见 `fingerprint_diagnostic_v1/online_bayesianrouter_action_replay_v1.md`。
- 正式报告：`task/task49-pharma-benchmark-baseline-freeze/baseline_raw_run_schema_audit_v1/report.md`；机器结果 `audit.json`；审计脚本已在六个真实 JSONL 上运行并通过 Python 编译检查。独立证据要求交叉审查见 `.scratch/aamas_evidence_audit.md`（commit `18ed1423`）。
- 结论：旧 development 结果保留，但 Gate B 公平合同继续 HOLD；不能把六类方法说成原生由一套统一 raw schema 完整复核。下一步先实现 CPU writer/replayer，小批迁移 Random/MIRA，并补 BayesianRouter action replay；若要统一合同，再重放 Matched/BayesianRouter。旧日志不事后补写，不因本次审计申请新的效果型 A800。

### 2026-09-30 — MIRA veto 机制消融
- 按机制审查新增预注册 `task/task49-pharma-benchmark-baseline-freeze/mira_veto_ablation_v1/preregistration.md`，只复用 v2 的 fit/feature snapshots、输入、episode order、event、delay 和 evaluator；不重训、不申请 GPU。
- CPU 完成 300 条 stream（Frozen / streak-only / beta-only / full MIRA / matched SGD；no-event 与 controlled-event；delay 0/1/4），共 43,320 decision/terminal/feedback。独立 verifier 通过；full 与 `run_v2_fixed` 的 60 streams/8,664 decisions 逐事件 action/version/block state 完全一致。
- 结果：controlled-event post delta vs Frozen，streak-only=`+4.26/+3.98/+3.98pp`，full=`+3.70/+3.29/+2.74pp`，beta-only=`-6.81/-4.31/-7.99pp`，matched SGD=`+5.51/+4.82/+5.50pp`（delay 0/1/4）；no-event full=beta-only=`-0.28/-0.42/-0.42pp`。Beta veto 是主要静态损失来源，runtime-failure streak 保留主要 event 响应，但 full 仍低于强 matched baseline。
- 报告与收据：`task/task49-pharma-benchmark-baseline-freeze/mira_veto_ablation_v1/report.md`、`experiments/logs/task49_mira_veto_ablation_20260930/run_v1/`。方法方向不降级，下一候选固定为 `availability-hard / correctness-soft`，先 CPU 验证，再决定是否使用 A800。

### 2026-09-30 — MIRA availability-hard / correctness-soft v1 CPU replay
- 针对 veto 消融暴露的 Beta hard block 误伤，新增候选 `MIRAAvailabilitySoftPolicy`：连续两次 selected runtime failure 仍 hard block；selected chemical correctness 只进入 selected-only Beta ledger，并按固定 `s=q0-[U-tau]_+` 排序惩罚（n_min=20、delta=.05、tau=.20、eta=1），不再触发 Beta hard block。冷启动 penalty 显式为 0，runtime failure 不进 Beta，unknown 不计数不清零。
- 预注册：`task/task49-pharma-benchmark-baseline-freeze/mira_availability_soft_v1/preregistration.md`。实现与测试：`src/probe_headless/mira_availability_soft.py`、`src/probe_headless/run_mira_availability_soft_cpu_v1.py`、`src/probe_headless/tests/test_mira_availability_soft.py`；相关 runner/verifier 已加入 candidate 的源码 hash 和 decision score payload。
- 权威 CPU 收据：`experiments/logs/task49_mira_availability_soft_cpu_20260930/run_v2/`。5 folds × 2 slots × 2 scenarios × 3 delays × 6 variants = 360 streams；decision/terminal/feedback 各 51,984；独立 verifier 通过，full v2 与旧 run_v2_fixed 的 60 streams/8,664 actions 完全等价，provider 篡改以 exit 1 拒绝。首轮父目录缺失的启动失败保存为 `launch_failure_v1.json`，不含科学结果；run_v1 仅保留现场，run_v2 为权威。
- 结果（controlled-event post delta vs Frozen）：availability-soft=`+4.26/+3.43/+3.43pp`，streak-only=`+4.26/+3.98/+3.98pp`，full v2=`+3.70/+3.29/+2.74pp`，matched Online-SGD=`+5.51/+4.82/+5.50pp`（delay 0/1/4）。no-event availability-soft=`-0.42/-0.48/-0.48pp`，仍有静态损失；它比 full v2 改善了 event 前 target hard block，但没有超过 streak-only 或 matched baseline。
- 机制诊断：8,664 条 candidate decisions 中 4,748 次有非零 penalty，base argmax 与 soft argmax 改变 3,582 次（no-event d0/1/4=681/626/488，controlled=683/627/477）；每个 scenario 选中 provider 自带非零 penalty 仅 16/13/10 次。主要行为是惩罚已观察 provider 后把选择让给未观察 provider，产生隐式探索，解释 no-event 损失；当前 additive penalty 的概率/utility 语义和 selected-only 可识别性仍不足。
- 结论：v1 候选不进入 A800；不撤销目标 claim。下一步等待独立理论审查，优先验证有界乘性可靠性因子或 context-conditioned selected residual 的数学必要性；必须先写新候选卡和 falsification condition，再做 CPU replay。

### 2026-09-30 — Nebula fingerprint diagnostic 完整日志回收
- 只读轮询任务 `01a0de1cf322cd9ee1ead48f41569649`：状态 SUCCESS，worker exit 0，A800-SXM4-80GB；没有重训或新提交。
- 用 `nebula-cli logs download --all` 再次取回四个完整日志文件：archive 10,832,734 bytes，当前 archive SHA `cd2379926e4bf30196e22671540ebd784064cb3f73215cb8743e4a3c9ad827f`；worker stdout 19,686,134 bytes，SHA `5b1ead687bb825c27e95cc015591c40160504782692ae4f244ca2bdd49abff9c`。与既有 download receipt 的解包文件逐一一致。
- 使用当前 stdout 重新运行 `recover_pharma_fingerprint_diagnostic.py`，`recovered_v5/summary.json` 状态 `verified`，计数 config=1、split=5、epoch=8,000、checkpoint manifest=40、checkpoint chunk=1,970、fit_complete=40、evaluation=5,776、complete=1；`recovered_v5/raw.jsonl` 与 `recovered_v4/raw.jsonl` 逐字节一致，summary SHA `45ffa7131a35a8c4407d233445b4911c9a293058ad3194d97536c47892574906`。
- 结果没有任何变化：Best Fixed=`0.915512`，MLP BCE=`0.896122/0.890582`，BT+CLS=`0.887812/0.894737`；仍是 static representation diagnostic，go-signal=false，不构成 MIRA 效果结果。Task49 fingerprint report 已追加本次回收记录。

### 2026-09-30 — availability-soft 只读机制审计与理论修正
- 独立理论审查写入 `task/task49-pharma-benchmark-baseline-freeze/mira_availability_soft_v1/theory_review.md`：Beta ledger 估计的是 selected-policy arm-level risk，不是当前分子 context risk；加性 penalty 把已观察 provider 降权后流向 penalty=0 的未观察 provider；`n_min` 还造成不连续状态跳变。
- 新增只读审计脚本 `src/probe_headless/analyze_mira_availability_soft_mechanism_v1.py`，不导入策略类、不重跑数据。对权威 `run_v2/events.jsonl` 重算 8,664 candidate decisions，状态 `verified_read_only_audit`、0 violations；收据 `run_v2/mechanism_audit.json` SHA `2e03d3c225783646ddf1290ac23bd39b2e5ca88e185a351b02472f23bb6201af`。
- 审计分类：rank change 3,582 次，其中 observed→zero-penalty 3,554、observed→observed 28；两个 provider 都低于 n_min 时没有 rank change；n_min 19→20 的 release 192 次但没有形成该类 rank change；实际 selected provider 自带非零 penalty 78 次。该结果把 no-event 损失归因到隐式探索，而不是“正确性软更新成功”。
- 理论下一候选收窄为连续的 selected-risk 相对可靠性因子：用现有 tau=.20、n_min=20 作为 Beta 先验均值/等效样本量，后验均值 `e_hat=(n_min*tau+f)/(n_min+v)`，`rho=min(1,(1-e_hat)/(1-tau))`，再用 `q_t=q0*rho`；availability hard ledger 保持不变。该候选仍未运行，不能写成已确认方法，也不能声称 context-specific risk；若要 context-specific，必须另有 propensity support/IPW residual。
- 下一步：先把 relative-reliability 候选写成独立 preregistration/method card，再做 Frozen/availability-soft/relative-reliability/matched-SGD 同 snapshots CPU replay；不申请 A800，结果负也保留。
- 独立 action/state replayer `src/probe_headless/verify_mira_availability_soft_actions_v1.py` 已完成：不 import policy，60/60 candidate streams、8,664 decision/terminal/feedback 全部通过；重算 score/argmax/version/Beta/streak/release/terminal correctness，runtime failure 108 次未污染 Beta，correctness hard block=0，fallback=0。反馈 state 和 decision score 篡改测试均 exit 1；收据 `experiments/logs/task49_mira_availability_soft_cpu_20260930/verification_v1/verification.json`、`tamper_tests.json`。

### 2026-09-30 — MIRA relative-reliability v1 CPU replay 与独立回放
- 完成上一轮理论审查提出的连续乘性候选：$\hat e=(n_{min}\tau+f)/(n_{min}+v)$，$\rho=\min(1,(1-\hat e)/(1-\tau))$，$\tilde q=q_0\rho$；availability hard channel、反馈时钟和所有输入保持不变。预注册与报告：`task/task49-pharma-benchmark-baseline-freeze/mira_relative_reliability_v1/{preregistration.md,report.md}`。
- 复用同一 snapshots 完成 Frozen、streak-only、relative-reliability、availability-soft、full MIRA v2、matched Online-SGD 的 360 条 CPU stream，decision/terminal/feedback 各 51,984。通用 verifier 通过，full 与旧 `run_v2_fixed` 的 60 streams/8,664 actions 逐事件等价。
- 独立 NumPy/SciPy action/state replayer `src/probe_headless/verify_mira_relative_reliability_actions_v1.py` 通过 60/60 candidate streams、8,664/8,664 events；selected-only、delayed release、version、runtime failure 不入 correctness ledger、correctness hard block=0、fallback=0 均重算一致。decision reliability-factor 与 feedback state 篡改均 exit 1；收据 `experiments/logs/task49_mira_relative_reliability_cpu_20260930/verification_v1/`。
- 结果：no-event 相对 Frozen 为 `+0.14/+0.14/+0.07pp`（delay 0/1/4，均在 ±1pp ROPE）；controlled-event 为 `+4.67/+4.39/+3.98pp`，三个 delay 均保留 streak-only 切换收益，delay 0/1 略高于 streak-only；matched Online-SGD 仍为 `+5.51/+4.82/+5.50pp`，仍更强。relative candidate 通过 development CPU gate，但尚不构成最终方法或优于强 baseline 的 claim。
- 机制读数：relative candidate 每个 scenario/delay 的 1,444 个 candidate decisions 中 rank change 约 465/353/287（no-event）和 468/356/290（controlled-event）；变化来自 selected evidence 的连续乘性降权，仍是 selected-only arm-level adaptation，不估计未选 provider 当前分子质量。
- 下一步：先补 Proposed 与主 baseline 的统一 raw-run/recovery schema及 provider lineage/source-fidelity 闸门；通过后再提交一个 A800 full matrix 任务，固定参数、一次一任务，之后只做冻结 confirmation。

### 2026-09-30 — MIRA raw-run contract smoke
- 针对 baseline raw schema 审计的具体缺口，新增 `run_mira_contract_raw_v1.py` / `verify_mira_contract_raw_v1.py`。在真实冻结 cache 上用 64 个 development molecules、33 个 provider、delay `{0,1,4}` 跑 3 条 contract-only stream；decision/terminal/feedback 各 192。
- MIRA 现在能原生记录 menu、decision read-set、released feedback IDs、feature hash、terminal selected output/correctness、due/release tick、version 和逐步 state hash；feedback 日志不含 evaluator chemical correctness。独立 verifier 通过，篡改 menu 和 state hash 均 exit 1 拒绝。
- 为支持这一合同，`TerminalFeedbackQueue` 增加结构化 policy 的 bind/release 生命周期；旧 online kernel 的 observe 路径和已有 36 项测试仍通过。报告与收据：`task/task49-pharma-benchmark-baseline-freeze/mira_contract_raw_v1/report.md`、`experiments/logs/task49_mira_contract_raw_20260930/run_v3/`。
- 这只补 MIRA 的证据合同，不改变效果结果，也不等于 C0–C8 已统一重放。下一步按同一 writer/replayer 迁移 Random、Matched-SGD、PILOT、BayesianRouter、Greedy，并绑定 immutable code commit；A800 继续受 Gate A/B/C 条件性 HOLD 约束。
- 对旧 C0/C4–C8 contract smoke 做回归：18 条 stream、1,152 个 decision/terminal/feedback，原 verifier 仍 `verified`；收据 `experiments/logs/task49_baseline_contract_smoke_20260930/mira_queue_regression_v1/`。这只证明 queue 改动没有破坏既有 adapter 合同。

### 2026-09-30 — MIRA contract provenance 闭合
- 为避免 Nebula gitless archive 再次造成代码身份歧义，`run_mira_contract_raw_v1.py` 增加 `--code-commit` 和 `code_identity`；新增 `verify_commit_provenance_v1.py`，直接从 immutable git tree 读取 9 个 source 文件并比对 SHA-256。
- 权威 contract 收据更新为 `experiments/logs/task49_mira_contract_raw_20260930/run_v5/`：raw verifier 3/3 streams 通过，provenance 9/9 source hash 匹配；把 declared commit 篡改为全零 hash，provenance verifier exit 1 拒绝。`run_v4` 的错误 declared commit 保留为 engineering failure，不作科学证据。
- 报告已更新：`task/task49-pharma-benchmark-baseline-freeze/mira_contract_raw_v1/report.md`。这只闭合 MIRA 的 provenance，不改变 Gate A/B/C 仍 HOLD 的判断。

### 2026-09-30 — baseline + MIRA 跨收据统一 schema 审计
- 新增 `src/probe_headless/audit_unified_contract_schema_v1.py`，把已独立验证的 C0/C4–C8 contract smoke 与 MIRA v5 放在同一字段合同下检查。两组收据共 21 条 stream，decision/terminal/feedback 各 1,344；64 分子、33 provider 输入 SHA 一致。
- 所有 run 都原生记录 menu、decision read-set、released feedback IDs、provider binding、terminal correctness、due/release tick、version、state hash 和 unknown 语义；结果 `verified`。报告：`task/task49-pharma-benchmark-baseline-freeze/unified_raw_schema_audit_v1/report.md`，机器收据：`experiments/logs/task49_mira_contract_raw_20260930/unified_schema_audit_v1.json`。
- 该审计补了 contract smoke 层的 Gate B 证据，但不把旧 effect runs 自动升级为统一主表；source fidelity、provider lineage、hERG/AMES 和确认集仍是阻塞项。

### 2026-09-30 — relative-reliability A800 full-matrix 只读计划
- 完成：审查现有 Nebula fingerprint/Laya 入口、日志恢复约定和 CPU gate；确认 CPU runner 不能直接作为 A800 `--entry`。新增 `task/task49-pharma-benchmark-baseline-freeze/mira_relative_reliability_v1/nebula_plan.md`。
- 计划冻结：输入 SHA `a73f1cf...e3bdf`、CPU gate sealed 20 个 fit/feature snapshots 及其 20 个 hash、360 streams/51,984 decision-terminal-feedback；OSS 只作输入，任务内先复制到本地并 hash 校验；relative 子集仍由独立 replayer 逐事件重放。
- 关键边界：不在本批用 GPU float32 重算 feature/direct head，避免改变 CPU gate candidate；现有代码没有 full entry，不能直接提交。未提交 Nebula 任务，未形成 A800 科学结果。
- 计划文档：`task/task49-pharma-benchmark-baseline-freeze/mira_relative_reliability_v1/nebula_plan.md`。下一步：新增 file-entry + recoverer，推送固定 commit 后再做一次 dry-run 和单条 full matrix。

### 2026-09-30 — Matched Online-SGD unified raw receipt
- 针对 Gate B 的旧日志合同缺口，新增 `run_matched_sgd_unified_raw_v1.py` 与不导入 policy 的 `verify_matched_sgd_unified_raw_v1.py`；先登记 `task/task49-pharma-benchmark-baseline-freeze/matched_sgd_unified_raw_v1/preregistration.md`，再执行真实冻结 cache replay。
- 小 smoke 首次发现 frozen protocol version 未推进，保留失败目录并修正；full_v1 raw replay 虽通过，但 provenance 因绝对 source path 退出 1，也保留为工程失败。
- 修正 protocol clock 与相对 source path 后，`experiments/logs/task49_matched_sgd_unified_raw_20260930/full_v2/` 完成 60 streams（frozen direct / matched Online-SGD × 5 folds × 2 seeds × delay `{0,1,4}`），8,664 decision/terminal/feedback；独立 NumPy verifier、6/6 source immutable provenance 均通过。events SHA=`019d70beda63cf0eb337a8dc085eacc6bbfb89ef065ed782ca29db48730fa693`。
- 当前 receipt 的描述性 accuracy：frozen=`0.901734`（三种 delay 不变），matched Online-SGD=`0.891375/0.888587/0.896868`（delay 0/1/4）；它与 MIRA 同 representation/warm start，旧 matched development receipt 不同表示合同，不能直接混表。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/matched_sgd_unified_raw_v1/report.md`；验收审计已更新，但 Gate B 仍 HOLD，剩余 unified full-effect receipts、provider lineage、endpoint equivalence 和 E1–E6。
- 下一步：按同一合同迁移 BayesianRouter 或 PILOT 的 full development effect（先登记范围和反证条件），完成后再重新审查是否满足 A800 入口条件；暂不提交新的效果型 Nebula 任务。

### 2026-09-30 — relative vs matched 科学优先级复盘
- 只读重算 relative CPU receipt（events SHA `8ceae41fa4ba444e03014bc94123becf8790f288ede3c8e2fb855a070000517f`），拆分 no-event stationary、controlled post suffix、controlled overall 和首次 failure release 后 target exposure，确认这些 estimand 不能混成“方法强弱”。
- 结论：relative no-event 相对 Frozen 为 `+0.14/+0.14/+0.07pp`，controlled post 相对 Frozen 为 `+4.67/+4.39/+3.98pp`；但 relative−matched controlled post 为 `-0.84/-0.43/-1.52pp`，overall 的 `+0.48/+0.62/-0.41pp` 主要由 matched event 前静态代价造成。matched 的 runtime-failure reward-0 update 是首要待验证原因，不把它写成已证实机制。
- 新文档：`task/task49-pharma-benchmark-baseline-freeze/mira_relative_reliability_v1/next_scientific_priority.md`。建议暂缓脱离科学问题的逐方法 raw schema 迁移；下一科学问题是 stationary preservation 与 persistent-failure recovery 的双目标冲突，后续最小方案为 transient/persistent failure 双事件 replay，固定 snapshots、fold/seed/delay，预登记 target exposure、post gain 和 ±1pp falsification 条件。

## 2026-09-30 queue-boundary impact audit

- **审查目标**：只读核对 `TerminalFeedbackQueue` 的 recovered-label binding bug 对旧 MIRA relative-reliability effect、MIRA contract v5、StructuredPolicy 与 Matched 的实际影响；不改 runner/queue/policy，不重跑全量。
- **结论**：旧 360-stream CPU effect 使用 `run_structured_cpu_effect_v1.DelayedFeedback`，绑定 raw label，未走 `TerminalFeedbackQueue`；不受影响。MIRA/JointResidual/Matched release 不读 `pending.label`，因此旧 queue 对 action/state/effect 不构成污染。MIRA v5 pinned `bd525311...` 确有 bug，但仅作为 lifecycle receipt 保留，不能证明 raw-label binding。StructuredPolicy 读取 `pending.label`；通过旧 queue 的 Structured 运行会错更新，但既有 structured effect 使用本地 DelayedFeedback。
- **证据**：`task/task49-pharma-benchmark-baseline-freeze/mira_relative_reliability_v1/queue_boundary_impact_audit.md`；合成边界测试结果记录于该文档。修复源为 `445611a6`。
- **下一步**：queue-based receipt pin 修复源并添加 raw-label binding regression；不因本 bug 重跑旧 relative full effect。

### 2026-09-30 — native Bayesian receipt、queue边界审查与 MIRA unified full receipt
- Bayesian common-direct-prior receipt 完成后发现先验来源不匹配：Matched unified snapshot 的 direct prior 与旧 A800 BT+CLS `score.weight` 不同，不能把前者标作 C8 native baseline。旧 `bayesianrouter_unified_raw` 保留为 scope 失配现场；新增 `bayesianrouter_native_bt_raw_v1`，从 `recovered_v5` 同一 BT+CLS checkpoint 提取 `score.weight` 和 encoder，固定 60 streams/8,664 events。独立 NumPy verifier、6 个 source provenance、checkpoint manifest 均通过；native receipt events SHA=`9d86adf20f41b895d5f91f010b57b2b6647060431a2d3694d09f32a4a3b1870e`，Bayesian online accuracy=`0.888587/0.889272/0.890675`，frozen=`0.891355`（delay 0/1/4）；仍低于 Best Fixed，不做 superiority claim。报告：`task/task49-pharma-benchmark-baseline-freeze/bayesianrouter_native_bt_raw_v1/report.md`。
- 队列边界审查确认旧 360-stream MIRA relative effect 走独立 `DelayedFeedback`，绑定 raw label，不受 `TerminalFeedbackQueue` bug 影响；MIRA/Matched/JointResidual release 不读 pending.label，StructuredPolicy 才敏感。审查记录：`task/task49-pharma-benchmark-baseline-freeze/mira_relative_reliability_v1/queue_boundary_impact_audit.md`。修复源为 `445611a6`，后续 queue receipt 必须固定修复源并保留 raw-label regression。
- 新增 `run_mira_unified_raw_v1.py`，在同一 722×33、5 folds×2 slots×2 scenarios×3 delays 合同上完成 Frozen/streak-only/relative-reliability/matched 四方法 240 streams、34,656 decision/terminal/feedback。schema verifier 240/240 通过；relative 独立 NumPy/SciPy verifier 60/60 通过；provenance 9/9 通过。权威报告：`task/task49-pharma-benchmark-baseline-freeze/mira_unified_raw_v1/report.md`，events SHA=`9f0aca7b3cd7f74576964de1c0a7678d6299bd8a7299fabf41ada70e6ed81be8`。
- MIRA unified overall（不是 post suffix）relative=`0.903113/0.903113/0.902423`，matched=`0.891375/0.888587/0.896868`；controlled relative=`0.894109/0.892725/0.890656`，matched=`0.889306/0.886513/0.894794`。旧 report 的 controlled post 数字仍是单独 suffix estimand，不能混表。
- 代码/测试：延迟 queue raw-label regression 已在 44 项 targeted tests 中通过；native Bayesian smoke 2 streams/10 decisions、full 60 streams verified；MIRA smoke 首次因完整 verifier 要求全 roster 而拒绝，保留为工程现场，full receipt 已通过。未提交新 A800/Nebula 任务。
- 当前下一步：先收口 native verifier 的 immutable-tree/fail-open 审查与提交，再更新 acceptance score；Gate A/B/C 仍 HOLD。下一科学问题仍是 stationary preservation 与 persistent-failure recovery，暂不调参、不打开 confirmation。

### 2026-09-30 — native Bayesian strict verifier gate review
- 将 `verify_bayesianrouter_native_bt_raw_v1.py` 改为 fail-closed 独立 verifier：按声明 commit 的 git tree 校验 source provenance，独立 NumPy 重算 recovered_v5 checkpoint 的 `score.weight` 与 `encoder.0`，逐事件检查 menu/read-set/provider IDs、decision→terminal→feedback 时序、selected-only reward、状态版本和 stream/root summary。
- smoke_v1 正向通过（2 streams/10 decision-terminal-feedback triples）；menu、read-set、terminal reward、summary、duplicate event、提前释放 feedback 六类篡改均被拒绝。full_v1 strict v2 通过（60 streams，8,664 decision/terminal/feedback triples），收据：`experiments/logs/task49_bayesianrouter_native_bt_raw_20260930/full_v1/verification_strict_v2.json`。
- gate review：`task/task49-pharma-benchmark-baseline-freeze/bayesianrouter_native_bt_raw_v1/gate_review.md`；native report 更新为 strict evidence。checkpoint provenance 明确为 recovered_v5 native BT+CLS，不能表述为旧 online A800 checkpoint 的精确迁移。未提交代码。

### 2026-09-30 — native Bayesian strict gate 已提交
- strict verifier 与完整 JSON/终端日志已定向提交并推送：`7dee814c`；验收审计同步记录为新的 protocol-integrity evidence：`31b31cb9`。
- strict 收据仍只支持 recovered-v5 native BT+CLS baseline 的可复核协议，不改变 Gate A/B/C 分数，也不打开 confirmation。下一步回到已登记的 transient/persistent failure replay，先 CPU 复现 stationary preservation 与 persistent recovery 的冲突，再决定是否值得做 A800 full matrix。

### 2026-09-30 — transient/persistent failure replay 预注册
- 新增 `task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/preregistration.md`。固定现有 inputs、20 个 fit/feature snapshots、5 folds、2 seeds、delay `{0,1,4}` 和四个方法，只比较一次 transient failure 与持续 persistent failure。
- 主指标固定为 transient overall、persistent post-event accuracy、首次 failure release 后 target exposure 和 stationary cost；明确 ±1 pp 证伪条件、selected-only 信息边界和失败处置。尚未运行，不提交 Nebula 任务。

### 2026-09-30 — transient/persistent failure replay 完成
- 完成 `mira_event_replay_unified_raw_v1`：transient/persistent × Frozen/streak-only/relative-reliability/matched × 5 folds × 2 seeds × delay `{0,1,4}`，240 streams、34,656 decision/terminal/feedback。原始 events SHA=`918050e6f045fc5d5bfd358ab8fe9d9291c8ceee97fd03a9914b15f836383865`，完整本地收据在 `experiments/logs/task49_mira_event_replay_cpu_20260930/full_v2/`。
- schema/evaluator verifier 240/240 通过；relative 独立 action/state replayer 60/60 通过。persistent relative post gain=`+4.67/+4.39/+3.98pp`，仍低于 matched=`+5.51/+4.82/+5.50pp`；transient relative 的首次 target failure release 比例为 `0/0/0`，因此该 kernel underpowered，不能支持偶发故障结论。
- 关键发现：当前 fit-only Best Fixed target 在 event tick 不在 MIRA 的 action path 上；下一步必须预注册 action-supported target（由 fit-only Frozen 在 event tick 的 incumbent 决定），再判断 transient recovery。当前不提交 A800，不打开 confirmation。代码/报告与收据元数据已提交 `ddc5e5aa`。
- 进一步协议审查写入 `mira_event_replay_v1/action_supported_design_review.md`：主协议推荐 method-conditioned incumbent（按同方法、fold、seed、delay 的 no-event prefix 冻结 event-tick 当前选择），Frozen incumbent 只做共同 target 的 secondary stress。这样保证 transient 事件真正作用在当前选择路径，同时避免把不同 target 的 raw post accuracy 误当成跨方法 superiority。
- 已将该协议正式预注册为 `mira_event_replay_v1/action_supported_preregistration.md`：target manifest 由 event 前 prefix 独立生成并 hash，transient/persistent 共用同一 target；下一步先实现 prefix target generator 和 manifest verifier，再做 CPU replay。

### 2026-09-30 — action-supported replay 收口与指标边界修正
- 完成 action-supported transient/persistent replay 的验收文档同步：240 条 CPU stream、schema/evaluator 240/240、relative 独立 action/state replayer 60/60；target manifest、event-tick action、延迟反馈和 source provenance 均有独立收据。
- 发现并记录指标问题：transient `post` 包含强制注入的失败 terminal，Frozen/streak/relative 均因此下降约 1.38pp，不能解释为恢复失败。报告保留原预注册触发结果，同时把 `injected-terminal loss` 与 event tick 后的 `recovery-only` 分开，禁止事后把 recovery 指标冒充原主指标。
- 科学结论未变：persistent 下 relative 相对 Frozen 有恢复收益，但仍低于 matched Online-SGD；transient recovery-only relative 与 Frozen 持平。Gate A/B/C 和总体 acceptance 仍 HOLD，未提交 A800/confirmation。
- 结果文档：`task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/action_supported_report.md`；验收同步：`research/AAMAS2027_benchmark_baseline_acceptance_audit_20260930.md`、`research/AAMAS2027_current_evaluation_scorecard_20260930.md`。
- 下一步：预注册 v2 recovery estimands（注入损失、恢复段、paired uncertainty）后做一次最小 CPU replay；通过后再评估是否值得 A800 full matrix，不能直接把本轮结果送入 confirmation。

### 2026-09-30 — failure replay 指标 v2 审计与边界闭合
- 只读指标审查指出：`post_recovery=T+1:` 仍把 feedback release 前的 blind window 和 release 后的 adaptation 混在一起。已在运行前登记 `action_supported_metric_v2_preregistration.md`，固定 `L_inject`、`Delta_blind`、`Delta_adapt`、release 后 target exposure、right-censored release-to-switch，以及按 fold 聚类、保留两 seed 的 paired bootstrap。
- 新增不导入 policy 的 `src/probe_headless/analyze_mira_action_supported_metrics_v2.py`。4-stream smoke（frozen/relative × transient/persistent，fold0/seed0/delay0）和 240-pair sealed receipt metric-only audit 均 `verified_metric_v2`；`delay=0` blind window 保持 `NA`，release tick、target、paired entities 和三段指标均由 raw receipt 独立重算。机器收据：`experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_smoke_v4/`、`metric_v2_reanalysis_v2/`；重分析 SHA=`5d0d375d0eda43f920b86a1c2a2b1eff1bf6eb521b670829c6274f3d16ddf9fd`。
- 修正后的读数：transient relative 的 release 后 recovery 三个 delay 均为 0pp；persistent relative 为 `-0.98/-0.85/-0.89pp`，均高于 streak-only `-1.54/-1.13/-1.48pp`，但仍低于 matched。v1 delay4 的表面劣势主要来自 blind window 混入；该只读审计不算新的 effect/confirmation 证据。
- 保留一次工程失败：`metric_v2_smoke_v2` 曾误传占位 declared commit，已写 `failure_summary.json`，不作科学证据；随后用 immutable HEAD 的 `metric_v2_smoke_v4` 重跑并收口。
- 验收文档同步：`task/.../action_supported_report.md`、`task/.../metric_v2_reanalysis_report.md`、`research/AAMAS2027_benchmark_baseline_acceptance_audit_20260930.md`、`research/AAMAS2027_current_evaluation_scorecard_20260930.md`。Gate A/B/C 仍 HOLD。
- 下一步：若要把 v2 指标用于新的完整 effect 证据，先登记新的 full-matrix raw receipt；在此之前优先补 unified PILOT/Greedy、provider lineage、endpoint equivalence、成本/特征公平和 confirmation，不提交 A800。

### 2026-09-30 — Greedy LinUCB unified full receipt
- 按 `task/.../greedy_unified_raw_v1/preregistration.md` 固定当前 unified 输入 `a73f1c…`、BT/CLS hidden-64、fit-only ridge prior、selected-only delayed feedback、delay `{0,1,4}` 和 `greedy_online/greedy_frozen` paired matrix；先跑 2-stream smoke，再完成 60 streams、8,664 decision/terminal/feedback 的 CPU full receipt。
- 新增 `src/probe_headless/run_greedy_unified_raw_v1.py` 与独立 `verify_greedy_unified_raw_v1.py`。verifier 从 immutable checkpoint 重建 hidden、重算 fit-only prior/UCB/rank-one state、核对 menu/read-set/terminal/reward/delay/version/state hash/summary；smoke 和 full 均 `verified`，smoke 的 state-hash/UCB score 篡改均被拒绝。
- 权威 full receipt：`experiments/logs/task49_greedy_unified_raw_20260930/full_v1/`；events SHA=`db6322383a05506154076f3ef5ed24b0847c5a8197fdf0a001e99550fde3cc13`。pooled accuracy online/frozen：delay0 `0.851108/0.856648`，delay1 `0.844875/0.856648`，delay4 `0.862881/0.856648`；Greedy 只在 delay4 高约 0.62pp，未通过“至少两个 delay 正且多数 fold 正”的开发 gate。报告：`task/.../greedy_unified_raw_v1/report.md`。
- 旧 Greedy `full_v3_fix` 使用 P-C1 `94580e…`，仍保留为历史证据；新 receipt 使用当前 `a73f1c…`，不混表。Acceptance audit/scorecard 已同步：Greedy full contract 已闭合，PILOT full effect、跨方法 feature/warm-start/budget/cost、provider lineage、endpoint equivalence、confirmation 仍阻塞，Gate A/B/C 不变。
- 下一步：按同一合同补 PILOT-adapted unified receipt；不提交 A800，先完成 baseline 资格和公平合同。

### 2026-09-30 — failure replay v2 exposure 口径纠正归档
- 将 focused audit `task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/focused_audit.md` 与机器证据 `experiments/logs/task49_mira_event_replay_cpu_20260930/metric_v2_reanalysis_v2/{focused_audit_v1.json,exposure_denominator_audit.json}` 纳入收据链。审计确认 v1 的 8.03%→v2 的 1.40% 差异主要来自旧分析器未将 failure release 绑定到本次 event：fold1 两个 seed 在 event tick 前已有自然 failure release；delay0 blind window 为零，因此不能归因给 blind window。
- 修正 `metric_v2_reanalysis_report.md` 与 `action_supported_report.md`：v1 exposure 保留为历史、未 event-bound estimand；v2 release-bound exposure 单独报告；补齐 release-to-switch（relative persistent 30/30 switched、0 censored、均值 0.80 tick）；明确当前 bootstrap 区间因指标 seed offsets 未事前登记，只作 sealed-raw posthoc 描述。
- 未改 raw、runner、方法或旧数字；Gate A/B/C 不变。下一步继续按统一合同补 PILOT full receipt，并在结果表中保持 event-bound metric 与旧 estimand 分离。

### 2026-09-30 — PILOT-adapted unified full receipt
- 按预注册 `task/task49-pharma-benchmark-baseline-freeze/pilot_unified_raw_v1/preregistration.md` 完成当前 `a73f1c…` 输入的 `pilot_frozen/pilot_online × 5 folds × 2 fit seeds × delay {0,1,4}`：60 streams、8,664 decision/terminal/feedback。权威 receipt：`experiments/logs/task49_pilot_unified_raw_20260930/full_v1/`，events SHA=`541819721b2e90187412a34958ae7f078b0f405467af42063652748fcc1a68e1`。
- 新增 `run_pilot_unified_raw_v1.py` 与不导入 policy 的 `verify_pilot_unified_raw_v1.py`；独立检查 fit-only precision、PILOT projected/normalized query、cosine+UCB argmax、selected-only delayed update、state/version/hash、terminal/provider binding、事件守恒和 immutable source provenance；full `verified`，2 次 state/UCB 篡改均 exit 1。smoke 中发现的二次归一化 hash/state 问题已修正，失败 smoke 保留。
- 结果（pooled online/frozen；paired stream mean）：delay0 `0.884349/0.885042`（`-0.069/-0.068pp`，4/10 正）；delay1 `0.882271/0.885042`（`-0.277/-0.275pp`，4/10 正）；delay4 `0.883657/0.885042`（`-0.139/-0.139pp`，3/10 正）。Best Fixed=`0.915575`。三种 delay 都未通过预注册稳定改善门槛，PILOT-adapted 保留为现代负向 baseline，不调参、不提交 A800。
- 报告与验收表已同步：`task/.../pilot_unified_raw_v1/report.md`、`task/.../pilot_greedy_unified_raw_v1/plan.md`、`research/AAMAS2027_benchmark_baseline_acceptance_audit_20260930.md`、`research/AAMAS2027_current_evaluation_scorecard_20260930.md`。Gate A/B/C 仍 HOLD；PILOT unified 合同已闭合，跨方法 feature/warm-start/budget/cost、provider lineage、endpoint equivalence、E1–E5、动态压力和 confirmation 仍阻塞。
- 下一步：将 Greedy/PILOT/MIRA/native-Bayesian/Matched 的统一 receipt 做一次 cross-method fairness audit，先查 feature、warm-start、update budget 和 cost accounting，再决定是否需要新的动态事件压力实验；不以这批负向 baseline 结果替代公平审查。

### 2026-09-30 — cross-method feature/warm-start/fairness audit
- 只读审查 MIRA、Matched Online-SGD、native-BT BayesianRouter、Greedy LinUCB、PILOT 五条 unified receipt；输入 SHA、33-provider menu、5 folds、2 seed slots、delay `{0,1,4}`、selected-only、feedback clock 和 unknown semantics 全部一致。
- 发现并固定主表解释边界：MIRA/Matched/Greedy/native-BT 虽都为 64D BT/CLS 相关表示，但 prior 来源与初始 covariance 不同；PILOT 是独立 projection/provider-vector/precision 表示。当前只允许各方法内部 `online−frozen` paired effect，不允许把 raw accuracy 混成一条 superiority 排名；cached replay 也没有真实 provider monetary cost，不能写 cost-aware superiority。
- 机器审计：`experiments/logs/task49_cross_method_fairness_audit_20260930/audit_v1.json`；报告：`task/task49-pharma-benchmark-baseline-freeze/cross_method_fairness_audit_v1/report.md`；预注册：同目录 `preregistration.md`。未读写 raw events、未重跑效果。
- Gate B 仍 HOLD；下一步把 common-stratum 的 prior/feature/update-budget/cost ledger 写成可执行方案，再决定动态事件压力和 confirmation，不用已有异质 receipt 拼接结论。

### 2026-09-30 — common-stratum snapshot 核对
- 将 fairness audit 从 config-level 扩展到 10 个 `(fold, seed)` sealed snapshot 的逐数组核对：MIRA/Matched direct prior 最大绝对差 `0`，MIRA/Matched/Greedy/native-BT evaluation feature 最大绝对差均 `0`；Greedy ridge prior 与 Matched direct prior 最大绝对差 `0.7531`，native-BT 仍使用独立 `score.weight`/variance。
- 因此当前可严格声称的 common warm-start 组只有 MIRA/Matched；Greedy/native 是 same-feature/different-prior 对照，PILOT 独立成组。报告与机器 JSON 已更新，未产生新 effect 数字。

### 2026-09-30 — unified receipt runtime/call accounting
- 对 MIRA、Matched、native-BT BayesianRouter、Greedy、PILOT 的 sealed raw events 做只读 call/runtime audit：每条 stream 的 selected cached provider call 与 feedback 数守恒；按 method ID 保存 selection/update p50/p95。
- 结果仅是当前 Python/cache replay 的策略计算开销，MIRA 还包含四方法×两场景，不能用总数直接跨 receipt 排名；没有真实 provider monetary/token cost，也没有 runtime-normalized deployment benchmark。机器收据：`experiments/logs/task49_cross_method_fairness_audit_20260930/runtime_audit_v1.json`；报告已补 runtime/call accounting 边界。
- 不将这些耗时写成 cost-aware superiority；Gate B 的统一 cost ledger 仍 HOLD。下一步优先完成 provider lineage/endpoint equivalence 与 E1–E5 主矩阵，若做成本实验须先冻结硬件、服务价格和计费规则。

### 2026-09-30 — hERG 端点语义补充核验收口
- 读取并核对 Wang 2016、Siramshetty 2020 的 PubMed 一手摘要、NCATS 固定仓库材料和 FP-ADMET SI 的 hERG 段落；没有把摘要中未出现的 cutoff 当成已知事实。
- 新增 `task/task49-pharma-benchmark-baseline-freeze/endpoint_audit/semantics_update_20260930.md`，并更新 `endpoint_audit/report.md`、`source_manifest.jsonl`、`research/AAMAS2027_benchmark_baseline_acceptance_audit_20260930.md`、`research/AAMAS2027_current_evaluation_scorecard_20260930.md`。
- 结论：TDC Wang 来源身份、Siramshetty 约 9,000+840 的数据叙述和 FP-ADMET 对 Cai blocker 的 `IC50 ≤ 10 µM` 规则可追溯；但 TDC/FP-ADMET liability/ADMETlab 的 assay、cutoff、逐分子训练谱系尚未闭合，NCATS 8,993 行也不能当作 FP-ADMET 9,204 行的精确成员表。hERG 继续是候选资格审查，不进入主 benchmark 或 confirmation；Gate A 仍 HOLD。
- 本轮没有提交 Nebula、没有生成新预测、没有修改 raw benchmark 输入；本地 PubMed 文本和 NCATS/FP-ADMET 来源资产的 hash 已登记。
- 下一步：围绕 Gate A 的 provider lineage 做最小可审计闭合，优先完成 GraphB3 checkpoint—训练脚本—train/test split 的绑定审计；若仍缺训练脚本，明确标为观察性 G3，转向冻结 confirmation 的独立来源设计，不提交新的效果型 A800。

### 2026-09-30 — GraphB3 checkpoint–split lineage 审计
- 按 Gate A provider lineage 缺口，对固定 GraphB3 包做只读审计；没有运行推理、训练或生成新预测。
- 机器收据：`experiments/logs/task49_graphb3_lineage_audit_20260930/run_v1/results.json`；源文件 SHA、xlsx 表结构、checkpoint state_dict 形状、canonical overlap 均已记录。
- 结果：train/test 表 6,029/1,771 行，canonical 交集 0；train 有 2 行无效 SMILES、1 个重复 canonical，test 无无效/重复；checkpoint 14 tensors，与当前 33→128 inference 结构一致。
- 关键缺口：包内没有训练脚本、optimizer/loss/backward、训练日志或 checkpoint→split 元数据。GraphB3 维持 `G3-observational`，不能升级为 model-fresh 独立测试 provider。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/graphb3_lineage_audit_v1/report.md`；同步更新 `research/provider_training_membership_audit_20260929.md`。Gate A 不变，未提交 Nebula。
- 下一步：查 GraphB3 作者仓库/发布材料是否能补齐 checkpoint provenance；若找不到，固定观察性 G3 标签，把精力转到独立 confirmation 来源和 E1–E5 主矩阵，不用表级 split 观察替代模型谱系证据。

### 2026-09-30 — GraphB3 原始论文与仓库追溯
- 使用 agent-reach GitHub/Exa 路线找到 GraphB3 一手论文与作者仓库；没有运行第三方代码。
- 论文（PMC12710471）确认 B3DB 来源、分层 split、6029 train-validation / 1771 final test、4823/1206 二次拆分、10 个 seed/split state 和 validation-MCC checkpoint selection；本地表格数量与论文一致。
- 作者 standalone 仓库历史与当前 tree 均没有训练脚本；发布的 `best_mcc_model_weights.pth` 没有绑定到具体 seed/split/member manifest。结论从 G3-observational 更新为 `G3-paper-backed-observational`，但仍不支持 checkpoint-specific model-fresh confirmation。
- 证据：`task/task49-pharma-benchmark-baseline-freeze/graphb3_lineage_audit_v1/paper_evidence.json`、`source_manifest.jsonl`、更新后的 `report.md` 与 `experiments/logs/task49_graphb3_lineage_audit_20260930/run_v1/results.json`。
- agent-reach `check-update`：当前 v1.5.0，已是最新。
- 下一步：若无法从 supplementary/release 补 exact checkpoint mapping，则结束 GraphB3 lineage 追溯，把它固定为 operational baseline；转向 confirmation 来源和 E1–E5 主矩阵，不再为无效独立性假设提交实验。

### 2026-09-30 — confirmation 计划同步 GraphB3 新证据
- 将 `fresh_confirmation_plan_v1.md` 的 GraphB3 资格更新为 `G3-paper-backed-observational`；它有论文支持的 split provenance，但仍不满足 checkpoint-specific 独立 confirmation。
- confirmation 候选仍固定为 157 条 scaffold-disjoint TDC BBB test manifest；不因 GraphB3 论文证据而提前打开 confirmation。

### 2026-09-30 — Gate A 状态细化
- 验收表补充：GraphB3 已有论文级 split provenance（6029/1771 与 10 seed/split protocol 一致），但具体 checkpoint-to-seed/member binding 仍缺；hERG assay/cutoff/lineage 也未闭合，因此 Gate A 仍为 `11/16 HOLD`。

### 2026-09-30 — confirmation 候选 × GraphB3 exposure 审计
- 只读比对冻结 157 条 scaffold-disjoint confirmation 候选与 GraphB3 train/test 表：107 条在 train，27 条在 test，23 条不在两表；157 个候选均为唯一 canonical、无无效 SMILES。
- 这证明 scaffold-disjoint 不能代替 provider training exposure：GraphB3 仍有 107/157 train exposure。27 条 test overlap 也不能自动称为 checkpoint-unseen，因为具体 checkpoint seed/split/member binding 尚未闭合。
- 机器收据：`experiments/logs/task49_graphb3_confirmation_overlap_20260930/run_v1/`；报告：`task/task49-pharma-benchmark-baseline-freeze/graphb3_confirmation_overlap_v1/report.md`。
- confirmation 继续关闭；下一步优先完成可绑定 provider 子菜单的 lineage manifest，再讨论一次性 confirmation。

### 2026-09-30 — LGBM-BBB 训练行谱系重建
- 第一次 Mordred 重建因多进程入口保护缺失失败，失败日志保留在 `experiments/logs/task49_lgbm_lineage_audit_20260930/run_v1/`；没有把失败当结果。
- 第二次改为单进程完成 7,805 个有效分子 × 1,613 描述符；按 notebook 的 MW 150–800 得到 7,296 行，固定 stratified seed42 split 为 5,836/1,460。文档和 README 目标是 7,298 行，且发布包缺失实际训练读取的 `dcorr_filtered_BBB_0.25.csv`。
- 结论：可以追溯 B3DB 来源和处理流程，但不能将重建 split 绑定到发布 `lgbm_model.pkl`；LGBM 继续 G2。报告：`task/task49-pharma-benchmark-baseline-freeze/lgbm_lineage_audit_v1.md`；收据：`experiments/logs/task49_lgbm_lineage_audit_20260930/run_v2/`。
- 未运行 LightGBM fit、provider inference、selector 或 confirmation；Gate A 不变。

### 2026-09-30 — 验收表同步 LGBM 资格边界
- Gate A 审计和 scorecard 增加 LGBM 只读重建结果：7,296 vs 文档 7,298 的版本/中间文件差异，不能把重建 split 当 checkpoint-specific 独立证据；Gate A 仍 `11/16 HOLD`。
### 2026-09-30 — provider-fresh BBB 菜单资格清单 v1

- 为 Gate A 建立了一条可执行的 model-fresh 补齐路线：`src/probe_headless/prepare_provider_fresh_menu_manifest.py` 从公开 TDC BBB Group `train_val` 生成固定 provider-train/provider-validation 成员；排除 8 个 canonical 标签冲突组、已读 selector development panel 和 157 条 confirmation 候选，同标签重复只保留最小 source row。
- 产物：`experiments/logs/task49_provider_fresh_menu_20260930/manifest_v1/`。1,624 个 train_val 原始行 → 1,025 个候选 canonical 分子；provider train=845、validation=180；505/127 个 scaffold，train/validation scaffold overlap=0。四个文件 hash 已写入 manifest，重复运行逐字节一致。
- 独立 verifier `src/probe_headless/verify_provider_fresh_menu_manifest.py` 通过：输入/source hash、逐行源恢复、canonical/scaffold 隔离、development/confirmation overlap、标签冲突和计数均 `verified`；收据：`experiments/logs/task49_provider_fresh_menu_20260930/verification_v1.json`。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_menu_v1/report.md`。候选菜单为 ECFP-LogReg、ECFP-MLP、ECFP-RandomForest、graph message passing；均未训练、未生成 confirmation 输出，Gate A/B/C 仍 HOLD。
- 工程现场：第一次命令使用 `tee` 写不存在的父目录，stdout 文件未生成；manifest 本身正常生成，故未把该命令当科学收据。随后用独立 verifier 和临时目录双跑复现收口；没有提交 Nebula 任务。
- 验收审计与 scorecard 已同步 provider-fresh 资格边界。下一步：为四个候选 provider 写统一训练合同和最小 CPU smoke；训练成员、源码 commit、checkpoint hash 和前向输出全部通过后，再决定是否上 A800。

### 2026-09-30 — provider-fresh CPU training/verification v1

- 按统一训练合同完成本地 CPU 训练：ECFP-LogReg、ECFP-MLP、ECFP-RandomForest、graph message passing；只读取 845 train + 180 validation，未读取 157 条 confirmation。
- 产物：`experiments/logs/task49_provider_fresh_menu_20260930/cpu_v1/`，4 个 checkpoint、720 条 validation prediction、逐 epoch loss/config 和源码 commit=`6962ac6c2607470bb8919ad3a1cf42df079a72ed`。
- 独立 verifier `src/probe_headless/verify_provider_fresh_menu_cpu.py` 重建四个模型的 validation probabilities，最大误差 `<5.0e-11`，checkpoint hash、row binding、threshold、provider roster 全部通过；收据：`experiments/logs/task49_provider_fresh_menu_20260930/cpu_verification_v1.json`。
- sanity metrics：LogReg accuracy/AUC/BCE=`0.9333/0.9759/0.1639`，MLP=`0.9167/0.9355/0.2681`，RF=`0.9111/0.9638/0.2538`，GCN=`0.7611/0.8384/0.4655`。这些不是 selector 或 confirmation 结果；graph 表现较弱保留，不换 split 追分。
- 报告已更新：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_menu_v1/report.md`。Gate A 仍 HOLD；下一步是审查这四个 provider 的训练源码/环境绑定，并在方法、baseline、预算冻结后再决定是否生成 confirmation 前向输出。

### 2026-09-30 — common-stratum baseline 合同

- 针对 Gate B 的主要缺口，新增执行前合同 `task/task49-pharma-benchmark-baseline-freeze/common_stratum_contract_v1.md`：统一 BT+CLS hidden-64 feature、同一 fit-only provider prior `$W_0$`、同一初始 covariance、selected-only feedback/update budget 和 runtime/call ledger。
- strict stratum 包含 Frozen、Matched Online-SGD、Greedy LinUCB、BayesianRouter-adapted、MIRA；PILOT 因额外 projection/provider-vector fitting 继续作为 adapted/out-of-stratum baseline 单列。
- 现有异质 prior receipts 不回溯改写；下一步先实现 2-stream smoke 和独立 verifier，再决定是否跑 60-stream full receipt。Gate B 仍 HOLD。

### 2026-09-30 — provider-fresh manifest v2 修复与 CPU receipt 重跑

- 独立审查发现 v1 的 source partition P0：1,624 条 TDC train_val 源行中，有 14 条同标签重复行既不在 train/validation，也不在 excluded ledger。v1 manifest 和 CPU receipt 保留为历史失败，不再作为当前证据。
- 修复 `src/probe_headless/prepare_provider_fresh_menu_manifest.py`：同标签重复 canonical 只保留最小 source row，其余全部写入 `same_label_duplicate_first_source_row`；记录 RDKit 版本和 canonical/scaffold 规则；记录 development/confirmation scaffold overlap；生成 v2 manifest。
- 修复 `src/probe_headless/verify_provider_fresh_menu_manifest.py`：强制 train+validation+excluded 恰好覆盖全部源行且每行一次，重算全部 source/dev/confirmation canonical/scaffold，检查 confirmation scaffold 隔离和计数。
- v2 结果：source partition `1624/1624`，candidate canonical `1025`，train/validation=`845/180`，excluded=`599`（其中重复额外行=`14`），train/validation scaffold overlap=`0`，candidate/confirmation scaffold overlap=`0`，candidate/development scaffold overlap=`118`（train 97、validation 21）。独立 verifier `verified`，收据：`experiments/logs/task49_provider_fresh_menu_20260930/verification_v2.json`。
- 按 immutable commit=`8afcf7c935a8c813872fedefe4bc94f526884bbe` 重跑四个 CPU provider；720 条 validation predictions、4 个 checkpoint 由独立 verifier 重算通过，最大概率误差=`4.99e-11`。收据：`experiments/logs/task49_provider_fresh_menu_20260930/cpu_verification_v2.json`、`execution_receipt_v2.json`。
- v2 validation sanity：LogReg `0.9333/0.9759/0.1639`，MLP `0.9167/0.9355/0.2681`，RF `0.9111/0.9638/0.2538`，GCN `0.7611/0.8384/0.4655`（accuracy/AUC/BCE）。这些不是 selector 或 confirmation 结果。
- 文档：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_menu_v2/{report.md,training_contract.md}`；v1 report 已标记 superseded；验收审计和 scorecard 已同步。Gate A/B/C 仍 HOLD，confirmation 未打开。
- 下一步：补齐 CPU receipt 的完整环境锁定与 source-tree binding，再做 provider menu 的最终资格审查；不要把 development scaffold overlap 隐藏成 scaffold-disjoint，也不要进入 confirmation。

### 2026-09-30 — provider CPU receipt 环境与源码绑定补齐

- 审查指出 v2 CPU receipt 只有 commit/hash，缺少完整环境和源树绑定。更新 `run_provider_fresh_menu_cpu.py` 与 `verify_provider_fresh_menu_cpu.py`，记录 Python、NumPy、scikit-learn、PyTorch、RDKit 版本，PyTorch 线程/确定性设置，工作树 dirty 状态和 status hash，并从声明 commit 校验四个相关源码 blob hash。
- 按新 commit=`1e523fb44ce28aefa4af321d15777f844ff05ff8` 重跑 v2 manifest 的四个 provider。v3 verifier `verified`：环境字段完整、声明 commit 存在、四个 source blob 全部匹配、720/720 prediction recovery 通过，最大概率误差=`4.99e-11`。
- 收据：`experiments/logs/task49_provider_fresh_menu_20260930/cpu_v3/`、`cpu_verification_v3.json`、`execution_receipt_v3.json`；报告已更新为 `provider_fresh_menu_v2/report.md`。
- 工作树记录为 dirty 是预期的仓库事实（存在其他未归档工作）；相关源码以声明 commit 的 git-tree blob 绑定，不能把 dirty 状态误写成 clean reproduction。
- 仍未打开 confirmation。下一步是对四个 provider 做最终资格审查：明确 graph family 的 menu status、fold-level common-prior 与 failure/unknown semantics，再决定是否值得上 A800。

### 2026-09-30 — common-stratum fold/seed manifest 封存

- 为 Gate B 建立 `src/probe_headless/build_common_stratum_manifest_v1.py` 与独立 verifier `verify_common_stratum_manifest_v1.py`，固定 5 scaffold folds × 2 seed slots 的 10 个 encoder/prior snapshot。
- 首次构建发现原 snapshot 只有 `q_prior/provider_prior/direct_prior`，没有 feature matrix；修正 manifest 假设，feature 来源改由 encoder checkpoint + inputs hash 绑定，未把不存在的数组写成证据。
- 当前 manifest：`experiments/logs/task49_common_stratum_manifest_20260930/v2/manifest.json`，SHA=`a9232a4003fc02f6a1d7fa7092aee63aa642b71c1ef33397149de72fa6cd2fe7`；独立 verifier `verified`，重算 checkpoint hash、direct prior hash、数组 roster、形状/有限性、fold 分区完整性。
- 固定语义：selected-only、delay `{0,1,4}`、release=`decision+1+delay`、runtime failure reward=0 且 chemical_correct=null、unknown truth 终止并 censor、一次 call/decision、一次 update/release、confirmation closed。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_manifest_v1.md`；合同已引用该 manifest。尚未运行 full receipt，Gate B 仍 HOLD。
- 下一步：用该 manifest 重新跑一条包含 failure/unknown 的最小 smoke，先验证语义和 event schema，再决定 60-stream full receipt；不要直接把无 failure/unknown 的 v3 smoke 当公平效果证据。

### 2026-09-30 — common-stratum 真实反馈语义冒烟通过

- 用真实缓存的 `z001303`（`b3clf_dtree_common` 运行失败）和 `z002970`（缓存返回正常但 reference unresolved）执行语义冒烟；没有读取 confirmation，也没有产生效果估计。
- failure：`score_status=scored`、`valid_return=0`、`chemical_correct=null`、`system_success=0`，delay=1 在 tick=2 释放 `reward=0/feedback_status=failure`，保留在分母。
- unresolved：`score_status=unscorable`、无 `tool_feedback`、无 numeric reward；`single_call_record` 明确拒绝并停止该 predeclared-cohort batch，未排队、未更新。
- 独立 verifier `verified`：`experiments/logs/task49_common_stratum_feedback_semantics_20260930/v2/verification.json`；报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_feedback_semantics_smoke_v1.md`。
- 相关单元测试 `75 passed`。这只关闭反馈语义风险，不解除 Gate B，也不证明任何方法优于 Frozen。
- 下一步：基于已封存 manifest 和已验证语义，生成包含完整 decision/terminal/feedback、state/update、调用与更新时间账本的 2-stream smoke；通过后再跑 60-stream full receipt。

### 2026-09-30 — common-stratum 两流协议冒烟通过

- 参数化 runner 后按 manifest 跑 fold0/seed0 与 fold1/seed0，各 8 个 evaluation 分子、7 个方法、delay=1；每流 168 条事件，未读取 confirmation。
- fold1 的 21 条 failure terminal 均是缓存中真实存在的 `b3clf_dtree_common` 失败（3 个分子 × 7 方法），独立 verifier 逐条重新绑定 selected output 到 cache；failure 没有被丢掉。
- aggregate verifier `verified`：`experiments/logs/task49_common_stratum_smoke_20260930/two_stream_verification_v1.json`；报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_two_stream_smoke_v1.md`。
- 该冒烟只关闭 common prior、delay=1、failure/event ledger 的工程风险，不提供方法效果或显著性结论；Gate B 仍 HOLD。
- 下一步：扩展为 5 folds × 2 seeds × 3 delays 的 60-stream full receipt，先冻结完整 ledger schema 和 paired uncertainty 计算，再提交唯一一条远程任务（如确需 A800）。

### 2026-09-30 — A800 指纹诊断最终日志恢复

- 只读查询确认 Nebula task `01a0de1cf322cd9ee1ead48f41569649` 已 `FINISHED`，worker 正常退出；没有重复提交。
- 从已下载的完整 worker stdout 重新运行恢复器，`recovered_final_20260930/summary.json` 状态 `verified`：config=1、split=5、epoch=8,000、fit=40、checkpoint manifest=40、checkpoint chunk=1,970、evaluation=5,776、complete=1。
- 恢复器重新核对 checkpoint hash、独立 NumPy 分数、选中 provider/output/status、Best Fixed/Oracle 和 validation-best epoch；没有新结果，结论仍是所有普通静态选择头低于 Best Fixed。
- 之前 `recovered_v4/v5` 的空目录不作为证据；报告已明确当前正式入口为 `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_final_20260930/`。原始 stdout 与 checkpoint 仍保留在本地 full-log 目录，没有把含环境信息的完整日志复制到聊天。
### 2026-09-30 — common-stratum full receipt v1 判为工程失败

- 首次全量回执实际完成 `150 streams / 64,980 events`，但合同复核发现 `greedy_online` 使用 `regularization=1.0`，而合同固定 `Σ0=0.02I`、noise variance=`1.0`，应为 precision=`50.0`。因此 v1 只作为失败证据保留，禁止解释结果；失败说明：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_full_receipt_v1/failed_receipt_v1.md`。
- 已修正 runner 为 `regularization=1/0.02=50`，同步合同与 preregistration，并明确 `direct_prior` 是 BCE/logit 权重，Gaussian 方法在没有 reward-scale prior 前只能列为 adapted/out-of-stratum comparator。
- 当前未关闭 Gate B：还需先强化独立 verifier 的 manifest/input/source-tree 绑定、精确 cohort/event roster、反馈时钟和重复 ID 检查，再用新 commit 跑 smoke_v3，最后跑唯一一条 full_v2；不能复用 v1/v2 的效果。
### 2026-09-30 — common-stratum full receipt v2 重跑并通过独立验收

- 修复 selected-only 边界：旧 structured release 接口会把 `chemical_correct/recovered_label/selected_output_label` 混进 feedback；`pharma_selection_stream.py` 现在在事件出口统一剥离 evaluator-only 字段。
- 新 smoke_v7：20 streams、480 events，独立 action/state/evaluator verifier `verified`；真实 failure 保留，路径：`experiments/logs/task49_common_stratum_full_receipt_20260930/smoke_v7/`。
- 新 full_v2：150 streams、64,980 events、21,660 decisions，真实 failure 149 条；独立验收 `verified`，路径：`experiments/logs/task49_common_stratum_full_receipt_20260930/full_v2/verification_v2.json`。报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_full_receipt_v2/report.md`。
- 描述性 pooled accuracy：Frozen=`0.901662`，Matched SGD=`0.892198`，Greedy=`0.887350`，Bayesian=`0.865420`，MIRA=`0.902816`。这些不是论文效果结论：Gaussian reward-scale 语义、探索与更新混杂、重复 molecule 统计和 provider-family macro 仍未关闭。
- 下一步：建立 reward-scale prior 或统一 logistic posterior；加入 no-update exploration-matched controls；做 molecule/scaffold paired、failure/conditional correctness/family macro 分解。Gate B/C 仍 HOLD。
### 2026-09-30 — direct prior scale diagnostic

- 新增 `src/probe_headless/audit_common_prior_scale_v1.py`，只使用 fit split 上的 model-implied probability 做固定 ridge=`1.0` 投影，不读取 evaluation truth。
- 结果：evaluation 中原 BCE/logit score 有 `0.824` 落在 `[0,1]` 外；ridge reward prior 降到 `0.146`，但 sigmoid 后与原概率 RMSE=`0.150`，仍不能视为已校准的 Gaussian reward prior。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/common_prior_scale_diagnostic_v1.md`；原始 JSON：`experiments/logs/task49_common_prior_scale_diagnostic_20260930/v1.json`。
- 决定：full_v2 的 Greedy/Bayesian 继续标 adapted/out-of-stratum；下一步优先做统一 logistic posterior 与 no-update exploration controls 的小流，Gate B/C 不变。
### 2026-09-30 — exploration-matched controls 全量回执

- 新增 `run_common_stratum_exploration_control_v1.py`，固定同一 prior、covariance、seed、cohort 和 delay，加入 `greedy_no_update` 与 `bayesian_no_update`。
- 全量 60 streams、25,992 events、4,332 decisions，独立 verifier `verified`：`experiments/logs/task49_common_stratum_exploration_controls_20260930/full_v1/verification.json`。
- pooled accuracy：Greedy no-update=`0.901662`，Bayesian no-update=`0.883887`；对应 online=`0.887350/0.865420`。这提示当前更新没有带来 pooled 提升，但仍需 molecule/scaffold paired 分解，不能当显著性结论。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_exploration_controls_v1.md`。Gate B/C 继续 HOLD，严格 baseline 仍需解决 Gaussian reward-scale 语义。
### 2026-09-30 — molecule-paired common-stratum aggregation

- 新增 `analyze_common_stratum_paired_v1.py`，按 molecule 配对计算 online−Frozen 与 online−no-update，不把 method/seed/delay 重复 cell 当独立样本。
- 结果：Matched=`-0.00946`、Greedy=`-0.01430`、Bayesian=`-0.03623`、MIRA=`+0.00115`（平均 stream delta）；Greedy/Bayesian 相对 no-update 也分别为 `-0.01430/-0.01845`。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_paired_analysis_v1.md`；原始 JSON：`experiments/logs/task49_common_stratum_paired_20260930/v1.json`。这只修正统计单位，不构成显著性或最终方法效果结论。
### 2026-09-30 — scaffold-cluster paired bootstrap

- 新增 `bootstrap_common_stratum_v1.py`：molecule 内先平均 seed/delay，outer fold 内按 scaffold cluster bootstrap，5 folds 等权，固定 seed=`20260930`、10,000 draws。
- 结果：MIRA−Frozen `+0.00115 [−0.00373, 0.00806]`；Matched `−0.00946 [−0.02291, 0.00219]`；Greedy `−0.01430 [−0.02786, −0.00063]`；Bayesian `−0.03623 [−0.05090, −0.02133]`。Greedy/Bayesian 相对 no-update 区间也偏负。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_bootstrap_v1.md`；原始 JSON：`experiments/logs/task49_common_stratum_bootstrap_20260930/v1.json`。MIRA 区间跨 0，不能写成稳定提升；Gate B/C 继续 HOLD。

### 2026-09-30 — 按生效评判标准补齐 full_v2 的 B11/B12/E5 分解

- 新增只读分析器 `src/probe_headless/analyze_common_stratum_metrics_v1.py`，从已验证的 full_v2 raw events 计算 all-provider task success、valid-return conditional correctness、runtime-failure、provider-family macro/exposure、selection/update p50/p95 和 state bytes；没有重新训练或读取 confirmation。
- 结果：150 streams、64,980 events 的计数与 full_v2 summary 一致；family 映射使用 `task/task38-p01-provider-pool/external_qualification.json`，补回 `fpadmet` 和 `admetlab`，不再把它们记作 UNKNOWN。机器结果：`experiments/logs/task49_common_stratum_metrics_20260930/full_v1/metrics.json`；报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_metrics_v1/report.md`。
- MIRA task success=`0.902816`，Frozen=`0.901662`，差值=`+0.001154`；已有 scaffold-cluster bootstrap 区间跨 0。MIRA selection p95=`97.560µs`、update p95=`61.098µs`、state p95=`17,721 bytes`；Greedy/Bayesian state p95 约 `1.10MB`。这些成本必须进入主表，不能只报 accuracy。
- 评判结论：B11/B12/B13/E5 证据入口已补强，但 Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16` 仍 HOLD；confirmation 继续关闭。下一步唯一科学任务是 method-conditioned incumbent 的 transient/persistent dynamic event recovery，主指标按 feedback blind window、release 后 recovery、target exposure、switch latency 和 cost 拆分；不再继续堆无科学问题的 verifier 字段，也不直接申请新的 A800。

### 2026-10-01 — action-supported dynamic event v2 完成复核

- 先按 preregistration `257cecf0` 生成并独立核验 target manifest：120 entries，manifest self-hash=`06fade1ee298d68942665107f98064105f75904e9bd8c85b73b198f1e7c6f764`，verification=`verified`。
- 完整 CPU receipt `action_supported_v2_full_v2/run/` 已完成：240 streams、34,656 decisions、104,208 raw event rows。首次短命令的现场后来也写出了完整重复 receipt，之前“半截/被终止”的判断已更正；它保留在 `action_supported_v2_full_v1/` 但不作为结果来源，指标只读 `full_v2`。
- 原始 schema/evaluator verifier `240/240 verified`，relative action/state verifier `60/60 verified`。对 paired analyzer 增加事件前 provider/version/state/released-feedback/menu 一致性和 control event-tick `ok` 检查；`metrics_v2_checked.json` 通过，且与旧 posthoc 机器结果逐条相同。
- 结果：transient 的 Frozen/streak/relative release 后差值均为 0；persistent 中 relative 的 release 后差值为 `-0.98/-0.85/-0.89 pp`，target exposure 约 `1.4/1.3/1.3%`，比 streak-only 更好但仍低于 matched SGD。结论只记为 `dynamic mechanism diagnostic passed; paper effect claim not passed`。
- 发现并记录两个 provenance 缺口：原始 receipt 未显式写 `dynamic_scope` 且 run id 复用 `20260930_v1`。新增 `receipt_binding_posthoc.json` 保存交叉核对结果；后续 runner 强制 target verification receipt、snapshot binding、`dynamic_scope=feedback-adaptive` 和唯一 run id。当前工作树修改后，旧 receipt 的 schema verifier 不再用当前源码重跑，旧 commit 绑定的原始 `verified` 收据保留为权威证据。
- 评判结论：Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16` 不变，confirmation 关闭。下一步科学任务是 common Frozen-incumbent secondary / provider-family 分层和 baseline fairness contract；不要直接申请新的 A800。

### 2026-10-01 — target family 分层补齐

- 只读读取已通过事件前 identity 检查的 `metrics_v2_checked_v2.json`，按 `external_qualification.json` 的 target family 分层；不重跑事件、不读取 confirmation。
- 120 个唯一 target 坐标中 `b3clf=106`、`lgbm_bbb=10`、`admetlab=4`，没有 fpadmet/admet_ai/deeppurpose/graphb3；结果说明当前动态事件主要覆盖 b3clf incumbent。
- b3clf persistent release 后 delta：streak-only `-1.71/-1.26/-1.64 pp`，relative `-1.09/-0.95/-0.99 pp`，matched `-1.00/+0.47/-0.16 pp`（delay 0/1/4）；relative 只是在方法内略好于 streak，不能转成跨方法 superiority。
- 新增 analyzer `src/probe_headless/analyze_mira_action_supported_family_v1.py`、机器结果 `family_stratification_v1.json` 和报告 `action_supported_family_stratification_v1.md`。Gate C 仍 HOLD；common Frozen-incumbent secondary 仍未解决，下一步先封存支持集/共同动作协议再考虑新实验。

### 2026-10-01 — family-outage full receipt 与独立验收

- 将 family-outage 协议收敛为 selected-only、activation-gated 的 common Frozen-incumbent family stress；独立审查记录：`research/debate_dynamic_event_20261001/common_target_review.md`。
- 新增独立 cross-run verifier `src/probe_headless/verify_mira_family_outage_replay_v1.py`：不导入 runner，检查 manifest entry、event entity/action、prefix identity、natural failure 对照、persistent synthetic failure、exactly-once feedback clock 和 unexposed full semantic equality。
- 新增 receipt-only 主分析器 `src/probe_headless/analyze_mira_family_outage_v1.py`，输出 ITT、S common support、family exposure、leave-family latency、paired fold bootstrap 和运行成本；smoke 4 streams 与 full receipt 均通过 dry-run。
- smoke v6：4 streams，2 exposed + 2 unexposed，12 injected events，独立 verifier `verified`；机器收据：`experiments/logs/task49_mira_event_replay_cpu_20261001/family_outage_v1_smoke_v6/verification_v2.json`。
- full v1：240 streams、34,656 decisions、104,208 raw event rows；226 exposed、14 unexposed、3,388 injected target-family failures；独立 verifier `verified`：`experiments/logs/task49_mira_event_replay_cpu_20261001/family_outage_v1_full_v1/verification_v1.json`。
- full metrics：`S=23/30`。persistent S 上 relative−streak 的 release-after delta 为 `-0.22/-1.26/-0.22 pp`，paired bootstrap 区间跨 0；relative−matched 为 `-4.04/-5.18/-0.96 pp`，exposure 仍高于 matched。transient relative delta=0，unexposed 全流语义等价。
- 结论：协议和分析链通过，当前 relative-reliability 没有超过 matched-SGD；问题定位为 provider-local availability ledger 对 family outage 隔离太慢。Gate C 仍 HOLD，下一步候选是 selected-only family hazard ledger + short gate/cooldown/hysteresis；旧 receipt 保留，不能覆盖。
- 代码、审查和 smoke 证据已提交并推送：commit `20557e57`。full raw receipt 留在本地日志目录，后续先写修复候选 preregistration，再决定是否跑下一条 CPU 任务；不申请 A800/confirmation。
- 已写入下一轮修复候选：`family_guard_v1_preregistration.md` 与 `family_guard_v1_design_review.md`。候选把 runtime availability 从 provider-local 扩展为 selected-only family/group hazard channel，带 posterior、连续 failure gate 和 cooldown；尚未实现、尚未运行，不计入当前结果。
- 新增候选策略 `src/probe_headless/mira_family_guard.py`：冷启动 group factor 归一化为 1，selected runtime feedback 更新 group posterior/streak，连续 failure 触发有限 cooldown gate；5 个纯策略单测通过。还没有接入 full replay，状态 hash、candidate no-event receipt 和独立 manifest 仍待完成。

### 2026-10-01 — MIRA-FamilyGuard candidate full receipt

- 完成 candidate no-event control：30 streams、4,332 decisions；使用真实提交 `3d1b60714d908c89683a6cd5b90aff4deab64c39`，独立 no-event verifier `verified`。此前人工伪造 commit 的 `no_event_smoke_v1` 保留为 provenance engineering failure，不进指标。
- 从旧四方法 control 与 candidate control 封存 150-entry FamilyGuard target manifest；manifest verifier `verified`，旧四方法 common support=`23/30`，加入 candidate 后 common support=`18/30`。没有扩充或改写支持集。
- 新增独立 FamilyGuard event runner/verifier/analyzer，提交并推送：`daf81af35a3a49e22c15f7c568b8289117f234a9`、`6c0a6b6344ab3b4754fc94766253518786276994`、`c68fb15b6dc6b8abcf3adb112e9a09c6d2219e19`。修复过一次 state-hash canonicalization：第一版 smoke 被独立 verifier 拒绝；统一后 40-stream smoke `verified`。
- 完整 event receipt：5 methods × 5 folds × 2 seeds × 3 delays × 2 scenarios = 300 streams、43,320 decisions、130,260 raw rows；274 exposed、26 unexposed、3,605 injected failures；独立 replay verifier `300/300 verified`。原始目录：`experiments/logs/task49_mira_family_guard_cpu_20261001/event_full_v1/`。
- 预登记 S=`23/30` 上 persistent release-after：FamilyGuard 相对 current relative 改善 `+7.60/+12.24/+22.47 pp`（delay 0/1/4），paired bootstrap 区间均在 0 以上；family exposure 下降 `9.00/14.00/24.32 pp`；transient 三个 delay=`0 pp`。相对 relative 的 selection/update p95 比值=`1.42/1.69`，numeric state bytes 比值=`1.012`。结果摘要和完整指标：`event_full_v1/family_guard_v1_summary.json`、`metrics_v2.json`。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_v1_report.md`；scorecard 已追加 2026-10-01 证据。Gate C 总体仍 `10/16 HOLD`：这轮关闭了一个 family-level dynamic failure mode，但 matched/common-stratum 主实验合同、机制消融、统一 failure/unknown/cost 主表和 confirmation 仍未闭合。
- 下一步：把 FamilyGuard 接入 common-stratum fairness contract，先做包含 family guard / streak-only / relative / matched 的小型机制消融和 failure/unknown/cost 分解；沿用同一 no-event/control、同一 S 与独立 recovery，不申请 A800/confirmation。

### 2026-10-01 — FamilyGuard common-stratum full receipt

- 新增独立 runner/verifier：`run_common_stratum_family_guard_v1.py`、`verify_common_stratum_family_guard_v1.py`；提交 `ec9b462808adf62adad01c977ece882a7c4cbbf8`。
- 同一 common-stratum manifest 跑完 180 streams（6 methods × 5 folds × 2 seeds × 3 delays）、25,992 decisions、77,976 raw events；159 条真实 selected runtime failures 保留。独立 action/state/evaluator verifier `verified`。
- FamilyGuard pooled task success=`0.907664`，Frozen=`0.901662`，MIRA-relative=`0.902816`；failure rate=`0.002308`，低于 Frozen/relative 的 `0.007618`。scaffold-cluster paired bootstrap：FamilyGuard−Frozen `+0.5977 pp [-0.6204,+1.5988]`，FamilyGuard−relative `+0.4828 pp [-0.6210,+1.1644]`，区间均跨 0，不能写成稳定 superiority。
- 结果报告：`task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_common_stratum_report.md`；机器结果：`experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/{metrics_v1.json,paired_v1.json,bootstrap_v2.json}`。
- 下一步：把这轮 common-stratum receipt 纳入统一主表，并做 FamilyGuard/streak-only/relative 的机制消融与 failure/unknown/refusal 分母核对；不申请 A800/confirmation。

### 2026-10-01 — FamilyGuard 结果纳入验收主表

- 对照生效的 Benchmark/Baseline 评价标准 v005，更新 `task/task49-pharma-benchmark-baseline-freeze/development_master_table_v1/report.md`：把 common-stratum 主分母和受控 family-outage 压力明确分成两张表，禁止把 synthetic stress 的正向结果混入普通 task success 排名。
- 更新 `task/task49-pharma-benchmark-baseline-freeze/current_acceptance_audit_v2/report.md`：记录 E4/E5 已补强、Gate C 仍为 `10/16 HOLD` 的具体原因，以及当前可以/不能写的 claim 边界。
- 更新 `task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_v1_design_review.md`：保留执行前“尚未有实验结果”的历史判断，追加执行后复核，不改写预注册历史。
- 做了只读一致性审计，结果 `verified`：`experiments/logs/task49_family_guard_document_consistency_20261001/audit_v2.json`。它核对 common-stratum verification/metrics 的 180 streams、77,976 events，family-outage 的 300 streams，并核对 FamilyGuard−Frozen 与 FamilyGuard−MIRA-relative 的 paired bootstrap 原数值。
- 审计第一次尝试 `audit_v1.json` 因错误假设 event verifier 有 `n_decisions` 字段而失败；该失败保留在同一日志目录，v2 改为按 stream protocol 核对 43,320 decisions 后通过。
- 当前判断：FamilyGuard 的受控 family-level repeated-trial 修复已经有可复核的机制证据；普通主分母的 paired 区间仍跨 0，不能宣称稳定 superiority。Gate A/B/C 与 confirmation 状态不变。
- 下一步：把 FamilyGuard、streak-only、relative、matched 和 Frozen 的 failure/unknown/refusal 共同分母及机制消融整理进统一 E1–E5 表，再重新按 C1–C4 评分；没有新的科学问题前不申请 A800。

### 2026-10-01 — common-stratum 成本列补齐

- 复核 `experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/metrics_v1.json` 后发现报告原先只给 Frozen、MIRA-relative、FamilyGuard 的 p95 成本，Matched/Greedy/Bayesian 仍是空白。
- 已把 6 个方法的 selection/update/state p95 全部写入 `task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_common_stratum_report.md` 和 development master table：Greedy/Bayesian 的 state p95 约 `1,098,504 B`，不能只按 accuracy 比较；FamilyGuard 相对 MIRA-relative 的 common-stratum selection/update/state 比值为 `1.42x/1.91x/1.013x`。
- 这些仍是 Python/cache replay 的策略开销，不是线上 provider 金钱、token 或 wall-clock 成本；Gate C 继续 HOLD。

### 2026-10-01 — 固定 unknown / refusal 的下一条科学任务

- 新增 `task/task49-pharma-benchmark-baseline-freeze/unknown_refusal_gap_v1/report.md`，把 provider runtime failure、unknown truth 和 verifier refusal 三种事件拆开，记录当前收据实际覆盖范围和不能声称的内容。
- 结论：common-stratum 的 `unknown_or_unscorable=0` 只说明开发输入没有 unknown；`episode.abstain()` 是 provider 无返回，不等于 agent 主动拒答；当前 selector 也没有显式 `refuse` action。E5 的 refusal claim 仍未执行。
- 下一步先封存 unknown censoring control、`accept/escalate/refuse` action contract、同动作率 random/固定阈值对照和 utility cost，再做 CPU smoke；不在 utility 未固定前调阈值，不申请 A800/confirmation。

### 2026-10-01 — E5-Semantic-Closure semantic smoke

- 按 `unknown_refusal_semantic_smoke_v1/preregistration.md`，用真实 cache 的 scored `z000282`、runtime failure `z001303`、unresolved `z002970` 和低 confidence `admet_ai_1_4_0` refusal 做 CPU semantic smoke；不读取 confirmation，不估效果。
- 首次 runner 因错误读取不存在的 `Decision.feature_sha256` 中止，保留失败目录 `experiments/logs/task49_unknown_refusal_semantic_smoke_20261001/v1/`；修正后提交 `81cf9d08` 重跑 v2。
- v2 收据：4 条主 stream、20 条事件；scored/failure/refusal 两个 delay 各有 `N_attempt=3, N_scored=1, N_failure=1, N_refused=1, N_correct=1`，unknown stop-batch 两个 delay 各有 `N_unknown=1`，没有 feedback/后续 decision。独立 verifier `verification_v2` 通过，source-tree hash 和生命周期检查通过。
- 4 个重新绑定 event hash 的篡改用例全部 fail-closed：unknown→0、failure 携带 chemical correctness、提前 release、refusal→wrong；结果：`experiments/logs/task49_unknown_refusal_semantic_smoke_20261001/v2/mutation_tests_v2.json`。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/unknown_refusal_semantic_smoke_v1/report.md`。该 smoke 只关闭了 E5 的语义/恢复入口，不证明 refusal utility 或 selector superiority。下一步若继续，先做 common-stratum failure/unknown 只读聚合，再单独登记五折 fit-only verifier full table；不申请 A800/confirmation。

### 2026-10-01 — 验收状态同步

- `current_acceptance_audit_v2/report.md` 和 `research/AAMAS2027_current_evaluation_scorecard_20260930.md` 已同步 semantic smoke 结果：E5 的 accounting/recovery 入口通过，但 refusal efficacy、统一 utility、五折 verifier full table 和 confirmation 仍缺；Gate C 维持 `10/16 HOLD`。

### 2026-10-01 — common-stratum 分母与 Gate-C 覆盖矩阵

- 新增只读分析器 `src/probe_headless/analyze_common_stratum_semantic_denominators_v1.py`，要求每个 decision 恰好有一个 terminal 和 feedback，显式区分 scored、runtime failure、unknown 和 refusal；不导入 runner、不读取新 truth。
- 对已验证的 FamilyGuard common-stratum full receipt 完成审计：180 streams、77,976 events、25,992 decisions/terminals/feedbacks；六方法各 4,332 attempts，unknown/refused 均为 0，runtime failure 为 Frozen/Matched/Greedy/Bayesian/MIRA-relative/FamilyGuard=`33/35/23/25/33/10`。机器结果：`experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/semantic_denominators_v1.json`；报告：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_semantic_denominator_v1/report.md`。
- 新增 `src/probe_headless/build_gate_c_unified_matrix_v1.py`，把 common-stratum、controlled family-outage 和 unknown/refusal semantic smoke 的 E1--E6 覆盖状态合成机器矩阵；缺少的估计量保留 `MISSING`，不跨 scope 填数字。结果：`task/task49-pharma-benchmark-baseline-freeze/gate_c_unified_matrix_v1/matrix.json`，说明：同目录 `report.md`。
- 评判结论：普通 common-stratum E1--E5 仍为 `PARTIAL`，E6 `MISSING`；family-outage 仅受控 E3/E4 通过；semantic smoke 只关闭 E5 语义入口。Gate A/B/C 仍 `11/16`、`11/16`、`10/16 HOLD`，confirmation 继续关闭。
- 下一步：先闭合主 baseline 的统一 input/feature/warm-start/预算/成本合同，再登记 refusal utility full table；没有新科学问题前不申请效果型 A800。

### 2026-10-01 — prior-scale 诊断实现更正

- 复核 `src/probe_headless/audit_common_prior_scale_v1.py` 发现 v1 把 probability-scale ridge score 再过 sigmoid 后计算 RMSE，并把 Gaussian 线性均值越过 `[0,1]` 当硬非法条件；这会夸大“Gaussian prior 不成立”的结论。旧代码、日志和报告保留为工程失败，不覆盖。
- 新增更正脚本 `src/probe_headless/audit_common_prior_scale_v2.py`，同一 inputs/manifest、同一 encoder/direct prior、同一 ridge=`1.0`，只改正确的同量纲比较 `s=XW_r` 对 `p=sigmoid(XW_BCE)`；不读 evaluation truth、不重跑 selector。
- v2 结果（10 fold/seed 宏平均）：fit/validation/evaluation 的 reward-score 区间外比例 `0.1326/0.1798/0.1464`，对 model-implied probability 的 RMSE `0.1360/0.1450/0.1605`，MAE `0.1054/0.1157/0.1260`。报告与更正合同：`task/task49-pharma-benchmark-baseline-freeze/common_prior_scale_diagnostic_v2/`；原始机器结果：`experiments/logs/task49_common_prior_scale_diagnostic_20261001/v2.json`。
- 科学结论修正为“BCE/logit 与 Gaussian reward likelihood 的语义尚未对齐”，不能写成 Gaussian 线性均值越界即非法。Matched Online-SGD 已覆盖合法 logistic 强对照；旧 direct-logit→Gaussian Greedy/Bayesian receipt 仍是 adapted/out-of-stratum，不追溯改写。若需要 strict Gaussian 近邻，下一步只登记 fit-only reward-ridge 小流合同，且分开记录拟合正则和 posterior precision。

### 2026-10-01 — baseline role roster 收口

- 新增 `task/task49-pharma-benchmark-baseline-freeze/baseline_role_roster_v1/roster.json` 与 `report.md`，逐方法固定 `strict_main / adapted_separate / audit_only / candidate` 角色、唯一证据路径和比较权限。
- 当前主控制层为 Frozen direct + Matched Online-SGD；Random/Best Fixed/Oracle 单列为任务锚点；MIRA-relative/FamilyGuard 保持 candidate；Greedy/Bayesian/PILOT 保留为可运行的 adapted 现代对照；GraphRouter/Laya online 只作 audit-only；static MLP/BT+CLS 单列静态控制。
- 角色分层不通过排除 adapted baseline 提高 Gate B；Gate B 仍 `11/16 HOLD`。后续如补 strict Gaussian 近邻，先登记 fit-only reward-ridge 小流和独立 recovery，不能把旧 receipt 追溯改写。

### 2026-10-01 — reward-ridge Gaussian baseline semantic smoke

- 新增合同 `task/task49-pharma-benchmark-baseline-freeze/reward_ridge_gaussian_smoke_v1/preregistration.md`：fit-only binary reward ridge (`lambda_fit=1.0`) 与独立 Gaussian posterior precision `50.0`，同一 hidden-64/menu/selected-only 生命周期；只做接口核验，不估效果。
- 扩展 `run_greedy_unified_raw_v1.py` 支持显式 `lambda_prior` 和唯一 `run_id`；扩展独立 verifier 支持动态 precision，并先用旧 receipt 验证回归仍为 `60/60 verified`。
- 首次 smoke 收据 `experiments/logs/task49_reward_ridge_gaussian_smoke_20261001/v1/` 因复用旧 run id 被保留为 provenance engineering failure；修复后 v2 使用 commit `ea1b8430620ef1b1fe4ec4db33606f885a00b5aa`、唯一 run id，2 streams、20 decisions，独立 verifier `verified`。
- v2 证明 prior、`0.02I` covariance、UCB score、selected-only feedback、rank-one update 和 state hash 可独立重放；Frozen/Online 各 10 题为 8/10，仅为 smoke 输出，不能作效果结论。报告：`task/task49-pharma-benchmark-baseline-freeze/reward_ridge_gaussian_smoke_v1/report.md`。
- baseline roster 已加入 `greedy_reward_ridge`，状态为 `strict_gaussian_baseline_pending_full_receipt`；Gate B/C 不变。下一步按同一合同登记 60-stream full receipt，再决定是否纳入主 baseline 表。

### 2026-10-01 — reward-ridge Gaussian full receipt

- 按 preregistration `reward_ridge_gaussian_full_v1/preregistration.md` 完成 60 streams、8,664 decisions、3 delays 的 full development receipt；原始日志：`experiments/logs/task49_reward_ridge_gaussian_full_20261001/v1/`。
- 独立 verifier `verified`，声明 commit=`7a8fd127913c13f41cfff5e9e522ad1911613b5c`；receipt binding 已保存 config/events/verification/analysis hashes。
- reward-ridge Frozen task success=`0.856648`，Online=`0.855494`；Online−Frozen molecule-paired delta=`-0.1154 pp`，scaffold-cluster bootstrap 95% CI=`[-1.1242,+0.9707] pp`。failure rate=`0.006233/0.006464`，family macro=`0.893501/0.862370`，Frozen/Online selection/update p95=`163227/1459 ns`、`161083/26583 ns`。
- 结论：这是合法但没有稳定在线收益的 Gaussian comparator；不包装为 MIRA 结果，不与 strict logistic 或 adapted direct-logit Gaussian 混排。报告与机器副本：`task/task49-pharma-benchmark-baseline-freeze/reward_ridge_gaussian_full_v1/`。
- baseline roster 已将 `greedy_reward_ridge` 从 pending 更新为 `strict_gaussian_baseline`。Gate B/C 仍 HOLD；下一步接入统一 E1--E5 主表，先补全 baseline cost/budget 汇总，再决定是否需要新的 MIRA 效果实验。

### 2026-10-01 — reward-ridge receipt 接入主表

- 将 reward-ridge Gaussian full receipt 加入 `development_master_table_v1/report.md` 的独立 Gaussian stratum：Online=`0.855494`、Frozen=`0.856648`、paired delta=`-0.1154 pp`、CI=`[-1.1242,+0.9707] pp`，不与 strict logistic/adapted raw accuracy 混排。
- 扩展 `build_gate_c_unified_matrix_v1.py` 并重新生成 `gate_c_unified_matrix_v1/matrix.json`，新增 reward-ridge 行：E2/E4 为 `PARTIAL`、E5 为 `PASS`、E6 `MISSING`，缺少 refusal utility 和 confirmation 仍显式保留。
- 验收结论：Gaussian baseline 的语义、事件、成本和 recovery 证据已闭合一条独立分层；Gate B/C 不变，下一步是统一所有 baseline 的 cost/budget/family/failure 主表，而不是继续堆标准在线算法。

## 2026-10-01 baseline cost/budget fairness contract

- 按当前验收标准补了只读机器表：`task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_v1/table.json`，报告：同目录 `report.md`，生成器：`src/probe_headless/build_baseline_cost_budget_table_v1.py`。
- 表内 16 个角色统一记录 input/menu、feature、fit-only warm start、likelihood、selected-only feedback、provider-call budget、feedback count、selection/update p95、state bytes、failure/unknown/refusal 和比较权限；所有 receipt 共用输入 SHA `a73f1c…`，脚本验证 `row_count=13`、输入一致、缺失字段显式、禁止跨 stratum raw ranking。
- common-stratum 的 Frozen/Matched/MIRA-relative/FamilyGuard 成本账本已有实测值；reward-ridge、Greedy、Bayesian、PILOT 的 unified cost 单独列，未记录的 state bytes 保留 `MISSING`；Random/Best Fixed/Oracle/static router 的缺失 timing/budget 也不补造。
- 这一步关闭了成本字段分散和角色混排的审计缺口，补强 B11/B12/B13/E5；Gate A/B/C 仍 `11/16`、`11/16`、`10/16 HOLD`，confirmation 继续关闭。
- 下一步：等待 baseline cost audit review；若无新的硬问题，按表中的 missing fields 判断是否值得登记 refusal utility full table 或 MIRA 修复候选，不申请新的效果型 A800。

## 2026-10-01 fingerprint A800 full-log recovery recheck

- A800 task `01a0de1cf322cd9ee1ead48f41569649` 已确认 `SUCCESS`；worker 退出码 0，scheduler 的 137 是平台收尾，不是用户训练失败。没有重新提交。
- 按 nebula experiment pipeline 只读流程下载 all-logs，收据：`experiments/logs/task49_fingerprint_diagnostic_20260926/remote_v2/download_all_03_receipt.json`；worker stdout 复制与 SHA 保存在 `remote_v2/stdout_full_worker_20261001.log`。
- 新恢复目录 `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v7_full_20261001/` 状态 `verified`：config=1、split=5、epoch=8,000、checkpoint manifest=40、checkpoint chunk=1,970、fit_complete=40、evaluation=5,776、complete=1；40 个 checkpoint、全部 evaluation 和输入/源码绑定通过。
- post-hoc analyzer 输出 `analysis_v7_full_20261001.json/summary.json`，paired rows、源码 hash、输入 hash 和结果守恒通过。结果与既有摘要一致：Best Fixed=`0.915512`、Oracle=`0.997230`，MLP=`0.896122/0.890582`，BT+CLS=`0.887812/0.894737`，Linear=`0.872576/0.879501`，direct property=`0.759003/0.760388`。
- 该复核只提高证据完整性，不改变静态诊断的 go-signal=false，也不打开 confirmation 或申请新的效果型 A800。

- 独立 reviewer 的只读 fairness audit 已返回：`task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_fairness_audit_v1/`，其 Replay/GraphRouter/Laya 缺口复核已合并进 canonical `baseline_cost_budget_v1/table.json`；该目录作为 review，不产生第二套生效标准。

### 2026-10-01 — post-call refusal / utility audit v1

- 按 `unknown_refusal_gap_v1` 和生效标准 v005，先写并审查 `task/task49-pharma-benchmark-baseline-freeze/postcall_utility_audit_v1/analysis_plan.md` 与 `review.md`。本轮明确只补 E5 的 post-call verifier utility 子项，不接回 MIRA direct selection，不申请 A800/confirmation。
- 零机时分母审计通过：五折 evaluation 共 722 行；第一 provider fit/evaluation 全部 status=ok 且 confidence 数值存在；threshold 接受 573、主动拒答 149；第二 provider cached failure 6 次单列。机器收据：`experiments/logs/task49_postcall_utility_contract_audit_20261001/v1/denominator_audit.json`，状态 `verified`。
- 新增独立 development runner/recovery：`src/probe_headless/replay_pharma_postcall_utility_audit_v1.py`、`src/probe_headless/recover_pharma_postcall_utility_audit_v1.py`。policy receipt 与 evaluator-only outcome 分文件；拒答 receipt 不含 truth、`answer_correct`、`tool_feedback` 或 `grade`，恢复器检查 read-set 和逐行一致性。
- CPU full replay v3/v4 均完成：6 policies × 722 = 4,332 policy rows 与 4,332 evaluator rows；两次 policy/evaluator receipt `cmp` 一致；独立 recovery v3/v4 均 `verified`。没有新 provider/API 调用，confirmation closed。
- 结果：always-accept wrong=61；confidence refusal wrong=22/refusal=149；confidence escalation wrong=52/final failure=3/calls=871；matched random refusal wrong=50/refusal=149；matched random escalation wrong=60/final failure=1/calls=868。拒答 break-even rho*=0.2617；追加调用 break-even kappa*=0.0403。
- 5,000 次 scaffold-cluster paired bootstrap（seed=20261001）：固定拒答审计点差 `+0.00293 [-0.01456,+0.02218]`，追加审计点差 `+0.00622 [-0.01149,+0.02466]`，两者区间均跨 0；不能写稳定 utility superiority。provider latency/update/memory 仍缺。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/postcall_utility_audit_v1/report.md`；原始收据：`experiments/logs/task49_postcall_utility_audit_20261001/v3/`；恢复：`experiments/logs/task49_postcall_utility_audit_20261001/recovered_v3.json`。
- 扩展 `build_gate_c_unified_matrix_v1.py`，机器矩阵新增 `postcall_utility_audit_v1` 行，E5=`PARTIAL`、recovery=`verified`、E6=`MISSING`；同步 `current_acceptance_audit_v2/report.md`、`development_master_table_v1/report.md` 和 scorecard。Gate A/B/C 保持 `11/16`、`11/16`、`10/16 HOLD`。
- 下一步：把 utility receipt 与 common-stratum failure/unknown/family 结果整理进统一 E5 主表；补齐全方法 latency/update/memory 和 strict fairness 缺口，再决定是否还有新的可观测信号值得做动态 verifier。没有新科学问题前不提交 Nebula A800，confirmation 继续关闭。

### 2026-10-01 — unified baseline verifier P0 repair v2

- 独立审查复现旧 Matched/BayesianRouter verifier 的 fail-open：伪造 menu、decision read-set、terminal reward、stream summary、事件顺序和 config roster 仍可通过。旧 verifier/receipt 保留在历史目录，不再引用。
- 新增 `src/probe_headless/verify_unified_raw_contract_v2.py` 及两个 CLI wrapper；现在绑定 canonical input SHA=`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`、PredictionCache 全部源文件 hash、声明 git tree，并逐事件重放 exact schema、菜单/read-set、terminal outcome、delayed feedback、state transition 和 root summary。
- Matched 最终收据：`experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/`，60 streams、8,664 decisions/terminals/feedback，v2 `verified`。关键 mutation 全部拒绝：`experiments/logs/task49_baseline_verifier_p0_20261001/mutations_v2/results.json`；最终 `oracle_label` mutation：同目录 `mutation_final_oracle_label/result.json`。
- Bayesian raw full receipt 中途有两次不完整运行，均保留为 aborted；完整 raw 目录 `pre_cache_manifest_v1` 经过可审计 provenance/summary 补全生成 `provenance_repaired_v1`，只补 cache hashes、methods 和对应 hashes，inputs/snapshots/events/action 未改。当前 v2 独立 verifier 通过 60 streams、8,664 triples：`.../provenance_repaired_v1/verification_v2.json`。
- 报告：`task/task49-pharma-benchmark-baseline-freeze/baseline_verifier_p0_repair_v2/report.md`；验收报告与 scorecard 已同步。Gate A/B/C 仍 `11/16`、`11/16`、`10/16 HOLD`；这一步只修证据合同，不改变效果 claim，不开启 A800/confirmation。
- 下一步：把这两份 strict raw receipt 接回 baseline readiness/cost table，明确 common-direct-prior Bayesian 与 native-prior baseline 的分层；不做新的在线算法实验。

### 2026-10-01 — baseline receipt pointers cleaned

- Matched 历史报告已明确标为 historical；baseline role roster 的 authoritative evidence 已切到 `baseline_verifier_p0_repair_v2/report.md`，避免后续误读旧 fail-open receipt。
- 当前唯一可引用的 strict raw evidence：Matched `full_v6`；Bayesian `provenance_repaired_v1`。两者都不改变 Gate A/B/C 分数或科学 claim。

- 最终检查：两个 `verification_v2.json` 均为 `verified`、60 streams、8,664 decisions/feedback；没有残留 Bayesian 进程。Gate B 表述已同步为“Matched/Bayesian raw contract 已修复，但全 baseline strict prior/成本/confirmation 仍 HOLD”。

### 2026-10-01 — 三份标准复核：baseline 成本表证据指针修复

- 对照生效的故事线、方法论、Benchmark+Baseline 三份评价标准，发现一个具体但不改变科学结果的审计断点：成本账本仍只指向 common metrics/历史目录，未直接绑定本轮 v2 strict raw receipt。
- 修改 `src/probe_headless/build_baseline_cost_budget_table_v1.py` 并重建 `task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_v1/table.json`，版本升为 `baseline_cost_budget_contract_v2`。Frozen/Matched 行追加 `baseline_verifier_p0_repair_v2/report.md` 与 Matched `full_v6/verification_v2.json`；Bayesian native-BT 行保留 native-prior 收据，并明确 repaired direct-prior receipt 仅作 raw-contract audit。
- 生成器机器校验：`row_count=16`、所有行 input SHA 一致、缺失字段显式、禁止跨 stratum raw ranking；没有读取 confirmation、没有重跑策略、没有修改 outcome 或比较权限。
- 同步更新生效 Benchmark 主文档、当前验收审计和 AAMAS scorecard。三门状态不变：Benchmark+Baseline=`41/60 HOLD`，Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16 HOLD`。
- 下一步：继续按三份标准检查 unified E1–E5 主表和 baseline fairness 的剩余硬缺口；优先做文档/机器一致性核验，不开新 A800。

### 2026-10-01 — 三份标准复核：纠正 development master 的过期 Matched 数字

- 对照最新通过 `verify_unified_raw_contract_v2.py` 的 Matched `full_v6` 逐 stream 汇总，发现 `development_master_table_v1/report.md` 仍保留旧 receipt 的 `0.9024/0.8975/0.8975` 和 `+0.78 pp`。
- 当前严格 receipt 的真实结果为 Matched `delay={0,1,4}`=`0.891274/0.888504/0.896814`，pooled=`0.892198`；Frozen=`0.901662`，Online−Frozen=`-0.946 pp`。旧数字没有删除，已在主表标为历史，不再作为当前 claim。
- 更新了 development master 的主表、恢复入口、统一结论，以及 current acceptance audit 的可写事实。没有重跑策略、没有读取 confirmation、没有改变任何实验结果。
- 这是 Benchmark+Baseline 标准要求的结果一致性修复；Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16 HOLD` 不变。下一步继续检查 E1–E5 主表是否还有历史 receipt 与当前 strict receipt 混用。
- 同步修正生效方法论文档对 Matched 的描述：旧 prior-specific 结果与当前 strict common receipt 分开，避免把历史小幅改善写成当前结论。
- 进一步清理成本表证据语义：direct-prior Bayesian 收据从 native-BT 行的主 `evidence` 移到独立 `audit_evidence` 字段；生成器重跑后 16 行、统一 SHA、MISSING 和禁止跨 stratum ranking 校验仍通过。
- fairness audit 追加后续指针校正：它原先的“尚无统一成本表”保留为历史状态，当前唯一机器入口改为 `baseline_cost_budget_contract_v2`；缺失字段、Replay strict manifest、state/refusal/unknown 和 confirmation 缺口不变，Gate B/C 不上调。

### 2026-10-02 — 三份标准复核：Replay scope、prior lineage、family registry

- 发现并修正 Replay-SGD 成本账本的维度错误：旧生成器把 90 条 source streams 的 Frozen/Online/Replay 事件一起算 p95，却将该行分母写成 Replay 的 4,332 decisions。生成器 `src/probe_headless/build_baseline_cost_budget_table_v1.py` 现按 `stream_suffix=replay_sgd_matched` 过滤 30 条 Replay streams；当前 Replay-only selection/update p95=`50,713.9/149,029.45 ns`，机器表 `budget.receipt_scope` 显式记录 `30/90`。旧 pooled 数字保留在历史现场，不再用于验收。
- 发现并补齐 Matched prior lineage：当前 strict Online-SGD 使用 `common-direct-prior`，历史 Matched/Replay 使用旧 BT `score.weight`；独立审查测得 provider prior 最大绝对差=`0.2243737`、相对 L2 约=`0.9938`。development master、方法论和 acceptance/scorecard 已明确：当前 Online `-0.946 pp` 与历史 Replay `+0.50 pp` 不能组成 paired superiority；同源 Replay 必须另行登记，当前不申请任务。
- 发现 FamilyGuard `config.json` 声明 external qualification registry，但旧 `metrics_v1.json` 实际使用旧 `data/bbb_census_v3/tool_registry.json`，导致 `fpadmet`/`admetlab_3_0` 被归成 `UNKNOWN`。在同一封存 events 上只读重聚合 `metrics_v2_external_registry.json`，并保存 `metrics_v2_external_registry_audit.json`（source/config/registry/analyzer hash、`scientific_inputs_unchanged=true`、`policy_execution=false`）。当前 family macro 已改为 v2 口径；task success、failure、cost、confirmation 均未改变。
- 更新了 `baseline_cost_budget_v1/report.md`、`baseline_cost_budget_fairness_audit_v1/report.md`、`development_master_table_v1/report.md`、`mira_event_replay_v1/family_guard_common_stratum_report.md`、`gate_c_unified_matrix_v1/{matrix.json,report.md}`、`current_acceptance_audit_v2/report.md`、`research/AAMAS2027_current_evaluation_scorecard_20260930.md`，以及生效方法论/Benchmark 文档。Gate C C2 文案改为“成本账本存在，但 baseline fidelity/likelihood/warm-start/state/refusal/unknown 合同不统一”。
- 机器校验：baseline table `row_count=16`、input SHA 全一致、MISSING 显式、禁止跨 stratum raw ranking；Gate C 矩阵引用 v2 metrics；生成器和 Gate builder `py_compile` 通过。三份标准评分不变：Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`。
- 下一步：继续对照三份验收标准检查 unified E1–E5 主表与 baseline receipt 是否还有历史/当前口径混用；不申请新 A800，不打开 confirmation。

### 2026-10-02 — 回到效果问题：FamilyGuard 相对 Best Fixed 的逐样本分解

- 新增只读 analyzer `src/probe_headless/analyze_mira_vs_best_fixed_v1.py`，严格复用 `run_structured_cpu_effect_v1._fit_best_provider`，按 fold fit-only `system_success` argmax 计算 incumbent；输入为封存 `events.jsonl`、fingerprint `inputs.json` 和 external qualification registry；不重跑策略、不读 confirmation。
- 机器结果 `experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/mira_vs_best_fixed_v1.json`：4,332 cells；FamilyGuard=`3,932/4,332=0.907664`，Best Fixed=`3,966/4,332=0.915512`，净差 `-34=-0.7849 pp`；both-correct=3,744、both-wrong=178、rescued=188、harmed=222；switch-away=3,619/4,332=83.54%。
- 损失分解：MIRA 400 个错误由 wrong-label=390、runtime failure=10 构成；harmed 的 222 个中 wrong-label=212、runtime failure=10，全部来自离开 incumbent 后的选择。替代工具仍救回 188 个固定赢家错误，所以不能简单改成永久固定。
- 误伤 family：b3clf=167（157 wrong-label、10 runtime），lgbm_bbb=52，graphb3=2，fpadmet=1；rescue family：b3clf=120、lgbm_bbb=63、graphb3=5。molecule-level scaffold bootstrap（5,000，seed=20261002）净差 CI=`[-2.7932,+1.5659] pp`，跨 0。
- 新报告 `task/task49-pharma-benchmark-baseline-freeze/mira_vs_best_fixed_v1/report.md`，并同步 development master、current acceptance、AAMAS scorecard、方法论。下一候选的问题明确为 incumbent-safe switching：correctness 与 runtime hazard 分开、posterior margin/最小证据量/cooldown；否定条件为不能降低 harmed 且保持 rescued。Gate A/B/C 与 Benchmark+Baseline 分数不变。
- 同步加强 Benchmark+Baseline v005 的 R3 结果合理性检查：动态 selector 除 pooled accuracy 外必须报告 harmed/rescued/switch-away，不能隐藏 incumbent 误切换。

### 2026-10-02 — 形成 SafeSwitch 候选，先理论后实验

- 根据 harmed/rescued 分解，新增候选方法卡 `task/task49-pharma-benchmark-baseline-freeze/mira_safe_switch_v1/proposal.md`：在原 MIRA-FamilyGuard 之上增加 provider-level incumbent safety gate，用 correctness Beta posterior、runtime hazard 上界、最小证据量和切换 margin；所有计数只由 released selected feedback 更新，单次更新 O(1)，选择 O(|A|)。
- 新增先于实验的 `mira_safe_switch_v1/preregistration.md`，固定 rho=.5、margin=.05、n_min=20、两类置信度=.05、hazard slack=.02、fit-only Beta pseudo-count=20；主 estimand 是 harmed/rescued/switch-away/runtime failure 及成本，不只看 pooled accuracy。CPU 小流先做接口和信息泄漏检查，尚未执行。
- 方法主文档已链接候选卡。下一步先审公式和事件实现，再按预登记跑小流；不直接申请 A800，不打开 confirmation。

### 2026-10-02 — SafeSwitch CPU 收据与独立复核：候选未通过，转向上下文安全门

- 发现并修复了候选预登记里的冷启动死锁：selected-only 且无探索时，`n_min` 不能只数在线已选反馈；资格改为 `N_fit + C+ + C-`，fit 先验的等效样本量改为当前 fold 的有效 fit 返回数，避免把 400 多条 fit 证据压成 20 个伪样本。
- 新增 `src/probe_headless/mira_incumbent_safe_switch.py`、`run_mira_safe_switch_smoke_v1.py`、`verify_mira_safe_switch_smoke_v1.py` 及 4 条单元测试。`pharma_selection_stream.py` 只增加可选 `decision_log_payload` 字段，不改变旧回放行为。真实缓存 v2/v3/v4/v5 均保留；v2/v5 独立 replay verifier 分别通过 64/290 个 decision-terminal-feedback 三元组。
- v2（双侧 LCB/UCB，固定 pseudo-count）在 32 题为 27/32 且 0 次切换；v3（fold0 全量 145）0 次切换；v4（fit-count prior、双侧门）仍 0 次切换。v5（fit-count prior、单侧 LCB、无额外 gamma）第一次相对上一动作切换后，后续 143 题保持 challenger，结果 126/145；fit-only Best Fixed 为 130/145，逐格 both-correct=117、both-wrong=6、rescued=9、harmed=13。不能写成安全切换收益。
- v5 暴露两个方法问题：provider 边际 Beta 账本不能代表当前分子上的 context risk；将 `current_incumbent` 每次改成新 provider 造成路径依赖。`safe_switched`（相对上一动作）也不能代替标准要求的相对 fit incumbent `switch-away`（本收据为 144/145）。报告：`task/task49-pharma-benchmark-baseline-freeze/mira_safe_switch_v1/report.md`。
- 方法主文档已改为：SafeSwitch v1 是负对照和问题定位；下一候选必须使用 context-conditioned posterior predictive、显式 fit-only fallback/reference、分离 correctness/runtime ledger，再决定是否 full receipt。没有申请新 A800，没有读取 confirmation；Gate A/B/C 和 Benchmark+Baseline 分数不变。
- 下一步：先完成独立数学/代码审查，修正 fit `valid/failure/total termination` 符号和 runtime Beta 参数语义；随后只做一个 context-conditioned incumbent shield 的 CPU 小流，不通过就停止该构造，不靠继续调 gamma 追分。

### 2026-10-02 ContextShield 停止与 ContextProbe contract smoke

- ContextShield 已完成当前候选的实现修订：mean-Fisher、Bernoulli local-MAP/Laplace、logit-space gate、immutable fit-only fallback、独立 runtime hazard；单元测试 4/4，独立 verifier 重放历史 v4 receipt 的 192 个事件通过。静态停止测试在 fold0/seed0 的 145 个 evaluation 分子上预登记 gate-open=`0/145`，候选停止，不申请 A800。
- 新候选 ContextProbe 已实现 `src/probe_headless/mira_context_probe.py`、runner、独立 verifier 和 6 条测试。第一次静态 schedule 诊断发现同一 seed 在每条 stream 重置会让大量 provider 永远没有 probe；这是协议问题，未拿它跑效果。
- 修复为 `SeedSequence([base_seed,fold,seed,delay])` 派生 stream seed；同一 stream 的 Frozen/Probe 共享 schedule，不同 stream 不复用随机片段。全 5 fold×2 seed×3 delay、4,332 个 decision 的 label-free coverage 诊断显示 31 个非 fallback provider 均至少被 probe 一次，收据：`experiments/logs/task49_mira_context_probe_schedule_20261002/v2_all_streams.json`。
- ContextProbe v3 CPU real-cache smoke（fold0/seed0/delay0/limit32）包含 192 个事件、9 次 probe；独立 verifier `verified`，tamper probe-index 测试拒绝伪造，pytest 共 10 条 ContextShield/ContextProbe 测试通过。该 receipt 只证明 PRNG、selected-only delay、posterior/hazard/state hash 可重放，不是效果结果。
- 文档已更新：`task/task49-pharma-benchmark-baseline-freeze/mira_context_probe_v1/{design.md,preregistration.md}`、生效方法论文档、current acceptance audit 和 AAMAS scorecard。验收分数仍 Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`，confirmation 关闭。
- 下一步：先把当前代码和候选材料提交；再运行预登记的 CPU full common-stratum 对照矩阵（probe/no-probe/Frozen/Best Fixed/Matched/FamilyGuard），重点看 probe/shield 分层、coverage、release 后 action/state change、harmed/rescued 和成本；没有新的科学问题前不提交 Nebula。

### 2026-10-02 ContextProbe v4 current-commit smoke

- 使用当前 commit `95039de2d514c91ea8759054d8d7e9bdb2a0ea45` 重跑 ContextProbe CPU contract：fold0/seed0、delay `{0,1,4}`、每条 32 题，6 streams、576 events。Probe 次数为 `9/9/8`；Frozen/Probe 共享每条 stream 的派生 seed。
- 独立 verifier 通过：`experiments/logs/task49_mira_context_probe_smoke_20261002/v4_current/verification.json`。它不导入候选 policy，重算 PRNG、菜单、propensity、shield action、局部 posterior/hazard、delay、state hash；篡改 probe_index 的测试拒绝通过。
- 小流 task success 仅作 contract 现场：Frozen `28/32`；Probe delay0/1/4 为 `28/32、30/32、27/32`。未做 probe/shield 的正式效果分解，不写 superiority。
- 候选报告已写入 `task/task49-pharma-benchmark-baseline-freeze/mira_context_probe_v1/report.md`。下一步仍是预登记 CPU full common-stratum 对照矩阵，先分析机制和成本，不申请 A800。

### 2026-10-02 ContextProbe full receipt mechanism result

- 以 commit `95039de2d514c91ea8759054d8d7e9bdb2a0ea45` 完成 5 folds × 2 seeds × 3 delays 的 ContextProbe candidate full receipt：60 streams、25,992 events、原始 JSONL 约 1.1 GB。独立 verifier 通过：`experiments/logs/task49_mira_context_probe_full_20261002/v1/verification.json`。
- 新增流式机制分析 `src/probe_headless/analyze_mira_context_probe_full_v1.py`，结果收据 `mechanism_analysis_v2.json`。ContextProbe 4,332 task 正确 `3,905/4,332=0.901431`，Frozen direct `3,906/4,332=0.901662`；ContextProbe runtime failure=6，Frozen=33。
- ContextProbe 有 895 次 forced probe、3,437 次 shield action；probe 正确率 `757/895=0.845810`，shield 正确率 `3,148/3,437=0.915915`。相对 fit-only fallback，harmed=99、rescued=38；895 次 switch-away 全部来自 forced probe，shield 分支没有额外 provider switch。selected feedback 更新 `4,332/4,332`，但未转化为 exploitation action。
- 结论：ContextProbe v1 停止。它证明 coverage、延迟反馈和实时状态可重放，但没有解决“探索结果进入非 probe 决策”的核心问题；不申请 A800、不打开 confirmation、不把 forced probe 动作变化写成动态收益。报告和验收同步：`task/task49-pharma-benchmark-baseline-freeze/mira_context_probe_v1/report.md`、current acceptance audit、AAMAS scorecard。
- 下一步：等待独立 methods audit；然后先提出新的可识别 gate/目标并做 CPU 静态 stop test。没有新方法合同前，不重复 full receipt。

### 2026-10-02 ContextProbe 独立审查与 MIRA-Relay 候选

- 独立审查核对 `mechanism_analysis_v2.json` 与原始 events：ContextProbe 30 streams、895 probes、3,437 shield，正确 `3,905/4,332`，harmed/rescued=`99/38`；所有离开 fallback 的动作来自 forced probe。32/32 非 fallback provider 是跨 stream union coverage，每条 stream 仍有未覆盖 provider，不能把 union 当作每臂实时可学习证据。
- 审查修正了一个归因边界：旧 receipt 的 decision payload 没有保存 `shield_feasible_challengers`、score 和可行集合，因此不能断言 gate 在每个 decision 都关闭，只能确认 shield branch 没有非 fallback action change。旧 `mechanism_analysis.json` 已由 v2 supersede，后续只引用 v2。
- 发现 ContextProbe probe 绕过 hard/family/runtime block，且 provider-uniform probe 使 b3clf 占约 75%；新候选改为 safe family-uniform→provider-uniform，记录动态 propensity，并按 runtime_success、chemical accuracy|valid_return、system_success 分层。
- 新增 `task/task49-pharma-benchmark-baseline-freeze/mira_context_probe_v1/independent_audit.md`；新增候选 `mira_relay_v1/{proposal.md,preregistration.md,review.md}`。MIRA-Relay 用已有 family registry 的 family-shared residual + provider bias，冻结 direct head 作为 fallback，selected challenger feedback 用对角 Laplace 实时更新。
- 还未实现 MIRA-Relay；先做 state→action sensitivity 单测、fit-only switchability、NoProbe/ProbeNoUpdate/ProbeMIRA-Relay 同 schedule CPU 矩阵和独立 verifier。没有通过这些 stop gate 前，不申请 A800；当前验收分数保持 Benchmark+Baseline=`41/60 HOLD`、Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`。

### 2026-10-02 MIRA-Relay kernel 与论文预写启动

- 新增纯 policy kernel `src/probe_headless/mira_relay.py` 与 `src/probe_headless/tests/test_mira_relay.py`。实现冻结 direct head + family-shared residual + provider bias、selected-only delayed lifecycle、runtime failure `y=0` residual update、safe menu/probe 注入、完整 branch/full propensity、可恢复 state snapshot 和 immutable fallback；fit valid/failure 计数分开。
- 单测 `6 passed`，`py_compile` 与 `git diff --check` 通过。kernel 不包含 fit L-BFGS/runner/verifier，未跑 real-cache、未申请 A800；下一步先补 fit-only residual 生成与独立 replay verifier。
- 独立审查已写回 `mira_relay_v1/{proposal,preregistration,review}.md`：修正 total-logit offset、diagonal-precision projection、common-random-number 语义、`ProbeIndependentResidual` 对照和 `+0.5 pp`/cluster-CI 科学门；UCB 不称化学安全保证。
- 已启动专门 writer agent 撰写 `paper/aamas2027/` 的 AAMAS pre-results draft；实验矩阵和 TBD 结果表先写入论文，未运行结果不填数字。论文草稿必须通过模板编译和 submission guard 后再 review。

### 2026-10-02 — AAMAS 论文内部 pre-results 草稿落地

- 启动专门论文 writer agent，按 paper-research-pipeline、paper-writer、academic-writing 和三份验收标准写作；保留已确认题目 `Learning to Select Scientific Tools from Outcome Feedback`。
- 新增并提交：`paper/aamas2027/main.tex`、`references.bib`、`INTERNAL_PRE_RESULTS_REVIEW.md`、`WRITER_REPORT.md`；同步更新 `README.md`、`abstract.tex`。主稿包括六段式 Introduction、任务/信息边界、MIRA-Relay 候选公式、benchmark/baseline 合同、RQ1–RQ4、E1–E6 实验矩阵、TBD 结果表、误差/成本、局限和 related work。
- 所有最终实验单元仍为 `TBD`；当前开发诊断数字只作为范围受限的边界证据，明确不能当作 MIRA-Relay superiority 结果。`make submission` 继续 fail-closed。
- 修正方法与账本的语义：`runtime_success`、`chemical_correct` 和端到端 `system_success` 分开定义；有效但错误的返回是 `system_success=0`，runtime failure 的 chemical label 保持 null。同步更新 `mira_relay_v1/{proposal,preregistration}.md`。
- 补齐官方 AAMAS CCS Concepts；Makefile 增加官方模板的 `BSTINPUTS/BIBINPUTS`，手动 BibTeX 验证通过（仅有缺 publisher/page 元数据警告）。隔离构建曾成功生成 7 页、标题正确的 PDF；本地完整 `make draft` 后续轮次受前台工具 60 秒限制终止，不能据此声称 clean build，已保留成功 PDF 和日志。
- 提交并推送 commit `1d399782`（`draft AAMAS paper and preregister MIRA Relay`）。
- 当前三份验收标准仍为 Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`；论文草稿不改变这些科学状态。
- 下一步：先完成 MIRA-Relay CPU state→action sensitivity、fit-only switchability、NoProbe/ProbeNoUpdate/ProbeIndependentResidual/ProbeMIRA-Relay 同 schedule receipt 和独立 verifier，再决定是否申请 Nebula；随后回填论文矩阵，禁止用未完成 receipt 填数字。

### 2026-10-02 — AAMAS 格式核验与图表版式预留

- 用官方 AAMAS 2027 Instructions、Main Track Call、Reviewer Guidelines 和最新 Q&A 复核格式：主文上限是 8 页，参考文献另计；附录计入 8 页；补充材料可选且单 zip 不超过 25 MB；主轨英文、LaTeX、双盲。链接和逐项检查写入 `paper/aamas2027/AAMAS_2027_FORMAT_AUDIT.md`。
- 阅读本地 `topconf-paper-figure-gallery` 的 README、方法说明、3516 张图的元数据和代表性 framework/pipeline 示例。把适合本论文的左到右数据流、有界 panel、反馈回路、颜色与线型冗余、图表页级预算写入 `paper/aamas2027/FIGURE_LAYOUT_AUDIT.md`。
- `paper/aamas2027/main.tex` 已预留 Figure 1 方法总览（跨双栏）和 Figure 2 延迟反馈生命周期（单栏）的位置，两个占位图都有 caption、label 和 `\\Description{}`。当前仍是内部占位，不是最终投稿图。
- 清理并发构建进程后，在 `paper/aamas2027/build/verify_current_20261002/` 顺序完成 pdflatex → BibTeX → pdflatex → pdflatex；`pdfinfo` 显示 6 页，标题正确，`pdftotext` 未发现 Temporary page 或 undefined citation/reference。有效 PDF 已复制到 `paper/aamas2027/build/main.pdf`。
- 6 页低于官方 8 页上限，不能据此强行扩写到 7 页；待真实结果表和正式矢量图替换占位后重新检查分页。当前仍有非致命的长表格/公式 overfull、模板 CCS warning、balance warning 和少量 BibTeX 元数据 warning；不改变字号、页边距或删除方法信息来规避。
- 验收状态未改变：Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`；论文所有最终实验格仍是 `TBD`，不能声称已达到投稿门禁。
- 下一步：继续 MIRA-Relay 的 CPU 证据链和 matched baseline 合同；之后先生成可编辑正式图，再统一处理表格 overfull 与最终 clean build。不要申请新的 Nebula 任务来填排版空位。

### 2026-10-02 — 核心论文图绘制、评审和整篇嵌入

- 按 `topconf-paper-figure-gallery` 的 framework/pipeline 规律确定最小核心图集：Figure 1 方法总览、Figure 2 selected-only 延迟反馈生命周期。暂不增加结果图或实验协议图，等真实 E1–E6 收据后再决定。
- Figure 1 协作者完成 v1/v2，均保留源文件、HTML/SVG、PNG/PDF、自然语言结构说明和 REVIEW；v2 通过跨栏目标尺寸检查，选定 `paper/aamas2027/figures/figure1/v2/figure1_v2.pdf`。
- Figure 2 经过 v1→v5 迭代：v2 因文字框重叠淘汰，v3/v4 修正信息边界和 lane label，v5 在 1020px 单栏预览中标签完整可读，选定 `paper/aamas2027/figures/figure2/v5/figure2_v5.pdf`。所有中间版本保留。
- 新增 `paper/aamas2027/figures/FIGURE_SET_PLAN.md`、`FIGURE_REVIEW.md`、`SELECTED_VERSIONS.md`；评审标准覆盖一句话信息、主线结构、信息边界、方法映射、目标尺寸、视觉克制、黑白可读性和可编辑性。
- `main.tex` 已替换两个占位框，嵌入 Figure 1 v2 和 Figure 2 v5，补充正式 caption、label 和 `\\Description{}`；Figure 2 caption 做过一次压缩以避免挤压正文。
- 独立目录顺序完成 pdflatex → BibTeX → pdflatex → pdflatex：`paper/aamas2027/build/verify_figures_v2_20261002/main.pdf`。结果：标题正确、7 页、801,396 bytes；`pdftotext` 未发现 Temporary page 或最终 undefined citation/reference，PDF 已复制到 `paper/aamas2027/build/main.pdf`。
- 当前 7 页由真实图表自然产生，低于官方 8 页上限；没有改模板字号、页边距或用无关文字凑页。仍有既有 overfull、CCS class warning、balance 和少量 BibTeX 元数据 warning，均非致命。
- 验收科学状态不变：Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`；结果表仍为 `TBD`，不能把图已完成误写成论文已完成。
- 下一步：继续 MIRA-Relay CPU 证据链和 matched baseline；真实结果出来后再画结果图，并再次做整篇分页与图表可读性检查。

### 2026-10-03 — AAMAS 版式收口与 MIRA-Relay 收敛门

- 论文公式把 `z^*`/`z_0` 以及均值/精度更新拆成独立行；预审框、label 段、RQ、表格 caption/列宽和 limitations 做了最小断行调整。Figure 1 v4、Figure 2 v8 保持选定，独立复核各 15/16，不把图评成满分。
- 隔离目录 `paper/aamas2027/build/verify_figures_v7_20261003/` 完成 pdflatex → BibTeX → pdflatex → pdflatex；最终 7 页、标题 `After the Decision: Learning to Adapt from Delayed Outcomes`、CCS metadata 存在。最终 `main.log` 没有 overfull、undefined citation/reference 或 temporary page；窄表格仍有 underfull 提示，模板条件警告已记录。`paper/aamas2027/build/main.pdf` 已同步，SHA-256 `0b639699238f8b5a4fd706385a5e5f6b62531d8f54e76dbc8e185068883b6069`。
- MIRA-Relay 队列失败状态映射已修复；fit/queue/diagnostic/online-kernel 单测共 `42 passed`，日志在 `experiments/logs/task49_mira_relay_semantics_20261003/v1/`。
- fold0/seed0 的 MIRA fit diagnostic 仍保留失败：目标 `5363.2964 → 3444.2793`，100 次 L-BFGS 达到上限，梯度无穷范数 `12.5075`，状态 `fit_not_converged`。116/145 非 fallback 和 state→action fixture 不能作为效果证据；没有 full replay、confirmation 或 A800 结果。
- 已启动 CPU-only 数值诊断，比较同一目标的优化器/收敛实现；下一步必须先判断是迭代预算还是目标/梯度实现问题，保留原失败 receipt，不静默改变预登记合同。收敛前不进入 full replay、不申请新 Nebula 任务。

### 2026-10-03 — MIRA fit optimizer 数值诊断完成

- 在相同 fold0/seed0 fit objective、direct offset、family registry 和数据 hash 下，比较了合同 L-BFGS-100、上限 L-BFGS-1000 与解析 Hessian `trust-exact`。收据：`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/optimizer_v1/`；报告：`task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/optimizer_diagnostic_report.md`。
- 结果：L-BFGS-100 `3444.2793 / grad 12.5075 / fail`；L-BFGS-1000 `3340.2848 / grad 0.3738 / fail`；trust-exact `3340.1063 / grad 2.33e-9`，SciPy status=2 但梯度数值达到停止阈值。481×481 Hessian 对称误差 `2.27e-13`，最小/最大特征值 `0.1723/31389.47`，有限差分梯度误差 ≤`2.07e-9`。
- 当前判断：100 次 L-BFGS 未收敛主要由目标条件数约 `1.82e5` 与预算造成，未发现目标/梯度/Hessian 实现不一致。该诊断不改变预登记、不构成方法效果，也不能直接把 trust-exact 叫作已成功的主优化器。
- 已启动 fold0–4 的同目标可行性诊断，先判断解析 Hessian 方法是否跨折稳定；完成前不改 prereg、不申请 GPU、不进入 full replay。

### 2026-10-03 — MIRA fit 解析 Hessian 跨折稳定性

- fold0–4 / seed0 的同目标 CPU 诊断完成，收据 `experiments/logs/task49_mira_relay_fit_diagnostic_20261003/optimizer_crossfold_v2/`（v1 保留，v2 修正外层状态字段），报告 `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/optimizer_crossfold_report.md`。
- L-BFGS-1000 五折都达到上限，grad∞=`0.207–0.450`；trust-exact 五折目标均更低，grad∞=`2.3e-7` 或更低，耗时约 `3.8–4.2s/折`，L-BFGS 约 `18–20.6s/折`。前四折 SciPy status=2，fold4 success；status 原样保留。
- 结论仍是数值预算/条件数问题，不是已证明的实现错误；trust-exact 只能作为候选 fit 实现和独立 verifier 参考，尚未替换 preregistration。full replay、confirmation 和 A800 仍关闭。
- 下一步：整理 `trust-exact/Newton` 候选的停止条件、status=2 接受规则、参数 hash 和独立 verifier；形成可审查的候选合同后，再决定是否更新 preregistration 并进入同 schedule CPU replay。
- 已写候选合同 `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/optimizer_candidate_trust_exact.md`：数学目标保持不变，trust-exact 仅作为数值求解器候选；要求独立 objective/gradient/Hessian/hash verifier，status=2 不得改写为 success，双重确认前不进入主矩阵。

### 2026-10-03 — trust-exact candidate fit 独立验证

- 新增候选生成器 `src/probe_headless/run_mira_relay_trust_exact_candidate_v1.py` 和独立 verifier `src/probe_headless/verify_mira_relay_trust_exact_candidate_v1.py`。候选只保存 fit 参数向量，不运行 replay。
- fold0/seed0 收据 `experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_v1/`：trust-exact `status=0/success=true`，目标 `3340.106321841232`，grad∞ `2.3267e-09`；独立重算 objective、gradient、参数 hash、Hessian 对称误差 `1.1369e-13` 全部一致。
- 参数包篡改测试被 verifier 拒绝（hash mismatch），失败日志 `trust_candidate_tamper_test.log` 保留。该候选仍未生效，不更新 prereg、不进入 online replay。

### 2026-10-03 — trust-exact candidate 跨折独立验证完成

- 5 folds × seed0 的 candidate 全部保存 NPZ、receipt、summary 和 verification；收据 `experiments/logs/task49_mira_relay_fit_diagnostic_20261003/trust_candidate_crossfold_v1/`，报告 `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/trust_exact_candidate_crossfold_report.md`。
- 5/5 verified：fold0/1/3/4 是 status0 accepted；fold2 是 status2，但满足预先写明的有限性、Hessian 对称误差和 grad∞≤1e−6，标为 `numerically_stationary_status2`，未改写成 success。测试共 17 passed；参数包篡改仍 fail closed。
- 这只封存了 fit 数值候选；未改生效 preregistration、未进入 online replay、未申请 GPU。下一步是候选合同的双重确认与 no-effect contract replay，不能把 fit 收敛写成选择收益。

### 2026-10-03 — MIRA-Relay contract replay runner 已就绪（未执行）

- 新增 `src/probe_headless/run_mira_relay_contract_replay_v1.py`、对应 5 条单元测试和事件 schema：固定 fold0/seed0、evaluation 前 32 条、delay 0/1/4，共 9 streams。
- 三个对照固定为 `probe_mira_relay`、`probe_no_update` 和 `no_probe_shield`；前者 epsilon=.2 并更新 residual，后两者保留 runtime ledger 但冻结 residual，最后一个 epsilon=0。
- 每条 decision/terminal/feedback 都记录菜单、eligible、score、propensity、版本、state snapshot/hash；`action_changes_after_feedback` 只统计 release 后下一次 gate action 的变化。
- runner 严格校验已验证 candidate theta、little-endian float64、archive/hash、direct prior；不重新拟合，使用 fit-only Hessian 对角线重算 diagonal precision，假设记录在 schema/config 中。
- 测试 `5 passed`，与已有 MIRA fit/queue/diagnostic 合计 `19 passed`；没有启动真实回放，等待候选合同落定和 root verifier。

### 2026-10-03 — MIRA-Relay contract replay v2 完成并独立核验

- 按提交 `280f09bafe5c5b6d551fb187f420dac8a975e53a` 重跑并封存 `experiments/logs/task49_mira_relay_contract_replay_20261003/v2/`；v1 保留，不混用。
- 真实缓存回放固定 fold0/seed0、evaluation 前32条、3 methods × 3 delays，共 9 streams / 864 events；输入、candidate archive、verification、fit initialization 副本均保存。
- `verify_mira_relay_contract_replay_v1.py` 独立通过：9/9 streams，各 32 decision/terminal/feedback，pending=0，due/release 时钟、provider 绑定、selected-only 读取集合和全状态 hash 链均通过。
- 观察到的 `action_changes_after_feedback`：ProbeMIRA-Relay `24/23/22`，ProbeNoUpdate `24/23/22`，NoProbeShield `22/21/20`（delay 0/1/4）。这只是状态路径证据，不是效果或优越性结果。
- 当前仍有一个明确边界：theta-only candidate 通过 fit-only Hessian 对角线重算 precision；尚未形成独立封存的完整 fit-state serializer/verifier，因此不进入 full CPU effect matrix、confirmation 或 Nebula。
- 下一步：完成 v2 fit-state pack（theta、precision/variance、valid/failure counts、provider/family order、state hash）及独立 verifier；随后重新审查是否允许 trust-exact 进入生效 preregistration。

### 2026-10-03 — fit-state v2 封存并接入 contract replay

- 新增 `seal_mira_relay_fit_state_v2.py` / `verify_mira_relay_fit_state_v2.py` 与单测；状态包显式保存 theta、direct prior、family/provider residual、Hessian 对角 precision/variance、provider-family order、fit runtime counts 和 canonical state hash。
- 修正了两个数据语义问题：冻结输入的 runtime failure 原值是 `error`，在回放边界归一为 `failure`；valid-but-wrong 仍计为 runtime success，不能从 `system_success=0` 推回 failure。unknown/refusal 在该 fit-state 合同中拒绝。
- 最终状态包 `experiments/logs/task49_mira_relay_fit_state_20261003/v7/` 独立 verifier 通过：objective `3340.106321841232`、gradient∞ `2.3267e-9`、33 providers/7 families；candidate artifacts 已随包冻结。v5/v6 保留为旧版本，v2/v3 空目录/旧版本不作最终证据。
- 对应 contract replay v4 使用 sealed fit-state v7，固定 9 streams / 864 events，独立 contract verifier 通过；结果仍只证明生命周期和状态路径，不是方法效果或优越性。
- 五折 status2 边界已单独审查：fold2 的 raw `scipy_status=2`、`success=false` 但数值梯度/对称性门通过；当前 fit-state v2 仍保持 status0-only，不把它混入 fold0 的 sealed state。边界说明见 `mira_relay_v1/status2_fit_state_boundary.md`。
- 下一步：先把 v4/v7 receipt 与方法合同写入实验报告并复核 prereg 变更边界，再考虑 full CPU matched matrix；不直接申请 Nebula 或读取 confirmation。

### 2026-10-03 — status2 fit-state 只读边界审查

- 审查 trust-exact candidate fold0–4 的原始 receipt 与独立 verifier：5/5 verified；fold0/1/3/4 为 status0 accepted，fold2 保留 `numerically_stationary_status2`、SciPy status=2、`scipy_success=false`，但 `grad∞=2.2856494087675205e−7` 和 Hessian 对称误差 `2.2737367544323206e−13` 满足预先约定的数值门。
- 写入候选边界说明 `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/status2_fit_state_boundary.md`。当前 fit-state v2 维持 status0-only；若未来需要封装 fold2，必须显式 `allow_status2`/独立 contract，receipt 保留原始 status/success 与数值条件，且 status0/status2 不得无标注混用。
- 本次只读审查未改代码或 preregistration，未启动 replay/GPU，也未产生效果结论。

### 2026-10-03 — same-query mechanism audit corrected and stationarity certificate completed

- The first same-query audit was explicitly superseded: it compared original decision-time scores, used a mutable fallback hazard reference, and included unavailable-arm sentinels in a score norm. The correction is retained at `experiments/logs/task49_mira_relay_same_query_20261003/v1/CORRECTION.json`.
- Recomputed the estimand with the same molecule feature/menu and `feedback.state_before` versus `feedback.state_after` at the exact release. Fit-only fallback hazard is immutable; unavailable scores are excluded. Source-bound authoritative receipt: `experiments/logs/task49_mira_relay_same_query_20261003/v3/`; report: `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/same_query_sensitivity_v2.md`.
- Receipt has 288 records (3 methods × 3 delays × 32 queries). ProbeMIRA-Relay has 22 residual updates and exactly 1 same-query exploit action change per delay; ProbeNoUpdate and NoProbeShield have zero. The changed query is `z002750`; `fpadmet` reward was 0 and the post-release exploit action became `lgbm_bbb`. Delay replicas are the same sequence and are not independent. This is mechanism evidence only, not an accuracy or superiority result. The earlier adjacent-decision counts remain invalid for causal interpretation.
- Read-only stationarity certificate completed for all 5 trust-exact fold candidates: `experiments/logs/task49_mira_relay_stationarity_20261003/v1/`; report: `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/stationarity_certificate.md`. The fixed regularized logistic objective is 0.01-strongly convex. Float64 gradient-derived objective-gap estimates are ≤5.06e-11; fold2 retains raw SciPy status2 and remains blocked by the active status0-only fit-state contract. No preregistration, replay, confirmation, or benchmark score changed.
- Relevant tests: `10 passed, 1 skipped` (stationarity, fit-state, contract replay). No new Nebula task was submitted.
- Next: implement and preregister the matched independent-residual update control and query-level rescued/harmed metrics; keep full CPU effect matrix and confirmation closed until the contract, status policy, and fold selection are independently reviewed.

### 2026-10-03 — contract verifier binding strengthened

- Extended `verify_mira_relay_contract_replay_v1.py` to bind feedback provider to the selected provider, scored reward to terminal chemical correctness, and reject labels/rewards on unknown/refusal or inconsistent failure feedback.
- Rechecked the sealed v4 replay without rerunning it: `experiments/logs/task49_mira_relay_contract_replay_20261003/v4/verification_v2.json` passes 9 streams / 864 events. The replay report now points to this verifier and explicitly marks the old adjacent-action counts as noncausal.
- This is a receipt-integrity improvement only; it does not create an effectiveness result or change the preregistration.

### 2026-10-03 — matched provider-independent residual contract replay

- Added `MIRAIndependentResidualPolicy` and `ProbeIndependentResidual`: same direct prior, fit bias, probe schedule, hazard/family shield, fallback, delayed queue and selected-only lifecycle; family residual/variance is copied only at fit cold start, then each selected provider updates its own residual.
- Added design/contract document `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/independent_residual_v1.md`, including claim, comparator, falsification conditions, schema and no-effect boundary. The policy and runner were committed in `3734824c` and pushed.
- Ran the small CPU contract receipt at `experiments/logs/task49_mira_independent_residual_contract_20261003/v1/`: 3 delays × 32 evaluation rows = 288 events. Initial first-query scores/actions/menu/availability match the MIRA receipt exactly (max score/mean/variance difference 0). Each delay has 22 selected provider-local updates and 10 nonupdates; no unselected provider residual row changed.
- Added `verify_mira_independent_residual_contract_replay_v1.py`; `verification_v2.json` passes all 288 events, selected-only timing, provider/reward binding, state hash chain, and provider-local update invariant. This is contract/mechanism evidence only; no accuracy or superiority result.
- Relevant tests: independent policy 7 passed; prior MIRA/fit-state/contract tests remain passing. No Nebula task submitted.
- Next: compare MIRA-Relay, ProbeIndependentResidual, and NoUpdate on the same full CPU development schedule with paired rescued/harmed metrics and unseen-sibling transfer. Do not treat the 32-row receipt as a performance result.

### 2026-10-03 — independent receipt provenance correction

- The first independent contract receipt (`.../v1/`) used a placeholder 40-character `declared_commit`; its `CORRECTION.json` marks it superseded. No scientific interpretation uses v1.
- Re-ran with the actual executable commit `3734824c0d4d593b3080ffdc314ffcf2a8f6bd8f`: authoritative receipt `experiments/logs/task49_mira_independent_residual_contract_20261003/v2/`. Independent verifier v2 passes 3 delays / 288 events and the same provider-local invariants. No method or data changes.

### 2026-10-04 — matched CPU effect matrix negative gate and provenance cleanup

- Ran the matched development matrix with three methods (family-shared MIRA, provider-independent residual, ProbeNoUpdate), delays 0/1/4, fold0/seed0 first 32 evaluation molecules: 9 streams and 864 events.
- Authoritative receipt is `experiments/logs/task49_mira_matched_effect_matrix_20261003/v4/`; `declared_commit == local_head == git rev-parse HEAD` is recorded. `verification_v4.json` passes lifecycle, reward/chemical-label binding, provider-local update invariant, state chain, and paired recomputation. v1–v3 remain locally as superseded provenance attempts.
- All methods score 28/32 selected correctness rewards and 32/32 runtime-valid returns at each delay. MIRA vs Independent and MIRA vs NoUpdate both have 0 rescued / 0 harmed / net delta 0. MIRA executes 22 residual updates with 2 family-transfer-capable events; Independent executes 22 provider-local updates; NoUpdate changes no residual. The only provider action difference versus NoUpdate has unchanged reward. This is a negative development gate, not a performance claim.
- Report/prereg: `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/matched_effect_matrix_v1_report.md`, `matched_effect_matrix_v1_preregistration.md`, and provenance addendum. A raw manifest was written; the large `events.jsonl` is retained locally and its hash is recorded in the manifest/summary.
- Relevant regression tests remain `24 passed, 1 skipped`; no Nebula job submitted.
- Next: register a new larger CPU development matrix (64 or all 145 fold-0 evaluation rows) before execution, preserving the 32-row gate. Measure sibling transfer and paired rescued/harmed again; do not use the negative gate to weaken the target claim or to justify a remote run prematurely.

### 2026-10-04 — v3 五折开发闸门预登记与 fit-state 封存（尚未跑回放）

- 复核 v2 的 145-row receipt：fold0/seed0、delay0/1 各相对两个匹配对照净 +1 条，delay4 持平；这只是单折开发信号，不能写成方法效果。v2 raw `events.jsonl` 为 439,335,963 bytes，事件 hash 为 `d2cc3ec129f115433a60e13e5f372442867261745afed9969b249b0918a7fbf0`。
- 补强 v2 独立 verifier，commit `97cdff7c`：summary 的 config/events hash、stream roster/count、逐方法标量、paired rescued/harmed/net 都必须由 raw JSONL 重算一致；篡改 summary 的 `n_correct` 会 fail closed。v2 receipt 的运行时声明仍是其真实运行 commit `6d1ed7f9`，不能与 verifier 后续 commit 混写。
- 新增并预登记 v3：`task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/matched_effect_matrix_v3_preregistration.md` 与设计审查 `matched_effect_matrix_v3_design_review.md`。目标是五个 scaffold-disjoint outer folds、seed0、三 matched methods、delay `{0,1,4}`，合计 6,498 decisions / 19,494 raw events；先判断 v2 小信号是否跨折重复，再决定 seed1/A800。delay4 保留为压力结果，不能删。
- 发现并更正文档中的计数/存储估计：45 streams、6,498 decisions、19,494 events，按 v2 状态快照约 2.2 GB；原先 97,470/10.9 GB 是把每折行数重复乘了五次，已不再使用。
- 新增显式跨折 fit-state v3：`src/probe_headless/mira_relay_crossfold_state_v3.py`、合同 `crossfold_state_v3_contract.md` 和 6 个单测。fold0/1/3/4 为 status0-only；fold2 只有显式 `status0_or_numerically_stationary_status2` 才允许，原始 `status2/success=false` 保留。五个 state package 已封存并逐折独立验证；fold2 gradient∞ `2.2856494087675205e-7`、Hessian symmetry `2.2737367544323206e-13`。
- 新增 v3 回放实现/独立 verifier：`run_mira_matched_effect_matrix_v3.py`、`verify_mira_matched_effect_matrix_v3.py`。事件额外记录 released feedback IDs、release ordinal、pending queue before/after；2 个 tracker 单测通过，源码 py_compile 通过。尚未运行 v3 replay，尚未提交 Nebula/A800，尚未产生跨折效果结论。
- 当前继续门：先做一次小规模 receipt 完整性检查，再运行五折同合同 CPU matrix；只有五折 raw/verifier/paired uncertainty 闭合后才考虑 seed1 或远程任务。若多数折无稳定方向，回到表示/共享结构诊断，不通过扩大算力追分。

### 2026-10-04 — v3 fold0 首次启动提前中止并修正回放成本

- 按 v3 预登记启动 fold0/seed0；命令和启动记录保存在 `experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold0_seed0_launch.json`。进程在第一条 replay event 前持续约 7 分钟，只有 `inputs.json` 与 `precompute_config.json`，没有 decision/terminal/feedback，也没有效果结果；失败收据：`v3_fold0_seed0_failure.json`。
- 原因是 replay runner 调用了严格的 `load()`，重复重建 fit problem、encoder 和 Hessian；这属于执行成本问题，不是方法结果。没有用这次启动推断任何性能。
- 已在同一份 v3 合同中加入 `load_sealed()`：seal/verify 阶段完成一次独立 objective/gradient/Hessian 核验，replay 阶段只复核 manifest、源码/候选/数组/status/canonical state hash，不在每条 stream 重复拟合。合同与预登记已更新；下一次先做快速 smoke，再重启 fold0。
- 五个 state package 的 source hash 已做 loader-only provenance refresh（旧 hash、旧 seal commit 与 `42d81f1b` 均保留在各 `config.json` 的 `provenance_patch`）；fit_state archive/canonical state hash 未变化，不是重拟合或新效果结果。

### 2026-10-04 — v3 fold0/1 receipts完成

- fold0/seed0 v3 raw receipt：9 streams / 3,915 events，`verification.json=verified`；delay0/1 相对 Independent 各 rescued/harmed=`1/0`，相对 NoUpdate=`2/1`；delay4 相对 Independent=`0/0`、相对 NoUpdate=`2/2`。
- fold1/seed0 v3 raw receipt：9 streams / 3,915 events，独立 verifier 通过；三个 delay 都相对 Independent=`1/0`、相对 NoUpdate=`1/0`，MIRA 128/145、两个对照 127/145。两折结果只作 development fold-level 输入，尚未计算跨折区间，也不支持论文 claim。
- fold0 report、fold1 report、summary/verification/raw manifest 已分别保存；原始 `events.jsonl` 保留本地但不提交，hash 在 manifest 中。
- 下一步：按同合同跑 fold2（显式 status2 policy），再跑 fold3/4；任一 verifier/fairness/schema failure 都停止合并，不挑选正折。

### 2026-10-04 — v3 matched-effect verifier hardening (subtask)

- 只读审查并修正 `src/probe_headless/verify_mira_matched_effect_matrix_v3.py`：消除 stream `rows` 变量遮蔽；config/precompute 的 input/candidate/fit/source hash 与 code identity 绑定；读取 receipt 自带 `inputs.json`，按冻结 fold evaluation 索引核验 `matrix.rows` 顺序与 provider menu；每条 selected terminal/feedback 从冻结 provider cell 独立重算 `valid_return`、`chemical_correct`、`system_success`、reward、feedback_status，并核验 selected output。
- 新增 `src/probe_headless/tests/test_verify_mira_matched_effect_matrix_v3_verifier.py`，4 个单测通过；py_compile/diff-check 通过。未启动回放、未提交 git。
- 使用新 verifier 对现有 `experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold0_seed0_r2/` 做只读核验：9 streams / 3915 events 通过。MIRA vs Independent d0/d1 paired net +1（1 rescued/0 harmed），d4 持平；MIRA vs NoUpdate d0/d1 net +1（2 rescued/1 harmed），d4 持平；145/145 runtime-valid。临时完整输出 `/tmp/v3_fold0_r2_verification_new.json`。
- 下一步：root 将该 verifier 结果合并进 v3 fold0 receipt，并决定是否继续其余 folds；本次不改变 preregistration 或效果 claim。

### 2026-10-04 — v3 fold2 receipt 完成并通过加强 verifier

- 按同一 v3 合同完成 fold2/seed0：144 evaluation rows、3 methods × 3 delays，9 streams / 3,888 events；原始 events、config、输入副本、fit-state、summary、verification 和 raw manifest 均保留在 `experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold2_seed0/`。
- 加强后的独立 verifier `verification_v2.json` 通过：冻结 fold 顺序、provider cell、terminal/feedback reward、selected-only 边界、pending/release 时钟、状态 hash chain 和 paired recomputation 全部通过。fold2 原始 SciPy status=2/success=false 仍保留，只按预登记的 numerically stationary status2 policy 运行。
- 结果：MIRA 130/144，Independent 130/144，NoUpdate 129/144；delay0/1/4 均相同。MIRA vs Independent 均 0 rescued / 0 harmed；MIRA vs NoUpdate 均 2 rescued / 1 harmed。MIRA family-transfer 为 16/16/15，Independent provider-local update 为 98/99/102；全部返回 runtime-valid。
- 这仍是单折 development receipt，不构成跨折效果或论文 claim。报告 `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/matched_effect_matrix_v3_fold2_report.md` 已写明边界。下一步跑 fold3，再跑 fold4；完成五折后按 fold 计算 paired uncertainty，不能按事件池化或挑选正折。
- 后续补充 selected-cell 类型边界：ok 必须二值 label，非 ok 必须无 label；v3 fold0 r2 重新只读 verifier 仍 PASS，输出 `/tmp/v3_fold0_r2_verification_new2.json`。

### 2026-10-04 — v3 fold3 receipt 完成，首次参数失败已隔离

- fold3/seed0 第一次启动因误传短 commit 前缀在 runner 参数校验阶段退出，0 条 replay event；失败收据 `experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold3_seed0_failure.json` 保留，未进入科学分析。修正为完整 SHA 后重新运行。
- fold3 正式 receipt：144 evaluation rows、3 methods × 3 delays，9 streams / 3,888 events；`verification_v2.json` 通过冻结输入/provider cell、selected-only、queue/release、状态 hash 和 paired recomputation；raw manifest 已生成。
- 结果：MIRA 131/144，Independent 131/144，NoUpdate 129/144；三个 delay 相同。MIRA vs Independent 均 0 rescued / 0 harmed；MIRA vs NoUpdate 均 4 rescued / 2 harmed。MIRA family-transfer 为 17/17/17，Independent provider-local update 为 80/79/79；全部返回 runtime-valid。
- 这仍是单折 development receipt，不能单独支持论文 claim。报告 `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/matched_effect_matrix_v3_fold3_report.md` 已封存。下一步只剩 fold4；之后按五折计算 paired fold-level uncertainty 和预登记 continue gate。

### 2026-10-04 — 将顶会论文叙事文章固化为写作评审基准

- 用户确认：将《Story Is All You Need：扒透35篇顶会最佳/杰出论文的叙事密码，我们找到了六大范式》作为本项目后续论文写作的标准参考；Codex 后续同时承担写作打磨者和 reviewer。
- 完成：把用户提供的文章正文规范化保存为 `research/2026_AI顶会最佳杰出论文叙事范式_六大范式_阅读整理.md`，包含来源、阅读边界、六大范式、35 篇索引、可迁移启示和待核验项。
- 完成：新增生效决议 `docs/决议/0008-论文叙事范式作为写作评审基准.md`，并更新 `docs/决议/README.md` 索引。
- 关键共识：写作审查先检查任务—失败现象—根因—机制—证据—反证条件的因果链，再润色表达；六类叙事范式只作检查框架，不硬套模板。
- 事实边界：文章中的会议、奖项、论文名称、指标和数字尚未独立核验，不能直接当作本项目事实或论文证据。
- 未完成：尚未把六类范式逐项映射到当前 AAMAS 论文草稿，也尚未开始新一轮故事线审查。
- 下一步：以 ADR-0008 为准，审查当前论文的主张、最近邻差异、根因定位、机制必要性、实验因果链和证伪条件。
- verifier 另补 method×delay stream 唯一性与每 stream tick=0..limit-1 完整顺序约束；v3 fold0 r2 重新只读通过，联合测试6 passed。

### 2026-10-04 — 阅读 AAMAS 论文 build/main

- 完整阅读 `paper/aamas2027/build/main.pdf`，并对照源文件 `paper/aamas2027/main.tex` 的摘要、引言、问题合同、MIRA-Relay 方法、基准与 baseline、实验矩阵、预结果表、局限性、相关工作和结论。
- 当前论文主线：在具体科学工具菜单中，agent 必须在看不到未选输出的情况下先选 provider；正确性反馈只对 selected call 生效且可能延迟到达；MIRA-Relay 以不可变 fit-only fallback、审计过的 family residual sharing、selected-only delayed update、result-independent probes 和 runtime hazard ledger 组成候选适应机制。
- 当前稿件明确是 pre-results design draft，经验 cells 仍为 TBD；已有开发诊断只作设计边界证据，不冒充最终效果。
- 初步叙事判断：论文最强主线是“新基准/事件合同暴露失效”与“根因手术刀”的组合，方法贡献是对信息边界和反馈时钟的显式重构；“极简统一美学”目前次于前两者，不能让 MIRA-Relay 的模块列表取代核心因果链。
- 初步审稿关注：引言需要更早、更尖锐地把现有静态 router / unconstrained online update 的共同失败根因压缩成一个可证伪命题；MIRA 的 family sharing、probe、hazard gate 要分别绑定到各自不可替代的失败模式；预结果与设计承诺的边界需要保持清楚。
- 未完成：尚未对全文逐段做正式 reviewer 标注，也尚未提出具体重写稿或改动源文件。
- 下一步：按 ADR-0008 的六问和六类叙事范式，对引言—方法—实验矩阵做一轮结构化故事线审查，先形成审查报告，再决定是否编辑论文。

### 2026-10-04 — v3 五折闭合，transfer diagnostic 完成

- fold4 原始回放先被旧 verifier 拒绝：runner 使用 `1e-12` 状态变化阈值，旧 verifier 用 exact float 差异，导致 Independent `provider_local_update_events` summary 111/116 与 raw 115/120 不一致。原始拒绝文件保留在 `v3_fold4_seed0/verification_rejected_summary_mismatch.json`。
- verifier 已按 runner 的 `math.fsum/sqrt` + `1e-12` 口径修正，同时对未选 provider 保留 exact state invariance；新增重复 stream、tick 顺序、family 维度和边界测试。7 个 verifier tests、联合 v3 tests 通过。对同一 fold4 raw receipt 重验为 `verified`，没有重跑。
- 五折现在均通过最新 verifier：9 streams/fold，722 evaluation rows，19,494 raw events。跨折汇总 `matched_effect_matrix_v3_crossfold_aggregate_v1.md/json`：MIRA vs Independent net 为 d0/1 `[1,1,0,0,0]`、d4 `[0,1,0,0,0]`；MIRA vs NoUpdate net 为 d0/1 `[1,1,1,2,0]`、d4 `[0,1,1,2,0]`。fold-level区间跨0，不能写成最终效果。
- 按 transfer diagnostic card 对 raw 做只读分析：1,369 条 MIRA update records；sibling opportunity 各 fold/delay 约 0.117–0.338，family state 变化大多可检测，但 MIRA vs Independent action difference 每 stream 0–4 条，rescued/harmed 很少。当前最可能的瓶颈是 sibling exposure 不足和 action margin 过大，而不是“没有更新”。报告与逐条 JSON 已保存。
- 按三份验收标准：Benchmark/online contract development 闭合，但 Gate C 仍 HOLD；方法 M5/M13/M15、故事 S4/S6 仍 HOLD。下一步先预登记一个只改变 transfer 可识别性/更新尺度的机制敏感性分析；在它给出 action-level evidence 前不申请 seed1/A800。

### 2026-10-04 — HRA fold0 机制闸门闭合

- 目的：检验 shared family residual 之外增加 selected-provider local residual，是否能把 transfer 变为 action-level consequence；沿用 v3 的输入、菜单、fit-only 初始化、probe seed、selected-only feedback 和 delay 合同。
- 完成：候选 runner 在 executable commit `5d4b49aa0192e4066b3d3e1d90f5c2784dd91ae2` 上跑完 fold0/seed0，145 molecules × 3 delays，1,305 raw events。独立 verifier 从冻结 provider cell 重算 reward，检查 event roster、queue/release、state hash chain、selected-only 和未选 family/provider residual 不变性，结果 `verified`。
- 结果：HRA 在 delay0/1 为 127/145，delay4 为 125/145；相对 MIRA 的 action changes 是 3/3/4，rescued/harmed 为 0/0、0/0、0/1；相对 Independent 为 1/0、1/0、0/1。候选有真实动作变化，但没有稳定收益；这不是最终效果。
- 留痕：`hierarchical_residual_sensitivity_v1_report.md`、`hierarchical_residual_sensitivity_v1_pair_metrics_fold0_seed0.{md,json}`；raw receipt/manifest 位于 `experiments/logs/task49_mira_hierarchical_sensitivity_20261004/fold0_seed0/`。原始 events 269,693,893 bytes，SHA-256 `0c5b4097b169b9aedd17077236f9972f4910b2b461a79fb5d2a876a815f96b95`，保留本地。
- 验收：Gate C、方法 M5/M13/M15、故事 S4/S6 仍 HOLD。已更新 acceptance review；本次没有改论文主张、没有读 confirmation、没有提交 A800。
- 下一步：按预登记 action-level consequence 条件继续 folds1–4，目标是跨 fold paired uncertainty；若收益仍不稳定或 delay 压力下受损，记录负机制结论并回到 transfer opportunity/margin slice，不通过扩算力追分。

### 2026-10-04 — HRA 停止规则更正

- 更正上一条“继续 folds1–4”的判断：预登记停止条件是析取条件，不能只看是否出现 action change。HRA 相对 MIRA 没有 rescued 且 delay4 harmed 1 条；相对 Independent 的唯一 rescued 是同一个 `z070852`（delay0/1 重复），触发 single-molecule gain 停止条件。
- 最终决策：HRA 仅作为通过事件/实现核验的负机制诊断，停止 folds1–4，不申请 seed1/A800，不升级为主方法。回到 transfer opportunity、score margin、family size 分层分析。
- verifier 进一步收紧：未选 state exact invariance、复制 fit_state hash、按 declared commit 的 git blob source hash、candidate menu/summary identity 均已核验；`verification.json` 将按 v2 输出更新。family update 与 non-singleton sibling transfer 分开，后者为 delay0/1/4 的 20/20/18。

### 2026-10-04 — HRA verifier hardening follow-up

- 在不重跑回放的前提下，独立 verifier 增加了 fit-state copied NPZ hash、declared commit 的 git-blob source hash、fit-state provider-family binding、terminal tick/feedback version 递增、feedback snapshot、candidate menu 和 exact off-target state invariance 检查。
- 对同一 1,305-event receipt 重新核验仍为 `verified`；verifier source SHA `2b9f5bc717310033d9a634e4f32aa4a2ecab7f1aa4f007e9137faa90b8da6eec`，commit `5d4b49aa0192e4066b3d3e1d90f5c2784dd91ae2`。这只增强证据可信度，不改变停止 HRA 的科学决定。
- 相关回归测试：MIRA/HRA policy 与 v3 verifier `11 passed`；raw events 仍只保留本地，manifest 已更新。

### 2026-10-04 — family-size / margin 只读诊断

- 目的：区分 family-sharing 没有收益是因为 sibling opportunity 少、action margin 大，还是更新本身无效；不新增回放、不读未选输出改变动作。
- 结果：复用 v3 五折 transfer records 的 1,369 条 selected updates，按 provider family size 重分层。singleton 更新 1,095/1,369（80.0%），opportunity=0；non-singleton 更新 274 条，opportunity=100%，但相对 Independent 的 action difference 14 条，rescued=0。family state 非零变化不能直接当作 sibling transfer。
- HRA fold0 vs MIRA 的 changed actions：delay0/1 各 3 条，均为 admet_ai size2；delay4 为 admet_ai 1 条、b3clf 2 条、admetlab singleton 1 条。此前“全部来自 non-singleton”的表述已更正。
- 留痕：`task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/margin_family_diagnostic_v1.{md,json}`，复算脚本 `src/probe_headless/diagnose_margin_family_v1.py`。
- 决策：HRA 不扩 folds；下一候选应先解决 family opportunity/graph construction 或明确收窄 family-sharing claim，不能靠增加 seed/A800 追分。

### 2026-10-04 — Provider-Graph Relay 候选卡（未运行）

- 目的：针对 80% singleton family 导致的 family-sharing opportunity 缺口，提出 fit-only provider error graph + selected delayed diffusion 的候选机制。
- 已固定的候选边界：pairwise valid-overlap Pearson + shrinkage `lambda=32`、`m_min=128`、top-`k=2`、传播质量 `rho=0.5`、degree-matched random graph control；只在 fit split 建图，evaluation 不更新图。
- 进入 gate 前置条件：完成 graph bandit/relational bandit/label propagation/algorithm-selection 最近邻核验；先做零机时 graph manifest、独立初始 logits 和 sparse-update/invariance 测试；不通过不写效果表、不申请 Nebula。
- 留痕：`task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/provider_graph_relay_v1_candidate.md`。该卡不改变当前主方法、benchmark/baseline 或目标 claim。

### 2026-10-04 — Provider-Graph Relay prior-art gate closed

- 完成一轮只用一手/作者来源的 prior-art 核验，覆盖 feedback-graph bandits、GOB.Lin/CoLin、correlated-arm bandits、label propagation 和 online algorithm portfolios；来源与逐项差异见 `research/provider_graph_relay_prior_art_20261004.md`。
- 结论：图加权邻居更新本身不能作为创新。候选只有在 fit-derived provider transfer graph、selected-only delayed feedback、未选结果不可见、degree-matched graph controls、相同更新质量和跨 molecule/delay paired action consequence 同时成立时，才有资格进入 CPU gate。
- 已把该决定补入候选卡 `task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/provider_graph_relay_v1_candidate.md` 和 v3 acceptance review。当前不实现、不申请 Nebula、不写效果表；如果接口/信息边界无法与最近邻区分，则降为 baseline/control。
- 下一步：只做零机时 graph manifest、独立初始 logits、sparse update/invariance 和 graph comparator contract；任何一项失败都停止，不扩大算力。

### 2026-10-04 — Provider-Graph Relay zero-cost contract check

- 先修正候选的数学/接口边界：top-k 相似度加入 `tie_tol=1e-12`；graph relay 明确为 selected-coordinate BCE 的迁移启发式，不是精确 Bayesian/PSD posterior；fallback 从可写邻居排除并重新归一；随机对照改称 out-degree-matched，避免暗示保持入度。
- 新增 `src/probe_headless/check_provider_graph_relay_v1.py` 与 4 个单测；独立 NumPy reference、fit-index/sealed-state 交叉核对、冷启动 real/random/identity graph 等价、fallback exact invariance、sparse update 和 relay mass 均通过。
- 五折零机时收据位于 `experiments/logs/task49_provider_graph_relay_zero_cost_20261004/`：5/5 fold，66 条正边/fold，静态 provider coverage=1.000。该 coverage 只是图结构覆盖，不是 selected replay opportunity；没有运行 replay，也没有效果结论。
- 独立数学审查和实现审查已落在 `research/provider_graph_relay_math_review_20261004.md` 与 `research/provider_graph_relay_contract_audit_20261004.md`。数学审查指出无条件 correctness Pearson 不保证 recipient residual alignment；下一步必须做 held-out alignment diagnostic，并保持随机图对照。
- 当前决定：零机时结构门通过，但不申请 Nebula、不写效果表。进入 fold0 CPU replay 前，先预登记 alignment diagnostic 和完整 fallback/delay contract。

### 2026-10-04 — Provider-Graph Relay held-out alignment gate

- 预登记 `provider_graph_alignment_v1_preregistration.md` 后执行只读诊断：只用 fit-only graph/sealed state，在五折 validation rows 上测 `||x||² δ_p δ_q`，并与保留 source writable out-degree 与 target in-degree 多重集的随机图比较；不更新 state、不跑 replay、不申请 GPU。
- 结果：5/5 folds graph mean alignment 为正，且 5/5 folds 高于 degree-matched random；fold mean 分别为 `2.47045/2.12750/1.55852/2.19913/1.98397`，graph-minus-random 为 `+0.78724/+0.58214/+0.49918/+0.56800/+0.82256`。达到预登记资源闸门，只说明值得支付一次 fold0 replay 成本。
- 收据：`experiments/logs/task49_provider_graph_alignment_20261004/`，含 `records.jsonl`、`summary.json`、`config.json`、`full.json`、报告；脚本 `src/probe_headless/diagnose_provider_graph_alignment_v1.py`，零机时测试 5 passed。
- 边界：alignment 不是 action/accuracy 效果，不证明 delay4 安全，也不把 graph relay写成 Bayesian/PSD 更新。下一步只预登记 fold0/seed0、delay `{0,1,4}` 的 replay，加入 MIRA、Independent 和 degree-matched random graph；若 paired action gate 不通过，停止候选。

### 2026-10-04 — alignment random-control bug correction

- fold0 replay second startup在随机 control policy 初始化阶段失败；根因是 random target generator 混用 provider index/ID，造成 source 内重复 target。该次没有进入随机流效果统计，失败现场保留在 `experiments/logs/task49_provider_graph_relay_fold0_20261004/receipt_v2/`，`failure_v2.json` 待补。
- 原 alignment 收据 `experiments/logs/task49_provider_graph_alignment_20261004/` 因同一 bug 不再作为有效比较，旧文件保留；修复后生成 authoritative `corrected_v2/` 收据，5/5 fold graph mean 为正且高于保留 in/out-degree 的随机图，结果仍通过资源闸门。
- 修复已提交 `70e5455b`，新增随机图唯一性测试；下一步用当前完整 commit 重新启动 fold0 receipt_v3，先完成结构/paired verifier，再判断 action gate。

### 2026-10-04 — Provider-Graph Relay fold0 gate closed

- 目的：把 held-out alignment 的静态正信号转成一次严格的 action-level replay；只跑 fold0/seed0 CPU，不申请 Nebula。
- 正式 receipt `experiments/logs/task49_provider_graph_relay_fold0_20261004/receipt_v3/` 已完成：4 methods × 3 delays × 145 molecules，12 streams、5,220 raw events。代码 commit `70e5455b7b3bdab333b9156dbcf8f19a4dcf365a`，events SHA-256 `b3ebe9c1045daf8a1b9fcae6cb3ae712e9dca7ef12062c2392588296621db448`。
- 独立 verifier v2 通过：从冻结 provider cell 重算 output/reward、delay queue、state hash、fallback invariant 和 paired metrics；旧 verifier 的过严拒绝保留在 `verification_rejected_v1.json`，修正版结果在 `verification_v2.json`。已有回放未重跑。
- Graph Relay vs MIRA：delay0 `1 rescued/3 harmed/net -2`，delay1 `1/3/-2`，delay4 `4/4/0`；vs degree-matched random：`0/1/-1`、`0/1/-1`、`1/2/-1`。动作变化真实存在，但没有任何 delay 取得正净差，触发预注册停止条件。
- 结果报告：`task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/provider_graph_relay_fold0_v1_report.md`；候选卡和 v3 acceptance review 已更新。Provider-Graph Relay 停止 fold1--4、seed1、A800、confirmation 扩展，降为已核验负机制诊断/对照，不升级为主方法。
- 测试：`test_provider_graph_relay_zero_cost.py`、MIRA/Independent policy tests 共 22 passed。
- 当前 Gate C、M5、M13、M15、S4、S6 继续 HOLD。下一步回到统一 E1--E5 主矩阵缺口（baseline fairness、paired uncertainty、failure/unknown/refusal 分母、cross-endpoint qualification、frozen confirmation 前主表），不再为 graph relay 增加算力。

### 2026-10-04 — acceptance scorecard synchronized after graph gate

- 将 Graph Relay fold0 的 verified 负结果写入 `research/AAMAS2027_current_evaluation_scorecard_20261004.md`，并在 `task/task49-pharma-benchmark-baseline-freeze/current_acceptance_audit_v2/report.md` 追加当前状态。
- 三门分数不变：Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`；没有用负结果给分，也没有降低目标 claim。
- 下一步仍只从统一 baseline 公平合同、完整 E1--E5 主表、unknown/refusal 分母、cross-endpoint qualification 和 frozen confirmation 中选择有可验证指标的任务。

### 2026-10-04 — baseline eligibility matrix v2

- 目的：不重复已有 bootstrap，直接回答“哪些 baseline 可以合法进入同一主表”。只读读取 role roster、cost/budget table、raw schema audit 和已封存 verifier/paired receipt；未重跑、未读 confirmation、未上 Nebula。
- 新增合同与产物：`task/task49-pharma-benchmark-baseline-freeze/baseline_eligibility_matrix_v2/{preregistration.md,matrix.json,report.md}`，生成器 `src/probe_headless/build_baseline_eligibility_matrix_v2.py`，测试 `test_baseline_eligibility_matrix_v2.py`。
- 16 行机器审计分类：strict_main=2（Frozen、Matched）、candidate=2（MIRA-relative、FamilyGuard）、adapted_stratum=4（reward-ridge/Greedy/Bayesian/PILOT）、anchor=3、static_control=2、blocked=3（Replay、GraphRouter、Laya online）。strict core 的输入、菜单、selected-only、family registry、verifier 和 paired 指针齐全；完整矩阵仍不完整。
- 结论：`strict_core_eligible_full_matrix_incomplete`。Gate B/C 继续 `11/16 HOLD`、`10/16 HOLD`；不能跨 stratum 混排 raw accuracy，不能把 adapted 或 audit-only 方法改名充数。
- 验证：机器生成无 identity error；matrix tests `2 passed`。下一步优先补 Replay SGD 的 feature/warm-start manifest 或明确保留 separate control；不重复 bootstrap、不为 Graph Relay 加算力。

### 2026-10-04 — paper build and format check

- 重新运行 `paper/aamas2027/Makefile` 的 `draft` 目标，当前题目仍为 `After the Decision: Learning to Adapt from Delayed Outcomes`。
- PDF 构建成功：`paper/aamas2027/build/main.pdf`，7 页，682,288 bytes；Figure 1 v4、Figure 2 v8 均能正常嵌入。
- LaTeX 没有 fatal error；日志只有模板表格和页底的 underfull box 警告，没有 overfull/error。当前仍是内部 pre-results draft，经验表格的 TBD 没有被填充。

### 2026-10-04 — Replay-SGD provenance 审计与 baseline 矩阵收敛

- 目的：处理 baseline 资格矩阵中 Replay-SGD 的 feature/warm-start 阻塞，不重跑 policy、不读 confirmation、不申请 Nebula。
- 新增：`src/probe_headless/audit_replay_sgd_provenance_v1.py`、`src/probe_headless/tests/test_replay_sgd_provenance_audit_v1.py`，以及 `task/task49-pharma-benchmark-baseline-freeze/replay_sgd_provenance_audit_v1/{audit.json,report.md}`。
- 证据：旧收据 10 个 fold/seed prior 均精确对应历史 BT checkpoint 的 `score.weight`；722×64 feature 与当前 strict snapshot 最大绝对差 `3.51e-7`；历史 prior 与当前 common-direct-prior 最大绝对差 `0.3328`；旧 receipt 没有 state bytes。
- 决定：Replay-SGD 固定为 `separate_control`，不进入 strict common-direct-prior 主表；GraphRouter/Laya online 仍为 blocked。矩阵已重建，分类为 strict_main=2、candidate=2、adapted_stratum=4、separate_control=1、blocked=2、anchor=3、static_control=2。
- 验证：`python3 -m pytest -q src/probe_headless/tests/test_replay_sgd_provenance_audit_v1.py src/probe_headless/tests/test_baseline_eligibility_matrix_v2.py`，3 passed；无实验重跑。
- 已更新：baseline eligibility report、cost report、current acceptance audit、evaluation scorecard。分数不变：Benchmark+Baseline=41/60，Gate A/B/C=11/16、11/16、10/16，均 HOLD。
- 下一步：转向统一 E1–E5 主矩阵的剩余缺口；不再为 Replay 旧 receipt 回填 state bytes，也不把 separate control 与 strict rows 混排。

### 2026-10-04 — adapted baseline raw field audit

- 目的：区分 adapted baseline 的“字段已记录”和“字段仍缺失”，不为缺字段申请新实验。
- 新增：`src/probe_headless/audit_adapted_baseline_receipt_fields_v1.py`、`task/task49-pharma-benchmark-baseline-freeze/adapted_baseline_receipt_field_audit_v1/{audit.json,report.md}`、测试 `src/probe_headless/tests/test_adapted_baseline_receipt_fields_v1.py`。
- 结果：Greedy LinUCB、PILOT、BayesianRouter native-BT、reward-ridge 均有 8,664 decision/terminal/feedback、state hash、runtime status 和 failure feedback，独立 verifier 均 `verified`；四者都没有 state bytes，也没有逐事件 unknown/refusal reason。
- 机器收据状态：`field_audit_complete_with_state_bytes_gap`。成本表已追加审计指针，缺失字段仍是 `MISSING`；不改变分层、分数或 confirmation。
- 验证：脚本实际扫描四条 raw receipt 完成；下一步转向统一 E1–E5 主矩阵的剩余缺口，不从 hash/模型维度推断 state bytes。

### 2026-10-04 — endpoint qualification matrix

- 目的：把 Benchmark 的 Gate A 缺口从分散材料收敛成一张可执行的资格表；不训练、不预测、不打开 confirmation。
- 新增：`src/probe_headless/build_endpoint_qualification_matrix_v1.py`、`src/probe_headless/tests/test_endpoint_qualification_matrix_v1.py`、`task/task49-pharma-benchmark-baseline-freeze/endpoint_qualification_matrix_v1/{matrix.json,report.md}`。
- 结论：BBB=`development_primary`（716 个可评分分子、33 provider）；AMES=`appendix_candidate`（129 个、4 provider，低覆盖且 lineage 未闭合）；hERG=`blocked_semantics`（43 个，TDC/FP-ADMET/ADMETlab assay/cutoff 未证明等价）。
- 验证：机器矩阵生成成功，测试 1 passed；Gate A/B/C 分数保持 11/16、11/16、10/16，confirmation 继续关闭。
- 下一步：只补表中明确硬缺口（provider training membership、独立 snapshot；hERG 先补 assay/cutoff），不把 AMES/hERG 结果提前并入主表。

### 2026-10-04 — 叙事范式审查迭代与七页论文稿收口

- 目的：按 `research/2026_AI顶会最佳杰出论文叙事范式_六大范式_阅读整理.md` 对论文做一轮“写作—独立审查—修订—终审”，主线固定为“根因手术刀”，辅以“新基准暴露失效”和“反直觉重构”。
- 已完成：引言改成 `scene → concrete disagreement → selected-only/delay root cause → shared-residual insight → MIRA-Relay mechanism → falsifiable commitments`；加入可复核开发例 `z000282`（TDC label=1；`admet_ai_1_4_0` 返回 0、`b3clf_dtree_common` 返回 1，均 successful runtime status）。
- 已完成：补齐正文中的 fixed registry + dynamic eligible mask、enqueue-order release、`system_success`/`chemical_correct` 分账、空 challenger propensity、Beta hazard 计数规则、`beta_0=(u^0,b^0)` 初始化；拆开 invariants、falsification、science gate；E6 统一为 `BLOCKED / TBD`；将 conditional metric 收窄为“only when emitted”；GraphRouter 标为 audit-only、MIRA-Relay 标为 proposed method。
- 独立审查留痕：第一轮 `research/AAMAS2027_narrative_review_v1_20261004.md`；终审 `research/narrative_iteration_20261004/review_round2_post_revision.md`。终审结论：当前稿作为明确标注的 pre-results design 为 `CONDITIONAL`；作为已有实证效果的最终论文仍 `FAIL`。未把 TBD 当结果，也未降低目标 claim。
- PDF：`paper/aamas2027/build/main.pdf` 编译成功，7 页，SHA-256 `996c16dd1aa74bc433b019ae29463d5d494c17b1daa1a223c038540934e8befe`。LaTeX 无 fatal error；仍有模板的 `\ifx` incomplete warning 和少量 over/underfull box，未改变页数或生成失败。
- 版本留痕：`research/narrative_iteration_20261004/main_post_revision.tex`、`snapshot_manifest.json`；主稿 SHA `f80973e5db417e66c87db5931dba7c1d9737f7be743d828fc46f3b912c97db23`。
- 未完成：E1–E5 真实结果、strict baseline 公平合同、E3 机制消融、paired uncertainty、provider lineage 和 E6 confirmation；当前三门分数仍为故事 47.5/100、方法 37/60、Benchmark+Baseline 41/60，Gate A/B/C=11/16、11/16、10/16 HOLD。
- 下一步：在统一 raw-run/recovery schema 下先做 E1/T1、E2/T2、E3/T4、E5/T5 CPU gate；结果闭合后再更新摘要、贡献段、主表和结论，不为当前 TBD 申请效果型 A800。

### 2026-10-05 — 状态索引与 selector 接口修复

- 独立方法审查发现一个真实的 H1/M2 索引矛盾：旧式 `S_{t+1}=U(...),\ \tau_s\leq t` 会让 `D=0` 事件错过下一 tick。已改为 `S_t` 是 tick $t$ release phase 完成后的状态，先处理 `s<t,\tau_s=t`，再由 selector 读取；并把接口显式改为 `\pi_{\\mathrm{sel}}(x_t,e_t,A_t,H_t,S_t)`。审查复核确认 `H_t`、`\tau_t=t+1+D`、queue 和状态索引一致。
- 叙事复核结论：设计合同层面 PASS；pre-results design 仍 CONDITIONAL；最终效果论文仍 FAIL，原因是 E1–E5、独立 transition replayer、paired uncertainty、strict baseline fairness、provider lineage 和 confirmation 尚未闭合。没有降低目标 claim。
- 新留痕：`research/narrative_iteration_20261004/review_round4_state_index.md`、`main_post_revision_round4.tex`、更新后的 `snapshot_manifest.json`。
- clean build 通过：`paper/aamas2027/build/main.pdf` 为 7 页、letter、692886 bytes；主稿 SHA-256=`0085a89b446ecdcd6701c6a35f3eb2138ece46c7109f40bf379dc4251ce16cf0`，PDF SHA-256=`795c9cea74362bb887ba3cd41e2d4af6b82ca87c260d8ca5caec5b21187d7901`。无 fatal error，保留模板 `\\ifx` warning。
- 下一步仍是统一 raw-run/recovery schema 的 E1/T1、E2/T2、E3/T4、E5/T5 CPU gate；不为当前 TBD 申请效果型 A800。

### 2026-10-04 — 释放时钟、收据边界与 pre-results 时态第三轮收口

- 按两名独立审查员意见修正文稿：正文写明 `\tau_t=t+1+D`、`D\in{0,1,4}` 的 decision → termination → enqueue → release 顺序，并说明 `D=0` 在下一决策 tick 的 release phase 处理；receipt verifier 限定为 read-set/action/queue/state-hash/schema/counts，完整 score/propensity/MAP/metric 重算留给 E5 transition replayer；公平记录加入 checkpoint 或 sealed-fit-state hash；摘要和方法改为 planned evaluation / will evaluate。
- 叙事终审仍为：明确标注的 pre-results design=`CONDITIONAL`；作为已有 superiority 实证的最终论文=`FAIL`。主范式仍为“根因手术刀”，本轮新增合同没有改变因果链。
- 证据留痕：`research/narrative_iteration_20261004/review_round3_contract_cleanup.md`、`main_post_revision_round3.tex`、`snapshot_manifest.json`。主稿 SHA-256=`1064656ebd304b0fdf40c61d49acb3aea75f1586826df3f4f24ee2516b0e0e9e`；PDF SHA-256=`7943ce3f7e16e6c0ddbc968eb6ba3408925bfe5b5422de3894d5949c3caa6cb7`。
- 重新 clean build 通过：`paper/aamas2027/build/main.pdf` 为 7 页、letter、692633 bytes；无 fatal error，保留模板 `\\ifx` warning 和少量排版 warning。未产生新 GPU 任务，也未把 TBD/诊断写成 headline 结果。
- 未完成仍是 E1–E5 主结果、独立 transition replayer、strict baseline 公平合同、paired uncertainty、provider lineage 和 E6 confirmation；下一步只推进统一 raw-run/recovery schema 下的 CPU gate，闭合后再决定 A800 成本。
# 2026-10-04 — provider lineage / E1–E5 coverage / paper build closure

## 本次完成

- 完成 BBB 33 个具体 provider 的只读训练归属与 freshness 审计：
  `task/task49-pharma-benchmark-baseline-freeze/provider_lineage_audit_v1/`。
  结果为 G3=1、G2=30、G1=2，model-fresh confirmation 资格为 0；明确 24 个 B3clf
  共享 B3DB family、3 个 DeepPurpose 共享 BBBP 来源。未重跑 provider、未启动 GPU。
- 完成 E1–E5 覆盖审计：
  `task/task49-pharma-benchmark-baseline-freeze/e1_e5_coverage_audit_v1/`。
  E1–E5 全部 `partial`，E6 `missing`。缺口已按 E5 schema/denominator → E2 分层主表
  → E1 控制 join → E3 机制表 → E4 scope-bound stress 排序，未跨 scope 填数。
- 恢复并核验真实 A800 指纹诊断：Nebula task
  `01a0de1cf322cd9ee1ead48f41569649` 正常完成；完整 worker stdout 已取回并核验，
  `recovered_v2/summary.json` 状态 `verified`，config=1、split=5、epoch=8000、
  checkpoint=40、evaluation=5776、fit_complete=40。结果未改变：MLP/BT+CLS 均低于
  Best Fixed，`go-signal=false`。报告已追加 recovery_v2 复核记录。
- 修复论文构建交付问题：发现并保留并发写 `build/main.pdf` 的损坏现场；改用单进程隔离
  目录 `paper/aamas2027/build/verify_current_20261004/` 构建成功，主文 7 页。源码/PDF/
  预览 hash 与编译警告见 `paper_format_review_v1/build_provenance.json`。
- 更新 `current_acceptance_audit_v2/report.md` 与
  `research/AAMAS2027_current_evaluation_scorecard_20261004.md`；三门分数不变：
  Benchmark+Baseline=41/60，Gate A/B/C=11/16、11/16、10/16，均 HOLD。

## 验证

- provider lineage + E1–E5 tests：7 passed。
- adapted receipt / Replay provenance / eligibility / endpoint tests：5 passed。

## 未完成与下一步

- 论文仍是内部 pre-results draft；E1–E5 未闭合，MIRA identity 与 strict/separate/adapted
  表格仍需在正文显式分层，Figure 2 规格仍需决定，E6 confirmation 继续关闭。
- 下一步只做统一 E5 schema/denominator 和已有收据的 role-qualified E2 主表；除非出现新
  可证伪科学问题，不提交新的 A800 效果任务。


### 2026-10-05 — 当前会话收口

- 修正并审查状态时序：`S_t` 是 tick $t$ release phase 后状态，selector 显式读取 `S_t`；`D=0` 事件进入 `S_{t+1}`，不会影响触发它的 action。方法合同审查与叙事审查均通过这一修补。
- 留痕：`research/narrative_iteration_20261004/review_round4_state_index.md`、`main_post_revision_round4.tex`、`snapshot_manifest.json`。
- clean build 通过：`paper/aamas2027/build/main.pdf` 7 页、letter、692886 bytes；主稿 SHA-256=`0085a89b446ecdcd6701c6a35f3eb2138ece46c7109f40bf379dc4251ce16cf0`，PDF SHA-256=`795c9cea74362bb887ba3cd41e2d4af6b82ca87c260d8ca5caec5b21187d7901`。
- 最终验收状态不变：设计合同层面 PASS；pre-results design CONDITIONAL；最终效果论文 FAIL。E1–E5、独立 transition replayer、paired uncertainty、strict baseline fairness、provider lineage 和 confirmation 仍未闭合。下一步继续 CPU gate，暂不申请效果型 A800。


### 2026-10-05 — CPU contract gate integrity check

- 在进入 E5 schema/denominator 与 E2 role-qualified 主表前，运行 coverage、baseline eligibility、endpoint qualification、Replay provenance 四组机器审计：`7 passed, 0 failed, 1.48 s`。
- 收据：`task/task49-pharma-benchmark-baseline-freeze/cpu_gate_contract_tests_20261005/{report.md,result.json}`。该检查只验证证据绑定和分层合同，没有重跑策略、估计效果、打开 confirmation 或授权 GPU。E1–E5 仍为 `partial`。
- 下一步：继续 E5 schema/denominator closure，再生成 role-qualified E2 development table；没有新的效果型 A800 任务。

### 2026-10-05 — paper writing and figure prompt iteration (scope: no experiments)

- 用户本轮将执行范围收窄为论文写作、架构图和效果图展示；本轮未提交 Nebula 任务、未跑 GPU/实验。
- 阅读并落档本地 TopConf Figure Gallery 与样例库：`paper/aamas2027/figures/reference_audit_20261005.md`。抽取的可复用规则是单一阅读方向、显式信息边界、四种语义色以内、线型冗余编码、短标签和可编辑 SVG/HTML；不采用海报式、3D 或装饰性图。
- 建立 AI 生图提示词版本库：`paper/aamas2027/figures/prompts/`。Figure 1/2 当前 prompt 为 v03，并保留 v01/v02；Figure 3 v03 是空结果布局，明确禁止任何虚构点、曲线、误差带和 Pareto frontier，待真实收据后再生成数据图。`.txt` 是可直接复制到外部生图工具的 prompt，`.md` 同时保存结构说明、版本变化和负面提示。
- Figure 1 新选 `figure1/v6/figure1_v6.pdf`，Figure 2 新选 `figure2/v9/figure2_v9.pdf`。修正了 `runtime verifier` 误导、`safe cold start` 安全保证误读、`challenger feedback` 即时更新歧义，并统一正式图配色为 amber=terminal/judge、purple=pending/delay；Figure 1/2 各保留 SVG、HTML、render.py、PNG、PDF、manifest 和 review。
- 独立图审查：Figure 1 v6=`15/16`，Figure 2 v9=`15/16`。语义、信息边界、selected-only 延迟、无障碍 title/desc、可编辑证据通过；各扣 1 分是目标栏宽下辅助文字偏小（Figure 1 约 6.5pt；Figure 2 正文约 7.8pt、ghost 约 5.4pt）。没有发现重叠、裁切或第二次调用暗示。
- 论文正文压缩并保持比较合同：方法身份、E2/E4 分层、诊断段、主表 caption 和机制表 caption 改为短句；补回 `N/calls`、`Fail/U/R`、各 strata 的 paired 对照、ContextProbe 的 Frozen 对照和 895 次 forced probe 限定，补入 `Independent residual / D` 行，E4b 改为 `BLOCKED / TBD`。Figure caption/Description 统一称 `frozen endpoint judge` 和 `released challenger feedback`。
- 当前隔离编译：`paper/aamas2027/build/verify_figures_final_20261005/main.pdf`，8 页 letter，SHA-256=`2f0d2abd2d32d1c33c99556e67954a4af8ed1fa7c2a3a5e98272b83ff0536514`。8 页符合已核对的 AAMAS 主文最多 8 页要求；仍是 pre-results 内部稿，保留 1.33pt 窄栏 overfull vbox 和模板 `ifx` warning，未将其伪装成提交稿。格式审计与论文 README 已同步。
- 下一步：继续只做写作和图形迭代；优先在不改模板字号/页边距的前提下处理窄公式/表格 overfull，并在下一轮独立图审查后决定是否只缩短辅助标签。Figure 3 在真实结果证据冻结前不生成数据图。

### 2026-10-05 — paper matrix strata and isolated build v3

- 目的：把论文中的比较资格层和动态事件 scope 直接写进实验矩阵，避免审稿人把 strict、historical separate-control、adapted baseline 和 controlled stress 读成一张可排序的总榜。
- 已完成：`paper/aamas2027/main.tex` 将 E2 拆为 E2a strict common-prior logistic、E2b Replay separate-control、E2c adapted likelihood/prior；E4 拆为 E4a controlled feedback-adaptive 与 E4b natural lifecycle（无冻结轨迹保持 `BLOCKED / TBD`）。主表保留 `N/calls`、success、paired CI、Fail/U/R、update p95；机制表保留 scope/delay、probe、action changes、rescued/harmed、paired CI。所有结果格仍为 `TBD`。
- 排版修复：状态更新公式改为分行形式，消除最终轮次 overfull hbox；没有改模型、数据、指标或结果。第 2 页公式和第 7 页表格目视检查通过。
- 构建证据：`paper/aamas2027/build/verify_current_20261005_v3/main.pdf`，8 页 letter，PDF SHA-256=`8e7366bc61c3ab47b23d0ab6ad2d33413c622f4af389b4a7a5b0b9d711fb71a1`；源码 SHA-256=`90450db411e41b631d8574c63522ae5d38f0e3cb694bb3664dfe352f5da955ba`；完整凭据 `task/task49-pharma-benchmark-baseline-freeze/paper_format_review_v1/build_provenance_20261005_v3.json`。最终 pdflatex 无 undefined citation/reference、无 overfull hbox，仅保留模板 `ifx` warning、underfull 提示和 1.333pt page-balance overfull vbox。
- 科学状态：这轮只改善论文审查可读性，三门评分不变，E1–E5、transition replayer、paired uncertainty、provider lineage 和 E6 confirmation 仍未闭合；未提交 Nebula 任务。
- 下一步：先等只读表格审查回执；随后把当前严格主比较的证据字段与已有 raw receipts 对齐，优先补 E5 分母/成本或生成 role-qualified E2 表，仍不凭已有诊断填入 headline result。

### 2026-10-05 — method identity and baseline-name closure

- 对照 `baseline_eligibility_matrix_v2/matrix.json` 后，补齐论文中的固定映射：MIRA 是框架、MIRA-Relay 是冻结实现、FamilyGuard 是其机制条件，ContextProbe 和 Provider-Graph Relay 是停止诊断；结果表恢复为 `Greedy LinUCB (adapted)`，与 baseline roster 一致。
- 这次只修审稿可读性和身份一致性，不改变数据、实验资格、方法 claim 或结果。
- 隔离构建 `paper/aamas2027/build/verify_current_20261005_v4/main.pdf` 通过：8 页 letter，PDF SHA-256=`aa93bfeb30d3e97140f39541dd6a4ab024117dbfb9e6100752326648d30b0210`，源码 SHA-256=`169d54af7f0f09d1e735a0395691575b21b14ee8c866dd7229a0de8e2c6394e7`；最终轮次无 undefined citation/reference、无 overfull hbox/vbox，保留模板 `ifx` 与 underfull 提示。凭据：`task/task49-pharma-benchmark-baseline-freeze/paper_format_review_v1/build_provenance_20261005_v4.json`。
- 科学状态仍不变：三门评分和 E1–E6 状态不变，未提交 Nebula 任务。下一步回到已有 raw receipt 的 role-qualified E2/E5 字段审计，不做没有新问题的 GPU 运行。

### 2026-10-05 — 用户生图审查：读取权限阻断

- 用户要求审查 Downloads 中两张 PNG。两次 view_image 都返回 Operation not permitted (os error 1)，未获得图像，未给出质量或语义判断。
- 已读取当前 Figure1/2 v03 提示词与 FIGURE_REVIEW 八项规则；待用户附图或复制到可访问目录后继续实际审查。
- 留痕：`paper/aamas2027/figures/external_review/20261005_downloads_access.md`。本轮未改论文、图像或实验。

### 2026-10-05 — 用户图稿实际视觉复核

- 用户直接附上两张 PNG 后完成实际视觉审查；不再依据文件名或提示词推断。
- 复核记录：`paper/aamas2027/figures/external_review/20261005_downloads_review.md`。
- 图 1（多阶段总览）评分 11/16：主线和 selected-only 边界清楚；`ledger only` 放在 selector 区、`released challenger outcome` 未显式写 selected-only、pending 期间的继续决策未画出，且当前仅有 PNG。
- 图 2（延迟生命周期）评分 10/16：结构清楚，但 `next decision / decision t+1` 与 `$\tau_t=t+1+D$` 在 `$D>0$` 时冲突，必须先改为 release tick 之后的首个决策或画出使用旧状态的 `$t+1$`；当前仅有 PNG。
- 结论：两张图可作为重绘底稿，暂不进入论文正文；下一步只修时序和信息边界，再重绘 SVG/PDF，并保留每一版提示词与源文件。

### 2026-10-05 — figure prompt v04

- 根据两张用户图稿的实际复核结果，新增并选定 `paper/aamas2027/figures/prompts/figure1/v04.md/.txt` 与 `figure2/v04.md/.txt`；v03 保留不覆盖。
- Figure 1 v04：将 `ledger only` 移出 selector，改为 fallback 终态记账路径；明确 `released selected-provider outcome`、`S_t` 输入和 pending 期间仍可继续决策。
- Figure 2 v04：新增 `$t+1$` 到 `$\tau_t-1$` 使用旧状态的旁路，改为“first decision at or after `$\tau_t$`”，并单独标出 `$D=0$` 情况，消除正延迟时序歧义。
- 同步更新 `prompts/VERSIONS.md`、`SELECTED_VERSIONS.md` 和 `FIGURE_SET_PLAN.md`。未生成新图片，未修改论文实验和方法结果。


### 2026-10-05 — shared methodology review: joint reliability candidate

- 目的：阅读用户提供的共享讨论，检查它是否能把当前 MIRA 的问题、机制和实验重新收紧。
- 已完成：读取并解析共享对话；核心候选是从 provider 边际可靠性推进到 paired-observation 的联合错误模型，并用同一模型计算当前后验、追加调用价值和接受/拒答损失。
- 判断：这是比单纯 family residual 更强的候选机制，但仍未证明可识别、可校准或优于严格 baseline；不改变生效三份主线文档、题目、当前方法身份和三门评分。
- 留痕：`research/external_review_chatgpt_share_20261005.md`。
- 硬闸门：配对数据资格、latent mode 非退化、holdout joint NLL/Brier/calibration、两次调用有限视界 planner 的 read-set/release 单测、Frozen Joint/Independent Evidence/Confidence Cascade matched controls。
- 研究边界：一次调用 direct selection 继续作为 primary benchmark；两次调用只作为 sequential evidence extension，不能静默替换主任务；不提交新的 A800。
- 下一步：先完成 CPU-only 资格审查和合成可识别性测试；只有候选通过后，才讨论是否更新方法文档和实验矩阵。


### 2026-10-05 — joint observation eligibility audit

- 目的：验证共享讨论提出的 paired-observation joint reliability 是否有合法的 fit-only 数据基础。
- 结果：固定宽度 722×33 记录通过；仅 178/722 行 33 个 provider 全部成功，544 行至少一个运行失败，3,735 个 provider-level failure cells；528 个 pair 的共同成功数为 349–648，中位数 560；当前输入没有 confidence 字段。
- 判定：complete-case joint fit 阻塞；带显式 missing/runtime-failure 状态的模型仍是候选。历史 cache 不能被写成在线 paired-call 成本或 receipt。
- 留痕：`src/probe_headless/audit_joint_observation_eligibility.py`、`task/task49-pharma-benchmark-baseline-freeze/joint_observation_eligibility_audit_v1/{report.md,result.json}`。
- 独立方法审查：保留 E2a 一次调用为 primary；H=2 只能作为 sequential extension；需要 Independent Evidence、同容量 pairwise control、matched fixed cascade、net utility/拒答成本和 controlled shift gates。
- 下一步：完成 synthetic same-marginal/different-joint 与 held-out pair likelihood CPU gate；不提交 A800、不改生效方法文档。

### 2026-10-05 — joint observation synthetic gate

- 目的：在真实 BBB 之前验证联合观测模型是否能区分相同 provider 边际、不同联合错误结构，并保留 runtime failure 语义。
- 结果：合成 gate 通过。same-marginal/different-joint 场景中 Joint 相对 Independent 的 held-out conditional NLL/Brier/accuracy 差为 `-0.3722/-0.1616/+0.3976`；显式 `⊥` failure state 场景仍保持第三类观测，差为 `-0.0505/-0.0234/+0.1121`。
- 边界：这是合成预测可行性，不是真实 BBB 效果、policy utility 或 online gain；不改变 MIRA-Relay 生效规格，不替换 E2a primary，不提交 A800。
- 留痕：`task/task49-pharma-benchmark-baseline-freeze/joint_observation_synthetic_gate_v1/{preregistration.md,report.md,result.json}`、`src/probe_headless/joint_observation_synthetic_gate.py`。
- 下一步：真实 BBB scaffold-disjoint held-out pair likelihood/NLL/Brier/ECE，加入 Independent、family-only、同容量 pairwise control，并先审查静态 cache 的合法性和 provider/failure 覆盖。

### 2026-10-05 — real BBB joint observation prediction gate

- 目的：在 5 个 scaffold-disjoint fold 上，用 fit-only panel 比较 Independent 与 2-mode Joint Evidence；error provider 作为显式 `⊥` 观测。
- 结果：full-panel conditional NLL 在 5/5 folds 改善，宏平均 `-3.8502`；528-pair 平均 NLL 在 5/5 folds 改善，`-0.0716`。但 full-panel posterior Brier `+0.0185`、ECE `+0.0211`，pair Brier `+0.0006`，因此 likelihood 信号不能直接解释为可靠 posterior 或 utility。
- 判定：结构预测 gate 通过，policy gate 未通过/尚未测试；不能直接实现 H=2 planner，不改变 MIRA-Relay、E2a primary 或当前 claim。
- 留痕：`task/task49-pharma-benchmark-baseline-freeze/joint_observation_real_panel_gate_v1/{preregistration.md,report.md,result.json}`、`src/probe_headless/joint_observation_real_panel_gate.py`。
- 下一步：family-only、同容量 pairwise conditional、fit-only calibration 和 pair/failure 分层；通过后再做 exact finite-horizon planner 的 matched CPU replay。

### 2026-10-05 — TopConf gallery round-2 and modular prompt v05

- 针对用户指出的“文字太多、模块太少、叉号缺乏美感”，实际查看本地 gallery 的 CRITIC、CRAFT、GPT4Tools、GenAL、CollabLLM、DARS、TableDART、AgentOccam 和 ShieldAgent 等 framework/pipeline/architecture 图。
- 归纳结果：Figure 1 改为模块化横向架构图，显示输入、双分支 selector、provider cards、一次调用、terminal/judge、pending queue、relay update；Figure 2 改为三泳道五时间列，显示 state、execution/judge、update/visibility。
- 新增 `paper/aamas2027/figures/reference_audit_20261005_round2.md`，记录样例、可迁移规则和改图原则。
- 新增并选定 `prompts/figure1/v05.md/.txt`、`prompts/figure2/v05.md/.txt`；v04 保留。提示词明确禁止文本墙、巨大叉号、长段落、第二次调用和未选输出泄漏。
- 同步更新 prompt 版本索引、选定版本和图组计划；未生成新图片，未修改实验结果或论文 claim。

### 2026-10-05 — v05 生成图复核与 prompt v06

- 用户提供 v05 生成图后完成实际视觉复核。Figure 1 已经具备模块架构感，但输入区提前出现 `selected provider`，与 selector 输出重复；runtime failure 又连到 diagnostic ledger，语义混淆。
- Figure 2 的三泳道五列结构基本成立；唯一主要问题是 `fallback (e.g., failure)` 把 fallback 和记账/运行失败混为一谈，chemical correctness 也应标明 valid return。
- 复核记录：`paper/aamas2027/figures/external_review/20261005_v05_generation_review.md`。
- 新增并选定 `prompts/figure1/v06.md/.txt`、`prompts/figure2/v06.md/.txt`；v05 保留。未生成新图片，未修改论文正文、实验结果或 claim。


### 2026-10-05 — capability-ceiling story alignment

- 用户明确纠正：总目标不是做一个 provider selector 或联合可靠性模型，而是通过让 agent 更聪明地调用工具提升能力上限。
- 新主线：agent 根据工具历史工作范围选择首个工具；把返回结果当作证据；决定验证、互补、直接回答、拒答或继续调用；用 delayed outcome 更新后续工作范围。
- 主任务边界：E2a 一次调用继续是 primary；两次/多次调用是 sequential evidence extension；首次选择、证据决策、持续适应和最终任务能力分层报告。
- 已落档：`docs/决议/0009-工具调用能力上限作为总故事.md`；故事线生效版本升级为 `docs/研究主线/生效/01-故事线与创新.md` v004，旧 v003 归档于 `docs/研究主线/版本/故事线与创新/v003.md`。
- 方法影响：联合可靠性模型是支撑组件，不是总目标；当前 MIRA-Relay、Joint candidate、评分和 confirmation 状态不自动改变。
- 下一步：同步方法论和 Benchmark+Baseline 的递进能力层级与 sequential extension 实验合同，再回到 calibration、同容量 controls 和 finite-horizon planner。


### 2026-10-05 — MIRA v1 implementation contract

- 目的：把“提升 agent 工具调用能力上限”的总故事落成可实现方法，不把联合模型当成总目标。
- 方法规格：长期工具工作范围状态 $L_t$、当前任务证据状态 $E_t$、预算 $b_t$、pending/release 时钟；动作 `CALL(a) / ACCEPT / REFUSE`，验证/互补是同一 call 的事后解释。
- 计算：显式 `⊥` failure state；fit-only endpoint prior + provider outcome kernel；$K=2$ joint candidate；有限视界 $h\le1$ exact VOI enumeration；selected feedback release 后更新 scope/hazard，paired audit 才更新关系模型。
- 文档：`docs/研究主线/生效/02-方法论.md` v003；旧 v002 归档于 `docs/研究主线/版本/方法论/v002.md`，新版本源文件为 `v003.md`。
- 验收前置：同容量 Independent Evidence、family-only、pairwise conditional、Frozen Joint、fixed cascade、no-prior/direct-property control；主估计量为同预算 end-to-end utility，同时报告 wrong/refusal/calls/calibration。
- 下一步：先写 CPU planner contract 与 independent verifier；通过 read-set、exact enumeration、failure/unknown、恢复一致性和 cost semantics 后，再进行 matched replay。

### 2026-10-05 — JEV + bandit + online adapter integration review

- 用户要求确认方法能否干净融合 JEV 判别式模型、多臂老虎机和实时轻量训练。结论：可以表达目标，但必须分层；JEV 负责预测，bandit 负责 provider 选择和探索，VOI planner 负责 accept/refuse/call，online adapter 负责 release 后更新。
- 关键边界：拒答不是普通 arm；当前任务 posterior 可以即时更新，跨任务参数只能在 delayed selected feedback release 后更新；provider relation/JEV joint component 只有 paired audit 才能在线更新。
- 文档：`research/mira_jev_bandit_integration_20261005.md`。不改变方法名、主 benchmark 或当前 claim。
- 下一步：实现 JEV adapter 输出均值/不确定度/failure risk，接入 delayed contextual bandit H=0，再实现 H=1 exact VOI planner 和 matched controls。

### 2026-10-05 — label contract closure

- 用户指出方法没有明确训练 label。已补齐四类标签：独立 truth `y`、provider output `o`、runtime status `s`、化学 correctness `r_chem`；另分开 runtime label `r_run` 和 episode-level utility/reward。
- 规则：`r_chem=1[o=y]` 只在 `status=ok` 时存在；error 的 `r_chem=null`，不能当化学错误 0。JEV correctness head 学 `P(r_chem=1|h,E,a)`，answer head 学 `P(y|h,E)`，runtime head 学 `P(r_run=1|h,E,a)`；bandit 只在 release 后收到 selected reward。
- 留痕：`docs/研究主线/生效/02-方法论.md` v003、`task/task49-pharma-benchmark-baseline-freeze/mira_capability_v1/implementation_card.md`、`research/mira_jev_bandit_integration_20261005.md`。
- 下一步：先用 `fingerprint_diagnostic_v1/inputs.json` 的 `truth/output_labels/output_statuses/system_success` 生成 label receipt，再实现 JEV correctness/runtime 两个 head 的 CPU smoke。

### 2026-10-05 — Figure 2 v07 no-grid rework

- 用户复核 v06 生图后指出：第二张图横竖格线过重、文字多而模块流弱，要求改成更接近 TopConf 样例的 fancy 架构图。
- 实际复核补充发现：`selected outcome` 被画在 terminal/judge 之前，judge/chemical correctness 被放进 pending/release 列，固定 `S_t` 轨道也容易误读；这些是时序语义问题，不只是审美问题。
- 依据本地 gallery 的 CollabLLM、TableDART、CRITIC、GenAL 像素样例，新增 `paper/aamas2027/figures/prompts/figure2/v07.md/.txt`：改为无横竖网格的横向 event spine + 紫色 delayed-feedback ribbon，保留 selected-only、`τ_t=t+1+D`、runtime/chemical/fallback 边界。
- v06 保留；同步更新 `prompts/VERSIONS.md`、`SELECTED_VERSIONS.md`、`FIGURE_SET_PLAN.md`；复核记录为 `figures/external_review/20261005_v06_generation_review.md`。
- 未生成新图，未改变论文正式 Figure 2 PDF、实验、方法 claim。下一步是用户用 v07 生图后，再做一轮语义与可读性复核。

### 2026-10-05 — Figure 2 v08 judge-gated rework

- 用户提供 v07 生图。视觉上已解决横竖网格和表格感，event spine、延迟带和模块层次成立。
- 发现一个关键语义错误：`terminal record` 直接连到 `selected outcome` 和 `chemical correctness`，绕过 frozen endpoint judge；`system failure`、chemical correctness 与 selected outcome 也没有显式终态汇合。
- 新增 `prompts/figure2/v08.md/.txt`：严格要求 `terminal record → frozen endpoint judge → chemical correctness`，runtime failure 走独立分支；两者先汇入小的 `terminal outcome`，再进入 selected-only queue；移除 Figure 2 的 fallback/diagnostic card，避免与 Figure 1 重复。
- 留痕：`figures/external_review/20261005_v07_generation_review.md`；v07 保留，版本索引和选定提示词同步到 v08。
- 未修改正式 PDF、实验、方法 claim。下一步是使用 v08 生图后再次检查箭头语义和最终字号。

### 2026-10-05 — Figure 2 v09 compact-chain rework

- 用户提供 v08 生图并指出：整体太散、留白过多、不够紧凑。
- 复核发现除紧凑性外还有一处结构错误：`terminal outcome` 直接落到 `released outcome`，跳过 `selected outcome` 和 pending queue。
- 新增 `prompts/figure2/v09.md/.txt`：画布压缩到 1600×560，改为两个靠近的 band，明确强制 `terminal outcome → selected outcome → selected-only queue → released outcome`，并禁止 shortcut/bypass。
- 留痕：`figures/external_review/20261005_v08_generation_review.md`；v08 保留，版本索引、选定提示词和图组计划同步到 v09。
- 未修改正式 PDF、实验、方法 claim。下一步是使用 v09 生图后检查空间密度、反馈链完整性和最终字号。

### 2026-10-05 — Figure 2 v09 generation accepted as redraw draft

- 用户提供 v09 生图。布局已明显压紧，空白减少；完整反馈链和 judge-gated terminal outcome 均通过复核。
- 判定：v09 是当前最优 AI 视觉草图，可进入正式 SVG/HTML/PDF 重绘；不再为小的美术问题继续新增 prompt 版本。
- 正式重绘局部规范：减轻底部 future-state 回路线；左侧 unselected-output ghost 移远或删除；chemical correctness 在 caption 中注明 valid return only；单栏尺寸复核字号。
- 留痕：`figures/external_review/20261005_v09_generation_review.md`。正式 Figure 2 PDF 尚未替换，实验、方法和 claim 未改变。

### 2026-10-05 — Figure 2 editable redraw v10/v11

- 按 v09 AI 草图完成可编辑 SVG/HTML/PDF 重绘：`figures/figure2/v10/`。渲染检查发现 ghost 卡片靠近回路、pending marker 挤到 system failure 下方，v10 保留为中间版本。
- 修订为 `figures/figure2/v11/`：删除 Figure 2 中重复且易误读的 unselected-output ghost，恢复简短 `event hidden until τ_t`，保留完整 judge-gated release chain。
- v11 PNG/PDF 已渲染并通过结构检查；候选评分 16/16，但它是宽幅 1800×620，当前 `main.tex` 仍使用单栏 v9，尚未替换正文。
- 留痕：`figure2/v11/REVIEW.md`、`figure2/VERSIONS.md` 和 `FIGURE_REVIEW.md`。下一步是决定是否将 v11 作为跨栏 Figure 2 嵌入论文，并做一次整篇 PDF 排版复核。

### 2026-10-05 — AI generated figures decision

- 用户明确要求：论文正文使用 AI 生成图，人工编辑的 SVG/PDF 不作为正式图；AI 的整体风格和构图优于人工重绘。
- 生效决议：`docs/决议/0010-AI生成图作为正式视觉资产.md`。后续每版必须保存 prompt、原始生成图和审查记录；内容错误只能通过 prompt 迭代解决。
- 已从用户提供的临时文件保存 Figure 2 AI 原图：`figures/generated/figure2/v07_ai.png`、`v08_ai.png`、`v09_ai.png`；当前视觉候选为 v09。
- `figure2/v10/`、`figure2/v11/` 保留为内部语义/版式参考，降级为非正式资产；同步修正 `FIGURE_SET_PLAN.md`、`FIGURE_REVIEW.md`、`SELECTED_VERSIONS.md` 和当前 v09 prompt 的资产政策。

### 2026-10-05 — label semantics correction and receipt closure

- 用户追问训练 label。回到原始构造代码确认：`truth` 是独立 endpoint truth；`output_labels` 是 provider 输出；`output_statuses` 的真实词表是 `ok/error`；冻结输入的 `system_success` 是端到端成功 `1[status=ok 且 output=truth]`，不是 runtime success。
- 第一次 receipt 生成误把 `system_success` 当 runtime label，得到 `wrong=0`；该尝试未进入训练或结果，保留于 `pharma_label_contract_v1/invalid_attempt.md` 并作废。
- 正式 receipt 已按 raw status 重建并通过逐格不变量：23,826 cells；`r_run=1` 为 23,682、`r_run=0` 为 144；runtime-ok 的 `r_chem=1` 为 20,091、`r_chem=0` 为 3,591；error 的 `r_chem=null` 为 144。机器产物：`task/task49-pharma-benchmark-baseline-freeze/pharma_label_contract_v1/{labels.jsonl,summary.json,report.md}`。
- 资格审计同步纠正：6 个 molecule 含 144 个 raw error cells，716/722 行全 provider runtime-ok；528 pair 的 runtime co-success 为 716–722。旧 `joint_observation_eligibility_audit_v1` 已加 `INVALID_SEMANTICS.md`，正式结果为 `joint_observation_eligibility_audit_v2/`。
- 更新 `fingerprint_diagnostic_v1/report.md` 与外部讨论笔记，明确旧 static diagnostic 学的是端到端 `r_sys`，不能冒充新 MIRA 的 `r_chem` 或 `r_run` 头；Joint real-panel gate 本身按 raw status 编码 `⊥`，结果仍只作 predictive gate。
- 修复两个 MIRA-Relay legacy no-fit-state 分支只匹配 `status == "failure"` 的 bug，统一按 `status != "ok"` 计 runtime failure；旧 fit diagnostic 仍因未收敛不能作 action/hazard 证据，说明写入 `mira_relay_v1/fit_diagnostic_semantic_followup.md`。
- 验证：label receipt invariants、audit invariants、四个脚本 `py_compile`、real-panel joint gate recheck 均通过；未重训、未提交 A800。
- 下一步：用正式 receipt 实现 JEV correctness/runtime 双头 CPU smoke，并将 `r_sys` episode utility 与 `r_chem/r_run` 训练监督分开。

### 2026-10-05 — JEV label-interface CPU smoke

- 目的：把正式 receipt 接入 JEV correctness/runtime 双头，验证 `r_chem` 的 null mask 和 `r_run` 的全量监督，不做选择效果声明。
- 协议：fold0 fit 444 molecules × 33 providers；冻结 Morgan1024 + provider one-hot，32维 ReLU shared encoder，两个 BCE heads，5 epochs，seed20261005；evaluation read-set 禁止 truth、raw output 和未选 provider 当前输出。
- 结果：receipt 对齐和逐格核对通过；fit `r_chem=1/0/null=12,550/1,958/144`，`r_run=1/0=14,508/144`；train total `1.5096→1.3899`，validation correctness `0.7234→0.6776`，runtime `0.7495→0.6705`，均有限且下降。
- 判定：label interface gate 通过；这不是 provider selection、VOI、bandit regret 或 online adaptation 效果实验，不申请 A800。
- 留痕：`src/probe_headless/jev_label_smoke.py`、`src/probe_headless/tests/test_pharma_label_receipt.py`、`task/task49-pharma-benchmark-baseline-freeze/pharma_jev_label_smoke_v2/{preregistration.md,report.md,result.json,evaluation_predictions.jsonl}`。
- 下一步：在同一 receipt/读集合同上实现 H=0 JEV selector CPU replay，先对 Frozen/Independent/Best Fixed 做 matched smoke，再决定是否进入 H=1 VOI planner。

### 2026-10-05 — AAMAS paper skeleton, result matrix, and page-budget closure

- 目的：在实验结果尚未闭合时先把论文写到“只剩真实结果可填”，避免临近截稿时再重排故事线、方法和表格。
- 官方要求：AAMAS 2027 主轨正文最多 8 页，参考文献页另计；官方 LaTeX 模板强制使用。来源与本地缓存：`paper/aamas2027/reference/official/submission_instructions.html`，官网 `https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/instructions/`。
- 改动：`paper/aamas2027/main.tex` 完成能力上限故事、明确 `r_run/r_chem/r_sys` 标签合同、MIRA-Relay 的 evidence/accept-refuse-call 接口、E1--E6 矩阵、主比较表、机制表和 one-step evidence 表；所有最终效果仍为 `TBD` 或 `[RESULT]`，没有把开发诊断冒充主结果。
- 回填规范：新增 `paper/aamas2027/EXPERIMENT_RESULT_FILL_MAP.md`，把摘要、每张表、成本分析和 confirmation 占位符绑定到 raw receipt、独立 recovery、分母和 CI 门禁；同步更新 `INTERNAL_PRE_RESULTS_REVIEW.md`、`WRITER_REPORT.md`、`AAMAS_2027_FORMAT_AUDIT.md`。
- 排版证据：隔离目录 `paper/aamas2027/build/review_20261005_v4/main.pdf`，`pdfinfo` 为 8 页，SHA-256 `502295c720f9102faa8912fdae23e028ef36a0f97d78cbbda04fe050ebe52170`；无 undefined citation/reference 或 overfull box，保留 underfull 表格提示和模板 `\\ifx` 条件警告。
- 判定：纸面结构和 8 页预算已闭合；科学投稿门禁仍未通过。未完成项是 MIRA-Relay stop test、完整 baseline/fairness receipt、E1--E5 实际结果与 E6 冻结 confirmation。
- 下一步：按填充映射先完成 H=0 selector CPU replay 和独立 recovery，再决定是否提交一次 Nebula A800 任务；结果写回前不改 claim、不填预期数值。

### 2026-10-05 — new architecture figures integrated into paper

- 目的：用用户提供的新架构图统一正文的故事线与方法论，把“能力上限—选择—终端判别—延迟释放—下一次行动”画成同一个可审计闭环。
- 新图：`paper/aamas2027/figures/figure1/v07/figure1_v07.png`（MIRA-Relay 四区域总架构）和 `paper/aamas2027/figures/figure2/v12/figure2_v12.png`（决策到反馈的终端流程）。原始文件、尺寸、哈希和语义合同已分别记录在 `MANIFEST.md`；历史 v1–v11 保留不覆盖。
- 正文：`main.tex` 已将 Figure 1/2 作为同一任务合同的两个尺度；图注和 `\\Description{}` 明确 fit-only reference、family residual/provider bias、runtime failure、frozen endpoint judge、chemical correctness、selected-only queue、`\\tau_t=t+1+D` 和 $D=0$ 的可见边界。
- 排版：隔离构建 `paper/aamas2027/build/review_20261005_figures_v2/main.pdf`，8 页；SHA-256 `327cfa1b69ebc641207bfaedfe2afa8c2807f625a667abaeb337f8d5d5a435d9`。页面 3/5 已做 100% 视觉检查，图中文字可读；日志没有 overfull box，只有矩阵窄列 underfull 和模板条件警告。
- 文档：更新 `figures/FIGURE_REVIEW.md`、`FIGURE_SET_PLAN.md`、`SELECTED_VERSIONS.md`、figure version indexes、`AAMAS_2027_FORMAT_AUDIT.md` 和 `WRITER_REPORT.md`。
- 判定：新图与当前故事线和方法合同一致，8 页预算未改变；科学结果仍未填入，不能据此声称方法有效。
- 下一步：继续按三份验收文档推进 H=0 同读集回放和独立 recovery，图和正文结构保持冻结，避免在结果阶段重新改故事线。

### 2026-10-05 — H=0 双标签 JEV 选择器 CPU 回放

- 目的：把正式 `r_chem`/`r_run` label contract 接到最小冷启动选择器，检验两个标签头的概率乘积是否能在一次调用任务上安全替换强固定 provider。该实验不包含 bandit、延迟更新、VOI、拒答或新 API 调用。
- 固定协议：`fingerprint_diagnostic_v1/inputs.json` 的 722 个 BBB development 分子、33 provider、5 个 scaffold-disjoint folds；Morgan-1024，shared ReLU-64，correctness/runtime 两个 33-way head，AdamW `1e-3/1e-2`，200 epochs，seed slots 0/1，validation 选 checkpoint。
- 结果：10 个 fold/seed 槽位均完成并通过独立 NumPy recovery。H=0 双头 selector OOF mean `0.887883`，同折 Best Fixed `0.915575`，差 `-2.769 pp`；逐槽位均为负。已有同容量 direct `r_sys` MLP 两 seed 均值约 `0.89335`，双头还低约 `0.55 pp`。
- 证据：`task/task49-pharma-benchmark-baseline-freeze/jev_selector_h0_v1/{preregistration.md,report.md}`；原始日志与 checkpoint 在 `experiments/logs/task49_jev_selector_h0_20261005/v1/`；`verification.json` 状态 `verified`。输入 SHA `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 证据修正：运行前配置的 source hash 曾对应输出目录幂等性 patch；原配置保存在 `config_preflight_unsealed.json`，实际执行 hash 已写回 `config.json`，并保存 `config_correction.json`。patch 不改变训练/评价计算，结果未重新训练但已重新独立核验。
- 裁定：预登记否证条件成立；不能把未经校准的双头乘积 argmax 作为主 cold-start selector，也不申请 A800。保留两个 head 作为 incumbent-relative 校准证据和 runtime hazard 输入；下一候选必须通过 incumbent-safe switch，再接 selected-only delayed residual/ledger，并与 Frozen、Matched Online-SGD、Best Fixed 同表比较。


### 2026-10-06 — role-qualified 主表与 E1 exact control join

- 目的：按 E5 → E2 → E1 的顺序关闭统一分母、角色分层和控制连接缺口；只读聚合，不重跑策略、不读取 confirmation、不申请 GPU。
- 实现：`src/probe_headless/build_role_qualified_main_table_v1.py` 从四份已核验 receipt/input 生成 `task/task49-pharma-benchmark-baseline-freeze/role_qualified_main_table_v1/{result.json,control_join.json,report.md}`。
- 结果：430 条 stream/control rows，16 个 scope-method aggregates；Best Fixed/Oracle 使用同一 fold evaluation 行；5 folds × 2 controls 的 10 个 exact join checks 全部 PASS（Best Fixed `0.915512`，Oracle `0.997230`）。no-event、controlled-event、family-outage 分开保留。
- 语义：unknown/refused 的观测零被保留且标记为无拒答效用结论；Random 同 schedule、paired scaffold uncertainty、统一 state/cost/refusal、E3 完整机制表仍显式缺失。
- 验证：脚本运行成功；结果 SHA 记录在 `result.json`；无 confirmation 读取、无新模型预测。
- 闸门：Gate A=`11/16`、B=`11/16`、C=`10/16` 仍 HOLD，分数不因证据整理上调。
- 下一步：在本表固定的普通任务分母和角色合同上做一次最小 transient/persistent failure mechanism diagnostic；先形成 E3 paired 表，再重新审计 E1–E5，仍不直接开 A800。


### 2026-10-06 — E3 机制消融矩阵收口

- 目的：把已有 MIRA veto ablation 收据纳入 E3 统一审计，不重复跑 no-event，不读取 confirmation，不申请 A800。
- 实现：`src/probe_headless/build_mechanism_ablation_matrix_v1.py` 读取已核验的 config/summary/analysis/verification，生成 `task/task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v1/{result.json,report.md}`。
- 结果：300 streams、43,320 decisions/terminals/feedback；同 schedule 覆盖 Frozen、streak-only、beta-only、full MIRA、Matched Online-SGD。runtime-failure streak 是主要恢复通道；beta-only 无法处理 runtime failure且有 no-event 损失；full 三个 delay 均低于 Matched。
- 闭合：E3 full、remove-correctness-gate、remove-runtime-channel、remove-state 四个子轴；interface-null、independent-residual、family-permutation、paired scaffold CI 仍缺。
- 闸门：Gate A=`11/16`、B=`11/16`、C=`10/16` 仍 HOLD；只更新缺口定位，不上调分数、不改最终 claim。
- 下一步：补最小 interface-null、independent-residual、family-permutation 同 schedule CPU 表；通过后再重新审计 E1–E5，暂不申请 A800。


### 2026-10-06 — E3 mechanism coverage v2

- 目的：把新增的 family hazard state 接入机制去除矩阵，避免把 FamilyGuard 与 relative 的差异留在叙述里。
- 实现：`src/probe_headless/build_mechanism_ablation_matrix_v2.py` 连接已核验的 veto ablation（300 streams）和 common-stratum FamilyGuard/relative receipt（60 streams），不跨 scope 排名。
- 结果：correctness gate、runtime-failure ledger、全状态去除、family hazard state 和 strict residual 最近邻均有对照；FamilyGuard/relative 的 state bytes、hash、failure、task success 保留。family permutation 仍标为可选 sensitivity，不冒充通过。
- 闸门影响：E3 从“机制去除不完整”收敛为“核心去除已覆盖、仍有边界敏感性和统一 CI 待补”；Gate C 仍 `10/16 HOLD`，因为 E1 Random exact stream、E2/E5 统一字段、paired uncertainty、E6 未闭合。
- 下一步：重新计算三份验收标准的机器评分；若 E3 不再是主阻塞，转向 E1 Random exact stream 与 E5 unified cost/unknown/refusal 表。


### 2026-10-06 — E1 Random exact join

- 目的：补齐 E1 的 Random 主控制，不用解析期望值冒充策略收据。
- 实现：把已核验的 `experiments/logs/task49_random_development_20260930/run_v1/` 接入 `role_qualified_main_table_v1`；每个 fold 的 Best Fixed/Oracle 与 common-stratum 48 条、Random 6 条同 evaluation 分母连接。
- 结果：5 folds × 2 controls 的 10 个 exact join checks 全部 PASS；Random pooled accuracy `0.844875`，Best Fixed `0.915512`，Oracle `0.997230`。
- 缺口：Random selection/update timing p95、统一全 baseline cost/state/unknown/refusal、paired scaffold uncertainty、E6 仍未闭合。
- 闸门：E1 控制缺口已补，但 Gate C 暂不改分数，仍 `10/16 HOLD`。
- 下一步：把 post-call utility 与 common failure/refusal accounting 接成统一 E5 表，再重新按三份标准评分。


### 2026-10-06 — E5 unified accounting closure

- 目的：把 online、Random 和 post-call utility 的错误/成本/拒答字段统一，先解决分母和语义不一致。
- 实现：`src/probe_headless/build_e5_unified_accounting_v1.py` 只读聚合五份已核验 receipt，生成 `task/task49-pharma-benchmark-baseline-freeze/e5_unified_accounting_v1/{result.json,report.md}`。
- 结果：17 行 scope-method/policy 账本；runtime failure、unknown、refused、attempts/calls、terminal、feedback、valid return、chemical correctness 分开记录；post-call policy/evaluator 与 online routing 明确隔离。
- 闸门：E5 accounting 子项已收口，但 unknown/refusal utility、全 baseline cost/state、Random timing、paired scaffold CI 仍缺；Gate C 仍 `10/16 HOLD`。
- 下一步：在 strict common stratum 上生成 paired scaffold CI 和统一 cost/state 表，完成后再评估 C4 是否达到 3。


### 2026-10-06 — paired uncertainty matrix

- 目的：补 C3/C4 的 paired uncertainty，不自己重新抽样，不把不同 scope 的区间混成一个总榜。
- 实现：`src/probe_headless/build_paired_uncertainty_matrix_v1.py` 只读收集 common-stratum bootstrap、FamilyGuard persistent event summary 和 post-call utility bootstrap，生成 `task/task49-pharma-benchmark-baseline-freeze/paired_uncertainty_matrix_v1/{result.json,report.md}`。
- 结果：15 行预计算区间，保留 estimand、单位、分母和 seed；Matched−Frozen、MIRA-relative−Frozen、FamilyGuard−Frozen 的区间均可追溯；Random CI 和没有发布区间的 baseline 仍缺。
- 闸门：C3/C4 的不确定性入口已补，但统一 cost/state/unknown/refusal 和 E6 仍缺，Gate C 暂不改分数。
- 下一步：把 role-qualified 主表、E5 accounting、paired uncertainty 合成最终 E1–E5 coverage matrix，按标准重新评分。

### 2026-10-06 — provider-fresh confirmation and dynamic-drift gate

- 目的：解决最终科学门中最硬的 provider-fresh / E6 缺口；不把固定 33-provider 旧缓存冒充 model-fresh confirmation。
- 先冻结合同：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_confirmation_v1/preregistration.md`。固定四个 checkpoint-bound provider、157 个 canonical/scaffold-disjoint BBB candidate、5 folds × 2 seeds × delay `{0,1,4}`、Random/Best Fixed/Frozen/Matched SGD/LinUCB/BayesianRouter/MIRA/Oracle、selected-only release 和停止条件。
- 自然 fresh confirmation 已完成：`experiments/logs/task49_provider_fresh_confirmation_20261006/v1/`，240 streams、113,040 events；Frozen `89.81%`，旧 MIRA-Relative `89.81%`，paired delta `0.00 pp`，CI `[0.00,0.00]`；Random `83.76%`，Oracle `90.45%`。独立回放 `verification.json` 为 `verified`，240/240，无决策 truth/output 泄漏，无 replay mismatch。
- 结论：全局 reliability factor 在平稳 fresh menu 中不会改变动作，预登记自然增益门不通过；这个负结果保留，停止该版本，不调阈值、不删 provider、不改分母。
- 新候选：`MIRA-Context-Reliability`，冷启动严格退化为 Frozen，selected release 后用 provider 相对 fit-rate 的 Beta residual 更新 molecule-conditioned score；实现与合同见 `src/probe_headless/run_provider_fresh_confirmation_v1.py`、`task/.../mira_context_reliability_v1/`。
- controlled correctness-drift stress 已完成：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v1/`，中点后翻转 fit-best selected label，120 streams；MIRA `87.56%` vs Frozen `73.38%`，+`14.18 pp`，scaffold CI `[+9.61,+19.03]` pp；独立回放 120/120 verified。该结果是受控机制证据，不写成自然漂移泛化。
- 新 gate 文档：`task/task49-pharma-benchmark-baseline-freeze/stage_b_scientific_submission_gate_v1/{scorecard.json,report.md}`；在“一次调用 + fresh BBB menu + selected-only delayed feedback + controlled correctness drift”范围内 A=`15/16`、B=`13/16`、C=`14/16`，`PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE`。post-call verifier/Agent、hERG/AMES、full 33-provider model-fresh 仍是边界。
- 新代码已 `py_compile`；聚合脚本与自然/压力报告已保存。尚未提交 git；下一步是把结果回填到论文主表/摘要、跑 8 页排版和三份验收文档的联合复核，再提交一组只包含本轮文件的 commit。

### 2026-10-06 — AAMAS 2027 page-limit confirmation

- 核对 AAMAS 2027 官方 Submission Instructions、Reviewer Guidelines 和 Q&A：Main Track PDF 正文最多 8 页，参考文献页数另计；插入主 PDF 的 appendix 计入 8 页，不能借 appendix 增页；可选 supplementary zip 不超过 25 MB，审稿人不保证查看。
- 官方来源：`https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/instructions/`、`https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/qa/`。

### 2026-10-06 — 当前正式论文中文版

- 目的：给出一份与当前正式英文稿同步的中文阅读版，便于逐段审查故事线、方法、实验矩阵和边界；不改变英文稿的 claim、数字或实验状态。
- 产物：`paper/aamas2027/main_zh.md`，包含摘要、引言、问题合同、MIRA-Relay 方法、benchmark/baseline、实验矩阵、结果、成本与错误分析、局限、可复现性、相关工作和结论；图 1/2 以当前正式图路径嵌入，公式使用 `$`/`$$`。
- 对齐：正文逐段对应 `paper/aamas2027/main.tex`；记录英文源码 SHA-256=`cc13999f243f69de2374980b6b0a0c4536e021f49625e8ab0051de905a1c9259`。引用键沿用 `references.bib`，中文文件明确标注为阅读版而非投稿文件。
- 文档索引：已在 `paper/aamas2027/README.md` 加入 `main_zh.md` 入口。
- 检查：Markdown 公式围栏、代码反引号和 `git diff --check` 均通过；未运行实验，也未引入新结果。
- 下一步：先由作者通读中文稿，指出需要重写的叙事段落或术语；英文稿后续若有结构/数字变化，需按源码 hash 重新生成中文同步版。

### 2026-10-06 — state-hash rerun、正文排版与科学门收口

- 目的：在不扩张 claim 的前提下，完成 MIRA-Context 状态可审计性修复、自然/受控流复核、正文 8 页排版和提交前文档同步。
- 代码：`src/probe_headless/run_provider_fresh_confirmation_v1.py` 的状态序列化现在包含 `fit_rates`、`successes`、`counts`、`prior_count`；变更已记录在 commit `f48e1d45`（待本轮合并推送）。
- 自然 fresh rerun：`experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/`，270 streams、127,170 events，contract/replay 270/270；MIRA-Context 与 Frozen 均 89.81%，paired delta 0.00 pp，CI `[-1.24,+1.40]`；events SHA-256=`69598343f9254ff9ea1b46a7ae5c3fb41acbf3aed82333aebe1447780d488aab`。
- 受控 correctness-drift rerun：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v2_statehash_run2/`，120 streams，contract/replay 120/120；MIRA-Context 87.56% vs Frozen 73.38%，+14.18 pp，CI `[+9.61,+19.03]`；events SHA-256=`4cac68d3a5c3b262da2d05e93c7651386980a0727f70da38e87f225e33f8902e`。该结果仍是 evaluator-side 受控机制证据，不写成自然部署漂移。
- 科学门：`task/task49-pharma-benchmark-baseline-freeze/stage_b_scientific_submission_gate_v3/` 保持 `PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE`；验证器表述已收窄为 contract/replay，未声称第二套独立实现；E6 范围外仍包括 post-call verifier/refusal、hERG/AMES、完整 33-provider model-fresh 和自然 lifecycle drift。
- 论文：`paper/aamas2027/build/review_20261006_v16/main.pdf` 为 9 页总计（正文正好 8 页，参考文献第 9 页），SHA-256=`6de7eb6673c2899f03fe44a95b87f8313384107ad1c8ffe5cd18be1e6f5a50f3`；最终日志无 `Overfull`、`Undefined`、`Missing $`、fatal 或 emergency。第 2、8 页已目视检查；旧 v07/v12 图和中断的 v14 PDF 均保留，不作为当前版本。
- 文档：同步更新 `paper/aamas2027/README.md`、`WRITER_REPORT.md`、`AAMAS_2027_FORMAT_AUDIT.md`、`main_zh.md`；中文阅读版源码 hash 已更新为 `67e24e3e89f25209b942532d70ed6a68f1f6c4e1c95742e83f93c02d7a3203df`。
- 验证：`py_compile` 通过；结构化 CPU/online baseline 测试 `38 passed`；v14 编译中断产生的损坏 PDF 已明确排除。
- 未完成：作者最终双盲/官网复核；本轮相关文件需做一次限定范围 commit 并推送。不得把声明范围外的 extension 写成已完成结果。

### 2026-10-06 — 长程目标复核、主线文档同步与干净构建

- 目标：继续推进到论文可提交状态，正文保持恰好 8 页，并让故事线、方法、benchmark、baseline 和三份评价标准与论文同口径。
- 文档同步：新增并切换生效版本：故事线 v005、方法论 v004、Benchmark/Baseline v004；评价标准为故事线 v005、方法论 v005、Benchmark/Baseline v006。当前 scope 明确为四个 fresh provider 的 one-call MIRA-Relay/MIRA-Context；33-provider 仅 development，Retrospective Best Fixed 取代模糊 Oracle 标签，verifier/VOI/lifecycle 为 extension。
- 论文修订：Introduction 的 `transfer hypothesis` 改为 adaptation hypothesis；重写 `WRITER_REPORT.md`，去掉旧占位符、Gate HOLD 和已过时的 family residual/fallback 表述。
- 干净构建：从 commit `c2656260` 的 Git archive 重建，输出 `paper/aamas2027/build/clean_submission_20261006_main.pdf`；9 页总计、正文正好 8 页、参考文献第 9 页，SHA-256=`543d845970325577dc458dbdc73f46d331c644a243d732b21fc38b58d423cf79`，clean log 无 `Overfull`、`Undefined`、`Missing $`、fatal 或 emergency。第 2、8 页已目视检查。
- 推送：文档同步提交 `c2656260` 已推送到 `origin/codex/progress-20260924`。
- 当前剩余：只剩作者最终双盲/官网复核和是否补跑 BayesianRouter controlled stress 的证据决策；不得把当前 gate v3 的 declared-scope pass 扩写成通用动态收益。

### 2026-10-06 — BayesianRouter stress closure and exact-eight-page rebuild

- 目标：在不扩大 MIRA-Relay claim 的前提下，补齐现代 BayesianRouter 在同一 controlled correctness-drift stress 中的对照，并保持正文恰好 8 页。
- 预注册：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_drift_stress_v2_bayesian/preregistration.md`；新增 runner 开关 `--include-bayesian`，默认旧 v1/v2 receipt 不变。
- 真实运行：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1/`，150 streams、70,650 events；其中 BayesianRouter 30 streams（5 folds × 2 seeds × 3 delays）。
- 验证：`verification.json`=`verified`，150/150 contract/replay，0 forbidden decision fields，0 replay mismatches。events SHA-256=`5e3744ce8ac1af04d3e8d9d81d215d4a1655e792e517377473f1cd4358e42a0e`，summary SHA-256=`8a18abe23a73b23cd7ab1af3c89d3b9d5d0bd534b6c05f1d77a25f1a4d541a1a`，config SHA-256=`2197079b0e639d3a3351bd411f0d5f2f83718bb29a088cbaa47c246c2347aaac`。
- 结果：BayesianRouter 80.28%，相对 Frozen +6.90 pp，scaffold bootstrap 95% CI `[+3.50,+10.41]`；action changes 1,654，rescue/harm 492/167。MIRA-Context 主结果不变（87.56% vs 73.38%，+14.18 pp）。聚合脚本和 result/report/hash 在 `task/task49-pharma-benchmark-baseline-freeze/provider_fresh_drift_stress_v2_bayesian/`。
- 论文：controlled stress 表加入 BayesianRouter；结果叙述更新为 270 natural + 150 stress、420 streams；中文版同步，旧图路径修正为 Figure 1 v08 / Figure 2 v13。
- 排版：从干净提交 `07d88e20` 重建 `paper/aamas2027/build/clean_submission_20261006_bayesian.pdf`；9 页总计，正文恰好 8 页、参考文献第 9 页；PDF SHA-256=`458f804f8750209e9377429b8cf14ad8deba2c94214759eba6ee4a7bf554c04a`，log SHA-256=`c5b48037bf53a4cbe98dabdc3557670b0f7997815d936d190637661880567a4e`。第 2、5、6、8 页目视检查，未发现 Overfull/Undefined/Missing `$`/fatal/emergency。
- 文档：Stage-B gate v3、WRITER_REPORT、README、格式审计和生效 Benchmark/Baseline 文档同步到新 receipt；相关提交 `cf4a96bd`、`07d88e20` 已推送。剩余仅作者最终双盲/官网/补充材料复核，不能把 controlled stress 外推为自然漂移或通用 online gain。


### 2026-10-06 — Figure 2 AI generation v20–v21

- 目的：继续压缩 Figure 2 的留白，同时保持论文主线要求的因果链：`terminal outcome → selected outcome → selected-only queue → released outcome`。
- 版本：v20、v21 的 prompt 和原始 PNG 均已保存；v20 明确保留为失败版本，因为生图模型重新画出了 `terminal outcome → released outcome` 的 queue bypass。
- 当前候选：v21，路径 `paper/aamas2027/figures/generated/figure2/v21_ai.png`，尺寸 2117×743，SHA-256=`ac7771eed788caa0b9ed91211407ec2eaba0a0ef6cb5a2f40f5b2652ceff6d8f`。
- 审查：`paper/aamas2027/figures/external_review/20261006_v21_generation_review.md`；模块、上游失败语义、selected-only queue、延迟时钟和无全局回路均通过，最终 PDF 宽度目视检查通过，16/16。
- 论文同步：`paper/aamas2027/main.tex`、`main_zh.md`、`README.md` 已切换到 v21；中文阅读版源码 hash 更新为 `d7a016adc6b7077a94df0124ece945b64989b625a0d7a56eb174372f236865d7`。
- 编译：`paper/aamas2027/build/main.pdf` 成功生成，9 页总计（正文 8 页、参考文献第 9 页），无 `Overfull`、`Undefined`、`Missing $` 或 fatal 错误；第 5 页的最终图宽度已目视检查。
- 结论：v21 替换 v19 成为当前 AI 视觉候选；v19、v20 不删除，继续作为审计和回溯版本。
- 下一步：回到三份验收标准做一次图文一致性检查；若没有语义问题，不再为 Figure 2 盲目增加版本。


### 2026-10-06 — E3 mechanism audit and family permutation CPU sensitivity

- 目标：审查 E3 的 interface removal、independent residual、family permutation 轴，明确哪些已有证据可复用，补一个不生成新预测的最小 family-label permutation replay。
- 审查：v1/v2 既有 `Frozen/no-update` 语义控制和同 schedule selected-only `matched Online-SGD` 最近邻控制；分别不写成 literal API-null 或数学等价 residual。
- 实验：family labels 在固定 33-provider menu 上做 seed=20261006 的确定性 shuffle；使用 sealed BBB input/snapshots，CPU replay，fold4/seed0/delay0，transient+persistent，5 methods，10 streams/1,440 decisions。custom verifier `verified_permutation_replay` 通过；persistent FamilyGuard 35 次 action changes、5 次 synthetic failures、accuracy 0.8681；transient 0 changes、1 failure、accuracy 0.8958。结果仅为 sensitivity，不进入主效果表。
- 产物：`task/task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v3/`；`research/experiment_section_style_20261006/e3_mechanism_audit/`。标准 FamilyGuard verifier 对 permutation 的 prefix-equality 拒绝已保留为 `production_verifier_failure_fold4_v3.txt`，未绕过升级。
- 下一步：主文只保留紧凑机制对照；hash、manifest、synthetic permutation 和 verifier 边界放 appendix/supplement；限定范围提交上述两个目录。

### 2026-10-06 — 三张 AI 投稿图长期目标与首轮候选

- 长期目标：把 Figure 1 overview、Figure 2 lifecycle、Figure 3 results/evidence 都打磨为 AI 生成的最终投稿图；每轮保存完整 prompt、原始 PNG、SHA-256、审查记录和版本索引，上一版有问题时重写完整 prompt，不在原图上手工修补。
- 规则文档：`paper/aamas2027/figures/AI_SUBMISSION_FIGURES_PLAN.md`，统一 16 分审查门为语义合同、因果/数据流、样例库风格、投稿可读性；硬错误直接淘汰。
- 样例库审查：对照 Top-Conf Figure Gallery 的 architecture/pipeline/results 样例，确认目标风格是模块优先、短标签、有限配色、统一线宽和最终缩放可读；当前正式 Figure 1 v08、Figure 2 v13 仍是人工/旧版本，不能宣称三图已完成。
- Figure 1：AI v01 因 selected-provider 卡中重复出现 A/B/C 候选而保留为失败版本；AI v02 已生成，路径 `paper/aamas2027/figures/generated/figure1/v02_ai.png`，SHA-256=`6ac7e5d247d076b14ae590fb9db4fba15d85989d495d344934f93adb12fd01cb`，候选审查见 `figures/external_review/20261006_figure1_ai_v02_review.md`。
- Figure 2：AI v22 已生成，路径 `paper/aamas2027/figures/generated/figure2/v22_ai.png`，SHA-256=`d23b23d14fb06ca8509f484f383a4555ee1fe8b717f231ce25794fcb3692e31c`；使用当前 `provider residual + released counters` 标签，替代 v21 的旧标签，候选审查见 `figures/external_review/20261006_figure2_ai_v22_review.md`。
- Figure 3：AI v01 已生成为空数据布局，路径 `paper/aamas2027/figures/generated/figure3/v01_ai.png`，SHA-256=`26e978eabdec9fc4bf1312d6e49068d5c211b75e954b29875965ee0909013445`；没有伪造点、曲线、区间或数字，审查见 `figures/external_review/20261006_figure3_ai_v01_review.md`。真实 marks 尚未接入，当前不进入正文。
- 当前形式稿状态：`paper/aamas2027/main.tex` 仍引用 Figure 1 v08 和 Figure 2 v13，正文没有 Figure 3；这与 AI 候选链并存，待三图均通过最终语义、数据绑定和 PDF 缩放检查后再统一切换。
- 下一步：先用真实 receipt 生成 Figure 3 的 exact-data layer；同时对 Figure 1 v02、Figure 2 v22 做最终通栏 PDF 缩放检查，再决定是否需要重写 prompt。


### 2026-10-06 — 图稿子目标收窄

- 用户要求后续只关注三张 AI 图，不再推进实验或其他正文任务。
- 系统 goal 仍保留旧的论文总目标且处于 blocked；由于工具不允许编辑未完成 goal，本地执行范围以 `paper/aamas2027/figures/AI_SUBMISSION_FIGURES_PLAN.md` 为准。
- 当前唯一工作目标：Figure 1 overview、Figure 2 lifecycle、Figure 3 results/evidence 全部完成 AI 版本链，保留 prompt/原图/审查，最终通过语义、样例库风格和 PDF 可读性检查。

### 2026-10-06 — 实验章节顶会式重排与科学 gate 复核

- 目的：按已阅读的 ICLR/ICCV/NeurIPS 高质量论文结构，把实验章节从说明性矩阵改成短 setup、四行实验地图、主结果表和机制解释；正文保持正好 8 页。
- 论文改动：`paper/aamas2027/main.tex` 已完成实验地图、natural fresh / controlled drift 两张主表、最新 BayesianRouter stress 对照和 v13 程序绘制 Figure 2；结果表先于解释段落，正文不再放状态/hash/未完成实验清单。
- 最新源码 SHA-256：`55eb99725c03b1b87b5b32056b663787ba65d5a3a85f82636ac07d2564fff513`。
- 当前构建：`paper/aamas2027/build/clean_submission_20261006_matrix_rewrite.pdf`，9 页总计，正文 8 页、参考文献第 9 页；PDF SHA-256=`05a5a4173e14417f83ae43ef4e5069b4e09736a09c58be1ad34d7f3d9b2af346`，日志 SHA-256=`ec74473182d9e0f077d33471c7a67c0f6b18b358daaa39dff72d6bc50cdaa9ac`。
- 验证：第 5、6、7、8 页已目视检查；无 `Overfull`、`Undefined`、`Missing $`、`Fatal` 或 `Emergency`，仅有官方模板常见 `Underfull vbox`。
- 新文档：`research/experiment_section_style_20261006/implementation_log.md`、`scientific_gate_review_20261006.md`、`nearest_work_matrix.md`。最近方法矩阵确认 strict core 为 Frozen/Matched SGD；Greedy/Bayesian/PILOT 为 adapted 分层；GraphRouter/RouterDC/RouterBench 暂 audit-only 或外部协议。
- 当前科学判断：Stage-B v3 在声明的一次调用动态切片内 Gate A/B/C 为 15/16、13/16、14/16；但三份投稿标准要求的统一最近邻合同、跨方法 state/cost/unknown/refusal 字段和更强 E3 机制反事实尚未闭合，不能写成全文科学 gate 已打开。
- 中文阅读版 `paper/aamas2027/main_zh.md` 已同步英文源码 SHA。
- 下一步：审核最近方法矩阵是否足够支撑 Baseline gate；随后固定统一 raw-run schema，再设计最小机制消融卡。没有清晰的科学问题前不提交新的 Nebula 任务。

### 2026-10-06 — 三张 AI 图通栏 QA 与 Figure 1 v05 收口

- 范围：本轮只处理三张 AI 图，不改实验、不改正文、不切换 `main.tex` 的正式图路径。
- Figure 1：完成 v03/v04 失败原因归档；v05 原图 `paper/aamas2027/figures/generated/figure1/v05_ai.png`，SHA-256=`5fd6838daa4b166fcaf5a99a166ba2166c2e2cfa59bcae3ea124f5c5e57c2355`。v05 修复了输入区反馈回流和 terminal outcome 断链，审查 `external_review/20261006_figure1_ai_v05_review.md`，16/16。
- Figure 2：v22 保持当前 AI 候选，修复旧 residual 标签和 queue bypass，审查 `external_review/20261006_figure2_ai_v22_review.md`，16/16。
- Figure 3：v01 只作为无数据布局候选，审查 `external_review/20261006_figure3_ai_v01_review.md`；无点、曲线、区间或实验数字，不能当作结果图。
- 独立纸面检查：新增 `paper/aamas2027/figures/qa/ai_candidates_fullwidth_20261006.tex` 和编译产物 `build/ai_candidates_fullwidth_20261006.pdf`；7-inch 通栏三页编译成功，PDF SHA-256=`aabf5af09e80d0c17a015ce3528216245235ecdfa6fde4a7752b83f6d0853308`。Figure 1/2 的标签、箭头和反馈链在通栏缩放下可读，Figure 3 空布局结构可读。
- 文档：更新 `AI_SUBMISSION_FIGURES_PLAN.md`、`generated/figure1/README.md`、`prompts/VERSIONS.md`、`SELECTED_VERSIONS.md` 及三份图审查记录；所有旧 PNG、prompt 和失败版本保留。
- 当前结论：Figure 1 v05、Figure 2 v22 是当前 AI 候选，已通过语义/因果/样例库/通栏 QA；Figure 3 v01 仍等待真实 receipt 绑定的数据层。三图未全部达到最终锁定条件，因此不切换正式稿路径。
- 下一步：只做 Figure 3 的 receipt-bound exact-data layer；若数据层未准备好，不生成新的伪数据图，不重复生成已通过的 Figure 1/2。

### 2026-10-06 — Unified baseline receipt schema v1 audit

- 目标：为下一次 baseline raw-run 固定统一 receipt 字段，覆盖 feature、warm-start、likelihood、budget、state bytes、unknown/refusal、timing、failure/correctness 和 release clock；不回写历史收据。
- 审计：核对 `baseline_cost_budget_v1/table.json`、`baseline_cost_budget_fairness_audit_v1/report.md`、`adapted_baseline_receipt_field_audit_v1/audit.json` 及 fresh natural v2/stress v3 raw events/verifiers。
- 发现：fresh events 有 decision/terminal/feedback、state hash、selection/update timing、chemical correctness 和 due/release tick；逐事件 `state_bytes_before/after` 与 unknown/refusal reason/action 全部缺失。fresh verifier 重放 action/output/correctness/due/identity，但未独立逐步核 state hash。旧 raw config 的 warm-start/likelihood 多为 null；adapted Greedy/Bayesian state bytes 仍 MISSING。
- 产物：`research/experiment_section_style_20261006/unified_receipt_schema_v1.md`（SHA-256=`d8224e09627e96c835d71938dc5ce3c847ebf836c5fe26b6428bc001749bc228`）。
- 下一步：统一 runner 前先实现 schema 级独立 verifier；任何缺字段 raw-run 保留 audit-only，不从 hash、模型维度或聚合 p95 推断。

### 2026-10-06 — Figure 3 receipt-aligned AI layout v02–v03

- 目的：让 Figure 3 的 AI 空布局与当前论文的真实结果口径一致，不让生图模型生成任何实验 marks。
- v02：`paper/aamas2027/figures/generated/figure3/v02_ai.png`，SHA-256=`c954416f3f37143d8d6df7dca22c24e3fbf13811f86b180d9fe59c93c11e114b`；保留为失败版本，面板 (c) 图例出现含义不明的 “Other”。
- v03：完整重写 prompt v06 后生成，`paper/aamas2027/figures/generated/figure3/v03_ai.png`，SHA-256=`9086f83e626bc8fbc100c697fc6816470778fe2ae68c1b8768c2a6061296ad8a`；面板 (c) 改为 Frozen / MIRA-Relay / matched baselines，面板 (a)/(b) 与当前论文结果角色对齐。
- 审查：`external_review/20261006_figure3_ai_v03_review.md`，布局门 16/16；新的 7-inch 通栏 QA `figures/qa/build_v03/ai_candidates_fullwidth_20261006_v03.pdf`，SHA-256=`29a764c12cd49da4f20cfca196bd95d6291ae964a1477c962b656f6131c5e281`，第 3 页可读。
- 数据绑定：新增 `generated/figure3/build_data_binding_v1.py`、`data_binding_v1.json`、`data_binding_v1.md`。脚本只读取已核验 natural/stress summaries 和 result，生成 panel A delay effect、panel B rescued/harmed、panel C update p95/state 的精确清单并记录源 hash；没有执行新实验。
- 复核：data-binding 脚本 `py_compile` 通过，连续运行两次 JSON SHA-256 相同：`ac467b303775e83b737ba9e807e806552dc4414651ce8d476aa081e1817c20fc`。
- 当前结论：三图 AI 候选链为 Figure 1 v05、Figure 2 v22、Figure 3 v03；前两张已通过 16/16 通栏审查，第三张布局已通过但 exact-data layer 尚未绘入，因此三图尚未正式锁定或切换到 `main.tex`。
- 下一步：在不改实验和正文的前提下，按 `data_binding_v1.json` 制作 Figure 3 的 receipt-bound marks，保留 AI 底图和每个数据层版本，并重新做通栏 PDF QA。

### 2026-10-07 — 正文证据措辞收口与状态回放审计

- 目的：在不改动科学结果的前提下，逐句核对正文是否把当前证据写过头，并继续推进投稿 gate。
- 完成：主稿 `paper/aamas2027/main.tex` 删除旧的 MIRA-Context 别名；把“独立状态回放”改成当前真实支持的合同/回放检查，保留 MIRA-Relay 主方法与自然/受控漂移结果 claim。中文阅读版同步方法名与边界，并更新英文源码 hash。
- 构建：`paper/aamas2027/build/direct_20261007_v7/main.pdf`，9 页总计（正文 8 页、参考文献第 9 页），PDF SHA-256=`a81a7708f1e5bcd899a911659fb3cd1fb0c19fa61ceb8526b85a2ee32e7a8d9e`，源码 SHA-256=`b11d919be5afd27179060e331356e25141672ee6cf6d633ba5f226edd1b8ecf1`；无 `Overfull`、`Undefined`、`Citation`、`Missing $`、`Fatal` 或 `Emergency`。
- 图文检查：Figure 1 v08 在最终 PDF 第 3 页目视通过；endpoint truth、chemical label、runtime failure 和 selected-only delay 语义一致。
- 新审计：`research/experiment_section_style_20261006/unified_receipt_schema_v1.md` 与 `state_hash_verifier_audit_20261007.md` 固化了当前收据缺口。现有 verifier 没有独立重算 state hash，且复用了生产 `run_stream`；旧 fresh receipts 保留为历史证据，不能把 `verification.json=verified` 解读为完整状态验证。
- 当前判断：主结果和 8 页排版已收口；科学投稿 gate 仍有一个关键工程证据缺口：独立 state-transition replay + H1–H13 mutation smoke。该缺口不改变已报告的自然/受控结果。
- 下一步：先在 4–8 条 CPU smoke 上补独立 replay、exact event allowlist、hash-chain 与 mutation tests；通过后只重生成 verifier receipt，不重跑 provider 训练或 Nebula 科学批次。随后从最终 Git archive 做一次干净 8 页构建，并更新格式审计文档。

### 2026-10-07 — 独立 fresh receipt 合同审计完成

- 目的：不导入生产 runner/policy，对现有 natural v2 和 controlled stress v3 raw events 做独立、fail-closed 的 event-level 审计；不重跑 Nebula、不改历史收据。
- 实现：新增 `src/probe_headless/audit_fresh_receipt_contract_v1.py` 和 `src/probe_headless/tests/test_audit_fresh_receipt_contract_v1.py`。检查 exact event allowlist、identity/order/cardinality、decision→terminal→feedback 绑定、due/release 时钟、decision 禁止字段、跨事件 opaque state hash/version continuity、summary counts 与 canonical stream digest。
- 结果：9 个本地测试通过（clean smoke + H1/H2/H4/H5/H7/H8/H9/H12 mutation）。natural 全量 270 streams/127,170 events，stress 全量 150 streams/70,650 events，均 `verified`、0 issues。结果 JSON 和边界说明在 `research/experiment_section_style_20261006/independent_receipt_checker_20261007/`。
- 边界：当前 receipt 只有 opaque state hashes，不能独立重算 learned policy state；审计只宣称跨事件 commitment continuity，不宣称独立策略状态重执行。下一步如需关闭该缺口，应在新 receipt 版本记录 canonical state bytes 或 bounded transition trace。

### 2026-10-07 — Figure 3 AI data layer v04–v05 and naming-consistency lock candidate

- 范围：本轮只处理三张 AI 投稿图，不改实验、正文或 `main.tex`。
- Figure 3 v04：用完整 prompt `paper/aamas2027/figures/prompts/figure3/v07.md` 从 v03 空布局生成 receipt-bound 数据候选；原图保留，SHA-256=`316cf46c019b216bd70f957e0a7bed34bdc7d27f3e7e3e7221c77d7cee980eac`。
- Figure 3 v05：发现 v04 面板 (b) 使用缩写 `MIRA`，与 Figure 1/2 和面板 (c) 的 `MIRA-Relay` 不一致；完整重写 v08 prompt，从 v04 重新 AI 编辑。v05 原图 `paper/aamas2027/figures/generated/figure3/v05_ai.png`，SHA-256=`f019e48a7cdd1d06687bfa30350052f5334362b2ee2a957667396f4ecaa2c1a3`。面板 (b) 改为 `MIRA-Relay`，rescued/harmed 柱高、数字、反馈曲线、散点、坐标轴和图例保持 receipt 口径。
- 数据绑定：所有可见结果均对应 `generated/figure3/data_binding_v1.json`，SHA-256=`ac467b303775e83b737ba9e807e806552dc4414651ce8d476aa081e1817c20fc`；没有新增实验、伪造区间、ProbeNoUpdate、Other、排名或 Pareto claim。
- 审查：`external_review/20261007_figure3_ai_v04_review.md` 和 `20261007_figure3_ai_v05_review.md` 均保存；v05 16/16，无硬错误。v04/v05 均保留，v05 为当前 Figure 3 候选。
- 通栏 QA：`figures/qa/build_v05/ai_candidates_fullwidth_20261007_v05.pdf`，三页、7-inch 通栏；PDF SHA-256=`b24bc653165afee01912781e50e4aa37447d5a6fcb4f290cb48b195b142b823e`。第 3 页在 180 dpi 检查通过，完整 `MIRA-Relay` 标签和柱状图数字可读。
- 当前三图候选：Figure 1 v05、Figure 2 v22、Figure 3 v05；三张图均已有 16/16 审查和独立通栏 QA。正式稿尚未切换，三图 joint lock 仍待完成。
- 文档：同步 `AI_SUBMISSION_FIGURES_PLAN.md`、`SELECTED_VERSIONS.md`、`generated/figure3/README.md`、`prompts/VERSIONS.md`；v01–v04 和所有旧 prompt/审查继续保留。
- 下一步：做一次三图 joint lock 的视觉/语义一致性复核；若没有硬错误，再生成最终三图清单和锁定记录，仍不手工改图，不改实验和正文。

### 2026-10-07 — 投稿 preflight 打开（声明范围）

- `make submission` 已从硬编码失败改为实际 fail-closed preflight，并成功通过：科学 scorecard 为 `PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE`，主稿为 anonymous class，无残留 `TBD/pre-results/MIRA-Context`，BibTeX/LaTeX 无致命或 overfull 错误。
- 候选 PDF：`paper/aamas2027/build/submission_20261006_181518/main.pdf`，9 页总计（正文 8 页、参考文献第 9 页），PDF SHA-256=`abf88071fe9e829d1207d354513396533ec8c1379e2fa9eb9001193aac097394`；源码 SHA-256=`ee5264e8819f1f3f2e6c06292105cc3d1c3837ef0f46fe3958d2d59ce81c3503`。
- `paper/aamas2027/abstract.tex` 已同步真实结果摘要，不再保留 pre-results 占位文本；中文阅读版同步英文源码 hash、方法名和独立回执审计边界。
- 当前 gate 判断：声明的一次调用动态切片技术 preflight 已打开；最终 AAMAS 提交仍需作者在干净环境复核官网、双盲信息和补充材料。旧回执的 canonical learned-state 独立重算仍是后续 schema 缺口，不伪装成已完成。

### 2026-10-07 — 三图 joint-lock 视觉与语义复核

- 对当前 AI 候选 Figure 1 v05、Figure 2 v22、Figure 3 v05 做同一轮 joint-lock 检查，范围仍只包含三张图。
- 交叉检查：`MIRA-Relay` 命名、蓝/灰/琥珀/紫色语义、selected-only 可见性、反馈链方向、无旧 comparator、无 `Other` 图例、无输入到反馈 shortcut、无未观察输出和无 AI 新数字。
- 三图均通过 16/16：Figure 1 v05、Figure 2 v22、Figure 3 v05；记录在 `paper/aamas2027/figures/external_review/20261007_three_figure_joint_lock.md`。
- 统一通栏 PDF 为 `paper/aamas2027/figures/qa/build_v05/ai_candidates_fullwidth_20261007_v05.pdf`，三页 7-inch full-width 检查通过；Figure 3 页保留完整 `MIRA-Relay` 标签和 receipt-bound 数字。
- 当前三图唯一候选链锁定为 v05 / v22 / v05；所有旧 prompt、PNG 和审查记录继续保留。正式稿 `main.tex` 尚未切换，等待最终 owner 接受后再做一次明确的路径替换。

### 2026-10-07 — clean-tree 投稿候选最终构建

- 从提交 `dff33da9` 的 Git archive 解包，在没有工作区 `.aux/.bbl` 的条件下完成 BibTeX + 三轮 LaTeX。
- 产物：`paper/aamas2027/build/clean_submission_final/main.pdf`，9 页总计（正文 8 页、参考文献第 9 页）；PDF SHA-256=`663d9fcdf779c6043b8c9b7883bf1887f2bba8fff6a556714e57f0fb0c08205f`，日志 SHA-256=`c03a6d0645317fc663fa42d7eec4b4a9b3aaaa5b4163edf76a1694bef376e212`。
- 源码 SHA-256=`ee5264e8819f1f3f2e6c06292105cc3d1c3837ef0f46fe3958d2d59ce81c3503`；第 2、3、5、8 页目视通过；日志无 `Overfull`、`Undefined`、`Citation`、`Missing $`、`Fatal` 或 `Emergency`。
- 结论：声明的一次调用动态切片已完成科学结果、独立事件级回执审计、投稿 preflight 和 clean-tree 8 页构建。剩余动作是作者最终官网/双盲/补充材料复核，以及未来 receipt schema 的 canonical state bytes 扩展。

### 2026-10-07 — 当前提交 preflight 最终复核

- 在提交 `c9def7d4` 工作树运行 `make submission`，科学 gate、独立 natural/stress receipt JSON、anonymous class、占位词、BibTeX/LaTeX 日志和 9 页总页数检查全部通过，退出码 0。
- 最新本地候选：`paper/aamas2027/build/submission_20261006_183703/main.pdf`，PDF SHA-256=`cb9777ae1cfd75ca28d610246ef99af9f57f11b3ca278c63e1f8a90237e962be`；日志 SHA-256=`456da37234990fe0c5746eb639cfcfb13422644e434d47c0eb7e5559b9437cd7`。
- 该候选只用于本地 preflight，不替代 clean-tree 归档构建或 OpenReview 最终上传。

### 2026-10-07 — 生效文档方法名一致性收口

- 发现：正文已统一使用 MIRA-Relay，但生效的故事线、方法论、benchmark/baseline 和评价标准仍把实现写成 MIRA-Context，容易让方法名、baseline 名和 artifact method_id 混在一起。
- 修复：只更新生效文档和当前 nearest-work matrix；论文方法统一写 MIRA-Relay，旧代码/raw receipt 中的 `mira_context_reliability` 明确标为实现标识。历史版本、原始 receipt 和代码字段不改写。
- 验证：生效文档与当前 paper README/nearest-work matrix 中不再出现 `MIRA-Context` 或 `MIRA-Relay/MIRA-Relay`；`git diff --check` 通过。

### 2026-10-07 — 三图接入整稿的页数 gate（副本）

- 用副本 `paper/aamas2027/main_ai_jointlock_20261007.tex` 把 Figure 1 v05、Figure 2 v22、Figure 3 v05 接入，并按正式模板/BibTeX/三遍 LaTeX 编译。
- 图像和编译均成功，但候选 PDF `paper/aamas2027/build/ai_jointlock_20261007_bib/main_ai_jointlock_20261007.pdf` 为 10 页总计，超过当前 9 页总计预算（正文 8 页 + 参考文献）。
- 结论：三张 AI 图的图内 joint-lock 仍然通过；整稿页数是独立 layout gate，不能靠偷偷缩小图或改实验解决。本轮不改正式 `main.tex`，失败证据记录在 `figures/external_review/20261007_three_figure_paper_integration_qa.md`。
- 后续若要把三图全部放进正式稿，需要单独决定删减/压缩正文材料；这不属于本轮只处理 AI 图资产的范围。

### 2026-10-07 — 绘图范围纠正：结构图用 AI，数据图用程序

- 用户澄清：架构图、流程图、概念图用 AI；折线/柱状/散点等数据图和数据表不需要 AI。Codex 确认并实施，不再将全部图都强制 AI 生成。
- 决议：新增生效 `docs/决议/0012-按图类型区分AI绘图与数据绘图.md`；0010 标为被替代，旧正文保留；0011 仍待双方确认。本轮不把制作方式当作会议图像政策审查。
- Figure 3 程序重绘：`paper/aamas2027/figures/figure3/v01_data/` 保留（标题偏贴上边界）；`v02_data/` 为当前数据图候选，改善上边距和 harmed 柱纹理。数据直接读取既有 `data_binding_v1.json`，没有从 AI 图读数或执行新实验。
- 验证：binding hash 与三份 source hash 匹配；精确校验 6 个曲线点、10 根条形、5 个散点的 artist 数据。面板 (c) 用明确标注的 log/log 坐标，不 jitter，不补未核验的区间。PDF 原生 7×2.8 inches、字体嵌入，已渲染检查。
- 产物：每版保存 render.py、data.json、plotted_values.json、values.csv、data_table.tex、PDF/SVG/PNG、manifest 与 render.log。v02 PDF SHA-256=`a072f7f81115fad46d1bc7cc4c14cea0d44f52151fb1aad563c8aac96238b5fc`；7 份输出 hash 复核一致。审查 `figures/external_review/20261007_figure3_data_v02_review.md`。
- 历史纠正：AI Figure 3 v01–v05 和旧审查保留；在 v04/v05 与三图 joint-lock 记录添加历史/已替代标识，明确原 16/16 是视觉自检，不是坐标精确证明。
- 文档：更新图稿计划、SELECTED_VERSIONS、FIGURE_SET_PLAN、QA README、AI prompt 索引与 Figure 3 README；旧计划/索引保存在 `figures/archive/20261007_drawing_scope/`。
- 当前资产分工：Figure 1 AI v05、Figure 2 AI v22、Figure 3 程序 v02_data。正式 main.tex、实验、方法、结论未在本轮更改。
- 下一步：AI 迭代只用于结构/流程/概念图，数据图按结果文件及程序版本维护。整稿接入和页数为独立排版任务，不再沿用“三张 AI 图”口径。

### 2026-10-07 — 独立审计补强与残差来源实测

- 上一轮分类：有进展。本轮目的：关闭已发现的事件审计漏检，随后补方法机制证据，不把本地构建预检等同于三份完整论文标准通过。
- 审计修复：发现并修复 `_truth_proxy` 下划线键绕过 allowlist；保留 1 failed/17 passed 的首次发现说明。最终 18 tests 通过；natural 270/127,170 events 与 stress 150/70,650 events 再审均 verified、0 issues。当前 checker 记录自身 hash；命令、环境、源码 hash 和退出码在 `research/experiment_section_style_20261006/independent_receipt_checker_20261007_v2/`。没有重跑效果实验。
- 科学问题：fit-rate-centered residual 可精确分为相对 frozen score 的误差项与 selected score composition 项；共同加性修正不会改变菜单排序，无需重复跑该类科学消融。先数学审查和实验卡，再分析真实 development 日志。
- 新实测：15 条 seed0 fold/delay streams、2,166 次决策、722 分子；独立分数重构误差 2.22e-16，恒等式误差 2.78e-17。实际与 Frozen 动作差异 769 次；在原已观察历史上移除 composition 项，shadow 动作差异 401 次（18.51%）。未计算 shadow accuracy 或闭环收益。
- 校准线索：released-selected 平均 frozen score=0.580164，正确率=0.791782；两项 signed mean 分别 +0.076017 与 -0.062692。class-balanced logistic 的 sigmoid 不保证自然正确性校准，因此不能把两项直接命名为真实漂移与纯选样偏差。
- 审查：独立 methods auditor 核对公式和脚本，无计算修复项；15 项定向测试通过，60 条双seed流/25,992事件合同核对通过，seed1语义重复仅用于校验，不重复计入指标。原始输入与 confirmation 均未改。
- 产物：`task/task49-pharma-benchmark-baseline-freeze/residual_decomposition_v1/{experiment_card.md,report.md}`；`experiments/logs/task49_residual_decomposition_20261007/v1/`；`src/probe_headless/analyze_mira_residual_decomposition_v1.py`。实验卡、初版报告、后续数学审查、逐决策/释放记录与 hash 均保留。
- gate：更新 `scientific_gate_review_20261006.md`，E3 获得可量化的机制疑问和修复方向，但三份完整论文标准仍 HOLD；没有改标准分母或声明达到完整目标。
- 下一步：仅在 development 中恢复 fit 分数均值，分离 score baseline offset 与 selected score mix；再固定同一校准 surface/对应 Frozen 的闭环中心化比较。当前 main.tex 已加入分解与实测，正在保持正文恰好8页；不回写 confirmation 超参。

### 2026-10-07 — fit基准恢复与8页主稿归档

- 目的：把残差中的样本组成变化与分数基准差分开，再确定值得执行的闭环改法；同步正文证据和排版。
- 真实计算：四个checkpoint在722个development分子上forward，原五折fit重建，共7.272秒；4332条Frozen logits和fit rates误差均为0。20个fit均分0.525782–0.543146，对应正确率0.693582–0.824324。
- 诊断：相对fit组成项均值+0.016355，fit基准差项−0.079047；同历史移除两项分别改变292/2166、374/2166次shadow动作，不代表新策略准确率。恒等式误差1.11e-16，5项测试通过；全部source/checkpoint hash未变。
- 产物：`task/task49-pharma-benchmark-baseline-freeze/fit_score_reference_v1/{experiment_card.md,report.md}`；`experiments/logs/task49_fit_score_reference_20261007/v1/`；新恢复脚本与tests。缓存含722条原生概率、5折priors、2382fit/722OOF分数，后续无需重复forward。
- 主稿：新增精确残差分解和2166条development诊断，修正校准/审计/自然结果边界；`make submission`改为每次从原始事件重跑独立checker。最终`paper/aamas2027/build/submission_20261007_034632/main.pdf`正文8页、参考文献第9页，预检exit0，页面6/8目视通过。源码SHA=`7eb05d595eeecc06267954c2496ca9dd62ff0880b6b23ffeb6420a7c8a16a712`；PDF SHA=`bce0888644a71a55f9d40a6ee4f1185b2e070dc56da3d821573cb303ffa66601`。
- 留痕：`research/experiment_section_style_20261006/paper_revision_20261007.{md,json}`记录版本、失败与最终hash；未改动协作者的中文稿和图稿。
- 下一步：`calibration_centering_v1/experiment_card.md`冻结raw/类别权重修正 × Frozen/平均正确率中心化/逐query中心化，在natural+controlled flip共180条development流配对检查；先审公式和测试，禁止把修正自动说成已校准。
- gate：三份完整标准仍HOLD；8页编译通过不等于科学投稿gate通过。长程goal继续active。

### 2026-10-07 — 六格闭环实测：校准有效，中心化未达升级线

- 目的：拆分静态分数修正与在线中心化的闭环收益，对应方法E3与Benchmark Gate C3。先数学审查并冻结6格、1pp资源线、LOO与负结果处理，未改变原confirmation。
- 实际运行：`experiments/logs/task49_calibration_centering_20261007/v1/` 完成180流、25,992决策、77,976事件，8.895秒；722个development分子，本地CPU，0 API/GPU/新forward。
- 结果：OOF Brier 0.234888→0.173292，log loss 0.661950→0.533768。corrected Q比corrected M自然+0.508pp、controlled+0.369pp，两组LOO均正，但均未到预登记1pp线；对对应Frozen自然仅+3/2166。raw Q两个环境均负。完整六格、各fold/delay、暴露差异与负结果保留。
- 核验：runner 6项合成测试、独立checker 40项合成/篡改测试通过；全部180流状态、队列、动作、聚合、90对因果前缀及30条legacy行为独立复算verified、0 issues。
- 产物：`task/task49-pharma-benchmark-baseline-freeze/calibration_centering_v1/{experiment_card.md,math_review.md,report.md}`；`src/probe_headless/{run_calibration_centering_v1.py,verify_calibration_centering_v1.py}`及tests。257.85MB原始事件保留本地，Git存13.11MB无损gzip，解压hash一致；恢复步骤见日志README。
- 论文：六格结果已加入正文Table 5，压缩重复artifact文字；`paper/aamas2027/build/submission_20261007_043532/main.pdf`已通过make submission，正文恰好8页、References第9页，页面7/8目视通过。PDF SHA=`d76952790290d39268971dc225de6554325ca7f1c0a013ff92da79546afcc119`，source SHA=`fd4fff41cdad26b7f735d3905630344d9cdfcbab28cd7d17ff4fcd09b84b2223`；proof在`research/experiment_section_style_20261006/paper_revision_20261007_centering.{md,json}`。上版与失败现场保留。
- gate：分数基准问题获得实测支持，Q未达到升级线，不替换原方法、不降目标、不扫超参找正数。完整科学标准仍HOLD；root README和scientific_gate_review已同步。
- 下一步：先读已有headroom与表示诊断，检查剩余可救错误的pre-choice可预测性、greedy造成的反馈缺失；给出可证伪卡后再运行。共同baseline可复用本轮完整state/成本合同；新方法确认需新的未读数据。

### 2026-10-07 — 四工具headroom与反馈覆盖定位完成

- 目的：对齐E1/E5，区分菜单共同错误、调用前排序不足与完全缺少反馈，避免为小收益盲目加探索或扫超参。先卡与独立数学审查，再运行；修正了“top1与concordance须同时改善”的过强判据。
- 实测：四checkpoint/722 development，90自然流/12,996决策；2.514秒，0训练/推理/API/GPU。471题全对、147题mixed、104题全错；fit-only572、raw Frozen573、corrected Frozen579、逐题Oracle618。当前Corrected Frozen还有39题机会，不能迁移旧33工具上界。
- 错误定位：原raw/M在最初D+1次之后的138个可救错误中，136个已有正确候选反馈、2个全部正确候选从未选；corrected/Q为109/112已有反馈。不能据此排除局部context支持不足或估计探索收益；下一步优先检验条件化使用已释放反馈。
- 核验：10项必要合成测试通过；独立标准库checker从原缓存和自然事件核对全部逐行字段、rank-sum AUROC、top1/一致率、选择/释放/pending计数和所有汇总，3.113秒、verified、0 issues。分析和验证首次均成功。
- 产物：`task/task49-pharma-benchmark-baseline-freeze/headroom_feedback_v1/{experiment_card,math_review,report}.md`；`experiments/logs/task49_headroom_feedback_20261007/`及`task49_headroom_feedback_verification_20261007/`；新analyze/verify脚本和定向tests。summary SHA=`e1e8a7bafdc16042727ee76849dd9676240ae57e00839472a0b8c2bdb7615e68`，verification SHA=`bb39e6f9a19e592abd534c85ad5b913d340d797ac61f5f7790b746e61d4cf831`。
- 对齐：E1/E5补上当前面板证据，三份完整科学标准仍HOLD；原Q未过升级线的决定不变。正文未改，保持已验证正文8页PDF；协作者中文稿、图稿和日志不动。
- 下一步：审查context ridge residual的单变量干预，constant-only须精确还原Q，并加入解析范数匹配控制以排除更新强度混杂。只做机制诊断，不把标准ridge命名为新算法贡献；先冻结新卡再运行。

### 2026-10-07 — 条件化残差闭环完成，转向当前面板的学习型prior

- 目的：在headroom诊断后只改变在线残差特征，检验按分子使用反馈是否优于每工具统一修正；增加解析范数匹配常数控制，避免把更强更新当context收益。数学审查和冻结卡已先提交7e4756d2。
- 实测：Q20 / norm_constant / context × natural/controlled ×5fold×3delay，90流、12,996决策/在线ridge更新、39,168事件；7.946秒，无新provider推理/离线训练/API/GPU。自然三格80.3324/80.0554/80.1939%，受控78.6704/77.7008/78.0240%。
- 结论：context−Q20自然−0.1385pp（救回19/新增错误22），受控−0.6464pp（43/57）；自然Q差的5个LOO全负。仅好于更强的范数常数控制，不通过≥1pp、LOO、动态保护规则，不升级、不扫正则；不否定整体研究目标。
- 核验：8项必要合成测试通过；Q20重现旧30流误差3.33e−16，N解析Q10误差2.22e−16。root另写不导入runner的独立checker，逐事件重建完整数值state/hash并在流末直接解ridge，误差1.46e−16；16.151秒、verified、0 issues。科学run和验证均首次成功。
- 证据：`contextual_residual_v1/{experiment_card,math_review,report}.md`；`experiments/logs/task49_contextual_residual_20261007/`与`task49_contextual_residual_verification_20261007/`；runner/tests/verifier。summary SHA=`73f611c61f667ba5340691309c7aa0fe65ec3d1a6428964fd2f40d2abae51a12`，verification SHA=`2089e7482a0e56565269761b2e8737378b9c6eca272e395fe36b8bae728bcb89`。88.16MB事件原文留本地，Git存11.86MB无损gzip，恢复hash已核。
- 下一步资产核查：当前四provider尚未训练原生BT+CLS、MLP correctness或Laya prior；已有这些结果全部属旧33provider。`fresh_learned_prior_v1/preparation.md`记录可复用RewardHead/loss/validation/checkpoint循环与容量/bias不匹配风险。下一步固定当前面板和匹配训练合同，补学习型prior/现代baseline，不重跑旧描述性审计。
- 对齐：本轮关闭一个具体构造的效果疑问，Baseline完整性和新机制收益仍未满足三份标准，gate继续HOLD。正文未改，仍为已验恰好8页；goal active，协作者中文稿/图稿/TODO段落未改。

### 2026-10-07 — AI Figure 1 恢复与真实图库风格迭代

- 目的：修复阅读 PDF 的 Figure 1 被程序图替换，并从实际样例图吸收模块表达和反馈回路。范围仅图稿、prompt、审查、构建；没有运行实验或改方法/数字。
- 原图核对：用户 Downloads 带 `(1)` 的 PNG 与仓库 figure1/v07 完全相同，SHA `f29cb6865b85f601e1969e8e145b2ca9052fbdcf20cf6f15191a6f8df8520f4c`；没有丢失。main.tex 原来引用程序 v08，AI 候选未接回阅读稿。
- 真实图库：逐张查看 TableDART、CRITIC、CollabLLM、GenAL；选择浅色模块分区、展开内部运算、用图形闭环表达反馈。分析/规格在 `paper/aamas2027/figures/gallery_style_study_20261007.md`；四张原始参考与出处/hash在 style_references/20261007_gallery_v1。
- AI迭代：完整 prompt v12–v17 留痕，真实生成 v06/v07/v08/v09。前三版各有身份、旁路、输入、Judge或符号错误，独立失败记录分别保存，不手工改图。v09 独立最终14/16、0必须修复；次级标签字号和重复说明仍可优化。
- 阅读稿：main.tex 默认选 AI v09；BibTeX+三遍LaTeX，9页总计、正文8页、References第9页、Figure1第2页。PDF `paper/aamas2027/build/reading_ai_20261007_v09/main.pdf` SHA `8b078e87e659d7921618f1c9cae680891ed0c7f646b31408ec5425bed5427cdd`；源码SHA `063a29377ff1c5c7c096b9dc344ca5cfbe6a4bdd2280c544a48b666f6090121e`。无overfull/undefined/citation/fatal等；第2/8/9页已目视。pdfimages逐像素确认嵌入AI原图，PNG SHA `c793d1d37c98133513d662317a7c9770fd5788d3816d4993be335d819c2ae97c`。
- 保存：旧build/main.pdf及前后源码在figures/qa/restore_20261007_v1；当前阅读稿同步到build/main.pdf；独立审查v17与恢复报告在external_review。页码检测首次因要求大写独占行而失败，修正后确认9页，记录保留。定向diff检查通过；中文稿已有的Markdown硬换行不作本轮无关修改，仅同步两张图路径，不宣称全文同步。
- 图像政策：官网本轮确认AI图片适用条件；阅读稿与投稿图源用明确开关区分。make submission定义MIRASubmissionFigures选择程序v08，Figure2程序v13；本轮没重跑科学preflight，也没有确认待议0011。README、版本索引、prompt索引与QA说明已同步。
- 未完成与下一步：F1减冗余/放大次级标签；F2同风格完整prompt v23已保存尚未生成；数据图继续程序源。当前是图稿进展，不代表三份完整科研标准通过或整篇论文目标完成。详见 `paper/aamas2027/figures/external_review/20261007_figure1_restoration_report.md`。

### 2026-10-07 — 四工具学习型prior实测与8页正文回填

- 目的：补当前四工具的匹配BCE/BT离线对照，检验固定Morgan64上的训练目标是否能改善调用前选择；原五折/两seed/200步及validation规则预先冻结。
- 真实执行：单A800任务01a114f6d306db0a283dce1d4453aa33，commit86543eca，success/FINISHED。20fits、4000epochs、2888OOF、30份初始/最优checkpoint全部回收；训练入口15.932秒，优化合计14.361秒，不包含平台启动。0新provider/LLM调用。
- 结果：BCE两seed78.53/78.95%，BT78.81/78.25%，均低于CorrectedFrozen80.19%；各组五个LOO全负。BT−BCE一正一负，两个目标均未过升级线；不重挑epoch、不扫超参、不修改旧confirmation。
- 核验：独立NumPy重算两头logits/动作/奖励、best validation forward、配对与LOO及go，verified/0issues，logit最大差1.924e−7。按原记录审核checkpoint选型，不宣称重执行4000步优化；8项合成测试已通过。
- 证据：task/task49-pharma-benchmark-baseline-freeze/fresh_learned_prior_v1/{report,execution}.md；experiments/logs/task49_fresh_learned_prior_20261007/。完整OSS归档1,420,185bytes、SHA2ac16485c3784976521e13b06dc000e5c9321d3efa084e7fe200af16bcdfe3bf；核验SHA3f62292bc2863319c2a950248a1fc9838ea102ce14e7116e4fd0f807ec0a1607。一次TLS状态失败保留，无重复提交。
- 正文：5行10数紧凑表与正式baseline引用已回填，删重复审计表/段。多文件候选完成BibTeX+三轮LaTeX，第7/8页目视通过；正文恰好8页、References第9页。PDF paper/aamas2027/build/fresh_prior_candidate_20261007_v1/main.pdf，SHA1e83ac1927844758093f2f65967f1679145227ac56f0c7714a208844c1649805；工作源码SHA266fcdf56d52f27b3eb893c3b91c1046ef1e454152694cc91b6a62f708bf7d48。proof在research/experiment_section_style_20261006/fresh_prior_paper_20261007/。保留协作者图开关/中文稿/Makefile；不是新clean-tree或完整科学gate通过。
- 下一步：固定BCE训练，对比64零填充与chiral1024输入；共同架构/初始化不等于有效容量控制。独立数学审查已完成，进一步冻结卡与实现准备中。三份完整标准仍HOLD、goal active，当前没有运行中的Nebula任务。

### 2026-10-07 — 单变量表示对照准备完成

- 目的：固定BCE+CLS训练，检验更细分子输入能否改善当前工具选择；不是继续扫loss或正则。两组为64零填充与同源chiral1024，共同1024→64双头/完整initial/五fold两seed，20fits；同登记参数不等于有效容量控制。
- 数学与实现审查通过；15项必要合成测试通过。CPU准备首次成功1.200秒，722行重建64与旧seal逐值完全相同，raw OR折叠全通过；10个旧initial扩展state已封存。root另独立重建特征、旧行和hash，0.473秒、verified，无训练/新provider/LLM调用。
- 输入SHA8109405b121472e7628f987ccae72ee90d7442f64f36301efca65c733ce58447；34,587,591bytes。Git存无损gzip并验证解压一致，运行用原JSON。卡/数学/代码审查在fresh_representation_v1/；证据在experiments/logs/task49_fresh_representation_20261007/。
- 资源线：1024在两seed均须胜补零和CorrectedFrozen至少1pp且各五LOO全正；不达标停止固定增维构造。旧157confirmation不变，科学gate继续HOLD，正文仍为已验8页。
- 远端：第一次任务列表查询超时，失败日志保留；仅进行只读重试，尚未提交。下一步固定推送commit，doctor/OSS回验/dry-run通过后单线A800执行。

### 2026-10-07 — 表示对照完成，升级线未通过；正文结构候选完成

- 目的：检验更高维的同源分子表示能否补足当前四工具的静态排序缺口；协议固定为64零填充控制 vs chiral Morgan1024，共同网络/完整initial/BCE+CLS/200步/五fold×两seed。
- 真实A800任务：`01a1153c076660894f3390124224a569`，commit `84c61207d7cccece592d0f5879e8122ef4ed2b7d`，success/FINISHED。20 fits、4000 epochs、2888 OOF、30 checkpoints、40文件完整回收；单卡训练入口14.665秒（平台启动和传输另计），0新provider/LLM调用。
- 结果：64零填充 seed0/1 为 78.53%/78.95%；1024 seed0/1 为 77.84%/79.09%；Corrected Frozen 为80.19%。1024相对零填充为−0.6925/+0.1385 pp；相对Corrected Frozen为−2.3546/−1.1080 pp。1024两seed均未满足双对照≥1pp和每seed五个LOO全正，停止该固定增维构造，不扫宽度/初始化/正则。
- 核验：独立NumPy verifier verified/0 issues；最大logit差1.850e−7，best validation loss差9.248e−8。输入特征、OR折叠、旧64复现、initial hash和所有逐样本选择均核对；完整报告 `task/task49-pharma-benchmark-baseline-freeze/fresh_representation_v1/report.md`。
- 研究解释：结果不支持“只增加Morgan位数即可改善选择”，也不能证明1024无信息或证明hash碰撞是唯一瓶颈。已有truth-recovered/PAR原型已核查，不能重新包装为新方法；下一方向要先解决监督效率/反馈结构，不能重复旧原型。
- 写作：独立审查指出13节过碎、内部计划语气和静态负结果衔接弱。已另存7节结构候选 `paper/aamas2027/main_structure_candidate_20261007.tex`，实验/结果/诊断归入一个问题驱动块；仍保留全部数字、负结果和receipt限制。候选目标正文8页，编译和视觉检查完成前不替换正式main.tex。
- gate：表示控制补齐了E1证据但没有新机制收益；三份完整标准仍HOLD，投稿声明范围preflight不变。旧confirmation保持只读，当前无运行中Nebula任务。

### 2026-10-07 — 7节正文 v4 提升为主稿并通过 submission preflight

- 目的：把实验矩阵和结果从碎片化章节收进 AAMAS 常见的 7 节结构，同时保持正文恰好 8 页和现有 claim 边界。
- 修订：Related Work 前置；Experiments 集中 setup/基线/矩阵/主结果/机制诊断；Discussion and Limitations 合并 validity/artifact。修复 released-state 公式同时传入 `r_chem`/`r_run` 的语义冲突，Related Work 不再声称已完成 family sensitivity，移除未定义的 MIRA framework 说法；没有新增实验数字或扩大 claim。
- 版本：旧主稿保存在 `research/experiment_section_style_20261006/structure_revision_20261007/main_before_promotion.tex`，v1 候选和 v3 10 页失败现场保留；生效源码 `paper/aamas2027/main.tex` SHA=`21d370509097ddebd5d8b1f796771a1e7f1c688ecd1adea2a789c5747c639c7f`。
- `make submission` 真实通过：natural receipt 270/270、stress receipt 150/150 均 verified/0 issues；投稿 PDF `paper/aamas2027/build/submission_20261007_084607/main.pdf` 共9页（正文8页、参考文献第9页），SHA=`cd52a061362619b964ba66cbb993dbaa010a55c103529d696fe1bc5cbdcd77d1`，最终日志无 Overfull/Undefined/Citation/Missing$/Fatal/Emergency；第8页已渲染目视。
- 科学结论不变：这是声明范围内的 one-call dynamic-scope preflight；三份完整 Benchmark/Baseline/机制标准仍 HOLD。controlled stress 只支持 action-dependent evaluator-side correctness-drift recovery，不能写成自然 provider drift、generic online gain 或端到端 agent 上限提升。
- 证据：`research/experiment_section_style_20261006/structure_revision_20261007/promotion_review_v4.md`、`revision.json`；下一步先重算三份验收标准和 baseline role matrix，再决定是否补统一合同或新方法实验，不盲目申请 A800。

## 2026-10-07 — submission preflight 后三份标准窄切片重检

- 已完成：将 7 节主稿 v4 提升为 `paper/aamas2027/main.tex`，并以 commit `60803acc` 推送到 `origin/codex/progress-20260924`。
- 版式证据：`paper/aamas2027/build/submission_20261007_084607/main.pdf`，9 页总计（正文 8 页 + 参考文献 1 页），PDF SHA `cd52a061362619b964ba66cbb993dbaa010a55c103529d696fe1bc5cbdcd77d1`；无 Overfull、Undefined、Citation、Fatal 或 Emergency 错误。
- 论文依赖核对：submission 构建使用已跟踪的 Figure 1 v08 与 Figure 2 v13；默认阅读稿的 AI 生成图仍是未跟踪历史素材，不进入这次提交依赖。
- 新增：`research/experiment_section_style_20261006/three_standard_recheck_20261007.md`，按三份生效标准重算当前窄切片的作者侧分数与缺口。窄切片 Gate A/B/C 为 15/16、13/16、14/16；长期 broad claim 仍 HOLD。
- 重要边界：当前结果支持 stationary fresh menu 的 null stability 与声明的 evaluator-side correctness drift recovery；不支持自然 provider/version drift、完整 33-provider model-fresh、post-call verifier/refusal、hERG/AMES 或端到端 Agent 能力提升。
- 未完成：第二位复核者的独立故事/方法打分、实验矩阵的基线逐行回执核对、remove-state/interface 消融与 family/lineage 限制的结果卡。
- 下一步：先接收实验矩阵风格审查，再只做能关闭上述缺口的文档/证据工作；不盲目启动新的 Nebula 批次。

## 2026-10-07 — 实验矩阵风格审查与最小修订

- 独立风格审查：`research/experiment_section_style_20261006/matrix_style_audit_20261007.md`。结论是当前矩阵已接近顶会写法，但需要让 map 覆盖诊断表、把 Cost/validity 改为实测指标、并明确两张主表的 `Δ vs Frozen`。
- 已保存修订前主稿：`research/experiment_section_style_20261006/matrix_style_revision_20261007/main_before.tex`，SHA `21d370509097ddebd5d8b1f796771a1e7f1c688ecd1adea2a789c5747c639c7f`。
- 已修订 `paper/aamas2027/main.tex`：新增 Static diagnostics 行；Cost/validity 改为 policy update p95、calls、receipt pass rate；两张主结果表显式标注 `Δ vs Frozen`，controlled-drift 表删除重复 N；未改任何数值、方法或 claim。
- 新 preflight：`paper/aamas2027/build/submission_20261007_092645/main.pdf`，9 页总计（正文 8 页 + 参考文献 1 页），PDF SHA `71b43d4a7c0530aef5c344496af9404afda3d72a90d614200573b7d1143059`，日志 SHA `b6bea83850f51fb1d678336fab0d969d2c0d3c844a1e9b28f7e2270bb9e5d8e8`。警告门无 Overfull/Undefined/Citation/Missing math/Fatal/Emergency；第 8 页已渲染复核。
- 下一步：提交并推送这次仅呈现层的修订；然后按三份标准重新绑定主稿 hash，继续补第二位复核和 baseline 逐行回执，不启动无关实验。

## 2026-10-07 — baseline 语义审计与方法 Gate 纠正

- 独立审计确认 `MIRA-Relative` 的真实实现：它把每个 provider 的 frozen score 乘以一个随 selected-only released history 更新、但对当前分子 x 不变的 provider-wise scalar factor；不是所有 provider 共用的 global scalar。审计见 `research/experiment_section_style_20261006/baseline_semantics_revision_20261007/implementation_audit.md`。
- 审计还发现旧报告“Relative/Frozen action sequence 完全相同”不适用于当前 v1/v2 receipt：30 对 stream 共 76 个 provider action changes，但 rescued/harmed correctness 都为 0，故 pooled accuracy 仍同为 89.81%。主稿现在只声称它是 aggregate negative control，不再声称 action identity 相同。
- 独立三标准复核纠正了上一版自评：方法 H7=UNKNOWN/HOLD；M5/M7/M8/M13 下调为 2，M14 为 3，严格总分 46/60。原因是现有 confirmation receipt 的 opaque state hash 不能独立恢复 learned state，且当前 fresh 契约缺 remove-state、independent-residual、interface-null 机制消融。纠正报告：`research/experiment_section_style_20261006/independent_standard_recheck_20261007.md`。
- `three_standard_recheck_20261007.md` 已同步：Benchmark/Baseline Gate A/B/C 仍为 15/16、13/16、14/16；方法 Gate 保持 HOLD；这不是降低 claim，而是把 controlled drift 的同实现证据与独立方法证明分开。
- 最后一次提交版 preflight：`paper/aamas2027/build/submission_20261007_100602/main.pdf`，9 页总计（正文 8 页 + 参考文献 1 页），PDF SHA `2f095eba2b6cfbaccd7dc90e912bbff4174913dc3f802acc076a8fc0816bf28f`，日志 SHA `d69d70db87b52e7d9ccba52f23264889b448f837f6340c899b152e1625f98e52`；无 Overfull/Undefined/Citation/Missing math/Fatal/Emergency；第 8 页已渲染复核。
- 下一步不盲目申请新 A800：先实现当前 fresh confirmation 的 canonical state/transition trace 与不依赖 production runner 的 NumPy replayer，再做同一 fresh contract 的 full/Frozen/no-update/independent-residual/interface-null paired mechanism ablations。

## 2026-10-07 — current fresh MIRA state 的独立回放闭环

- 目的：关闭方法验收 H7 的第一个具体缺口。现有 natural/stress receipt 只有 production state hash，不能独立恢复 selected-only 更新；本轮不改 benchmark、checkpoint、超参和任何效果数字。
- 实现：新增 `src/probe_headless/verify_fresh_context_replay_v1.py`。只用 Python 标准库和 NumPy，不导入 production runner、`MIRAContextReliabilityPolicy`、model loader 或 stream implementation。以同坐标 Frozen 的 immutable score surface 为静态输入，独立重算 `sigmoid(w_a^T x)+(q_a-r_a)`、argmax、selected-only `successes/counts`、版本和 release clock，并输出 canonical dynamic-state trace。
- 实跑：natural v2 与 controlled-drift v2 各 30 MIRA streams、4,710 decisions、4,710 state transitions；两份 receipt 的最大 score 重算误差均为 `0.0`。结果在 `experiments/logs/task49_fresh_context_replay_20261007/{natural_v3,stress_v1}/`，报告在 `task/task49-pharma-benchmark-baseline-freeze/fresh_context_state_replay_v1/report.md`。
- 篡改门：复制 receipt 后改一个 reward，verifier 在下一决策的 score mismatch 处拒绝；改一个 score `+0.001` 也拒绝。失败现场保留在同一实验日志目录，未纳入论文数字。
- 限制：该回放关闭 dynamic learner state/transition 的独立复核，但历史 production hash 还包含 immutable weight array，当前复核不重新跑 provider inference 或 offline optimizer，也没有把旧 hash 伪装成已重新计算。未来 fresh runner 仍应直接写 initial weight bytes、feature bytes 和 lifecycle queue state。
- 测试：定向 Python assertions 2 项通过，`py_compile` 通过；机器没有 `pytest` 可执行文件，未声称 pytest 通过。
- gate：H7 从“完全没有独立回放证据”推进到“dynamic-state replay verified，但 immutable prior/interface 仍有边界”；方法完整标准仍 HOLD。下一步先把这份边界写入三标准重检，再冻结最小 fresh mechanism matrix；不直接提交新的 A800。

## 2026-10-07 — paper-level main.pdf 主版入口

- 目的：把论文主版和迭代构建分开维护，避免把阅读稿 `build/main.pdf` 或某个临时版本误当成投稿主版。
- 实际构建：`make submission` 通过；新迭代 `paper/aamas2027/build/submission_20261007_105246/main.pdf` 共 9 页（正文 8 页、参考文献第 9 页），日志无 Overfull/Undefined/Citation/Missing math/Fatal/Emergency。PDF SHA `fd7e09c42a96411a0d26e3e0c638311c8e8bff7c96d5e55e46f32fac49557f2d`，日志 SHA `4a3c946c75dd0348c9db2b94a43d6664d9aa9ece79848dc93fbd1bc24cb3005c`。
- 主版：新增 `paper/aamas2027/main.pdf`，由 `promote_main_pdf.py` 从该已验证 submission build 提升，逐字节比对通过；`main_provenance.json` 记录 source build、源码 SHA、页数和 PDF SHA。
- 维护规则：先 `make submission`，再 `make promote-main SOURCE=build/submission_<timestamp>/main.pdf`；所有迭代仍保留在 `build/submission_*/`，不直接复制 `build/main.pdf`。

### 2026-10-07 — AI 图完整留存、版本画廊与主版 QA 维护

- 范围：只推进两张 AI 结构图的风格、因果连线和 paper-scale QA；未运行实验、未改变方法/数字、未自动切换论文 main.tex。
- 决议：新增生效 0013（全部 AI 原图保留并展示供用户挑选）与 0014（独立迭代完成后同步上一层主版 main PDF）。每版 PNG、完整 prompt、manifest、生成请求及失败审查分别保留。
- Figure 2：v25 / prompt v26 已独立原图与7-inch QA审查，14/16、0硬错误；报告 external_review/20261007_figure2_gallery_v26_independent_review.md。仍等待用户最终审美选择，不据评分自动覆盖正文。
- Figure 1：v10/v18 不通过11/16；v12/v20 不通过10/16，Release直接绕过Update并停在counter底边。v13/v21 修成唯一Release→Update→B链，13/16、0硬错误，但Release→Update是长紫折线；v14/v22 为14/16、0硬错误，仍有L形Update→B边；v15/v23预检失败，反馈/队列布局又漂移。全部raw保留，包括一次辅助重复生成；不删除、不像素编辑、不把后版自动当最终版。
- SHA：F1 v12=`431a45e543ba1bc4e387740f7d5c2501769bd59c64e206ea4027562c22d1b7a8`；v13=`8f49d0e08c09c5450966e332d918827dfa22d523f20a6758f29aa6b8128b643e`；v14=`aa0a4fa3f84e58393da9baf438fd0d40d7599461ce293103102855dc6d3ec320`；v15=`0a1ebcb75d2311bbdd055df2cebd0593948a7989929c1bee1ebfd0fb570ffca3`。
- QA：build_v09(F1v12/F2v25)、build_v10(F1v13/F2v25)、build_v11(F1v14/F2v25)各保留3页PDF、compile log和140dpi page1/2/3。v09内置编译两次失败（相对路径、absolute权限），后用本地pdflatex完成，失败说明保留。v11 PDF SHA=`56f3b9700e2e43f467bf3822ca5415b91480d1ad1fdbae287c48634788d6c7f8`。
- 主版：figures/qa/main.tex、main.pdf 当前仍为已审查F1v09+F2v25；PDF SHA=`b5b883665cefda7b1eabc1850f0a7f951da7ab6be50094d2e904343a9b683c6e`。main_manifest、main_render_20261007与维护说明保留；用户选定并审查通过前不合并未通过版本。
- 展示：figures/version_gallery_20261007/index.html、manifest.json、两张contact sheet已扩至35个原图记录（F1 v01–v15+辅助输出；F2 v07–v25），旧画廊快照也保留。README/prompt VERSIONS/SELECTED_VERSIONS/PLAN 已同步，F2 README链接修正。
- 下一步：用户可比较全部版本；继续优化时从有正确因果链的F1v14或当前F1v09出发，用更简洁且无冲突的完整prompt，仅修模块位置/短边。达图级要求后，再按0014同步主版。长程图稿goal仍active，整篇科研标准是否通过不由本轮图稿评分裁定。

### 2026-10-07 — 版本继续迭代至 Figure 1 v15

- v14 独立审查已完成：14/16、0硬错误；Release→Update 已短水平，但 Update→B 仍是 L 形边，报告 `paper/aamas2027/figures/external_review/20261007_figure1_gallery_v22_independent_review.md`。
- v15 已由 AI 生成并完整留存（prompt v23、manifest、generation_request、SHA=`0a1ebcb75d2311bbdd055df2cebd0593948a7989929c1bee1ebfd0fb570ffca3`）。预检发现 feedback/queue 进入 Update、结构漂移，标记拒绝，不送独立审查。
- 画廊已扩至 35 条记录，Figure 1 contact sheet 包含 v01–v15 与 auxiliary 输出；上一版 HTML/manifest/contact sheet 均有 before_v15 快照。主版 `figures/qa/main.pdf` 仍保持已审查的 F1 v09 + F2 v25，未把 v14/v15 自动合入。
- 当前可选图：F1 v09（14/16，主版候选）、F1 v13（13/16，因果正确但长紫折线）、F1 v14（14/16，因果正确但 L 形 Update 边）；F2 v25（14/16）。用户最终选择后按 ADR-0014 合并主版。

### 2026-10-07 — 画廊 contact sheet 可读性修正

- 发现中文状态标签在系统 Helvetica 中渲染成方框；没有丢原图或审查记录，保留了修正前的 `figure1_all_versions_contact_sheet_before_ascii_fix.jpg`。
- 当前 `figure1_all_versions_contact_sheet.jpg` 改为英文短状态（history/rejected/review pass/precheck reject 等），16 个 Figure 1 输出均可辨认；HTML 仍保留中文说明和完整链接。

### 2026-10-07 — fresh MIRA full-state replay v2

- 目的：在不改 benchmark、provider checkpoint、超参或既有结果的前提下，补齐 fresh MIRA 回放的独立证据，直接关闭 H7 的 opaque-state 缺口。
- 实现：新增 `src/probe_headless/verify_fresh_context_replay_v2.py`。只用标准库、NumPy 和 RDKit；不导入 production runner、policy、model loader 或 stream implementation。直接从 sealed prior cache、BBB manifest 和 receipt 重建 Morgan64、4×65 prior weight、fit-rate anchor、selected-only counts/successes、version、延迟队列和全量 score/action。
- 实跑：natural v2 与 controlled drift v2 各 30 MIRA streams、4,710 decisions、4,710 feedback transitions；每份 receipt 的 9,420 个可用 production state hashes 全部核对通过，score 最大绝对误差 0。历史 terminal 没有 state hash，summary 没有 final-state hash，verifier 将字段缺失记为 unavailable，不记作匹配。
- 篡改门：复制 receipt 改一个 reward 被拒绝；改一个 score `+0.001` 被拒绝。定向 v2 测试 2 项通过，py_compile 通过；未声称 pytest 可执行文件通过（机器没有 pytest 命令）。
- 证据：`task/task49-pharma-benchmark-baseline-freeze/fresh_context_state_replay_v2/{experiment_card,report}.md`；结果在 `experiments/logs/task49_fresh_context_replay_20261007/{natural_v8,stress_v5}/`；verifier SHA=`1ad88223e41af4ee1a84803558a3466db70922b7f54f7a56c6d8a2343a4065f6`；natural verification SHA=`9c3315fd2d0d8d6cd6107214cbb84af25ec00784d9ef2a03bbfbec3a42bb6eec`；stress verification SHA=`9a0771a9e563321adf637c1bcaf083570b2f4a983a17792fa60394703649d627`。
- 验收更新：`independent_standard_recheck_20261007.md` 与 `three_standard_recheck_20261007.md` 将 H7 从 UNKNOWN 更新为 PARTIAL/PASS_FOR_REPLAY，方法严格分数从 46/60 更新为 50/60；M5/M13 仍低于 3，方法 Gate 继续 HOLD。
- 结论：独立回放证据已补齐，但 fit cache、provider inference、terminal/reward 仍是 replay 输入；当前四-provider fresh roster 仍不能证明 provider-local residual 的机制必要性。下一步只做同一 fresh contract 的 full/Frozen/no-update/independent-residual/interface-null mechanism matrix，不申请无关 A800 批次。

### 2026-10-07 — fresh mechanism ablation and main paper integration

- 目的：关闭 M5/M13 的机制必要性缺口，检验提升是否来自 selected-provider credit，而不是“只要收到任何反馈就会变好”。不改 benchmark、provider checkpoint、truth 或原始 confirmation receipt。
- 实现：新增 `fresh_shared_credit_policy.py`（MIRA-Shared wrong-credit control）、`run_fresh_mechanism_ablation_v1.py`（同一 fresh contract 的自然/controlled receipt）和 `analyze_fresh_mechanism_ablation_v1.py`（124 scaffold clusters、10,000 bootstrap、seed20261007）。
- 真实回放：natural/controlled 各 210 streams（7 methods × 5 folds × 2 seeds × 3 delays），各 98,910 events；读取 sealed provider prediction、manifest、prior cache，不重新调用 provider/API/GPU。原始 JSONL、config、summary、analysis 分开保存。
- 结果：natural MIRA-Local−Frozen `0.00 pp`, scaffold CI `[-1.09,+1.43]`, rescue/harm `82/82`; Shared-Credit `−0.25 pp`, `[-1.65,+2.90]`, `114/126`。controlled MIRA-Local−Frozen `+14.18 pp`, `[+9.83,+19.18]`, `780/112`; Shared-Credit `+7.13 pp`, `[+4.42,+11.48]`, `486/150`; direct Local−Shared `+7.05 pp`, scaffold CI `[+3.11,+9.98]`. Local 的 controlled gain 在 delay 0/1/4 为 `+13.63/+14.52/+14.39 pp`。
- 解释：结果支持 selected-provider credit assignment 是受控 recovery 的关键机制；natural null 保留为 stationary safety。四个 provider 是 singleton families，不能从这次实验声称 family sharing 或 independent-residual 已被识别。
- 证据：`task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v1/{experiment_card,report}.md`；natural raw SHA=`fb044f629e339143fec6ad82ffaf9a0dfbd048586f298864b9cb0ecf0a6b9d5b`，controlled raw SHA=`8af050756386d1daa0058f3b58bf9b32ec93797ab8aab736bb8f077e2ffc4da9`；analysis v2 SHA=`b242956b170d5cfde1e73b481291a300f9a73252f6b2e8bbe32a0e019cfe27e1` / `249349a48c70597e498178014f6108a064e3644aee06222a5249ce1d0d6c3b88`。
- 论文：`paper/aamas2027/main.tex` 已加入 Shared-Credit baseline、自然结果行、controlled drift 行和 Local−Shared 机制解释。`make submission` 通过，最新候选共9页（正文8页、参考文献第9页），无 warning gate；已用 `make promote-main SOURCE=build/submission_20261007_122046/main.pdf` 更新 paper-level `main.pdf`。PDF SHA=`8faa50ed36cc4080ff70822f050ebaa73499e8bcd08735b3d1b5a7bc37cf0ae4`，source SHA=`93a5d40c309c7d11ceb3898d4683882983a5f45614f8c447914bcab06b47dc8d`。
- 验收：方法证据 provisional 52/60，H7=PARTIAL/PASS_FOR_REPLAY；M5/M13 达到 declared-scope 最低分，但 paper-level Gate 仍待第二位复核和主稿整合后重检。下一步是做主稿数字逐行回执核对、重跑三份标准，并保留 family/lineage 限制，不启动无关实验。

### 2026-10-07 — paper result binding audit

- 目的：逐行核对主稿旧主表、新机制表和关键正文数字是否能从 raw receipt/summary/analysis 重算，避免编译通过但数字绑定错误。
- 第一次审计按错误手算值把 controlled Shared-Credit 写成 3,789，返回 rejected；失败输出保留在 `experiments/logs/task49_paper_results_binding_20261007/results.json`。定位后确认原始 receipt 是 3,792/4,710，与正文 80.51% 一致。
- 修正后 `src/probe_headless/audit_paper_results_binding_v1.py` 返回 verified；natural/stress 全部旧方法计数、新 Local/Shared rescue-harm 和正文关键数字均通过。输出 `results_v2.json` SHA=`165be45d18ccb014e2afb9e6725df4d7f2969796262197557bda86d1dc72bc54`；checker SHA=`500beb99ea412da01471d312d0ca646bd6c1bf2b49084691cceed7b5f43e771e`。
- 报告：`research/experiment_section_style_20261006/paper_results_binding_audit_20261007.md`。新机制 receipt 仍明确标为 runner/analysis 证据，尚未伪装成 independent full-state replay。
- 下一步：将这次 binding audit 写入三份 paper-level 标准复核，重点检查 benchmark/baseline 角色和机制表语义；不新增无关实验。

### 2026-10-07 — Shared-Credit mechanism receipt independent replay

- 目的：补齐新机制表的证据链，避免只对原始 MIRA receipt 做 full-state replay，而新 Shared-Credit receipt 只剩同实现日志。
- 实现：新增 `src/probe_headless/verify_fresh_shared_credit_replay_v1.py` 和定向测试。只用标准库、NumPy、RDKit，从 sealed prior cache、manifest 和 mechanism receipt 独立重建 Shared-Credit full state、score、argmax、selected-only scalar posterior、delay lifecycle 和 summary。
- 结果：natural/controlled 各 30 streams、4,710 decisions、9,420 个可用 production state hashes，score 最大误差 0。自然与 controlled 的 canonical traces 和 verification JSON 已保存；复制 receipt 改一个 score `+0.001` 或 reward 都被拒绝。2项定向测试、py_compile 通过。
- 证据：checker SHA=`6bfdbc2a73b079756b59360e2760795c890f698e5649d827eb97264df90e495e`；natural verification SHA=`65d15907242d3791cba44553271a3509fcd1c2ea7311e47fabfd0a76e055bc80`；controlled verification SHA=`c807d86dc1680c1e3105591c55aff3c1cd4069afa54c69ad9c62f5603524ba71`；失败 mutation 输出保留在 `experiments/logs/task49_fresh_mechanism_replay_20261007/`。
- 文档：`fresh_mechanism_ablation_v1/report.md`、`paper_results_binding_audit_20261007.md`、三份标准复核已更新。方法 declared-scope 证据保持 52/60；仍不外推 family sharing、natural drift 或端到端 agent。

## 2026-10-07 — Shared-Credit mechanism correction and stable main.pdf promotion

- 独立方法审查发现前一轮 Shared-Credit 对照不能隔离 selected-provider credit：实现为 `q_a^0(x)+c_t-\bar r_a`，其中 `c_t` 对所有 provider 相同，在 `argmax` 中严格抵消；它还是从第一步就静态重排，初始动作不等于 Frozen。此前“反馈恢复/机制已隔离”的文字全部撤回。
- 点估计与区间也已纠正：旧分析把 pooled event-level 差值和 equal-scaffold mean bootstrap 区间配在一起。旧数值和原始日志不删除，改标为探索性实现诊断；权威解释见 `task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v1/report.md`，原报告、旧主稿和旧 PDF 见 `research/experiment_section_style_20261006/shared_credit_correction_20261007/` 与 `paper/aamas2027/archive/20261007_shared_credit_correction/`。
- 主稿已移除 Shared 的 primary mechanism rows 和 Local--Shared 因果结论，只保留一段实现边界说明；受控结果的结论收窄为“selected-only released state 在声明的 evaluator-side drift 下能改变后续动作并相对 Frozen 恢复正确率”。主稿仍保持方法名 MIRA-Relay 和原定目标，不把未证机制写成已证事实。
- `make submission` 通过：`paper/aamas2027/build/submission_20261007_140604/main.pdf`，9 页总计（正文 8 页，参考文献从第 9 页开始），PDF SHA `0856cbf58291ad24daab0e9c0a9a127a5de69a30f3c5742298e39824eb104228`，日志 SHA `8ebb2b86d00d0a06ac8e93ee343e9c0b6ec6a03feb6dd40db5b8aefdd2dc78b9`；第 8 页已渲染检查，warning gate 无 Overfull/Undefined/Citation/Missing math/Fatal/Emergency。
- 稳定主版已通过增强后的 `promote_main_pdf.py` 提升为 `paper/aamas2027/main.pdf`，与 submission PDF 逐字节一致；`main_provenance.json` 记录来源、页数、hash、构建 manifest、日志和前一主版归档。提升工具现在要求不可变 source snapshot 与当前源码逐字节一致，检查第 9 页 References、构建日志，并原子替换主版。
- 独立复核文档已恢复原始 46/60 与 M5/M13=2，另加作者侧更新，不再冒充独立重评分；三标准重检同步为：Benchmark/Baseline declared-scope 可复核，方法 Gate HOLD，H7 仅为回放补充证据。
- 未完成：设计一个初始动作与 Frozen 完全相同、只改变 released-state 消费方式的合法机制对照，并在运行前预注册且统一 pooled/scaffold estimand；当前不启动新 GPU。

## 2026-10-07 — Figure 1/2 role separation and top-conference candidate pair

- User feedback: Figure 1 v07 is the visual density reference because it avoids excessive arbitrary provider-letter text; Figure 2 v23–v25 duplicated Figure 1's architecture. New decision/contract `docs/决议/0015-两张结构图分工与去重复规范.md` and `paper/aamas2027/figures/figure_role_contract_20261007.md` make Figure 1 the method architecture and Figure 2 the single-event temporal timeline.
- Figure 2 v26 / prompt v27 is archived and independently reviewed 15/16 with zero hard errors: no A/B/C, no Features/scorer/residual/counters/menu, complete terminal→judge→chemical feedback→selected-only pending→release→released state→later decision path; runtime failure ends at ledger. Review: `paper/aamas2027/figures/external_review/20261007_figure2_role_v27_independent_review.md`.
- Figure 1 iterations v16–v21 and two auxiliary outputs are all retained with raw PNG, prompt, manifest, generation request and gallery record. v16 review found Update stopping at register border; v17/v18 fixed parts but were rejected/held for routing or U-shaped visual issues; v19 was precheck rejected because Update visually passed through residual; v20 passed 14/16; v21 fixes waiting/update edge semantics and passes 15/16 with zero hard errors, recommended final candidate. Review: `paper/aamas2027/figures/external_review/20261007_figure1_role_v31_independent_review.md`.
- Current candidate pair: Figure 1 `generated/figure1/v21_ai.png` + Figure 2 `generated/figure2/v26_ai.png`. Both use the topconf gallery grammar: compact modules, concrete icons/grids, blue/gray/amber/purple semantics, no grid/swimlane, no provider-letter clutter. Figure 2 does not repeat Figure 1's architecture.
- QA build `paper/aamas2027/figures/qa/build_v17/ai_structures_fullwidth_20261007_v17.pdf` compiled and rendered at 140 dpi; page 1/2/3 retained. PDF SHA-256 `f2244b2dfefee1b613d048b0969b5b2b5534068a6dc49f3aca6c53f506f1a458`.
- Gallery `paper/aamas2027/figures/version_gallery_20261007/` now has 44 records (23 Figure 1 including auxiliaries, 20 Figure 2, plus prior auxiliary accounting), no missing links and all image hashes verified. Previous gallery snapshots remain.
- Master `paper/aamas2027/figures/qa/main.pdf` remains unchanged (old Figure 1 v09 + Figure 2 v25) until the user makes the final visual selection; no submission/main PDF was silently replaced.
- Next step: user reviews the candidate pair in the updated gallery. If accepted, sync `figures/qa/main.tex/main.pdf` under ADR-0014, preserve the previous master and record the new master hash. If not, start a new full prompt/version from the specific review issue; do not edit v21 in place.

## 2026-10-07 — mechanism v2 pre-run card

- 为关闭 Shared-Credit v1 的混淆，新建 `task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v2/experiment_card.md`。v2 固定首次动作与 Frozen 完全一致，只比较 no-update、MIRA-Relay selected-provider update 和预先固定的无不动点 permutation credit；不复用 v1 的 pooled 结果或区间。
- v2 尚未运行。运行前门槛是：卡片和实现先提交，构造案例证明无反馈 action equality、reward mutation 能区分归因、单一 pooled/scaffold estimand、独立回放和失败条件全部固定。当前不申请新 Nebula 任务。
- 版式记录：最新主版满足正文 8 页、参考文献第 9 页和 warning gate；第 8 页右栏仍有可见留白，因此只报告“页数合规”，不宣称页面已完全铺满。

## 2026-10-07 — fresh mechanism ablation v2 正式 replay 与独立复核

- 目的：在纠正 v1 Shared-Credit 静态初始重排和估计量混用后，验证“selected-only
  released outcome 的源 provider 归因”是否能解释受控 evaluator-side drift 下的动态恢复。
  v2 不改 benchmark、provider checkpoint、truth、prior 或调用预算，也不新增 Nebula 任务。
- 设计和代码：`task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v2/`
  的 experiment card、pre-run review、policy、runner、analyzer、import-independent
  verifier 已冻结并推送；当前提交 `a389bd2aa1ac6452f3766fe44944f00fbd720977`。
  三臂为 Frozen、MIRA-Relay/Local 和 fixed cyclic permutation credit；permutation
  使用 source-indexed counters/fit-rate anchor 计算同一个 residual 增量，只写目标槽。
- 正式 natural receipt：`experiments/logs/task49_fresh_mechanism_ablation_v2_20261007/natural_v2/`；
  controlled receipt：同目录 `controlled_v2/`。每个条件 90 streams、14,130 decisions、
  42,390 events。两个 deterministic schedule replicas 不解释为 seed robustness。
- 两份独立 verifier 均通过：90 streams、14,130 decisions、56,520 state-hash checks、
  14,130 score checks、最大 score 重算误差 0、release 前 score/argmax 与 Frozen
  逐事件一致；严格 release tick/ordinal、输入 SHA、实现 SHA 和 selected output/truth
  binding 均核验。构造案例 `construction_cases.json` 通过。
- 统一主 estimand 为等权 124 scaffold cluster 的 stream-level correctness difference，
  点估计和 10,000 次 bootstrap 使用同一聚合。natural Local−Frozen 为 `+0.10 pp`
  （95% CI `[-1.08,+1.42]`），Local−permutation 为 `-0.30 pp`
  (`[-1.42,+0.76]`)；controlled Local−Frozen 为 `+14.40 pp`
  (`[+9.79,+19.23]`)，Local−permutation 为 `+15.67 pp`
  (`[+10.65,+20.74]`)。natural 不支持普适 stationary gain；controlled 结果支持
  在声明 drift replay 中源对齐的 selected feedback 有效，但不外推 family sharing、
  verification/refusal 或端到端 agent gain。
- 完整性：复制 controlled receipt 并把第一条 feedback `release_tick` 加 1 后，
  verifier 以 `ReplayError` 拒绝；摘要为 `experiments/logs/.../mutation_rejection.json`。
  紧凑索引和哈希在 `fresh_mechanism_ablation_v2/artifact_manifest.json`，原始 JSONL
  保留在本地 logs，不把 v1 Shared-Credit 数字恢复为主稿证据。
- 下一步：把 v2 报告绑定到三份标准复核，检查是否能提升 M5/M13；若复核允许，再以
  最少字数把 mechanism control 写入 8 页 main.tex，并重新编译/验证 main.pdf。不要
  直接把 natural null 或 controlled synthetic drift 当作通用工具选择收益。
## 2026-10-08 — Figure 1 视觉重构与 7-inch 纸面复核

- 用户反馈 Figure 1 v21 仍被拒绝，要求两张图统一样例库质感、职责不重叠。未修改方法、实验或正文主版；只推进 AI 架构图/流程图资产。
- 新增完整 prompt 与原图：Figure 1 v22–v29（prompt v32–v39），每版均保存 PNG、prompt、manifest、generation request；v25–v27 的失败审查和中间版本全部保留。
- 视觉方向从“大分区说明图”收紧为样例库式 central-hub architecture：短标题、具体网格/寄存器/算子/卡片、白底、蓝/灰/琥珀/紫角色稳定；Figure 2 v26 继续只表达 selected event 的时间线，不引入 Figure 1 的 Features/Frozen score/residual/counter。
- v25 修正 eligible-tools→Selector，但独立审查发现 Update 停在 released-state 卡片边底（10/16）。v26、v27继续尝试垂直写入，分别仍被判为写入箭头停在边界外（10/16、11/16）。失败报告：`figures/external_review/20261007_figure1_role_v35_independent_review.md`、`...v36...`、`...v37...`。
- v28 将下方写入改为水平 `queue → Release → Update → released state`，v29 在高亮行内部加入无文字写入端口，Update 蓝线穿过卡片边框并收于端口。v29 独立审查 15/16、无硬错误，Figure 1/2 角色分离和样例库风格均通过；报告：`figures/external_review/20261007_figure1_role_v39_independent_review.md`。
- 新建 7-inch QA：`figures/qa/build_v21/ai_structures_7inch_20261008.pdf`，Figure 1 v29 / Figure 2 v26 各 7 inch 独立页；两次 pdflatex 无 warning，180 dpi 页面保留。独立 follow-up 确认主链、公式、水平 Update→高亮行端口在 7 inch 下清楚，次级字段略小但无新硬错误；报告：`figures/external_review/20261008_figure1_role_v39_7inch_followup.md`。QA PDF SHA `059907f167ba3907872ba3fa1b03f0772f70af87b50f67103c3c7e4cda6debb8`。
- 画廊 `figures/version_gallery_20261007/` 扩至 52 条记录（Figure 1 32 条、Figure 2 20 条），v29 与 v26 分别标为当前候选；所有图片 hash 校验通过。新增快照 `manifest_before_v29*`、`index_before_v29*` 和 contact sheet 快照。
- 当前候选：Figure 1 `generated/figure1/v29_ai.png`，Figure 2 `generated/figure2/v26_ai.png`；两者尚未同步 `figures/qa/main.pdf`，等待用户最终选择。主版保持不变，遵守决议 0014。
- 下一步：用户确认候选后，再保存旧 master、更新 `figures/qa/main.tex`、编译并生成新 master；若用户仍拒绝 v29，应基于具体新问题重新写完整 prompt，不在原图上做像素编辑。

## 2026-10-08 — Figure 1 中心架构重绘与职责分离

- 用户继续拒绝 v29 的视觉方向：虽然图级审查 15/16，但仍像两条串行流程，Figure 1 的评分合成没有成为唯一视觉中心。未修改实验、方法公式或论文主版，只推进 AI 架构图/流程图。
- v30（prompt v40）首次把 Figure 1 改成中央 selector hub；生成图保留，但 `select one` 文字渲染错误，不能作为候选。原图、manifest、generation request 和 prompt 均保存。
- v31（prompt v41）只修文字；独立审查 10/16，发现两处硬错误：released state 通过 `next selection` 直连 Selector，绕过 `Residual→plus→Scores`；eligible-tools 托盘孤立。报告：`figures/external_review/20261008_figure1_v41_independent_review.md`。
- v32（prompt v42）尝试局部修补，预检仍发现旁路/菜单语义问题，保留为失败版本。没有覆盖 v31 或 v29。
- v33（prompt v43）采用完整重绘而非继续打补丁：Figure 1 只保留 `Query→Frozen scorer→q⁰→plus`、`Released state→Residual→plus`、`plus→Scores→Select`、`Eligible tools→Select`、`Select→Selected tool→Feedback lifecycle→Released feedback→Update→highlighted state row`；详细 call/judge/pending/release 时序交给 Figure 2。当前原图：`paper/aamas2027/figures/generated/figure1/v33_ai.png`，SHA=`b6e6b1f532a849421e202d3dc4291c30797ee38472aebf413af1e7e3500177d2`。
- v33 7-inch QA：`paper/aamas2027/figures/qa/build_v22/ai_structures_7inch_20261008_v33.pdf`。内置 LaTeX 编译器因隔离临时目录不能读取项目图片，失败现场保留；随后用本地 `pdflatex` 两次编译成功，无 fatal、overfull、undefined-reference 或 missing-font warning。PDF SHA=`b8dcd23df681f39993d21e84912d8766fd46f9ff5c19eb4688cefc869319d29d`；Figure 1 页面 SHA=`d66d2f1b76dee7ce81740a256c933f40792a0436c36a167198b61039d4644e7b`。
- Figure 2 继续使用 v26，不改图；7-inch 页面仍为 `814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa`。两图共享白底、线宽、图标族和蓝/灰/琥珀/紫色语法，但 Figure 1 只讲计算与状态回写，Figure 2 只讲时间延迟。
- 画廊更新为 56 条记录（Figure 1 36，Figure 2 20），contact sheet 更新；所有记录图片 hash 校验 `missing=0, bad=0`。v29 标为用户拒绝，v31/v32 标为旁路失败，v33 标为当前候选；`figures/qa/main.pdf` 保持不变。
- 已更新 `SELECTED_VERSIONS.md`、`AI_SUBMISSION_FIGURES_PLAN.md`、`prompts/VERSIONS.md`、Figure 1 README 和架构重构迭代文档。独立审查 v33 已请求，报告路径预登记为 `figures/external_review/20261008_figure1_v43_independent_review.md`。
- 下一步：等待 v33 独立审查；若通过，补充联合纸面审查并交给用户选择，之后才按 ADR-0014 同步 `figures/qa/main.tex/main.pdf`。若仍失败，只按报告中的具体硬错误重新生成完整 prompt，不对 AI 原图做像素编辑。

## 2026-10-08 — Figure 1 v33 独立审查完成

- v33 独立审查完成：`paper/aamas2027/figures/external_review/20261008_figure1_v43_independent_review.md`，16/16，无硬错误；审查实际查看原图、7-inch F1/F2 页面和 TableDART/CRITIC/CollabLLM 样例。确认 Figure 1 是中心计算架构，Figure 2 是单事件延迟时间线，两图共享样例库的白底、线宽、蓝/灰/琥珀/紫语法但不重复内部模块。
- v33 的主链核验为：Query → Frozen scorer → q⁰，Released state → Residual，二者在 plus 汇合为 q → Scores → Select；Eligible tools 进入 Select；Selected tool 只连接到 Figure 2 的反馈生命周期接口，Released feedback 经 Update 写回高亮 state row。没有 state→Select 旁路、第二条反馈回路或 Figure 2 时间链。
- 7-inch QA 成功：`paper/aamas2027/figures/qa/build_v22/ai_structures_7inch_20261008_v33.pdf`，PDF SHA=`b8dcd23df681f39993d21e84912d8766fd46f9ff5c19eb4688cefc869319d29d`；Figure 1 页面 SHA=`d66d2f1b76dee7ce81740a256c933f40792a0436c36a167198b61039d4644e7b`，Figure 2 页面 SHA=`814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa`。本地 `pdflatex` 无 fatal、overfull、undefined-reference 或 missing-font warning；内置编译器的路径隔离失败现场仍保留。
- 画廊 `version_gallery_20261007/` 更新为 v33“独立审查 16/16；无硬错误；7-inch通过；当前候选”，更新前快照为 `manifest_before_v33_review.json` 和 `index_before_v33_review.html`。v29、v31、v32 的拒绝原因和原始 prompt/PNG 不变；`figures/qa/main.pdf` 未同步，等待用户选择。
- 可选的极小视觉调整只有两项：Query 卡增加小分子 glyph，或把 q 公式向下的注释线改为无箭头线；它们不是硬错误，不触发新一轮生成，除非用户明确要求继续改图。
- 下一步：向用户汇报 v29 为什么被拒、v33 如何修复，并展示当前候选路径/画廊；不修改主版 PDF。

## 2026-10-08 — Figure 1 v34 AI 微调版

- 为回应“继续优化”要求，在 v33 通过 16/16 审查的基础上生成 v34；只增加 Query 内的小分子 glyph，并把 `q = q⁰ + Δ` 下方的引线改为无箭头注释线。没有改变模块、数据边、反馈接口、颜色或 Figure 2 的职责。
- 已保存 `paper/aamas2027/figures/generated/figure1/v34_ai.png`、`v34_manifest.json`、`v34_generation_request.json` 和 `prompts/figure1/v44.txt`；v34 SHA=`e37f8dd8c0648f4b442157d54095a8c4de134705c2457470b3ab1b52fed1589f`。生成方式为 AI image edit，未做像素级手工修改。
- 画廊扩为 57 条记录，新增 `manifest_before_v34.json` / `index_before_v34.html` 快照；v34 标为“独立审查待完成，v33 仍为当前候选”。`figures/qa/main.pdf` 仍未修改。
- 已请求独立审查：`external_review/20261008_figure1_v44_independent_review.md`。审查重点是新增 glyph 是否挤压 Query、公式引线是否仍被理解为数据边、v34 是否保持 v33 与 Figure 2 的风格/职责分离。

## 2026-10-08 — Figure 1 v34 审查通过并晋级候选

- v34 独立审查报告 `paper/aamas2027/figures/external_review/20261008_figure1_v44_independent_review.md` 已完成：16/16，无硬错误，推荐候选。审查实际查看 v34 原图、v34/F2 v26 的 7-inch 页面和 topconf gallery 样例。
- 结论：Query 内分子 glyph 与文档 glyph 都清晰；`q = q⁰ + Δ` 下方为无箭头注释线；exact graph、唯一 feedback→Update→highlighted-row 回写、Figure 1/Figure 2 风格与职责分离均通过。v34 不含 provider letters、时间轴、state→Select 旁路或第二反馈环。
- v34 已在画廊中标为推荐当前候选，v33 降为上一版已审查候选；更新前快照为 `manifest_before_v34_review.json` 与 `index_before_v34_review.html`。画廊记录总数 57，图片 hash 校验 `missing=0, bad=0`。当前候选图、prompt、manifest、request 路径分别为 `generated/figure1/v34_ai.png`、`prompts/figure1/v44.txt`、`generated/figure1/v34_manifest.json`、`generated/figure1/v34_generation_request.json`。
- v34 QA 当前证据：`figures/qa/build_v23/ai_structures_7inch_20261008_v34.pdf` SHA=`b3a84b838b93dfa587780fefa2a56994b8b45161b2e2ea8c7c063168b7d1ed57`；F1 page SHA=`25598f4e2470dbd9a4b4ff517d9a98914c2caa7b89f0e220ee18e42669c0ad15`；F2 page SHA=`814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa`。两次本地 `pdflatex` 无 fatal、overfull、undefined-reference 或 missing-font warning。
- 仍未同步 `figures/qa/main.pdf`。下一步不是继续随机生成，而是让用户在画廊中比较 v34 与 v33/F2 v26；用户确认后才按 ADR-0014 保存旧 master、更新主版并编译。
- Figure 1 contact sheet 已重建为 37 条记录并加入 v34；旧 contact sheet 备份为 `version_gallery_20261007/figure1_all_versions_contact_sheet_before_v34.jpg`。当前 contact sheet SHA=`402bc45ccfb9a399b3fc29abfbf9d494b4d33a3e6b509e42b17154b309f89859`。

## 2026-10-08 — Figure 2 v27 AI 微调版

- Figure 2 v26 的独立审查为 15/16，唯一扣分是 7-inch 下次级状态说明和图例偏小，无结构性错误。为完成两图的最终纸面验收，基于 v26 AI 生成 v27，只放大 `runtime error`、`runtime ledger`、`feedback hidden until release`、`visible only at or after τ_t` 和图例文字；没有加入任何模块、时间边、provider 字母或 Figure 1 内部结构。
- 已保存 `paper/aamas2027/figures/generated/figure2/v27_ai.png`、`v27_manifest.json`、`v27_generation_request.json`、`prompts/figure2/v28.txt`；图片 SHA=`10642a1fb7e8f4df1b0bdb1caee6666c6566e0d2303cafd8c64fc2c0c10dbbb2`。v26 原图和 prompt 不变。
- paired 7-inch QA：`figures/qa/build_v24/ai_structures_7inch_20261008_v34_v27.pdf`，PDF SHA=`e668d5aa47c49fa65f1aacb940de6dedc2cfde66e43f5b87028def5e5f39b8e9`；F1 page SHA=`25598f4e2470dbd9a4b4ff517d9a98914c2caa7b89f0e220ee18e42669c0ad15`；F2 page SHA=`ab03df4ac747ba1b9568bbe2c13b279319976f145e4911654fe75c4f8044ddb8`。两次本地 `pdflatex` 无 fatal/overfull/undefined/missing-font warning，页面已实际查看。
- 画廊扩为 58 条记录，新增 `manifest_before_f2_v27.json` / `index_before_f2_v27.html`；v27 标为待审查，v26 仍为当前候选。已请求独立审查 `external_review/20261008_figure2_v28_independent_review.md`；审查重点是次级字号改善、时间因果链、runtime dead-end、与 Figure 1 v34 的角色分离。

## 2026-10-08 — Figure 2 v27 审查通过并晋级候选

- 独立审查报告 `paper/aamas2027/figures/external_review/20261008_figure2_v28_independent_review.md` 已完成：16/16，无硬错误，推荐候选。实际查看 v27 原图及 paired QA 的两页；次级字号、时间因果链、runtime failure 分叉和与 Figure 1 v34 的风格/职责边界均通过。
- v27 已在画廊中标为推荐当前候选，v26 降为上一版候选；更新前快照为 `manifest_before_f2_v27_review.json` 和 `index_before_f2_v27_review.html`。画廊共 58 条记录，图片 hash 校验 `missing=0, bad=0`。`figures/qa/main.pdf` 仍未同步。
- 当前推荐配对为 Figure 1 v34 + Figure 2 v27。两者均 16/16、7-inch 通过，F1 只讲计算架构与状态回写，F2 只讲 selected event 延迟时间线；共享 topconf 图库的色板/线宽/图标语法但不重复内部模块。
- 下一步：用户在画廊中确认配对后，才按 ADR-0014 保存旧 master、替换 `figures/qa/main.tex` 的图引用、重新编译并检查主版；在此之前不修改主版 PDF。
- Figure 2 contact sheet 已加入 v27；旧版备份为 `version_gallery_20261007/figure2_all_versions_contact_sheet_before_v27.jpg`。

## 2026-10-08 — AI 图目标逐项完成审计

- 新增 `paper/aamas2027/figures/final_candidate_audit_20261008.md`，按目标逐项核验样例库参考、AI 来源、两图职责、exact graph、风格一致、7-inch 可读性和历史留痕。
- 当前推荐配对 Figure 1 v34 + Figure 2 v27 均为独立审查 16/16、无硬错误；画廊共 58 条记录，所有 manifest 图片 hash 校验通过。目标审计证据齐全。
- 仍不自动替换 `figures/qa/main.pdf`：主版同步属于用户最终选图后的发布动作。后续任何调整都从新版本开始，不能覆盖 v34/v27 或历史记录。
### 2026-10-08 — v2 机制证据合入 paper-level 主版

- 完成：把 `fresh_mechanism_ablation_v2` 的 matched control 以独立段落合入 `paper/aamas2027/main.tex`；没有把 v2 的 scaffold-mean 数字混入旧 pooled primary tables。
- 完成：baseline 表把 fixed cyclic permutation 标为 mechanism control，matrix 说明 cyclic control；正文明确 90 streams per condition、两个 deterministic schedule replicas、$D\in\{0,1,4\}$ 和 source-aligned credit 的声明范围。
- 完成：压缩 `What does the residual correct?` 与 Artifact scope，恢复正文 8 页；第一次超页（10 页）构建和源文件变化中的构建都保留，未提升。
- 完成：`make submission` 最终通过，`build/submission_20261007_161243/main.pdf` 为 9 页（正文 8 页、References 第 9 页），warning gate、natural/stress receipt audit 均通过；p8/p9 以 140 dpi 检查无截断或溢出。
- 完成：按主版规则提升 `paper/aamas2027/main.pdf`，PDF SHA `788488c2c3d02e613f38cca0b91af6c2cbb7c10cbd19724e6b0ebe346669269a`；旧主版在 `paper/aamas2027/archive/promotions/`。
- 记录：`research/experiment_section_style_20261006/main_v2_integration_20261008.md`、`paper/aamas2027/main_provenance.json`、`paper/aamas2027/README.md`、三份标准复核已同步。
- 当前状态：主版页面/文本绑定 gate 通过；Benchmark/Baseline declared-scope gate 可进入联合复核，方法 Gate 仍 HOLD（H7 为 replay-scope partial），不能外推 natural drift、family sharing、verification/refusal 或 agent gain。
- 下一步：检查三份生效验收文档的最新分数与主稿 topic-sentence/claim binding；优先关闭 H7 的 terminal/final-state 和 crash/restart/乱序证据缺口，或明确把它们作为投稿前必须完成的实验卡；不新增无关 baseline。

## 2026-10-08 — H7 receipt closure v1 completed and audited

- 目的：关闭当前 fresh policy replay 的 terminal/final-state、checkpoint restart、logical-clock reorder、缺包/重复/篡改证据缺口；不改 benchmark、v2 效果数字或主版 PDF。
- 先冻结并修订实验卡：`task/task49-pharma-benchmark-baseline-freeze/fresh_receipt_closure_v1/experiment_card.md`。独立 adversarial review 指出 provenance 输入与 restart 语义不足，已明确可选 provenance audit、90-stream canonical 全量、代表性 restart/reorder、checkpoint 边界和 fail-closed 规则。
- 真实 provenance reconstruction 已完成（CPU、0 API、0 GPU）：四 checkpoint 对 722 development molecules 生成 2,888 probabilities；4,332 个原始 Frozen decisions；logit/fit-rate 最大误差均为 0，summary SHA `25ee8a7fc301bb27a815c42cbdfdcfed21961ee1534650ffcbab7ed4bb24aa4d`。
- receipt v3 已在 natural/controlled 各完成 90/90 streams、42,480 events、15 个 checkpoint suffix exact、270 个 logical-clock reorder exact；terminal/final state bytes、event chain、summary 和 state blobs 均经 import-free verifier 通过。v3 verification SHA：natural `ea1f1b97b782a5614e60ef0cf43146f929d120d2b4179d9cd4b4668c22ce4d40`，controlled `52cdf6d4ce951e0495d2ec72b30aa4ad0ee4e3105cfb6816e85fe1c6577cbc84`。
- 14/14 mutation（两条件各 7 类）均 fail closed；delete/reinsert 从 immutable spool 恢复为 byte-identical receipt，recovery SHA `da3250a4991a1c3d107bdece5164dab1b63ba5b4cc28a5660f3fdec68c57cb7c`。此前 471/472 schema drift 的 v1 失败现场保留，不计入通过。
- 新代码：`src/probe_headless/run_fresh_receipt_closure_v1.py`、`verify_fresh_receipt_closure_v1.py`、`check_fresh_receipt_recovery_v1.py`；定向测试 `test_fresh_receipt_closure_v1.py` 3 passed。
- 结果文档和索引：`task/task49-pharma-benchmark-baseline-freeze/fresh_receipt_closure_v1/report.md`、`artifact_manifest.json`。三标准复核已追加当前状态：M14 3→4，H7=`PASS_FOR_DECLARED_POLICY_REPLAY`，声明范围方法约 52/60；broader method Gate 仍 HOLD。
- 主版核对：`paper/aamas2027/main.pdf` SHA 仍为 `788488c2c3d02e613f38cca0b91af6c2cbb7c10cbd19724e6b0ebe346669269a`，与 provenance 一致；已单独提交并推送主版二进制。
- 已保留未完成事项：三份标准对 broader scope 联合复核；natural lifecycle drift、family sharing、verification/refusal、端到端 agent gain、hERG/AMES、完整 33-provider model-fresh 仍不属于本次证据。下一步先做标准联合审查和主稿 claim binding，不新增无关实验。

### 2026-10-08 — receipt closure wording bound to the paper master

- 目的：把已完成的 H7 receipt-closure v1 证据准确绑定到正文，避免旧文案暗示“旧 receipt 可独立重算完整 learned state”，同时不扩大论文的声明范围。
- 修改：`paper/aamas2027/main.tex` 的 Released state、Action rule、Validity/reproducibility 段落现在明确写出：v3 closure 从 sealed replay 物化 canonical state bytes，独立重算 scores/lifecycle metrics，并做 checkpoint、logical-clock、mutation、terminal/final commitment 检查；该审计只覆盖 declared policy replay，不构成新的 provider inference、offline-fit 或 effect estimate。
- 构建：`paper/aamas2027/build/submission_20261008_044332/main.pdf`；自然/controlled receipt audit 仍为 270/270 与 150/150 verified；PDF 9 页（正文 8 页，参考文献第 9 页）；LaTeX warning/fatal/undefined gate 通过；p8/p9 以 140 dpi 实际检查。
- 主版：按主版规则提升为 `paper/aamas2027/main.pdf`，SHA256=`f6c0b6ca473d9b6a65d8ef3e24fad12ce32b5aa6fc0a9beee9e9d19b308cd3d0`；旧主版归档于 `paper/aamas2027/archive/promotions/main_before_20261008T050406Z.pdf`；绑定信息见 `paper/aamas2027/main_provenance.json`。
- 未完成：method broader gate 仍 HOLD；natural drift、family sharing、verification/refusal、agent gain、hERG/AMES 和 full fresh 33-provider claim 仍不属于当前主稿证据。下一步优先做三份标准的联合复核与主稿 claim binding，不新增无关 baseline。

### 2026-10-08 — main-claim binding audit

- 新增 `research/experiment_section_style_20261006/main_claim_binding_audit_20261008.md`，逐项核对摘要/方法/结果/Artifact/结论的 claim 是否有对应证据。
- 结论：Benchmark+Baseline declared-scope gate 仍为 A=15/16、B=13/16、C=14/16；窄切片方法约 52/60，H7=`PASS_FOR_DECLARED_POLICY_REPLAY`；broader method gate 继续 HOLD。主稿没有把 controlled drift 写成 natural drift，也没有把 receipt closure 写成新的 provider inference 或 effect estimate。
- 仍需解决的科学缺口：natural lifecycle drift、family-shared credit、verification/refusal、端到端 agent gain、hERG/AMES 和 model-fresh 33-provider confirmation。后续实验必须至少关闭其中一个缺口，否则不推进。

### 2026-10-08 — fresh post-call verification/refusal confirmation and paper master promotion

- 目的：补齐此前只有 development evidence 的 post-call verification/refusal 扩展；不把它混入 MIRA-Relay 的 pre-call selector 主估计。
- 先冻结协议：`task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/experiment_card.md`。五个 development scaffold folds 只用 fit rows 选 provider 和阈值；157 个 fresh confirmation truth 只在 evaluator 侧使用；always-accept、confidence-refuse、same-coverage random、always-refuse 四个 policy 固定。
- 新增 runner/verifier：`src/probe_headless/run_fresh_postcall_verifier_v1.py`、`src/probe_headless/verify_fresh_postcall_verifier_v1.py`，定向测试 `test_fresh_postcall_verifier_v1.py` 与 receipt closure 测试共 5 passed。
- smoke：16 个 confirmation、320 policy rows，独立核验通过；action mutation 与 evaluator correctness mutation 均 fail closed。
- full：5×157×4=3,140 policy rows 和 3,140 evaluator rows；confidence-refuse coverage 84.46%、accepted accuracy 94.57%，same-coverage random 88.69%，always-accept 88.15%；pooled gain over random +5.88 pp，五折均胜出，scaffold bootstrap 95% CI [+3.34,+9.72] pp；固定 $(\rho,\kappa)=(0.25,0.01)$ utility 相对 always-accept +0.0338。结果、utility grid、receipt、verifier 和 hashes 见 `task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/report.md` 与 `experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/`。
- 新增标准扩展复核：`research/experiment_section_style_20261006/postcall_fresh_confirmation_recheck_20261008.md`。该证据支持 post-call refusal utility extension；不外推 escalation、natural lifecycle、family sharing、agent gain、hERG/AMES 或 full 33-provider model-fresh。
- 主稿新增一段 post-call verification extension，未改变 primary direct-selection tables；`make submission` 通过，receipt audit 270/270 和 150/150 verified，PDF 9 页（正文 8 页，参考文献第 9 页），p6/p8/p9 视觉检查无溢出。构建：`paper/aamas2027/build/submission_20261008_063938/main.pdf`，SHA `efef0c134e105064b10bd68b72fb5fc6ba204370e33f6b5b370195c78daf512b`。
- 按主版规则归档旧 PDF 并提升：`paper/aamas2027/main.pdf`，当前 SHA 同上；旧版在 `paper/aamas2027/archive/promotions/main_before_20261008T064733Z.pdf`，provenance 已更新。
- 下一步：对三份生效标准做一次联合 recheck，决定 post-call extension 是否进入摘要/主表；主版只接受新的 8+1 页 submission build，不覆盖历史迭代。

### 2026-10-08 — post-call extension bound to experiment matrix

- 修改 `paper/aamas2027/main.tex`：实验矩阵新增 `Post-call verification` 行，列出 157 fresh confirmation、fit-only threshold、四个 policy 和 coverage/accepted accuracy/utility 指标；不改变 direct-selection 主表和 MIRA-Relay primary claim。
- `make submission` 通过，receipt audit 270/270 与 150/150 verified，正文 8 页、参考文献第 9 页；第 6 页实验矩阵和第 8/9 页实际渲染检查通过，无 `Overfull`、`Undefined`、`Citation`、`Missing $`、`Fatal` 或 `Emergency`。
- 新构建：`paper/aamas2027/build/submission_20261008_070925/main.pdf`，PDF SHA `616296b7431a2146e99b7d0af2696f3917830f125d6c066b59cf5ee73f889985`，日志 SHA `cb026993184bc12238b2e02010ef8eacee0e68159ceaa5e6d241b1381c3ed05f`。
- 主版已提升为 `paper/aamas2027/main.pdf`，同 PDF SHA；旧版归档于 `paper/aamas2027/archive/promotions/main_before_20261008T071722Z.pdf`，provenance 已同步。
- 提交并推送：`bd47471d bind postcall extension in experiment matrix`。下一步是三份标准联合 recheck 和投稿 gate 复核，不再为已闭合的 post-call 结果重复跑同一实验。

## 2026-10-08 — 正文实验矩阵与结果表独立排版预览

- 目的：按用户要求做可直接阅读的 LaTeX 实验矩阵稿，地图与拟放入正文的结果表一一对应，不运行新实验。
- 完成：六组实验/六张结果表：E1 主比较（natural + controlled + delay 分层），E2 source-aligned credit 消融，E3 冷启动与 headroom，E4 score × update 因子诊断，E5 效率/调用预算，E6 调用后拒答次级分析。保留当前正文四张结果表的引用键；三个原结果段落升级为候选表。
- 文件：`paper/aamas2027/experiment_tables/main.tex`、`main.pdf`、`README.md`、同源导出 `body_tables.tex`；版本和来源 hashes 在 `versions/v01_20261008/`、`versions/v02_20261008/manifest.json`。这是表结构候选，不是新增生效研究主线或决议。
- 编译：内置编辑器两版均 success；最终 v02 本地两轮 PDF 导出成功，3 页，无 overfull、undefined reference、LaTeX/package warning 或 fatal。140 dpi 查看三页，证据在 `versions/v02_20261008/qa.md`、`page-*.png`、`main.log`。预览 master PDF SHA 为 `1755645be15a844735b32eb9fff601d31d18b9bc324428bd6fa460c60f9d52ba`。
- 展示：Codex 的 tex/PDF 打开请求返回 queued；另已用系统 Preview 打开 PDF。
- 数据口径：157 独立分子与每方法 4,710 记录调用分开；E2 scaffold 等权与 E1 pooled point estimate 分表。延迟和成本只由已有 summary 重聚合，没有 API/GPU/provider 调用。现有 p95 数字实际是各流 p95 的均值，已在表注写明。
- 占位：统一 live learner state 测量保留 TBD，不能以含历史的序列化 receipt 大小冒充内存。post-call 标为既有留出集次级分析，random 是 evaluator-only conditional null。
- 未修改：paper-level `paper/aamas2027/main.tex/main.pdf` 和六份生效文档。最终正文表号由 LaTeX 合入时生成；README 记录了每张拟定表与当前正文的对应关系。
- 留痕：写日志时一次 Python stdin UTF-8 错误，改用 apply_patch 完成追加；不影响源码、PDF、数据或编译。
- 下一步：依据用户对预览的反馈调整布局，再将同源 result blocks 合入全文、复核 8 页浮动布局。此前参考文献补全尚未完成，需继续核验优秀论文的引用数量/分布，只补与论证相关的已验证文献。

### 2026-10-08 — post-call 修正、严格校验与唯一主版同步

- 修正此前 TODO 和正文中两处错误：random 对照的 accepted-accuracy CI 应为 [+2.77,+9.29] pp；[+3.34,+9.72] 属于 always-accept。157 分子已用于先前 holdout routing，本轮是 secondary analysis，不能计作新的 untouched E6。旧 report/recheck/manifest 快照保留。
- 独立 verifier 验证 3,140 policy +3,140 evaluator rows、232 fit/summary fields；26 tests passed，涵盖22 corruption cases。原始阈值 tie-inclusive count 实际与历史数据一致；怀疑真实 tie bug 的错误测试在 strict_audit_v3/runs/001 保留，成功审计在 runs/002。
- 统计重算保存执行前配置、input/source hashes、源码快照、全部 bootstrap draws：analysis_audit_v3。固定 utility 相对 always-accept +0.0338，95% CI [+0.0088,+0.0609]；对照相同调用成本下，拒答 break-even 为 rho≈.467。没有新 API/GPU/provider inference。原 full_v1 的同期 provenance 缺口已披露，未补造。
- 正文新增4行数值表与效用解释；构建前后使用 bind_postcall_results.py 检查表与 CI。去掉重复说明后，build/submission_20261008_082437 完成：正文恰好8页、References第9页；p6/p7/p8/p9实际检查。溢出/中断版本保留。
- 主版 paper/aamas2027/main.pdf 已原子提升，SHA 90deeeeda2bd15892a7d0d74df8cd1d99d12c8dfa4802c9e22a3b51494d9f1d9；旧版归档 archive/promotions/main_before_20261008T083109Z.pdf。README/provenance 已同步。
- 主报告 research/experiment_section_style_20261006/postcall_correction_and_master_20261008.md；修正复核和 task49 postcall report 已更新。
- 独立科学审查建议先检验条件第二次调用净价值，再验证 delayed update 附加收益；候选推导、目标移动反例、强 cascade 对照和停止规则已落 conditional_second_call_v1/design_review.md。尚未运行该候选，且不替换生效方法。
- 完整科学 gate 仍 HOLD；下一步先核对 Cascade Routing/Online Cascade Learning 的反馈合同与 paired-label 规则，然后做 development 小测试。目标保持原范围。

### 2026-10-08 — 全文主版规则确认与远程同步状态

- 按用户本轮要求落盘决议 0016：全文唯一主版 `paper/aamas2027/main.pdf`，独立迭代完成后合入源码，经检查再提升；图稿 QA 与实验表预览不能代替全文主版。
- 已有修正与主版提交为 `eff4f346`；当前 PDF 来源为 `build/submission_20261008_082437/main.pdf`，正文8页、References第9页，README/provenance 和旧版归档均已保存。
- `git push origin codex/progress-20260924` 失败；随后只读 `git ls-remote` 也返回 `Connection closed ... port 22`。尚未确认远程收到该提交，本地主版和提交完整保留。
- 未启动新实验或修改科学结论。下一步在远程连接恢复后推送，并按现有候选评审推进条件第二次调用的近邻与反馈合同核对。

### 2026-10-08 — 条件第二次调用五折诊断与失败定位

- 上一goal turn有实际进展：唯一主版规则与提交已保存；本轮继续科学缺口，未将排版完成作为最终目标完成。
- 最近邻原文/源码审查完成：OCL正式20页PDF、固定官方代码和CR原文已落conditional_second_call_v1/literature。OCL已有付费选择性在线更新；CR在共同终局loss下与G决策等价，不能换名重复比较。近邻合同和可复用边界见nearest_contract.md。
- 执行前冻结A/B/C三段划分、9参数冻结验证器、16参数条件value与12参数pair-mean、因果1.25调用预算、exact offline DP上界和重跑预算的scaffold bootstrap。原先贪心oracle不是真上界的反例在运行前修正。
- 真实缓存运行完成：13定向测试通过；fold0 smoke .936秒，完整5fold 5.576秒，722分子。条件utility−.20407，简单cascade−.19594，差−.00813，95%CI[−.01406,+.00335]；未通过预设升级线，不声称显著更差或任何在线收益。
- 独立脚本不导入runner，核对23,820条A训练输入、1,518条B目标、2,888条policy+2,888条evaluator；检查A logistic目标梯度、B ridge方程、全部动作/成本、用min-heap核DP上界并重跑全部10,000次bootstrap，verified。full summary SHA=8edd5955077387be843bfb57d05b29c892a343d2f2a84fb9855aae6a2fd44542。
- 事后分解：条件策略162次追加，136次动作未变、19次降低损失、7次升高，净收益总和−1.12；全信息预算上界仍+.04900/input，152题有正收益候选。失败指向当前价值表征/学习，不能说整个目标无可用空间。B是506 molecule-fold observations、341个去重分子，v1不精确字段名保留并在diagnosis_v2修正。
- 数学候选：同动作paid pair的gain=−κ可立即知道；其余等待真值。单次Gram写入+响应残差修正可在pending排空后精确恢复同一已取得样本的batch ridge，但中间代填偏差与探索支持必须另验。独立只读审查已保存在early_feedback_candidate.md，不宣称新颖或有效。
- 规范结果：task/task49-pharma-benchmark-baseline-freeze/conditional_second_call_v1/report.md；全部原始日志：experiments/logs/task49_conditional_second_call_20261008/。零API、零provider forward、零GPU、未读confirmation。主版PDF未改，科学gate仍HOLD。
- 下一步分开做状态表达修复与提前反馈干预：先保存有符号q及终止动作做matched开发比较；另核对partial/informative-delay近邻，再在同一paid stream检验提前标签账目、偏差与delays0/1/4。效果通过后补CR强对照与闭环agent实验，不同时改多个组件。

- 本轮提交 `eb041d83` 已保存全部诊断代码、证据与报告；再次推送仍因远程SSH关闭失败。本地主版仍与provenance一致，未将负结果候选合入正文。

### 2026-10-08 — 再核对唯一全文主版

- 用户要求维护固定 main PDF；规则已由决议 0016 双方确认，本轮复用该决议，不重复新增。入口是 `paper/aamas2027/main.pdf`，源码合并到 `main.tex`，迭代在下层 `build/submission_*/`，旧主版在 `archive/promotions/`。
- 本次实测主版哈希与构建/provenance 一致，正文恰好 8 页，References 第 9 页。核查记录：`paper/aamas2027/archive/master_checks/20261008T093501Z.json`。
- 实验表重排仍在共享源码中迭代；最新已检查的 `submission_20261008_092630` 有 9 页正文/第 10 页参考文献，manifest 未完成，不能覆盖已通过的主版。后续完成合并、8 页检查及受影响页面目视检查后，通过 `make promote-main SOURCE=...` 同步，不能把独立表预览完成混报成全文已更新。
- 并行主线未停止：conditional second-call v2 的 worker 已完成 11 tests、fold0 smoke 和 5-fold full，报告未达到预设收益线；原始记录在 `experiments/logs/task49_conditional_second_call_v2_20261008/`，尚待主线程独立核验，暂不提升为论文结果。
- 提前反馈近邻审计已落 `conditional_second_call_v1/partial_feedback_literature/report.md` 和 `baseline_contract.md`；五篇正式论文的来源、原文和 hash 已保存。主要可复用内容是刷新 pending 预测的等价对照，以及即时动作变化概率/延迟条件收益的分解基线。候选推导与现有方法的关系需纳入下一轮方法审查，未变更生效方法。
- 下一步：独立核验 v2，形成规范结果报告；完成表格合并的 8 页布局后更新主版。完整科学投稿 gate 仍 HOLD，长期目标继续生效。

### 2026-10-08 — v2 独立核验与成本隔离推导

- 上一 goal turn 分类为 progress：保存了主版实测绑定记录、决议入口与未完成合并状态；本轮推进方法证据，未把 8 页合规当科学目标完成。
- v2 已独立核验：20 个新增模型、1,518 条 B 记录、5,776 policy+5,776 evaluator、10,000 组重采样全部重算；四项重新封 manifest 的实质篡改均拒绝。原始 full summary SHA `07724e6b1aab7a280c0aac2d99ccfc78dd6c20588bba258abe75220f666e9e00`；独立 verification SHA `0b5819ed91c19927315731ca18be28eb273c0774b07fa15cf5f032e9b6a963bd`。
- 科学结论：signed_refusal 每题效用 −.198061，简单 cascade −.195942；178 次追加中 142 次终局动作不变，未通过原预设修复线。规范结果和全部条件见 `conditional_second_call_v2/report.md`，未挑赢家或扩大 basis 搜索。
- 新发现并修正解释：旧 bootstrap 拼接 scaffold blocks，改变原始逐分子到达顺序；预算依赖顺序，旧 `ci95` 只保留为历史重放敏感性范围，不解释为原到达机制的常规 95% CI。v1/v2 规范报告均已更正，原始数字不改。
- 五篇原文审查和数学对照已落 `conditional_second_call_v1/partial_feedback_literature/`；标准响应修订/预测代填不能独自承担创新，但即时动作变化状态可作为下一轮结构假设。`conditional_second_call_v2/next_method_review.md` 证明已知成本偏移，枚举所有终局损失变化，并要求分开容量与时序收益。
- 已冻结且获独立数学 GO 的下一张卡：`conditional_cost_isolation_v1/experiment_card.md`。同 16 维、同 B、同 λ，仅比较拟合净收益 vs 拟合改善后显式减成本。128 个逐分子均匀排列只报告描述性顺序敏感性，不造总体 CI。worker 已获执行 GO，独立 verifier 并行准备；未提前填写效果。
- Task49 索引已链接新证据；生效研究合同、三份评分和主版未因诊断自动提升。下一步完成成本隔离的真实运行/独立核验，再决定提前状态与延迟收益的同模型消融。

### 2026-10-08 — 成本隔离实验与独立恢复完成

- 实验严格按冻结卡完成：7 tests；smoke .526 秒；完整五折与 128 次逐分子排列 2.069 秒。无参数搜索、provider/API/GPU 调用或 confirmation 读取。
- 原订单 known_cost utility −.203878，对原 net-g ridge 仅 +.000194，对 cascade −.007936。随机 128 订单平均相对旧模型 −.000258，仅 37/128 胜；相对 cascade −.005543，仅 7/128 胜。只作固定样本的顺序敏感性，不称总体 CI。
- 精确分解：取消 10 次调用效用变化 −.15，新增 21 次变化 +.29，净变化 +.14/722；总调用反而增加 11，拒答少 1。确定成本的正则偏移存在，但没有修复当前价值预测的主要不足。
- 独立 verifier 不导入 runner：核对 1,518 条 B、5 模型、3,684 分数偏移、645 折内订单、465,690 决策×15字段、2,888 旧控制和完整调用变化账目，通过。verification SHA `917464758d26f7a2d88b4170122c1f3feca0bc27d7b7f2aa3605c73df6d5a496`。
- 规范结果：`task/task49-pharma-benchmark-baseline-freeze/conditional_cost_isolation_v1/report.md`；完整日志：`experiments/logs/task49_conditional_cost_isolation_20261008/`。v2、部分反馈原文审查、统计解释更正及成本隔离卡先已提交 `d3e001e9`；本轮结果待本地定向提交。
- 下一步：以外置已知成本为共同对照，先冻结同一分解模型/同一已取得 pair 流的即时 vs 延迟动作变化标签比较，再做预算相同的闭环效用；不再用扩展静态 basis 追分。另有主稿区间口径只读审查进行中，需区分固定记录的 scaffold 重采样与改变到达过程的重放，不能把诊断统计问题未经核查扩展到主表。
- 完整科学 gate 继续 HOLD，长期目标未完成；主版仍遵守源码合并后检查、提升的规则。

### 2026-10-08 — 主稿统计口径核查与本轮收尾

- 成本隔离结果、代码、原始记录、独立核验与规范报告已定向提交为 `c2ce0a53`；v2 与文献/推导提交为 `d3e001e9`。远程 SSH 此前连续关闭，本轮没有宣称推送成功，也未因它阻塞本地科学工作。
- 主稿区间只读审查已完成：`research/experiment_section_style_20261006/primary_uncertainty_scope_audit_20261008.md`。当前 main/source-credit/post-call 都重采样已记录结果，不重跑策略，因此不继承 v1/v2 的到达顺序错误；原数字与主版保留。
- 新的 P1 是推断对象：自适应策略的历史使不同 scaffold 结果相关，固定结果的重加权不能独自证明新轨迹上的总体 95% 覆盖率。post-call 静态 gate 在固定模型条件下更适合 cluster 推断；random 对照仍条件于固定接受抽样，不是跨随机化显著性。
- 已安排只读方法协作者把问题变为 `primary_arrival_robustness_v1/experiment_card_draft.md`：同 157 已查看分子、同冻结 prior/菜单/延迟/实际现代对照，重新随机排列后完整执行反馈与策略；明确轨迹随机性和 scaffold 泛化的区别。当前只是草案编写，未授权执行草案、未运行这批实验。
- 下轮优先审查这张卡和可复用 runner，补主稿依赖的到达顺序证据；同时保留同模型即时/延迟动作变化标签作为方法下一候选，先冻结再运行。不要重新做已独立核验的 v1/v2/成本隔离，也不要修改阈值追胜。
- 本轮属于实际进展：排除了两条不充分修复，保存负结果，完成独立恢复，找到并分开统计缺口；完整目标仍未达成，科学 gate 保持 HOLD。

## 2026-10-08 — 实读获奖论文实验页，整改图表并同步唯一主版

- 目的：解决用户指出的“乱”，指标是能迅速找到比较行/列、清晰分组、可读字号、无冗余列、估计口径不混、正式正文仍为8页。没有新实验/API/GPU/provider inference。
- 实际参考：VAR p7、SAM p8，以及官方名单核实的两篇 ICLR2024 Outstanding（Registers p7/p8/p17；Never Train p4/p5/p9）。原始PDF、页面截图、字号/行距测量与哈希在 `paper/aamas2027/experiment_tables/references/`，归纳在 `reference_style_report.md`。此前 DoLa 的 Outstanding 误标已更正并保留旧快照。
- 整改：六张结果表统一9pt表体/8.5pt表注；方法族行组、真实两级表头、自然表宽和统一精度；移出恒定Calls、整列TBD和重复W/R计数；延迟分层改为两幅共享图例/单位的数据panel，MIRA蓝色、其他方法低饱和线型。数值/CI未改，natural null、scaffold/pooled区别、p95均值、reused-secondary/E6 conditional null均保留。
- 预览：`experiment_tables/main.pdf`已提升v05，两页，SHA `eb61a65c286f9f37ba20ed1c198940c84e0fc366c94be71b615c5aa45a315ab4`；v01-v05全保留，v03尾页失败也留档。内置编译success、本地两轮无warning；v04独立审查后补回W/R/C/N，v05闭环通过。同源导出脚本和六表/数据图LaTeX已保存。
- 全文：六表合入 `paper/aamas2027/main.tex`，正文Tables3-8、Figure3；E6只改renderer，输入/hash/count/statistic校验代码逐字不变。两个超页构建保留；最后 `build/submission_20261008_093454/`通过既有receipt270/270+150/150、post-call绑定、8+1页、overflow/reference/fatal gate。已知4underfull vbox、1underfull hbox与模板ifx诊断仍存在，不称zero-warning。
- 视觉：独立审查实际看候选2 p5-p8；最终闭环p8/p9通过。根代理实际看最终p5-p9，无裁切/重叠，结论在p8结束、References从p9开始。报告在 `experiment_tables/reviews/`，最终render+JSON QA在该submission目录。
- 主版：按决议0016归档旧版并提升 `paper/aamas2027/main.pdf`，SHA `67fcf08e3d4eae094c072428eb3e98a17f5f833d90d2b87a183d6fed76141eec`；旧版 `archive/promotions/main_before_20261008T094758Z.pdf`。README/provenance/合入报告已同步，系统Preview已打开；Codex面板请求为queued。
- 边界与下一步：本轮完成实验展示整改，完整科学gate仍HOLD，六份生效主线/标准未更改；标题和原Figure1/2资产选择未改。前序参考文献补全仍未完成，下一步继续核验与论证相关的引用；不以本轮排版改善代替科学验收。未提交网站或执行新的训练。

## 2026-10-08 — 独立水印实验目标 Guide

- 用户要求把合理预期目标填入独立guide PDF。本轮只做写作/展示，不运行实验。目的与衡量指标已先写入 `paper/aamas2027/experiment_tables/guide/README.md`：六组一一对应正文Tables3–8/Figure3，M/T分明、每页水印、计数/差值/上界/utility可验算、真实主版隔离。
- 目标：E1 natural4278/4710=90.83%，controlled4182/4710=88.79%；净增48/58次正确记录，相对Frozen净48/726。E2 natural Local−permutation目标1.00pp；E3两seed591/590 out722；E4同corrected prior下1770/1730 out2166；E5选择≤30/35µs、更新≤5µs；E6同663接受量W28，accepted accuracy95.78%、utility−.0845。这些是规划情景，不是预测、实验结果或新预登记。
- 来源与验算：10份实际来源JSON/CSV及hash记录在target_spec；由原有sealed输出只读算fresh natural逐题oracle150/157=95.54%，不同于固定provider控制90.45%。独立methods auditor核对全部锚点/hash、整数和delay加总、rescued−harmed、E2contrast、oracle约束和E6utility，PASS。不填写目标CI/p值或未测live memory，不生成伪实验事件。
- 版本：v01警告版、v02caption段落错误版、v03/v04孤立尾页版完整保留。v05目标与10来源hash全部不变，压缩重复说明；最终本地顺序两pass，无Warning/Overfull/Underfull/undefined/fatal，3页。早期两次v01导出误重叠，记录在其qa，最终v05顺序导出不依赖它。根代理和独立视觉审查员实际看全部3页，水印/页眉/页脚、M/T、最后source段落完整，PASS。
- 交付：`paper/aamas2027/experiment_tables/guide/guide.pdf`、guide.tex、README、target_spec、arithmetic_audit、manifest、reviews；v01–v05在versions/。Guide只作为目标稿；真实main.tex/main.pdf和实测表预览PDF的三个hash与运行前一致，未进入真实结果导出/binder。
- 边界：已被检查的157题不能为新候选冒充新confirmation。E1–E6是展示编号，标准E6的untouched confirmation另有含义；目标稿不替换六份生效文档或决议，完整科学gate仍HOLD。下一步由用户审阅目标/展示；科学执行线程需要冻结候选与新评价合同后用真实结果验证目标，不能反复调旧集凑这些整数。

### 2026-10-08 — 用户主版要求落实后的独立核对

- 复用已确认的决议0016，不新增重复决议。固定全文入口 `paper/aamas2027/main.pdf`，候选构建在下层 `build/submission_*/`，旧主版保存在 `archive/promotions/`。
- 独立只读核对通过：主版与 `submission_20261008_093454` 逐字节相同，PDF SHA `67fcf08e3d4eae094c072428eb3e98a17f5f833d90d2b87a183d6fed76141eec`；当前源码与构建快照匹配，provenance/manifest/日志及旧版归档有效。实测正文8页，References只在第9页开始。没有为重复核对重新编译或重复归档。
- 主线中间材料继续保存：`primary_arrival_robustness_v1/experiment_card_draft.md` 和 `implementation_review.md` 已收齐统计设计及代码审查。后者说明主表与机制v2的score/浮点差别、可复用缓存、紧凑回执合同和资源边界。128-order实验尚未运行，不能提前计作结果。
- 下一步：先修订并冻结arrival实验卡，完成原订单逐动作复现与资源冒烟，再决定完整执行；科学gate保持HOLD。已完成的论文修改今后均须合入全文源码、编译及8页检查后同步唯一主版。


### 2026-10-08 — 到达顺序完整重放、独立核验与主稿合并中

- 目的：补齐三份标准中 C4/B13 所需的完整自适应到达顺序证据；不以固定记录重加权冒充新轨迹不确定性。执行前冻结完整现代对照、128 均匀排列、主比较 MIRA−SGD、统计单位、否定条件及资源界限，原始顺序只作复现/资源冒烟。
- 原序 smoke_v1 因比较器逐条解压 NPZ 过慢而中断，现场保留；只改解码效率，smoke_v2 完成 480 流，独立轨迹复现通过，再签资源 GO。11 个定向测试及独立变异测试保留；没有改模型/超参追分。
- 完整 128 排列执行 61,440 流、9,646,080 次决策；530.51 秒、峰值 RSS 323.7MB、压缩回执 3.145GB。真实缓存 + 实际在线更新，零 API/provider forward/offline refit/GPU。full_v1 results SHA=dbaf3e64f9f3695b0d0a20ade3124b25b1732e24cacad46e76e8ec0e12044946。
- 独立 full_v1 全部策略状态/分数通过，末尾 secondary histogram 因等价百分比运算的 1.42e−14 末位差而分箱不同，失败保留。固定浮点表示合同并保留最小反例后，full_v2 重新核对全部 128 排列，38,584,320 个分数、全末态/RNG/主次统计精确一致。verification SHA=99fb3c3c0d0e9995641bdcf22f4c5e61f5333b9c13c182ae60585dc26e2a87b9；累计 CPU 1040.78秒<1800秒。
- 主比较受控 MIRA−SGD +4.736pp，MCSE .212，近似均值区间[+4.317,+5.156]；125/128胜，精确正收益概率区间[.9330,.9951]。自然 MIRA−Frozen −.287pp，区间[−.372,−.202]。均针对固定已查看队列的到达分布，不是新 confirmation、新 scaffold 或自然部署漂移。
- 事后错误分解先落卡：全157分子×3840配对暴露、124scaffold、两固定size组完整留存。natural singleton净+2034，多分子scaffold净−3764，解释pooled负/等scaffold正。根代理另脚本独立从回执恢复314分子行/248scaffold行并通过；不删样本、不换指标、不认定化学根因。
- 规范报告 primary_arrival_robustness_v1/report.md；完整/失败/独立/分解证据在 experiments/logs/task49_primary_arrival_robustness_20261008/。大体积原始数组留本地，完整hash清单保留。
- 写作协作者核对全部新旧数字和8个公式块，修正natural条件/随机到达混淆、credit范围和same-contract表述。v03合入后实测正文9页/References10，未提升主版；v04把原始与128-order均值合为10行5列主表，9pt字体和全部baseline保留，原详细表单独存档。一次merge断言写错浮动体数量，shell继续启动冗余构建；已定向终止，interruption.json保留，源码/主版未被该失败修改。修复后当前build为submission_20261008_113459，尚待最终布局与主版提升。
- 下一方法材料：概率一致性推导获独立审查，但不能从harm/Brier认定根因；B-only融合诊断卡已草拟并独立审查，需统一S/U完整分组键和明确分流谓词。未运行该诊断、未读C结果、未改生效方法。主版规则复用决议0016，科学gate继续HOLD。
- 本轮早先只读远程探测仍遇SSH连接关闭，不能称已推送。后续先完成8页主版合并与归档，再审查/冻结下一项有鉴别力的科学干预。


### 2026-10-08 — 新证据已合入并提升唯一8页主版

- 科学结果/独立核验/错误分解/理论诊断卡已保存为本地提交 `fffd84c5`。原3.145GB紧凑逐事件数组留本地，Git保存输入、汇总、完整hash清单和恢复/审计记录；不称远程备份已完成。
- v04合表仍出现最后post-call跨栏表单独第9页，全部正文文字已在8页内；保留 `submission_20261008_113459`。v05只交换完整post-call与cost小节，所有句子/公式/数据绑定段不变，提前浮动位置。
- 最终 `submission_20261008_114253` 完整编译通过：正文8页、References仅第9页；270/150原轨迹审计、前后post-call绑定、结果单元格与来源一致性通过。前两遍使用原生draftmode，最终遍实际输出；没有改字号/边距/官方class。
- 根代理实际看p1/5/6/7/8/9；独立审查实际看全部9页，主表10行5列、9TeXpt通过。无overfull/undefined；6underfull、已知模板ifx诊断及图1旧末行约9PDFpt越框小项明确保留，不称零瑕疵。
- 主版 `paper/aamas2027/main.pdf` 已原子更新，PDF SHA `be5e1f50da9ba5313ef5bd2159c288d2a2e9de84ddd271569900562a5c6b3cdc`；source SHA `0377e70fe9aec39158651b25bff6a898dbc9f4f13957cd478ec2cc1b02226930`。旧主版归档 `archive/promotions/main_before_20261008T115550Z.pdf`；README/provenance、迭代入口与规范报告已同步。决议0016复用，无新重复决议。
- B-only融合诊断卡已按独立审查修正完整比较键和布尔分流；原稿快照保留，仍draft、尚未执行。下轮先完成该卡的最后冻结审查，区分融合与价值学习问题；不重新调旧确认集。图1旧排版小项可另作有边界的图稿修复。完整科学gate仍HOLD，长期goal未完成。

- 主版交付收尾：再次核对 main.pdf、main.tex 与来源构建哈希全部一致；规则沿用决议0016。源码、主版、迭代历史及关键构建核验记录作本地定向提交，不重复编译或重复归档。科学gate仍HOLD，下一项仍是B-only融合诊断卡的最后审查。

## 2026-10-08 guide v07 adversarial review
- Status: completed read-only adversarial review.
- Artifact: `paper/aamas2027/experiment_tables/guide/reviews/guide_v07_adversarial_review.md`
- Conclusion: v07 is useful as a fixed-cohort conditional reference, but cannot alone support MIRA superiority/generalization; review assigns 58/100 and Major Revision.
- Required follow-up: label reference-only scope, complete baseline fidelity/nearest-work checks, freeze candidate-specific matched replay, and report practical-margin paired uncertainty.
- Next step: parent agent integrates this review into the joint gate decision; no main Guide edits or new experiments were performed.

### 2026-10-08 — B-only 融合诊断执行与独立核验

- 目标：在不读取 C、策略汇总、bootstrap 或 confirmation 的前提下，把“第二输出没有带来收益”拆成终局融合、选择条件和 gain 价值预测三个竞争解释。执行前冻结卡 `conditional_second_call_v2/fusion_diagnostic_card_v1.md`，SHA=`b0da0f3f72fbe08d3beacd8073a2f2754f38554d67283fe387e2548aee9fc319`；独立方法审查只修订分支4的正面谓词，未改数据或模型。
- 真实 B-only 分析完成：1518 个 S 行、6072 个 U12 行、3036 个 U10 行、506 个分子级 gain 审计；CPU 0.26 秒、产物约5.9MB。白名单只含两份 B 回执和十份每折冻结模型元数据，零 provider/API/GPU。
- 分支2触发：固定 `K=(a=0,b=3,d1=拒答)` 在 fold0/1/3 同时满足 S 和 U 的风险差、Brier差均为正，且每组支持≥10个 entity。分支1只在一个组/一个 fold满足；分支3有3个fold但按优先级不先处理；分支4只有2个fold，未触发。独立 verifier 重新重建6072个有向pair并核对选中组，verification SHA=`3965c824d0e1d4c659b7f8db4de74d4d202f22e9a8ee4dbb86963f390f4b9b2d`。
- 结论改变下一步：不再扩展当前 gain basis；下一张实验卡只比较终局融合候选 vs ignore-second/同预算 cascade，保持首选、pair流、阈值、损失、成本和训练权限不变。融合干预若不超过最强合法对照，停止该候选；本轮不改生效方法、不进主稿、不关闭科学投稿 gate。

- 更正上条诊断完成状态：v1实现不满足冻结卡。U10漏singleton、ranking漏选计算错误、跨fold去重和输入manifest核验/完整分组未闭合；旧独立脚本只核一个子组，不能称完整验证。原报告/脚本保存于real_v1，contract_failure.json列偏差。不得据此直接启动新融合干预；下一步修同一冻结协议、执行v2并独立核验，不改阈值。

## 2026-10-08 — Guide 与实验 Gate 关系判定、公开 baseline 结果归档

- 用户问题：如果实验矩阵与 Guide 一致，是否可以关闭实验 gate、达到实验验收标准。
- 判定：不能仅凭一致关闭。Guide v07 通过的是固定已查看队列和既有策略的联合分布/条件预算参考；固定队列顺序参照科学复核 22/24，v06 条件参考 21/24，未来候选预测仍 UNESTABLISHED，反方审查 58/100 Major Revision。
- 依据生效标准：Gate C 要求 C1--C4 每项≥3/4、总分≥12/16、E1--E5 不缺；还要所有主 baseline 同协议闭合、候选自己的 raw/recovery/paired uncertainty/failure-unknown/cost/negative evidence。Guide 数字不能替代 candidate-specific matched replay，旧策略 SD/covariance/win rate 不能继承给新候选。
- 当前最新记录仍为 Gate A=11/16 HOLD、Gate B=11/16 HOLD、Gate C=10/16 HOLD。Guide 与矩阵一致最多支持 C3 的方向合理性和 C4 的统计模板，不能自动关闭任何一门。157 题已查看，不能承担 Stage-B untouched confirmation。
- 多角度材料已保存：`paper/aamas2027/experiment_tables/guide/reviews/gate_closure_decision_v1.md`、`guide_v07_distribution_coherence_review.md`、`guide_v07_adversarial_review.md`。coherence review 核对分母、统计单位、区间、协方差和正文映射一致；建议明确原始顺序与128-order均值、统一“MIRA”含义。
- 公开 baseline 结果已落 `~/work/reasonable_results/pharma_tool_selection_public_baselines_v1/`：两篇一手来源提供 LinUCB/LinTS/BayesianRouter 的跨域参考、URL、PDF hash、表格定位和不可比性说明。不得直接把公开 0.6166/0.6430/88.23% 填入 BBB Guide；只能作为 baseline 家族与数量级的动机锚点，本地同合同结果仍需独立运行。
- 本轮没有新训练、新 provider/API 推理、新 GPU 任务或主版修改；Guide 主版仍 v07 六页，真实论文结果与目标 Guide 分离。下一步应先按 gate_closure_decision 的清单补 candidate-specific E1--E5 与 baseline fidelity，再重新评分，不为贴合 Guide 追调数字。

### 2026-10-08 — B-only 融合诊断 v5 完成并独立核验

- 前一版实现审计后确认未满足冻结卡：U10缺singleton、ranking miss错误、跨fold实体门槛/manifest/完整分组/关联审计不足；v1–v4均保留，失败清单见 `real_v1/contract_failure.json` 与 `implementation_independent_review.md`。没有用这些版本改变方法。
- 修复后按同一冻结卡执行 `real_v5`：S=1518、U12=6072、U10=5060、molecule gain=506，完整 group statistics=2252、pair correlations=36；0.73秒、峰值RSS112,590,848 bytes、9.33MB。B hash、artifact manifest、v2 projection/base feature和model lineage均核验。
- 独立 verifier 不导入runner input loader，重新重建6072个有向pair，核对U10 singleton/pair总数、完整统计/相关性记录、分支证据；verification SHA=`b86a4ff3b0416543aa59132952aea2d540a0a872ca80bdbbad2f474eec8e17f0`。
- 分支2触发：`K=(0,3,拒答)` 在fold0/1/3的S和U风险差/Brier差均严格为正，支持分别≥10，跨折去重entity=63；分支1/3/4不优先。结论只改变下一项开发干预：冻结单因素终局融合对照，保留ignore-second/cascade/强合法baseline；不改论文、不改变生效方法、完整科学gate仍HOLD。

### 2026-10-08 — 融合诊断 v6 最终合同核验

- 对 v5 合同审查的剩余问题逐项修复：v2 projection 每行按冻结 transform 重算并核对；v2 old-model lineage 与 manifest hash 校验；group statistics 补齐所有固定空格；ordered-pair/first-action 与 action transition 分开；pair correlation 扩展到 all/四个 actual-first 子集、fold/all 共180格，并改为真实 provider 预测错误指示相关；U10 保留 singleton 和 pair 共5060行。
- 最终 real_v6 运行按同一冻结卡完成：S1518、U126072、U105060、gain506、group2232、transition324、correlation180；0.91秒、峰值RSS112,640,000 bytes、9.47MB。未读C、策略、bootstrap、confirmation，未调用provider/API/GPU。
- 独立 verifier 核对real_v6选中分组、U10总数、统计/相关性行数、branch decision，verification SHA=`6ce7265ea97fc33e68e6fc5dcd83a0344f9b6460a09e725bd46f6b6c3b0646d0`。分支2仍为`K=(0,3,拒答)`，fold0/1/3，跨fold去重entity=63。
- 最终结果报告：`conditional_second_call_v2/fusion_diagnostic_report_v3.md`。下一张卡仅冻结终局融合干预 `conditional_fusion_intervention_v1/experiment_card.md`，候选CGEF与原verifier/ignore-second/cascade在同一已取得pair流比较；卡尚未执行GO，生效方法、主版和科学gate不变。
# 2026-10-08 — CGEF 固定采集终局融合干预

- 目的：按 `task/task49-pharma-benchmark-baseline-freeze/conditional_fusion_intervention_v1/experiment_card_v2.md`，只替换终局融合规则，区分“联合分布假设不足”是否是 B-only 分支2的可修复原因。没有读取 C、confirmation、provider/API 或 GPU。
- 已执行：A fit 722 行、B paired 1,518 行、506 个 fold–molecule；CGEF、原 masked-logistic、post-paid `ignore-second` 使用同一已取得 pair 流、动作阈值和 `kappa=.01`。原始产物在 `experiments/logs/task49_conditional_fusion_intervention_20261008/cgef_v1/`；规范报告在 `task/task49-pharma-benchmark-baseline-freeze/conditional_fusion_intervention_v1/report.md`。
- 结果：pooled CGEF−masked-logistic utility `+0.000000`（约 `8.8e-19`），CGEF−ignore-second `−0.000659`；仅 fold3 相对原规则超过 `+.01`，相对 ignore 没有 fold 通过。按卡内停止线，CGEF 不进入闭环，不调 covariance shrinkage 或 Gaussian basis。
- 独立核验：新增 `src/probe_headless/verify_cgef_intervention_v1.py`，不导入 runner，重算五折参数、1,518 行、506 entity 和六行聚合；参数及逐行结果最大绝对差均为 0。`independent_verification.json` SHA `e77d35e07a7dcd8197dcbbf60d383d44205530631c85a86bd17c5d9fe5957eb3`。
- 文档：结果已追加 `task/task49-pharma-benchmark-baseline-freeze/report.md`；负结果、输入 hash、卡 hash 和复算命令均保留。`paper/aamas2027/main.pdf` 未修改，8 页主版规则不受影响；科学投稿 gate 继续 HOLD。
- 下一步：不再追 Gaussian 终局融合，先审查/冻结“同一 gain/value 模型下即时 vs 延迟反馈动作变化”实验卡，再决定预算相同的闭环 acquisition；每一步继续先写合同、独立核验，再运行。

## 2026-10-08 — 投稿范围 baseline receipt join 完成

- 目的：解决旧 broad audit `11/16,11/16,10/16 HOLD` 与 Stage-B v3 窄 scope `15/16,13/16,14/16 PASS` 并存，以及 GraphRouter/Laya-online 在 baseline 名单中角色不够清楚的问题。没有运行新 provider/API/GPU/confirmation。
- 产物：`task/task49-pharma-benchmark-baseline-freeze/baseline_scope_lock_and_receipt_join_v1/`，含 `matrix.json`、`verification.json`、`report.md`；生成器为 `src/probe_headless/build_baseline_scope_lock_v1.py`。
- 结果：机器矩阵固定 fresh 投稿切片为四 provider、157 个 scaffold-disjoint BBB 分子、一次调用、selected-only、delay `{0,1,4}`；主行 9 个，另列 Replay-SGD/PILOT/Gaussian stratum、GraphRouter/Laya-online separate/audit-only。59 个 raw/recovery 证据路径存在且 hash 一致；跨 likelihood raw ranking、privileged control deployable ranking、missing-field 推算和 confirmation 回流均显式禁止。
- 状态解释：Stage-B v3 PASS 只覆盖声明的一次调用 fresh BBB slice；broad HOLD 仍适用于 hERG/AMES、自然 lifecycle、完整 33-provider model-fresh、post-call refusal 和 agent 闭环。该 join 提高可审查性，没有凭文档关闭科学 gate。
- 同步：`task/task49-pharma-benchmark-baseline-freeze/report.md` 已追加 receipt join 结论。arrival `implementation_review.md` 已标注为 pre-freeze 历史快照，arrival report 补全流/失败分母、输入 hash 和 Online-SGD 预选说明。主版 `paper/aamas2027/main.pdf` 未修改，SHA 仍为 `be5e1f50da9ba5313ef5bd2159c288d2a2e9de84ddd271569900562a5c6b3cdc`。
- 下一步：对三份评价标准重新做一次基于该矩阵的 scope-aware 评分；只有发现主行缺少不可替代回执时，才另写定向 baseline 实验卡，不重复已完成结果，也不运行弱化版 GraphRouter/Laya-online。

### 2026-10-08 — primary arrival runner 接口交接（复用已有 smoke/full 证据）

- 核对发现 `run_primary_arrival_robustness_v1.py` 与独立 `verify_primary_arrival_robustness_v1.py` 已完成；原订单 smoke_v2（480 streams/75,360 decisions）和 full_v2 的独立核验回执已存在，本轮没有重复跑 full 或新增 GPU/API 调用。
- 增加实现交接说明 `src/probe_headless/primary_arrival_robustness_v1_implementation.md`，记录复用原始 policy 接口、不可替换的数学边界、历史 smoke_v1/full_v1 失败和 `PYTHONPATH=src` 启动要求。文件 SHA-256=`4e4b5e5af2b38f37cb3e10f9a10ce76d678725865e791a3f2b9f4168163b37aa`。
- 真实接口检查：裸 `python3 -m pytest ...` 因 src-layout 缺少 `PYTHONPATH` 在收集期失败，保留为接口失败；`PYTHONPATH=src python3 -m pytest -q tests/test_primary_arrival_robustness_v1.py` 通过 11/11。该检查不构成新的科学结果，主版 PDF 未修改。
- 下一步：将交接说明与本轮 scoped artifacts 一并由根代理决定是否提交；arrival 结果继续按既有 report/independent_full_v2 解释，科学 gate 不因该接口检查改变。


### 2026-10-08 — 桌面周报完成

- 已阅读指定 Codex 线程 `01a0f526-bf1f-7221-8983-161166923f51`、最新项目入口文档、`TODO_LOG.md` 和桌面周报模板。
- 新周报已写入 `/Users/jingwu/Desktop/周报汇总/1008/周报_2026-10-02_10-08.md`，按桌面上一份周报的结构整理 10-02 至 10-08 的进展；说明了 09-25 至 10-01 的承接背景、声明范围、独立核验、负结果、主版状态和待决事项。
- 本轮只做事实汇总和文件写入，没有运行新实验、调用 provider/API、修改论文主版或改变科学判据。baseline scope lock 和 receipt join 已在本轮线程中完成；下一步按周报所列基于该矩阵做三份标准联合复核。

### 2026-10-09 — 主稿 claim-scope 审计

- 目的：按 scope-aware gate 的 P0，逐段核对 `paper/aamas2027/main.tex` 是否把窄范围证据写成 broad 结论，并继续维护唯一主版 PDF 的边界。
- 产物：`task/task49-pharma-benchmark-baseline-freeze/paper_claim_scope_audit_v1/`，包括 `audit.json`、`report.md`、`independent_verification.json`；脚本为 `src/probe_headless/build_paper_claim_scope_audit_v1.py` 和 `src/probe_headless/verify_paper_claim_scope_audit_v1.py`。
- 结果：所有范围锚点存在；主稿明确四 fresh providers、157 scaffold-disjoint BBB、one-call、selected-only、$D\in\{0,1,4\}$，并把自然 null、受控 correctness drift、33-provider development boundary、arrival 条件范围和 primary scope 之外的扩展写清楚。独立 verifier=`verified_independently`。
- 没有发现需要修改主稿的越界正向 claim；`main.tex`、`main.pdf` 均未改，主版仍为正文8页、References第9页，SHA 保持不变。科学 gate 的宽范围状态仍 HOLD；本轮只提升 claim 可审查性。
- 下一步：整理主表与 baseline role/fidelity receipt 的论文可读映射，再做一次独立 reviewer walkthrough；若不暴露不可替代缺口，不重复跑已有实验。

### 2026-10-09 — `build/main.pdf` 中文审阅译稿

- 用户要求先停收敛，只读 `/Users/jingwu/work/benchmark/paper/aamas2027/build/main.pdf`，并以该旧 build 版为源翻译；未读取其他版本以补数字，也未修改 `main.pdf`、`main.tex` 或候选迭代。
- 源 PDF 已核对为 9 页（正文 8 页、参考文献 1 页），SHA-256=`8b078e87e659d7921618f1c9cae680891ed0c7f646b31408ec5425bed5427cdd`。提取命令、页数、译法和版本说明记录在 `paper/aamas2027/build/main_zh_review_receipt.md`。
- 完整中文审阅译稿写入 `paper/aamas2027/build/main_zh_review.md`：摘要、1–13 节、全部 6 张表、2 个图注以及参考文献条目均覆盖；公式保留 `$`/`$$`。译稿 SHA-256=`05890e9357dcf0f3cfac50fbf7a62322e6a0b2b6ac70007c9fd62021a3e4a4c8`。
- 术语按要求统一：agent→智能体；工具评分函数/离线拟合的评分函数；正确性预测头/运行状态预测头；`r_s^{sys}` 历史；$n^+,s^+$ 释放更新；逐次先预测、后评估；动作选择概率；fresh（独立确认）。源 PDF 第 6 节“164 actions”忠实保留，未合入其他版本数字。
- 下一步：由根代理审阅译稿是否符合中文阅读需要；本翻译任务不触碰唯一英文主版 PDF 的 8 页规则。
