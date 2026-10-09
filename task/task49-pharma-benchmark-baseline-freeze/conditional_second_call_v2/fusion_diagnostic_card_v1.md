# B-only 终局融合与选择支持诊断卡（草案）

2026-10-08。**Frozen v1：2026-10-08 独立方法审查通过，已落实唯一分支 4 修订；真实执行尚未开始，需单独 GO。** 本轮只核对了本地代码、B 回执 schema 与输入文件哈希，未计算下述分组结果。本卡是已查看开发集上的事后诊断，不是新确认实验、方法采用或概率一致性证明。由协调者审查、冻结并另行授权后才能运行；本文件仍属任务方案，不新增决议。

依据：[最终方法审查](fusion_diagnostic_final_review.md)、[价值审查](verification_value_review.md)、[独立审查](verification_value_independent_review.md)；本修订落实[诊断卡独立审查](fusion_diagnostic_independent_review.md)，原草案逐字节保存在 [v01 快照](drafts/fusion_diagnostic_card_v01.md)（SHA-256 `d13013e50bc86ce9526a452d432775d0fb8d468b6c2a0e65dd0809a5257db101`）。沿用当前 A-only 九参数 masked-logistic 验证器、首工具选择、ρ=.25、κ=.01、provider 顺序；不拟合、不重校准、不改阈值、不采样、不读 C、不跑 provider/API/GPU。

## 1. 问题与决定用途

要区分三个仍竞争的解释：

1. **价值预测或候选支持不足**：已有可获益的终局变化，但当前 gain 表示／拟合不能在候选间找到它。
2. **概率变化未跨终局阈值**：第二输出改变拟合概率，但在当前损失下仍应使用同一动作。这可能是正确的决策结果，不能直接归咎融合器。
3. **当前终局融合的经验风险退化**：收到第二输出后，冻结规则的真实损失比沿用首动作更高。需要再区分选择条件转移、一般拟合误差和跨 mask 建模问题；不能从 harm 或 Brier 单独认定概率不一致。

收益不是再列一张结果表：第 7 节在看分组结果前规定何时优先做**训练展示分布匹配干预**、**只改终局融合的对照**或**保持验证器的动作变化／价值预测干预**，以及何时停止而不给根因。各分支只产生下一张独立执行卡，不在本卡中采用新方法。

## 2. 已确认可复用的输入及缺口

所有路径均相对仓库根目录。执行器采用显式文件白名单，不调用现有 v1/v2 `load_inputs` 或完整 CLI；这些入口会装载 C 的 outcome/policy 数据。

**B-only 是 fold-local 的记录口径，不是 globally unseen entity。** 某 fold 的 B entity 可能已出现在其他 fold 的 A 或 C；既有轮换划分的 C 汇总覆盖全部开发分子。本卡只读取该 entity 在当前 fold 的 B 回执，不读取其他 fold 的 C outcome，也不据此声称新独立 holdout、全局未见分子或未触碰的确认集。

| 白名单文件 | 已核对的可用字段／作用 |
|---|---|
| `experiments/logs/task49_conditional_second_call_20261008/full_v1/B_paired_targets.jsonl` | JSONL 的 `payload` 含 `fold,row_index,entity_id,first,candidate,native_first,native_second,single_q,pair_q,single_action,pair_action,truth,single_loss,pair_loss,gain,features,weight`。是双输出、双动作和双损失的权威 B 来源。 |
| `experiments/logs/task49_conditional_second_call_v2_20261008/full_v1/B_features_and_targets.jsonl` | 同一 `(fold,row_index,entity_id,first,candidate)`，含 `base_features`、四种 `new_models` 的 `raw_added/transformed_features`、gain 和权重。它不是独立的一批双输出样本；须与上行逐键连接。 |
| 两个目录中的 `fold0_models.json` 至 `fold4_models.json` | A-only `verifier_coefficients`、A/B 成员索引、首工具原始系数/fit rates、ordered-pair 支持；v1 的 12/16 维 gain 系数，v2 的四组扩展系数及固定投影。只用 A/B 元数据和冻结系数，不使用同文件中的 C 索引作分析。 |
| 两个目录中的 `artifact_manifest.json` | 校验以上实际读取文件；读取清单不会授权读取清单列出的 C 文件。 |
| 本卡、两份理论审查及上述函数的源码快照 | 绑定定义和 provenance；只读代码，不运行旧实验入口。 |

已核对文件 SHA-256：

- v1 `B_paired_targets.jsonl`：`6826f43960e2ceede02482b674e9fa0b4f3874c971824b85575ce4e79500585f`。
- v2 `B_features_and_targets.jsonl`：`789e2d0c1052fdfd001e3a7869a6b08533e34ae6d03bc66652ac196fce1eac60`。
- v1 `artifact_manifest.json`：`7142e3aa014b0ae7ae83d301b0d3debeb979f5338dd2c7df0f2c3e9487ef6b48`。
- v2 `artifact_manifest.json`：`7bf398da66751559685608725f5d581d89cd8fcde75ba27ecae838957376e501`。

执行前还要把十份实际读取的 model 文件逐一对 manifest 验哈希并保存解析字段清单；不以当前源码 HEAD 代替来源绑定。

**三条 B 记录能还原一个分子的四个 native 输出。** 固定首工具 `a` 的 `native_first` 重复三次，三个不同 `candidate` 分别给出其余输出。因此可以仅从 B 回执，以同一个 A-only verifier 重算 B 的全部四个 singleton 和六个 unordered pair，无需 provider 推理或新拟合。重算只用于下面标明的 evaluator 诊断面板，不得冒充实际 acquisition policy 的已调用分布。

**字段缺口必须保留。** B 回执没有完整四维分子 selector 分数、分子特征或 scaffold 字段。现有 `first` 足以定义选择事件；本卡不重建 cascade 排序、不作化学子群归因，也不为补字段而打开包含 C 的全开发输入。需要这些信息时另冻结 B-only 导出合同。

**gain 的 B 读数是拟合内读数。** 现有 gain 系数和 v2 投影都在 B 上拟合。复用其预测可发现拟合内排序／停止符号／支持不足；表现良好不能证明泛化，表现差也不能单独解释历史 C 上的失败。本卡不再读取、援引或重新计算 C 的效果来替它补证据。

## 3. 两个诊断面板及权重

provider 固定顺序：`ecfp_logreg, ecfp_mlp, ecfp_random_forest, graph_message_passing`，编号 0–3；所有 argmax 同分按此顺序取首项。

### S：原始首选工具面板（主要描述对象）

对每个 `(fold,entity_id)` 保留原始首工具 `S=a` 和全部三个 `a→b` B 候选，原记录每行权重 1/3，合计一个 fold–molecule 权重 1。这是**冻结首选以后，候选第二工具等权**的离线面板；不是某个已调用 policy 的分布，也不受 token controller 选择。不得只保留预测为正、真实为正或最终被某旧 policy 调用的 pair。

先逐 fold 报告，再给全 B fold–molecule 出现次数加权的描述性汇总；汇总权重正比各 fold 的 B 分子数。不得把这个汇总写成五个独立实验的推断，或事后改用有利 fold 权重。

### U：相同 B 分子的全 mask 展示面板（竞争解释检查）

用冻结 verifier 和还原的四个 native 值重算：

- **U10**：四个 singleton、六个 unordered pair，各 mask 每分子权重 .1，模仿 A 的展示权重；singleton 与 pair 的概率误差另各自平均，避免混淆 4:6 比例。
- **U12**：十二个有向 `a→b`，每分子权重 1/12，配对比较该 `a` 的 singleton 与 `{a,b}` 的 pair；两方向的 pair 概率必须相同，但首动作、Δ 和 g 可不同。
- 对每个固定 `a→b`，并列显示全 B、真实 `S=a`、真实 `S≠a` 三个支持集合；也保留四个真实首工具 `S` 的完整支持列，不合并稀疏 provider。**用于第 7 节分流时，三者必须同时限制在完整键 `(fold,a,b,d1)`，其中 d1 都由相同假定首工具 a 的 singleton 概率得到。** 不得把 S 的某个首动作组与 U 混合首动作总体比较；只含 `(fold,a,b)` 的边际表仍可展示，但不触发首两条分流。

U 是同一 B 总体里的显示／条件对照，包含对应的 S 子集，二者并不独立；它不是新 A 数据，更不是随机化首工具干预。`S=a` 与 `S≠a` 的差异仍可能包含不同分子组成、一般失配和有限样本误差。U10 与 S 的差异不能被命名为已识别的选择偏差或 tower 违背。

## 4. 固定分组、关联与支持

所有组及空组预先输出，不看结果后选切点或合并：

1. 每个 fold 和上述固定全 B 汇总。
2. 四个真实首工具 `S`；十二个 ordered pair `a→b`；六个 unordered pair `{a,b}`。unordered 汇总保留其方向组成，不能把反向首动作混成相同状态。
3. 首动作固定三组：答 0（q≤.25）、拒答（.25<q<.75）、答 1（q≥.75）。主要交叉组为 `ordered pair × 首动作`；共 36 个槽位。U 使用其假定首工具的 singleton 动作，并显式标识是否 `S=a`。
4. 固定 3×3 首动作→pair 动作转移表；它是事后动作结果，不是调用前可用的分组变量。

每格记录原始 pair 行数、权重和、不同 `(fold,entity_id)` 数、**去重 entity_id 数**、各 fold 支持量及首工具/方向组成。S、同组 U 全 B 和同组 `S≠a` 的比较保存同一 `(fold,a,b,d1)` 键；后两者不能借其他首动作组补支持。保留逐 entity 的跨 fold 成员表与三候选向量。三个 pair 来自同一分子，跨 fold 又可能重复同一 entity；原有 1,518 条 B 行不能写成 1,518 个独立样本，fold 间 A/B 重叠亦不提供独立训练重复。

用于第 7 节的“有支持且可复现方向”预定为：该调用前组在至少三个 fold 中各有 ≥10 个去重 B entity，跨这些 fold 合计去重 ≥30 个 entity；方向条件须在至少三个这样的 fold 同时成立。低支持组仍完整报告，均值未定义的空组记 null；不补伪零、不删负组。这个门槛只安排开发优先级，不是显著性或泛化门槛。

**pair 相关性同时保留两个层面：**

- 统计依赖：上述 molecule/fold 关联，所有同分子候选和 mask 共同保留；不 bootstrap、不把 pair 行当独立项。
- 输出关联：对六个固定 unordered pair，逐 fold 与固定全 B 汇总，报告 native 概率 Pearson 相关、按原固定 clip `[1e−6,1−1e−6]` 后 logit 的 Pearson 相关、native `.5` 标签不一致率，以及其二元错误指示的经验协方差／相关。全 B 与四个真实 `S` 子集均给支持量；零方差相关记 null。高相关不等于无新增条件信息，低相关也不保证决策收益；不在看结果后选择相关阈值或改 provider 集合。

## 5. 固定统计量与控制

记 `q1,d1,L1` 为首输出后的冻结概率、动作、实际损失，`q2,d2,L2` 为 pair 对应量；`Y` 只供 evaluator 使用。

### 5.1 真实毛改善与成本必须分开

- `Δ=L1−L2`；`g=Δ−.01`。逐行核对保存的 g 和重新算出的两种值。
- `C=I(d2≠d1)`；保存 unchanged/changed、Δ>0/Δ=0/Δ<0、g>0/g≤0 的完整计数及离散 Δ、g 分布。
- 在当前二值真值、三个终局动作及 ρ=.25 下，不同动作对同一 Y 的损失分别来自 `{0,.25,1}`，所以 `C=0 ⇔ Δ=0`；因此 unchanged 的 `g=−.01` 精确成立。只要不等式身份失败就停，不能修标签来迎合推导。
- 逐组报告平均 L1/L2、Δ/g、救对／伤害数以及完整动作转移；平均净负而毛改善为零不能计为“错误融合”。不裁负 Δ 标签。
- **已调用后的 ignore-second 控制**：保留 d1，故 Δ_keep=0、g_keep=−.01。**调用前 stop 控制**：不付追加成本，增量 g_stop=0。两者不能混写；融合器相对前者的差异是 Δ，相对后者的差异才是 g。

### 5.2 概率变化与真实效果并列

报告 Brier `(q−Y)²`、平均校准残差 `q−Y`、平均 `q2−q1`、平均/中位数/固定 .1/.5/.9 分位的 `|q2−q1|`、首概率到 `.25/.75` 最近边界的距离、跨动作区比例。固定 `|q2−q1|≥.05` 仅作“有可见概率移动”的诊断计数，不改变动作阈值。

模型内部 `R(q)=min(q,1−q,.25)` 及其差仅作辅助；不得替代 Y 上的实际风险。组内平均 `q2−q1` 只是粗分组必要矩的偏离信号，而且**恰等于两套平均校准残差之差**，不能当两份独立证据。均差零不证明逐条件 tower，非零、Brier 上升或单条 harm 都不能唯一识别不一致。

### 5.3 冻结 gain 的支持／拟合内排序审计

固定报告全部六个已拟合模型：pair-mean12、base-confidence16、signed-only20、refusal-only20、signed-refusal24、magnitude-polynomial24。主描述锚点是 base16；其余作为完整已有对照，**不选 B 上赢家**。从现成特征与系数点积重算预测，不拟合、不重新求投影；unsupported ordered pair 保留 unavailable 状态。

**这里的 `pair_support` 是该 fold 生成 B 拟合目标时累计的 B 支持数。** S 面板每个原始候选都参与了这一累计，因此每个 B 分子的三个原始 ordered pair 支持必为正；若不是正数，就是完整性失败。U 的假定首工具可能产生不在该 fold 原始 S 面板中的 ordered pair，才可能标为 unavailable。此检查不能诊断 C 上的 unsupported pair，不能把 B 的正支持外推为 C 的覆盖；可描述的只是 B 内调用前子组稀疏性与拟合内漏选，不是泛化误差的下界。

每个原始 S 面板分子保留三候选的预测 g、真实 Δ/g、首动作与候选支持。报告各模型的加权拟合残差、正净值符号错配、三候选排序同分率、预测首名真实 g，以及：

- `has_positive = I(max_b g_b>0)`，和 `has_action_change = I(any_b C_b=1)`；都是 evaluator 诊断，调用前不可见。
- “漏掉已有正候选”：`has_positive=1`，但模型全预测≤0，或其支持内预测首名的真实 g≤0；拆开 stop-sign 与 ranking 两部分，按 provider 顺序处理预测同分。
- `max_b Δ_b`、`max(0,max_b g_b)` 只作逐分子全信息参照，不称可部署策略或 token-budget 上界。不在本卡重放预算控制，也不把它与真实调用 policy 的 utility 混比。

κ 对同分子三个候选是相同常数，故真实 Δ 与 g 的候选排序相同，正值／停止判断不同。已有模型直接拟合 g；把其预测加 κ 只是同一预测的代数重表达，不是新的毛价值模型。

## 6. A 均匀 mask 与 B 首选条件的竞争解释

A 对每分子均匀展开 4 singleton+6 pair，权重 .1；S 面板则先由分子特征上的冻结 selector 选首工具，仅保留该工具的三个 pair。真实决策信息 E 包含首工具选择事件及合法分子信息，而当前 verifier 的九参数输入只有 mask 和已见 native logits。

因此并列检查同一个固定 mask 在 U 全 B 与其 S 子集的支持、校准残差、Brier 和实际风险，具有开发分流价值。但两个集合的表现差异只说明**选择条件关联值得单独干预检查**；不能证明 uniform-mask 训练是原因，也不能排除一般拟合失配、正则或有限支持。不得拿 A 的拟合内误差与 B 的验证误差直接判“分布转移”；本卡根本不读取 A outcome 记录来做这种对比。

## 7. 结果如何决定下一项真正干预

本表的量和门槛在执行前固定；所有分支都完整报告。多个条件成立时按表中从上到下决定**先做的一项**，其余保留为竞争解释。没有触发条件则结论为未识别，停止本诊断扩展。以下“同向”均指第 4 节支持门槛下至少三个 fold，**不构成统计显著性声明**。

首两条分支使用以下机械谓词，不留事后兜底：

- 固定组 `K=(a,b,d1)`。在每个相同 fold f，`S_f,K` 是真实首选 `S=a` 的该组，`U_f,K` 是全 B 的相同 `(a,b,d1)` 组；两组 d1 使用相同 a 的 singleton 动作。`S≠a` 表同样限制于该完整键，仅作对应描述。
- 定义 `rP(f,K)=mean_P(L2−L1)`、`bP(f,K)=mean_P(Brier2−Brier1)`，P 为 S 或 U。每项都在本面板既定权重下计算。
- 分支 1 的 fold 条件是 `rS>0 AND bS>0 AND rU≤0 AND bU≤0`；分支 2 的 fold 条件是 `rS>0 AND bS>0 AND rU>0 AND bU>0`。某分支只在**同一 K、同一组至少三个 fold**逐个满足其全部条件，且这些 fold 各自 S/U 支持均 ≥10 个去重 entity、其 S 支持跨 fold 合计去重 ≥30 个 entity 时触发。不得由不同 fold 分别满足风险项、Brier 项或 U 项后拼接通过。
- S 任一差为零不会触发首两分支；U 两项均≤0（含零）只允许分支 1，分支 2 必须两项严格>0。符号冲突、其余零值组合、低支持或不足三个共同 fold 的该组均标为“未识别”，不能触发终局融合分支。
- 首两分支没有触发时，只有表中第三／第四分支自己的完整条件成立才可转向其预定干预；冲突或低支持本身不等于“概率／风险未退化”，不能作为第四分支的通过依据。其余情况进入最后一行停止。

| 预定可观察读数 | 下一项要冻结的干预 | 仍不能宣称 |
|---|---|---|
| 存在同一 `K=(a,b,d1)`，至少三个相同合格 fold 同时满足 `rS>0 AND bS>0 AND rU≤0 AND bU≤0`，并满足上述共同支持门槛 | **先检查选择条件**：另写 A-only 训练展示分布匹配 vs 原均匀10mask 的同九参数对照，冻结首工具、已取得输出、损失阈值、成本与 gain learner；保留已调用后 ignore-second 控制。先解决 A 上如何合法构造 selector 匹配展示／权重和支持，再授权训练。 | 选择条件转移已被识别为根因；内部概率一致性被证伪。 |
| 存在同一 `K=(a,b,d1)`，至少三个相同合格 fold 同时满足 `rS>0 AND bS>0 AND rU>0 AND bU>0`，并满足上述共同支持门槛；没有其他兜底条件 | **只改终局规则的干预**：单独冻结原 verifier、ignore-second 与一个明确限定的融合候选的对照；保持相同首选、同一已获 pair、阈值/成本和 gain 不变。若候选是可边缘化联合模型，必须先冻结分布/容量/训练假设与 A-only 合同，不能由本诊断直接采用。 | 已证明 mask 不一致；联合模型一定正确或更好；所有 harm 都可消除。 |
| 前两项未触发；S 整体每 fold ≥80% 候选动作不变，且其中 ≥50% 的 `|q2−q1|≥.05`，在至少三个 fold 成立 | **保持 verifier 与阈值**：下一卡先隔离对“动作变化”概率的预测支持，再比较同一固定价值模型的即时 C 标签与延迟标签／修订释放。先固定模型与已获 pair 流，后研究闭环选择；不通过改阈值制造动作变化。 | 第二输出无信息；同动作说明融合器坏；提前标签必有收益。 |
| 前三项未触发。对每个 fold $f$，令 $P_f$ 为 S 原始面板中 $\max_b g_b>0$ 的去重分子集合，令 $m_f$ 为 base16 在 $P_f$ 中按第 5.3 节定义漏掉正候选的分子比例；令 $r_{all}(f)=\operatorname{mean}_S(L_2-L_1)$、$b_{all}(f)=\operatorname{mean}_S((q_2-Y)^2-(q_1-Y)^2)$，两者均在该 fold 的完整 S 面板、每候选权重 $1/3$ 上计算。定义 $F=\{f:|P_f|\ge10\land m_f\ge.20\land r_{all}(f)\le0\land b_{all}(f)\le0\}$. 仅当 $|F|\ge3$，且这些 fold 的 $P_f$ 合并后至少有30个去重 entity 时触发；空值不通过，零值按 $\le0$ 处理。 | **优先价值预测／支持干预**：保持终局规则，在另卡中单独比较一个预定的 gain 目标/表示或反馈时间改动与当前模型；保留停止符号与候选排序两个控制，不同时换融合器。B 上拟合内也漏掉候选是当前训练面板中值得修查的拟合限制，不能作为泛化误差下界；仍须独立评价新干预。 | 已证明 C 失败来自 gain；当前 B 上排名好的扩展可直接采用。 |
| 支持稀疏、跨 fold 方向不稳定、只有成本造成净负、仅有单条 harm／Brier／矩偏离，或上述条件均未满足 | **不增加模型或 basis 搜索**：记录为未识别，停止在这批 B 上扩展诊断；下一步先解决新的评价支持或已有同模型反馈时序合同。 | 未发现问题等于 verifier 正确；B 拟合良好等于泛化良好。 |

分支 4 的“未退化”只指 S 完整候选面板上的两项经验平均量不增加；它不表示所有条件子组、U 面板、真实 posterior 或新数据均未退化。这里不要求实际风险、Brier 与动作变化得出同一方向。冲突读数保留，不能把一项不利读数用另一项覆盖。任何下一项干预都需要自己的预冻结执行卡与评价边界；已查看的 B 不会因换名变成新的确认数据。

## 8. 执行边界、停止线与产物（待另行 GO）

- 单次确定性 B-only 分析：**≤2 分钟 wall、≤1 GiB RSS、≤50 MiB 新产物**；CPU-only，零新拟合、零模型更新、零采样、零 provider/API/GPU。超限即保留 incomplete，不删模型、mask、稀疏或负组来缩任务。
- 运行前保存 config、argv、卡/脚本/实际依赖源码快照、所读输入 SHA、模型 SHA、环境与 UTC；来源路径只用白名单。不得读取 `C_evaluator_panel.jsonl`、`policy_decisions.jsonl`、`evaluator_outcomes.jsonl`、fold/总效果 summary、bootstrap 文件或 confirmation 输入。
- 第一阶段仅工程核验：每 fold B 成员一致；每分子恰三候选、first/truth/native-first/single-q 一致；四 native 值可完整重建且有限；原 q/动作/损失/g、v2 特征/投影连接一致；数字容差预定 `atol=rtol=1e−10`，动作、标签、ID、计数精确。任何不匹配停止，不靠重拟合或读 C 修补。
- 保留全部 B 原键、关联表、重算 U10/U12 mask 记录、六 gain 模型预测、完整预定组／相关性／动作转移 CSV+JSON、支持和代数守恒、各 fold 描述性读数、分支触发依据、JSONL 进度/失败及 traceback、资源实测、manifest 和短报告。
- 不生成新 CI、p 值、重排 bootstrap、显著性标签、择优组总分或新主指标。有限分组均值只指导下一项待检验干预，不证明真实 posterior 或化学原因。
- 完成执行及独立核验前保持“未执行／待核验”；即使诊断通过，也只更新开发优先级。**生效方法、主论文与完整科学 gate 均不因此改变。**

## 9. 此卡留下的可复用内容

可直接复用的是已存 B 的双分数/双动作/双损失、由三个候选还原的四输出面板、A-only verifier、B 内冻结 gain 系数和投影，以及明确保留选择事件与重复 entity 的分组合同。当前尚未生成诊断结果或新模型；执行器必须先绑定本卡 hash、白名单输入和源码 hash，再等待单独 GO。
