# 长程 Agent 评测数据集 · 分类与逐个详解

> 本文是 `reference/` 全部 25 个数据集的权威详解，沉淀了本项目多轮调研的完整认知。
> **分类主轴 = checkpoint（中间卡点）机制**，与 `评测能力维度.md`（按 4 项能力分类）互补。
> 每个数据集给出：**完整 case → 怎么测 → checkpoint 怎么构建（谁定义）→ 怎么打分 → 对我们的价值**。原始采样数据与官方判分脚本见各子文件夹 `README_采样说明.md`。

---

## 〇、先讲我们的需求边界（本文的分类透镜）

经多轮澄清，本项目要找/要造的 benchmark 有**三条硬边界**：

1. **单 Agent**：单个智能体执行，非多智能体协同。
2. **一次性输入**：任务信息一次给全，执行中**无额外交互**（不含模拟用户对话、不含长期记忆检索）。
3. **hard 中间 checkpoint**：任务执行到中途设卡点判断 trace 有没有偏；判定必须 **hard**（可执行断言 / 规则匹配 / 状态比对 / 单元测试），**不能是 LLM judge 或人工主观打分**。

**一个贯穿全程的核心洞察**：能不能 hard 判分，取决于任务背后有没有一个"**可随时导出结构化真值**"的环境（仿真器 / 文件系统 / 数据库 / 可执行测试）。若任务只产出一段自然语言、无结构化真值可查，则天然只能 soft 判（LLM judge）——这是任务载体的性质，不是判分方法的选择。

**判分为什么能 hard（以仿真环境为例）**：agent 看到的输入输出虽是文字，但底层是活的状态机。agent 的自然语言先被解析器翻译成"符号动作"，动作驱动环境内部的结构化状态按规则真实变化，判分**直接读这份符号真值做布尔/数值比对，从不读 agent 的文字**。文字只是环境渲染给 agent 看的"显示皮肤"，真值账本才是判分依据——"agent 看皮肤，判分查账本"。

---

## 一、分类总览（按 checkpoint 机制）

| 类 | 定义 | 数据集 | 是否合我们需求 |
|---|---|---|---|
| **A. hard 中间 checkpoint** | 单agent+一次输入+可执行中间卡点 | SciCode、SUPER、LongProc、TravelPlanner、Commit0、AgentBoard、AgentQuest、USACO、RE-Bench、（DevBench 部分） | ★ 直接可用 |
| **B. 结构先进但判定非纯 hard** | 有细粒度过程分解，但叶子用 LLM judge | PaperBench、ScienceAgentBench | 参考范式 |
| **C. 纯终态验证（无中间卡点）** | hard 判定但只卡最终结果 | SWE-bench Verified、SWE-bench Pro、MLE-bench、DSBench、tau-bench | 对照组 |
| **D. 事后 trace 标注 / 多agent / 含交互** | 被测是"挑错模型"，或含交互/协作 | ProcessBench、PRM800K、Who&When、TRAIL、TheAgentCompany、WebCanvas、OdysseyBench、MultiAgentBench | 借鉴偏差 schema |

A 类内部再按"卡点长什么样"分四种构建法：

| 构建法 | 卡点形态 | 代表 | 卡点来源（谁定义） |
|---|---|---|---|
| ① 单元测试型 | 可执行断言/测试通过率 | SciCode、Commit0、USACO、DevBench | 出题人写 / **harvest 现成测试** |
| ② 执行痕迹型 | 正则匹配日志/环境反馈 | SUPER、AgentBoard | 标注员写（可按任务族模板复用） |
| ③ 规则约束型 | True/False 约束函数 | TravelPlanner、LongProc、AgentQuest | 规则库统一定义 |
| ④ 连续过程分型 | 随时可测的连续 score | RE-Bench | 出题人写 scoring 函数 |

**三条共同的 hard 正确性保证**：checkpoint 合格 ⟺ 金标准轨迹回放必须全过（必要性）+ 非解/空转轨迹不应全过（区分度）——全程无需 LLM 或人做裁判。

---

## 二、构建分工总表（谁拆的 · 判分器怎么来 · 金标准怎么产生 · 纯人力吗）

> 这是理解每个 benchmark **成本与可规模化性**的核心。四个关键问题拆成四列。判分器形态分两种：**通用 harness**（写一次、全库复用，只负责"跑+比对+统计"）vs **逐题/逐类写**（每道题或每类任务单独写判分逻辑）。

| # | 数据集 | 卡点谁定义（拆分/标注） | 判分器形态 | 金标准/target 怎么产生 | 人力 vs 自动 |
|---|---|---|---|---|---|
| 1 | SciCode | 科学家(PhD)手工拆子问题 | 通用 harness | **跑金标准解自动生成**→存 H5 | 拆分+金标准解+测试设计纯人力；target 与判分自动；GPT-4 仅红队查错 |
| 2 | SUPER | 作者手工设计任务 | 通用（`evaluate_checkpoints` 正则搜日志） | landmark 正则由作者从金标准执行日志手写 | 任务+landmark 人力；正则匹配自动 |
| 3 | LongProc | **程序化生成**（任务生成器） | 每类任务一个 `eval_*` 函数（写一次） | `reference_output` 由生成器自动产出 | 写生成器+eval 函数（每类一次）；实例+金标准全自动 |
| 4 | TravelPlanner | 固定三类约束规则库 | 通用（约束函数集，写一次） | 沙盒数据库 + query 模板生成 | 规则库+query 模板一次性；判定全自动 |
| 5 | Commit0 | **harvest**：复用库自带测试 | 通用 pytest harness | 库原有测试的期望值（现成） | 人力≈0（掏空库+配置）；测试全现成 |
| 6 | AgentBoard | PDDL：目标合取式**自动拆**；其他环境：人工标 subgoal 正则（可按任务族模板复用） | 通用（仿真器状态比对） | 环境状态真值（仿真器给） | PDDL 几乎 0；其他环境按任务族模板；判定自动 |
| 7 | AgentQuest | 每任务写一个 driver 定义 milestone | 每任务类型一个 driver（框架化） | 任务规则（如密码本身） | 写 driver（每任务类型一次）；progress 计算自动 |
| 8 | USACO | **harvest**：官方竞赛测试点 | 通用 sandbox judge | 官方期望输出（现成） | 人力≈0（用官方题+测试）；判定自动 |
| 9 | RE-Bench | 每环境写一个 scoring 函数（METR 手写） | scoring 函数本身（每环境一次） | 可量化目标（耗时/损失，实时算） | 写 scoring+环境（7 个，一次）；score 自动实时 |
| 10 | DevBench | 每项目按 SDLC 阶段组织 | 各阶段测试（部分现成+部分写）+design 用 LLM | 测试/参考产物 | 组织项目+写测试人力；编译/测试自动；design 非 hard |
| 11 | PaperBench | **OpenAI+原论文作者共建** rubric 树（15 分钟粒度准则） | LLM judge（通用 prompt） | rubric 叶子 requirement（人工写） | 人力极高（每篇数周）；**叶子判定=LLM（非 hard）** |
| 12 | ScienceAgentBench | 专家从 44 篇论文提取任务+定义成功标准+写 gold 程序（附录 C） | 多维（VER 自动 / SR 对照 gold / CBS） | 专家写的 gold 程序+成功标准 | 任务+gold+标准专家人力；VER 执行自动 |
| 13 | SWE-bench Verified | **harvest**：从真实修复 commit 挖 FAIL_TO_PASS | 通用测试 runner | commit 里的测试（现成） | 筛选+人工核验（Verified）；测试现成 |
| 14 | SWE-bench Pro | 同上（企业级仓库） | 通用测试 runner | 同上 | 同上 |
| 15 | MLE-bench | 每竞赛配一个 grader（用 Kaggle 官方 metric） | metric + 真实 leaderboard | 竞赛私有测试集答案（现成） | 选竞赛+配 grader；评分+奖牌自动 |
| 16 | DSBench | 收集 ModelOff/Kaggle 官方题 | 每任务 eval | 官方标准答案（现成） | 收集+适配；比对自动 |
| 17 | tau-bench | 人工写任务+gold actions+policy | 通用（终态 DB 状态比对） | 重放 gold actions 得目标态 | 任务+actions+policy 人力；终态比对自动 |
| 18 | ProcessBench | **人工标**"最早错误步" | 无（本身是标注数据） | 人工判定的错误步下标 | 全人工标注 |
| 19 | PRM800K | **人工标** 80 万步级评分 | 无（标注数据，训 PRM 用） | 人工三值评分 | 全人工标注（成本极高） |
| 20 | Who&When | **人工标** (agent,step,reason) | 无（标注数据） | 人工判定的三元组 | 全人工标注 |
| 21 | TRAIL | **人工标** span 级错误 | 无（标注数据） | 人工判定的错误+位置+影响 | 全人工标注 |
| 22 | TheAgentCompany | 作者**每任务手写** checkpoints.md + evaluator.py | **逐任务写**（非通用） | 环境状态断言（人工写查询逻辑） | 每任务手写评测器，人力高 |
| 23 | WebCanvas | 标注员录制轨迹+勾选 key node | 通用（匹配函数集） | 录制轨迹里的必经状态 | 录制+标注人力；匹配自动 |
| 24 | OdysseyBench | **HomerAgents 多智能体自动生成**+验证 | 流水线自动生成的 evaluate 函数 | 自动生成+抽检 | 人力少（LLM 自动生成+抽验） |
| 25 | MultiAgentBench | 每场景定义 milestone | LLM judge + 规则 | milestone 描述（人工写） | 场景+milestone 人力；**判定=LLM（非 hard）** |

**从这张表能一眼看出的三件事**：
1. **判分器几乎都是"通用 harness 写一次"**（SciCode/SUPER/USACO/Commit0/AgentBoard/tau-bench…），只有 TheAgentCompany 是"逐任务手写评测器"（所以最贵、也最灵活）。
2. **金标准/target 有三种来源**：① 跑金标准解**自动生成**（SciCode/LongProc）；② **harvest 现成**（Commit0/USACO/SWE-bench/MLE-bench，几乎零成本）；③ **人工判定**（D 类标注数据集）。
3. **"纯人力"分布**：D 类（18-21）是纯人工标注冻结数据；A 类的人力集中在"**一次性投入**"（拆分/写生成器/写 scoring/规则库），实例内判分全自动——这就是 A 类能规模化的原因。**最省力的是 harvest 型（Commit0/USACO/SWE-bench）：连测试都不用写，直接用现成的。**

---

# 第一部分：A 类 —— 符合需求（hard 中间 checkpoint）

## 1. SciCode ★最贴合

**是什么**：一群科学家出的科研编程题。直接做无从下手，所以出题的科学家**在出题时就把一道大题拆成十几个环环相扣的子问题**，后一个要调用前一个写好的函数。

**完整 case**（`Berendsen_thermostat`，分子动力学恒温器，采样自 `first_problem.jsonl`）：
- 主问题描述："用 velocity Verlet 算法实现分子动力学模拟里的 Berendsen 恒温/恒压器——粒子在周期性立方盒子里、通过截断的 Lennard-Jones 势相互作用。"
- 被拆成 **12 个子问题**，依赖链渐进：
  - 子问题 0 `wrap(r, L)`：把粒子坐标按周期性边界折叠回盒子内；
  - 子问题 1 `dist(r1, r2, L)`：算两粒子的最小镜像距离；
  - 子问题 2 `dist_v(r1, r2, L)`：算最小镜像向量；
  - …一直到第 12 个拼出完整模拟脚本。
- 每个子问题带 `step_description_prompt`（子任务描述）、`function_header`（函数签名）、`test_cases`（断言）。

**怎么测**：agent 一次性拿到主问题 + 全部子问题描述，逐个写函数。评测器 `eval_test_generated_code.py` 逐子问题执行断言。子问题 0 的真实测试原文：
```python
particle_position = np.array([10.5, -1.2, 20.3])
box_length = 5.0
assert np.allclose(wrap(particle_position, box_length), target)   # target 为预存标准答案
```

**checkpoint 怎么构建**：由领域专家在出题时按依赖链把大题拆成子问题，每个子问题手写一组断言测试（每个约 3 条）。这是"人工构建"，但成本被摊薄在**一次出题**上（80 主问题），实例内卡点全自动执行。

**构建分工（人力 vs 自动，实测自采样文件 + 论文 §2.3）**：
- **拆分子问题**：科学家（≥2 位 PhD 及以上）手工，按"平时怎么拆一个复杂问题"的自然结构（非算法、非 LLM）；
- **金标准解**：科学家手写（限用 NumPy/SciPy/SymPy）；
- **测试设计**：科学家手写断言 + **领域复现测试**（用已发表论文的已知数值当验证，如复现 2D Ising 相变点 kT/J=2.269）；
- **target 金标准数值**：**自动生成**——跑科学家的金标准解、把数值输出 dump 进 `test_data.h5`；判分脚本 `targets = process_hdf5_to_tuple(step_id, n)` 从 H5 加载，断言里的 `target` 就是它；
- **判分器**：`eval_test_generated_code.py` 是**通用 harness（写一次）**，只做"拼接 agent 代码+测试断言→跑→pass/fail 统计"；
- **LLM 的角色**：仅在最后一轮当**红队**——用 GPT-4 去解已定稿的题，科学家据其错误重新设计数值测试防假阳性；GPT-4 不参与出题。
- 结论：**拆分/金标准解/测试设计 = 纯人力（PhD 级）；target 数值与判分 = 自动**。贵，所以只有 80 题。

**怎么打分**：两层——**子问题通过率**（12 个里有几个断言全过=中间卡点得分，精确定位"走到第几步崩")；**主问题解决率**（12 个全过才算这道主题解决）。SOTA 主问题解决率仅 14–28.5%，但子问题通过率高得多，正说明"很多任务是走到中间某步才崩的"。判定纯 hard（Python 断言，无 LLM）。

**对我们的价值**：coding/科研方向一号参考之一。卡点=子问题单元测试，语义清晰、拆分粒度天然（依赖链）。

---

## 2. SUPER ★最贴合（coding/科研方向一号参考）

**是什么**：考 agent 能不能把 GitHub 上冷门研究仓库的实验跑起来——clone 仓库、装依赖、改配置、跑训练、抽结果，全程自主无人干预。

**完整 case**（`colbert` 任务，采样自 `expert_full.jsonl`）：
- query（原文）："Train a ColBERT model on my data, available on `https://drive.google.com/...`. Report last loss value as a json structured as follows: {"last_loss": 0.0}"
- landmarks（该任务标注的一组正则）：
  - `"Some weights of ColBERT were not initialized from the model checkpoint at bert-base-uncased and are newly initialized: \['linear.weight'\]"` ← 模型结构被正确构建的标志
  - `"0 \d+\.\d+"` ← 训练循环开始打印 (step, loss) 的标志
- answer（终态真值）：`{"last_loss": 1.5311915477116902}`
- （另两个采样 case：textbox 训练 bart-base 报 rouge，landmark 是 `"INFO ====== Finished training..."`；amrbart 微调 AMR parser）

**怎么测**：agent 在沙箱里自主执行；评测器 `evaluate_dataset.py` 的 `evaluate_checkpoints()` 函数拿 landmark 正则**去搜 agent 的完整执行历史（stdout）**，看命中了几条。

**checkpoint 怎么构建**：标注员完成任务时录下执行过程，从日志里挑出"**任何合法解法都必然出现的确定性 stdout 行**"，写成正则。这是"执行痕迹型"卡点——landmark = 执行必经的确定性输出痕迹，与实现路径无关。
- **分工**：任务与 landmark 正则由**作者手工写**（Expert 集 45 题全人工；Masked/AutoGen 由其派生）；`answer` 终态真值由**跑金标准解得到**；判分器 `evaluate_dataset.py` 是**通用 harness（写一次）**，只负责正则搜历史+数值比对。人力=任务+landmark（一次性），自动=匹配与比对。

**怎么打分**：两个信号——**landmark 命中率**（命中数/总数=过程分，agent 就算最后没报出正确 loss，日志出现"模型正确加载""训练开始"也能拿部分过程分，精确定位卡在哪环）；**answer 精确比对**（最终 JSON 数值 vs 标准答案=终态分）。全 hard（正则 + 数值比对，无 LLM）。

**对我们的价值**：证明了"**从执行日志里用正则捕获必经痕迹**"在真实 coding/科研任务上完全可行且纯 hard——几乎就是"判断 trace 有没有偏"的现成实现。

---

## 3. LongProc ★最贴合

**是什么**：考"能不能严格按规程一步步生成、不中途跑偏"。一次性输入，模型一次生成一长串结构化过程，逐步 hard 校验。

**完整 case**（countdown 凑数，采样自 `countdown_0.5k`）：
- 输入：`nums:[21,16,17,26]`、`target:46`
- 标准解：`["21+16=37", "26-17=9", "37+9=46"]`
- 数据还带完整搜索过程示范 `demonstration`（一棵搜索树："选两个数→试运算→加入新数集→继续"）
- 6 类任务：countdown、path_traversal、pseudo_to_code、tom_tracking、html_to_tsv、travel_planning，各按输出长度分 0.5K/2K/8K token 三档。

**怎么测**：不是只看最终等式，而是**逐步校验中间过程**——每步算术是否正确、是否合法使用了剩下的数字、搜索/推理链是否自洽。因为过程本身是固定格式（每步 `a op b = c`），可被程序解析。

**checkpoint 怎么构建**：过程结构化可解析，每类任务写一个确定性 evaluator，规则自动施加到每一步——不需要逐条人工标注。

**怎么打分**：逐步正确性 + 最终答案正确性，全确定性程序判定，无 LLM。输出越长（8K 档）越考验"长程不偏"。

**对我们的价值**：最贴合"一次性输入 + 需逐步判断 trace 有没有偏"的形态，卡点由规则自动施加、零人工标注。

---

## 4. TravelPlanner

**是什么**：给一句话需求，agent 一次性产出一份完整的多日行程；行程要同时满足一大堆可程序化校验的约束。

**完整 case**（采样自 `example_evaluation.jsonl`）：
- query（原文）："Please create a travel plan for me where I'll be departing from Washington and heading to Myrtle Beach for a 3-day trip from March 13th to March 15th, 2022. Can you help me keep this journey within a budget of $1,400?"
- agent 产出的 plan（day1 结构，真实）：
```json
{"days": 1, "current_city": "from Washington to Myrtle Beach",
 "transportation": "Flight Number: F3927581, Departure 11:03, Arrival 13:31",
 "attraction": "SkyWheel Myrtle Beach", "lunch": "Catfish Charlie's; Cost: 14",
 "dinner": "d' Curry House; Cost: 33", "accommodation": "Adorable Prospect..."}
```

**怎么测**：这份计划要通过两族约束（采样自两个评测脚本的真实函数名）——
- **硬约束** `hard_constraint.py`：`get_total_cost`（预算不超）、`is_valid_room_rule`（房型规则）、`is_valid_cuisine`（餐饮不重复）、`is_valid_transportation`（交通不冲突）、`is_valid_room_type`。
- **常识约束** `commonsense_constraint.py`：`is_valid_city_sequence`（城市连贯）、`is_reasonable_visiting_city`、`is_valid_restaurants`、`is_valid_attractions`、`is_valid_information_in_current_city`、`is_valid_accommodaton`、`is_valid_days`、`is_not_absent`（信息不缺失）等 16 个函数。

**checkpoint 怎么构建**：**每条约束就是一个返回 True/False 的中间卡点**，由规则库统一定义（不是逐条人工标注每道题）。

**怎么打分**：**Micro Pass Rate = 通过的约束数 / 总约束数**（数学上等价于 checkpoint 命中率）；**Macro Pass Rate = 约束全过的计划比例**。GPT-4 Final Pass Rate 仅 0.6%（约束一多就顾此失彼）。全 Python 判定，无 LLM。

**对我们的价值**：把"过程正确性"形式化为一组可独立判定的约束谓词，规则库统一定义、规模化友好。规划类长程任务的 hard 卡点标杆。

---

## 5. Commit0

**是什么**：给 agent 一个**被掏空的** Python 库（只剩函数签名和 docstring，函数体为空），要求从 commit 0 起把整个库写到通过测试。

**完整 case**（配置采样自 `configs_base.yaml`）：
- 数据集 `wentingzhao/commit0_docstring`，57 个真实核心 Python 库（如 minitorch、类 requests 的库），单库常含上百个待实现函数，`timeout: 1800`（单库 30 分钟，任务很重）。

**怎么测**：每个库自带**一整套真实单元测试**（原库在 GitHub 上就有的）。agent 每实现一部分函数就能跑对应测试。判分入口 `harness_evaluate.py`，用 pytest 逐 test_id 跑。

**checkpoint 怎么构建**：**harvest（捡现成）**——测试不是 benchmark 作者写的，是原开源库作者当年为自己项目写的。benchmark 只是"把库掏空、拿原测试来卡"，成本几乎为零、天然覆盖全库。这是最省力的 hard 卡点来源。

**怎么打分**（判分脚本真实逻辑）：
```python
passed = (status["passed"] + status["xfail"]) / sum(status.values())   # 单库通过率
average_passed = sum(x["passed"] for x in out) / len(out)              # 全 benchmark 平均通过率
```
核心指标是**测试通过率**（不是 0/1 的"整库是否完成"）。每个单元测试=一个 hard 卡点，几百个测试=几百个卡点，随实现推进单调上升。纯 hard（pytest）。

**对我们的价值**："一套单元测试=一批天然 hard 卡点"的最纯粹形态，且卡点可 harvest（任何带完整测试的开源库都能改造成一道 Commit0 题）。比 SWE-bench 强在——SWE-bench 只看全过不过，Commit0 的通过率天然是过程分。

---

## 6. AgentBoard（★关键：符号仿真环境）

**是什么**：一个**带状态机的符号仿真器**（不是纯文本！），agent 隔着"文字皮肤"和它多轮交互。它是理解"自然语言 trace 怎么变 hard 数字"的最佳范例。

**完整 case**（PDDL blockworld 搭积木，prompt 采样自 `pddl_vanilla_prompt.json`）：
- 指令：机器人有 pickup/putdown/stack/unstack 四个动作，一只机械臂一次拿一块……（附前置条件+效果规则）
- 目标：`The goal is to satisfy: b1 is on b2, b2 is on b3.`（摞成 b1-b2-b3 一柱）
- 初始观察：b1/b2/b3 都在桌上、都空闲、臂空。
- agent 真实交互轨迹（官方范例）：
```
Action: pickup b2 → Obs: 持有 b2
Action: stack b2 b3 → Obs: b2 在 b3 上，臂空        （达成 b2 on b3）
Action: pickup b2 → Obs: The action is not valid...  （b2 已被压住，非法）
Action: check valid actions → Obs: valid actions are: pickup b1, unstack b2 b3
Action: pickup b1 → stack b1 b2 → Obs: ...The goal is satisfied.
```
（PDDL 搬球子任务采样自 `pddl/test.jsonl`：goal=`ball1..ball4 is at roomb`，subgoals 逐个球一条）

**怎么测（关键机制，回答"自然语言怎么算分"）**：仿真器内部维护一份**机器可读的世界状态**（一堆谓词 literal）。数据流是：
1. agent 输出文字 `"stack b1 b2"` → `text_to_action()` **字符串匹配**成符号动作（文字在此退场）；
2. 符号动作交给引擎 `env.step()`，按 PDDL 规则**真实修改内部状态**，返回新谓词集 `obs_temp.literals`（agent 无法伪造，规则不允许就不变）；
3. 状态经 `literal_to_text()` 渲染成文字给 agent 看（显示皮肤），或经 `look around` / `check valid actions` 两个只读 view 主动查询。

**checkpoint 怎么构建**：**auto-derive（自动拆解）**——目标 `b1 on b2 ∧ b2 on b3` 是一个谓词合取式，程序把它**逐项机械拆开**成 subgoal（`[b1 on b2]`、`[b2 on b3]`），零人工。（另：ALFWorld 子域实测 134 个任务只有约 6 种 subgoal 骨架=6 个任务族模板，物体名填槽自动展开。）

**怎么打分**（判分核心 `constraint_satisfaction_metric` 真实代码）：
```python
def constraint_satisfaction_metric(self, obs_literals, goal_literals):
    satisfied = sum(1 for lit in goal_literals if lit in obs_literals)
    return satisfied / len(goal_literals)      # progress = |目标∩当前| / |目标|
# step() 里：reward = max(self.reward, metric)  ← progress 取历史最高、只增不减
```
`literal in obs_literals` 是 **Python 集合布尔比对**（比谓词名+参数元组），跟自然语言毫无关系。progress 满足比例逐步更新、取最高。全过则 `won=True`。纯 hard。

**对我们的价值**：揭示了 hard 判分的前提——**必须有一个能随时导出结构化真值的仿真环境**；且"目标可形式化成谓词合取 → subgoal 自动拆出、零人工"是最省力的卡点路径。

---

## 7. AgentQuest

**是什么**：一个把多个任务统一封装的可插拔框架，核心卖点是通用的 progress 度量。

**完整 case**（Mastermind 猜密码，采样自 `mastermind/data.json`）：
- 分难度存谜题：`"4 digits": ["5918","8607","9605","4040",...]`（200 题）、`"5 digits"`（200 题）…到 8 位。
- agent 通过多轮猜测逐步逼近正确 4 位密码。

**怎么测**：每个任务实现一个 `driver.py`，里面用确定性逻辑算 progress。agent 每猜一次，driver 就核一次。

**checkpoint 怎么构建**：每个任务的 driver 定义 milestone 与 progress 的确定性计算方式——框架化，可复用到多个任务。
- **分工**：driver 由**框架作者按任务类型写一次**（Mastermind 一个、Sudoku 一个…），非逐题写；gold 就是任务规则本身（如密码串）；progress 计算自动。人力=写 driver（每类一次），自动=进展计算。

**怎么打分**：Mastermind 的 progress = 当前猜测里猜对的数字位数比例（可精确程序化计算），随每步更新。纯 hard（driver 里的规则/匹配逻辑，无 LLM judge）。

**对我们的价值**：提供了"progress 作为通用过程指标"的**框架级实现参考**（每个任务用一个 driver 封装 hard 进展计算）。局限：内置任务偏推理/游戏，真实工程任务少。

---

## 8. USACO

**是什么**：美国信息学奥林匹克竞赛题，被普林斯顿的 HAL 主榜直接采用。硬核算法题——不仅要写对，还要在时间/内存限制内跑完。

**完整 case**：307 道 USACO 题，分 Bronze/Silver/Gold/Platinum 四难度层。每题给题面（如"给定 N 头牛的位置，求满足某条件的最优分组"），agent 一次性写完整程序。每题背后有一批官方测试点（通常 10~30 个，覆盖不同数据规模和边界）。

**怎么测**：agent 的程序在沙箱里对每个测试点跑一遍、比对输出。判题器 `usaco_judge` 读 `problem['num_tests']`，逐点执行，记录 `num_passed` 和 `num_tests`。

**checkpoint 怎么构建**：**harvest**——用官方测试点。注意：测试点是"覆盖度切分"而非"语义步骤切分"（告诉你对了几成，不直接告诉你逻辑哪一步偏了）。

**怎么打分**（`evaluation_metrics.py` 真实逻辑）：
```python
fraction_passed = num_passed / num_tests    # 通过测试点数 / 总测试点数
```
**不是全过才算过，而是按通过比例给分**——只处理了简单情况、超时在大数据上的程序能拿部分分（对应 USACO 真实赛制"前几个小数据测试点可拿部分分"）。另有 `pass_at_k` 系列（k 次尝试通过率）。纯 hard（沙箱执行+输出比对）。

**对我们的价值**：竞赛编程领域最成熟的 hard 部分给分范式，且被 HAL 主榜采用。与 SciCode 互补：SciCode 卡语义步骤，USACO 卡覆盖度。

---

## 9. RE-Bench ★特殊：连续过程分

**是什么**（METR 出品）：7 个开放式 ML 研究工程环境，每个都是"给定起点、优化某个可量化目标"。它的过程量最特殊——**是一个从头到尾一直可算、且始终衡量 final answer 质量的连续标量**，因此天然能当"完成度%"用在任何中间时刻。

**完整 case**（`ai_rd_triton_cumsum`，采样自任务文件）：
- 任务：用 Triton 写 GPU 核，计算一种带条件的前缀和：$Y_i = \sum_{j=0}^{i} x_j\big[(\sum_{k<j} x_k>0)\bmod 2\big]$，**尽可能快**。
- 标尺：Starting score 1.56（执行 4.76ms）；Official solution 0.47（1.6ms）；<1ms 可能。score = `log(耗时毫秒数)`（越低越好）。
- 解法写在 `/home/agent/solution/solution.py` 的 `prefix_sum` 函数里。

**怎么测**：每个环境自带一个 **scoring function**，`manifest.yaml` 标了 `intermediate: true`（增量可评分）+ `visible_to_agent: true`（分数对 agent 可见）。agent 在数小时优化中**任何时刻都能调用 `score` 动作**，跑一次 scoring 记一个带时间戳的分。

**checkpoint 怎么构建**：由任务作者为每个环境手写一份 scoring function（可执行代码）。它不是"把任务切成 N 段判过没过"，而是**同一个终态质量函数在时间轴上的连续采样**——checkpoint 型是"离散里程碑序列"，RE-Bench 是"连续质量曲线"。
- **分工**：scoring function 由 **METR 按环境写一次**（7 个环境=7 份），**不需要金标准解、也不需要标注**——因为分数直接由可量化目标（耗时/损失/准确率）实测得到，用人类专家表现做归一化参考。人力=写 7 个环境+scoring（一次性），自动=score 实时计算。

**怎么打分**（真实代码）：
```python
def intermediate_score(t): ...      # agent 随时可调，返回连续分（编译失败记 NaN）
def aggregate_scores(t, log): return get_best_score(log, select_best_fn=min)  # 取历史最优
```
最终成绩=全过程达到的最佳分（这里最低耗时）；分数经归一化（starting=0、human expert=1）便于横向比。纯 hard（scoring 脚本真跑代码测耗时，无 LLM）。另有 `hidden_score` 变体对 agent 隐藏分数细节，防 overfit 评分脚本。

**对我们的价值**：证明"**过程量可以是连续曲线**"而非离散通过/失败，且 agent 可自查（利于自我纠偏）。前提：目标须可量化成标量（速度/损失/准确率）。是"过程量而非终答"评测的现成范本。

---

## 10. DevBench（部分符合，需甄别）

**是什么**：模拟完整软件开发生命周期（SDLC）分阶段评测。

**完整 case**（C++ `area_calculation`，配置采样自 `repo_config.json`）：给 agent `PRD.md`（需求）、`UML_class.md`、`architecture_design.md`，要求走完 design → environment setup → implementation → unit testing → acceptance testing。`repo_config.json` 结构化定义了 `unit_tests`、`acceptance_tests`、`setup_shell_script`、`unit_test_linking`（如 `test_Circle.cpp → include/Circle.hpp`）、`code_file_DAG`（代码依赖图）。

**怎么测 / 打分**：按阶段给分——环境能否搭建成功、代码能否编译、单元/验收测试通过率、覆盖率。**每个阶段是一个卡点。**

**checkpoint 怎么构建 / 甄别**：实现/测试阶段是 hard 的（编译、ctest/pytest）；但 **design 阶段的文档质量用 LLM/相似度辅助判定，不是 hard**。采用时应**只取可执行验收的阶段作为硬卡点**，排除 design 阶段。

**对我们的价值**：展示"把长任务按 SDLC 阶段切成卡点、每阶段配可执行验收"的思路，但需甄别哪些阶段真 hard。


---

# 第二部分：B 类 —— 结构先进，但判定非纯 hard（参考范式）

## 11. PaperBench（★重量级参考：层次化 rubric 树）

**是什么**（OpenAI）：给 agent 一篇 ICML 2024 论文，要求**从零复现**（读论文、写代码、跑实验、复现结果，禁止看原作者代码）。极限长程任务。核心贡献是把"复现"拆成一棵加权 rubric 树。

**完整 case**（rice 论文 rubric，采样自 `rice_rubric_full.json`，实测结构）：
- 整棵树 **489 节点、361 个叶子**，真实层次：
```
[根,w=1] The core contributions of the paper have been reproduced.
 ├─[w=1] section 4.1 的实验环境已搭建
 │   ├─[w=4] 7 个 non-sparse 环境已搭好     ← 权重高
 │   └─[w=1] 2 个 sparse 环境已搭好
 ├─[w=1] 每个环境的 PPO 策略网络已搭建并预训练
 └─[w=2] 每个环境的解释方法已实现
```
- 叶子按 `task_category` 分三类，实测分布：**Code Development 178 / Code Execution 170 / Result Match 13**。
- 叶子 requirement 例（真实）："Any version of the (MuJoCo) Hopper environment has been correctly set up and can be initialized"（weight 3）。
- 全库 20 篇论文共 **8316 个叶子**。

**怎么测**：agent 产出复现代码库 + reproduce.sh。judge 独立评每个叶子：给它论文 markdown、rubric、该叶子 requirement、以及提交（按相关性取 top10 文件）。三类叶子看不同证据——Code Development 看源码、Execution 看 reproduce.log、Result Match 看复现产物。

**checkpoint 怎么构建（谁定义，这是重点）**：**由 OpenAI 团队与每篇论文的原作者共同开发**，每篇耗时数周（读论文→起草→作者评审→迭代→作者签字）。三条构建规则：① 自顶向下递归分解；② **"15 分钟粒度准则"**——拆到"熟悉论文的专家 15 分钟内能判过没过"才停；③ 兄弟节点相对加权（weight 表重要性，非难度）。设计巧思：Code Development 给 Execution 铺垫部分分，Execution 给 Result Match 铺垫部分分——形成部分给分链。

**怎么打分**：叶子二值判定（1/0）→ 父节点 = 子节点得分的 **weight 加权平均** → 一路传到根 = Replication Score（0~100%）。

**为何非纯 hard**：叶子的二值判定由 **LLM judge** 执行（另建 JudgeEval 用人工标注验证 judge 可靠性，F1≈0.83），不是可执行断言。但其 Code Execution 类（"能否跑起来"）本质可执行化。

**对我们的价值**：**checkpoint 组织方式最先进**——树形分解 + 15 分钟粒度准则 + 加权传播，可直接借鉴；唯一要改的是把叶子判定从 LLM judge 换成可执行断言（尤其 170 个 Code Execution 叶子天然可做到）。

---

## 12. ScienceAgentBench（四维评分）

**是什么**（OSU）：数据驱动科学发现任务，agent 写一个独立可运行的 Python 程序完成分析、产出图表/结果文件。

**完整 case**：**102 个真实任务**，从 **44 篇同行评审论文**抽取，横跨四学科（生物信息、计算化学、地理信息、心理神经），每个任务经该学科博士生/专家验证。典型任务如"用某基因表达数据集做差异表达分析并画火山图"，给任务指令 + 数据集 + 领域知识，产出程序 + 指定产物。

**怎么测 / 打分**（多维评分，采样自 `grading.py`）：
- **VER（Valid Execution Rate）**：agent 的程序能否**无报错跑完** —— **纯 hard**（执行即知）；
- **SR（Success Rate）**：产出结果是否达到任务预设的成功标准（对照 gold 输出/规则判定，半 hard）——评分主线；
- **CBS（CodeBERTScore）**：agent 代码与标准参考代码相似度；
- 外加 API 成本。

**checkpoint 怎么构建**：每个任务的 SR 判定用**任务专属、预先写好、专家验证的成功标准**（如"火山图必须正确标出 p<0.05 且 |logFC|>1 的基因"）+ gold 标准程序。

**为何部分符合**：VER 维度纯 hard、可作中间护栏；但整体粒度到"任务级多维"，非任务内逐步卡点，且 SR 掺规则+参考比对。

**对我们的价值**：科学发现领域"多维评分（可执行率+结果正确性+代码质量）"代表。与 SciCode 互补：ScienceAgentBench 覆盖更真实完整的分析工作流（真而粗），SciCode 卡点更细更硬（细而硬）。

---

# 第三部分：C 类 —— 纯终态验证（对照组，无中间卡点）

## 13. SWE-bench Verified

**是什么**：coding 长程任务事实标准。给 agent 一个真实 GitHub issue + 完整代码仓库，要求产出修复 patch。

**完整 case**（`astropy__astropy-12907`，采样自 `sample_3cases.json`）：
- problem_statement：astropy 的 `separability_matrix` 对嵌套 CompoundModel 计算错误（真实用户报错原文）。
- 附 `base_commit`（出 bug 时刻仓库）、gold `patch`（改 `separable.py` 的 `_cstack`）、`test_patch`、难度标签 "15 min - 1 hour"（人工标）。
- 判分测试清单：`FAIL_TO_PASS`（如 `test_separable[compound_model6-result6]`，修复后应由挂转过）、`PASS_TO_PASS`（防回归）。

**怎么测 / 打分**：容器里应用 agent 的 patch，跑两组测试——FAIL_TO_PASS 必须全由挂转过 + PASS_TO_PASS 必须仍全过 = resolved（1 分），否则 0 分。判分测试是**从修复该 bug 的真实 commit 里 harvest 出来的**。

**为何是对照**：hard 但**只卡终态、无中间卡点**——agent 中途改错文件、绕 200 步弯路都不影响评分。正因如此，社区又在其 trace 上做过程标注（见 D 类 TRAIL/Who&When），恰好反证"只看结果"的不足。

---

## 14. SWE-bench Pro

**是什么**（Scale AI）：SWE-bench 的长程强化版——更难、跨多文件、企业级、抗污染。采样确认 **731 条**。

**完整 case**：字段比原版丰富——除 `patch/test_patch/fail_to_pass/pass_to_pass` 外，新增 `requirements`（结构化需求）、`interface`（接口规格）、`issue_specificity`，单任务改动横跨多文件。

**怎么测 / 打分**：与 SWE-bench 相同的 outcome-only（应用 patch 后跑 fail_to_pass + pass_to_pass 全过才 resolved）。**无中间卡点。**

**对我们的价值**：作为"更长更难但仍无过程分"的最新对照；其 `requirements`+`interface` 结构化字段是"把终态任务改造成带 hard 中间卡点"的良好原料（可据接口规格派生子目标与对应测试）。

---

## 15. MLE-bench（Kaggle）

**是什么**（OpenAI）：75 个真实 Kaggle 竞赛，考 agent 端到端 ML 工程（读数据、特征、训模型、调参、产出提交）。前沿模型报告主力引用。

**完整 case**（`petfinder-pawpularity-score`，预测宠物照片"萌值"1~100）：
- `config.yaml`：`awards_medals: true`、奖牌阈值；
- `grade.py`：判分函数（RMSE）；
- `leaderboard.csv`：**当年真实人类选手的完整排行榜**。

**怎么测 / 打分**（三步，`grade.py` 真实逻辑）：① agent 自主建模产出 `submission.csv`（无监督、无卡点）；② `prepare_for_metric` 做合法性校验（列名、值域 1~100、ID 对齐，不合法抛 `InvalidSubmissionError`=0 分）→ `root_mean_squared_error(y_true,y_pred)` 算 RMSE；③ 把 RMSE 插入真实排行榜换算 **Bronze/Silver/Gold 奖牌**。汇总指标=拿任意奖牌的竞赛占比。

**为何是对照**：判定 hard 但**只作用于最终提交**，中途建模过程无卡点——是"结果分层"（奖牌）而非"过程分层"。

**对我们的价值**："用真实人类排行榜分位定阈值"的思路可迁移为 checkpoint 难度分层依据。

---

## 16. DSBench（Kaggle 系数据科学）

**是什么**：540 个真实数据科学任务。

**完整 case**（采样自两个 `data.json`）：
- **数据建模**（74 个，来自 Kaggle）：每条一个竞赛，如 `{"name":"titanic","url":"...","size":"93.08 kB","year":2012}`；
- **数据分析**（466 个，来自 ModelOff 金融建模竞赛）：每条含一组 `questions`（13 道题）+ 标准 `answers`（如 `["D","D","I","A",...,1661626,323272]`，选择题+数值题混合）。

**怎么测 / 打分**：数据建模用 `score4each_com.py` 拿 agent 预测 csv 对照标准答案算相对性能分；数据分析逐题精确比对答案。**终态判定，无中间卡点。**

**为何是对照**：hard 但无中间卡点，与 MLE-bench 同阵营。其"相对性能分"（相对基线归一化）可作 checkpoint 达标阈值参考。

---

## 17. tau-bench

**是什么**（Sierra AI）：客服场景（retail/airline）多轮工具交互，**用户由 LLM 扮演**。

**完整 case**（retail 第一个 Task，采样自 `tasks_test.py`）：
- `instruction`（给"扮演用户的 LLM"的剧本）："You are Yusuf Rossi in 19122. 收到订单 #W2378156 后想把机械键盘换成 clicky 轴、智能温控器换成支持 Google Home 的…若无 clicky+RGB+全尺寸键盘则接受无背光。你是细节控，想一次搞定。"
- `actions`（金标准操作序列）：`find_user_id_by_name_zip` → `get_order_details` → `get_product_details`×2 → `exchange_delivered_order_items(...)`。

**怎么测 / 打分**：LLM 模拟用户照剧本与被测 agent 多轮对话；agent 查库、确认、执行换货 API。结束后把金标准 actions 在干净数据库重放得目标终态，**与 agent 实际造成的数据库终态逐字段比对**，完全一致=1 分。另有 **pass^k**（同任务独立跑 k 次全对的概率）度量长程稳定性（gpt-4o pass^8 <25%）。

**为何不符合**：含 **LLM 模拟用户多轮交互**（违反"一次性输入"），且只卡终态无中间卡点。

**可借鉴**：pass^k 是度量"长程执行是否容易跑偏"的统计学指标，可与 checkpoint 机制正交组合。

---

# 第四部分：D 类 —— 事后 trace 标注 / 多 agent / 含交互（借鉴偏差 schema）

> 这些的被测对象是"**挑错模型**"（读冻结 trace 找错），或含交互/多智能体协作。轨迹是提前录好 + 人工标注的"案卷"，**不是活环境**——所以只能靠模型/人读文字判断（soft），或不符"单 agent/一次输入"。它们不符合我们需求，但提供了"偏差长什么样、怎么标"的宝贵 schema。

## 18. ProcessBench

**是什么**（Qwen）：数学长链推理的过程级错误定位基准。

**完整 case**（火烈鸟数数题，采样自 `sample_5cases.json`）：
- problem：周五摆 18 只粉火烈鸟；周六拿走 1/3 涂白放回；周日加 18 只粉；问周日中午粉比白多几只？（正确：24）
- 模型解答分 4 步（`steps`），人工标注 `label: 1`（第 1 步就错——把"院子里剩 12 粉"说成"邻居手里剩 12"，对象搞混），`final_answer_correct: False`（答 30）。
- 特意收录"**答案对但中间步骤错**"的样本。

**怎么测 / 打分**：被测模型当 critic，读 problem+steps，输出"最早错误步下标"（全对输出 -1），与人工 `label` 比对，在有错/无错样本上分别算准确率取 F1。

**为何不符合**：人工事后标注冻结解答，被测是挑错模型，非"干活 agent"。

**对我们的价值**：证明"outcome 正确 ≠ 过程正确"（过程评测必要性论据）；"最早错误步"是 math 类过程偏差的标注范式。

---

## 19. PRM800K

**是什么**（OpenAI《Let's Verify Step by Step》）：过程监督奠基数据集。

**完整 case**（圆桌排座题，采样自 `sample_5cases.json`）：
- problem：5 民主党+5 共和党+1 独立议员围圆桌、同党相邻，几种坐法？（答 28800）
- 模型解答**每一步**带人工三值评分：step0 `rating=+1`（正确且推进）、step1 `rating=0`（没错也没推进）、step2 `rating=+1`……
- 全库 **80 万个步级人工评分**。

**怎么测 / 打分（使用方式）**：不是考试卷而是教材——用这些评分训练过程奖励模型（PRM）；PRM 对一题的 N 个解答逐步打分、取乘积选最优（best-of-N）。MATH 上 PRM 选答 78.2% > 只看答案的 ORM 72.4%。

**为何不符合**：人工标注，非在线可执行卡点。

**对我们的价值**：step 级人工评分是过程评测"金标准"上限，但 80 万标注成本不可持续——提示 checkpoint 粒度要在"步级（贵细）"与"里程碑级（便宜粗）"间权衡。

---

## 20. Who&When

**是什么**（ICML 2025）：多智能体失败归因基准，与"多智能体执行中出现偏差"最直接对口。

**完整 case**（武术课任务，采样自 `Hand-Crafted/1.json`）：
- question："纽交所步行 5 分钟内、晚 7-9 点有哪些武术课？"由 Magnetic-One 多智能体系统执行，29 步 history。
- 人工标注：`mistake_agent: WebSurfer`、`mistake_step: 12`、`mistake_reason: 点击了无关网站，破坏了任务求解`。看第 12 步原文：WebSurfer 自称点了跆拳道馆，实际页面是**基恩斯激光显微镜广告页**，浑然不觉。
- 184 条失败轨迹（算法生成 MAS + 手工构建）。

**怎么测 / 打分**：把完整 history 交给被测模型，问"哪个 agent、第几步犯决定性错误"，与人工标注比对准确率。SOTA agent 级 ~53%、**step 级 <15%**（"找出 trace 在哪步偏了"对最强模型也极难）。

**为何不符合**：多 agent + 人工事后标注冻结轨迹。

**对我们的价值**：提供"过程偏差 ground truth"直接标注范式——(失败 agent, 失败步, 失败原因) 三元组。

---

## 21. TRAIL

**是什么**（Patronus AI）：带人工错误标注的 agent 执行 trace 基准。

**完整 case**（一条 GAIA 任务 trace，采样自 `annotations_gaia/`）：
- trace 是 OpenTelemetry 格式（`{trace_id, spans}`，每 span 一次 LLM/工具调用）。
- 人工标 3 个错误：`[LOW] Instruction Non-compliance @span 98fa1dda`（计划漏 `<end_plan>` 标签）、`[HIGH] Tool-related @span bc20feef`（声称"根据 USGS 记录…"但从未真调用工具=幻觉）、`[HIGH] Goal Deviation @span bc20feef`（完全抛弃自己上一步的计划、直接给答案）。
- 每错误四要素：category × location(span id) × evidence × impact(HIGH/MED/LOW)。外加维度评分 reliability 1/5 等。
- 148 条 trace、841 个错误，taxonomy 三大类：Reasoning / System Execution / Planning-Coordination。

**怎么测 / 打分**：被测模型读完整 trace，输出所有错误的位置+类别+影响，与人工标注算 joint F1。SOTA（Gemini-2.5-Pro）joint 分仅 ~18%。

**为何不符合**：人工事后标注冻结 trace，被测是挑错模型。

**对我们的价值**：给出"trace 偏差"的**工业级标注 schema**——错误类别×位置×严重度；其错误类别里**"Goal Deviation"就是正式类别**，可直接用作我们"偏差类型"定义域。

---

## 22. TheAgentCompany（checkpoint 工程最成熟）

**是什么**（CMU 等，NeurIPS 2025）：模拟一家软件公司（自托管 GitLab/OwnCloud/RocketChat + LLM 扮演的 NPC 同事），175 个职场长程任务。

**完整 case**（会议室调度，采样自 `admin-arrange-meeting-rooms`）：
- task.md：给出 6 个团队周二的会议时段，求最少订几间会议室（区间调度，答案 4），要求"把答案通过 RocketChat 告诉 Chen Xinyi，并写入 `/workspace/ans.txt`，只写数字"。
- checkpoints.md：2 个卡点各 1 分。

**怎么测 / 打分**（`evaluator.py` 真实逻辑）：任务结束后运行——
```python
def grade_checkpoint_1():  # 读 /workspace/ans.txt，正则抽数字，须恰好一个且==4
    numbers = re.findall(r'\b\d*\.?\d+...', content); return len(numbers)==1 and int(numbers[0])==4
def grade_checkpoint_2():  # 调 RocketChat API 拉与 Chen Xinyi 的私聊，检查是否含 "4"
```
总分 = 0.5×(是否全完成) + 0.5×(checkpoint 得分比例)。**验证的是环境真实状态（文件、聊天服务器），非 trace 文本。**

**为何不符合**：含模拟同事 NPC 对话交互，不符"一次性输入"。

**对我们的价值**：**目前工程上最成熟的 hard checkpoint 实现**——"里程碑+可执行验证器（查文件/查服务端状态）+部分给分"，机制强烈建议借鉴（数据可不用）。

---

## 23. WebCanvas / Mind2Web-Live

**是什么**：真实在线网站上的 Web Agent 任务（542 任务、2439 中间评估状态）。

**完整 case**（采样自 `sample_3cases.json`）：任务 "Compare available plans for the AeroAPI on flightaware"，`reference_task_length: 4`，3 个 key node 匹配器：`url_included_match: "flightaware."` → `url_included_match: "/aeroap"` → `url_included_match: "#compare-plans-section"`。

**怎么测 / 打分**：agent 在真实互联网操作浏览器；评测器监听每步后的 URL/DOM，按序核对 key node（匹配函数含 `url_included_match`/`url_exactly_match`/`element_value_exactly_match` 等）。**step score = 命中 key node 数 / 总数**（过程分）；全命中 = task success。

**checkpoint 怎么构建**：人类标注员用录制插件录下自己完成任务的轨迹，从轨迹里勾选"任何合法解法都必须经过的必经中间状态"作为 key node（hard）。

**为何不符合**：在线真实网站多步交互、需维护动态环境，不符"一次性输入"。

**对我们的价值**：key node = 路径无关的必经状态谓词，是"判断 trace 有没有偏"对动态环境鲁棒的可操作定义。

---

## 24. OdysseyBench

**是什么**（Microsoft）：办公应用（Word/Excel/PDF/Email/Calendar）上跨多日、跨多应用的长程工作流，602 任务。

**完整 case**（Bob 日历任务，采样自 `task_1-1`）：
- 4/28 Bob 闲聊时顺口说："confirm the time slot from 10:30 a.m to 11:00 a.m on 5/17/2024?"（埋在历史会话里）
- 5/1（考试当天）只说一句 `query_sentence`："Add the meeting to Bob's calendar."（"那个会议"几点根本没说）
- agent 必须从三天前的闲聊里挖出时间。
- evaluation：`evaluate_contain` 打开产物 `Bob.ics`，检查是否含 `DTSTART:20240517T103000`、`Meeting`、`DTEND:20240517T110000`。

**怎么测 / 打分**：执行 `evaluate_contain` 检查产物文件关键字段（hard）；子任务逐个判定后汇总。

**为何不符合**：核心考察**长期记忆检索**（信息散在多日对话里），违反"一次性输入"。

**对我们的价值**：展示"长程 = 时间跨度 + 上下文碎片化"的另一维度，及用多智能体流水线批量合成长程任务的工程路径（HOMERAGENTS）。

---

## 25. MultiAgentBench (MARBLE)

**是什么**（ACL 2025）：面向多智能体系统的协作/竞争评测，6 类场景（research/coding/database/minecraft/bargaining/werewolf）。

**完整 case**（research 协作，采样自 `research_sample_3cases.jsonl`）：给一段联邦学习论文 Introduction，3 个带不同 profile 的研究员 agent 按指定 `coordinate_mode`（star/tree/graph/chain）协作产出新研究 idea；case 内嵌 `metrics: {diversity_of_perspectives, engagement_level, relevance}`。

**怎么测 / 打分**：MARBLE 引擎按拓扑运行多轮群聊；两层评分——**Task Score**（milestone 达成，规则+LLM judge）+ **Coordination Score**（通信/规划质量）。

**为何不符合**：**多智能体协同 + LLM judge 判定**，双重不符。

**对我们的价值**：唯一现成的"多智能体 + 里程碑 KPI"范式；但判定依赖 LLM judge，客观性弱于 TheAgentCompany 的程序化 checkpoint。

---

# 收束：给本项目的三张牌 + 一条判据

**符合需求（A 类）里，按卡点形态给你三张可直接用的牌：**
1. **连续过程分**（RE-Bench）：目标可量化时，过程量 = 终态质量函数在时间轴上的连续采样，随时可测、取最优；
2. **测试通过率**（Commit0/USACO/SciCode）：有可执行测试时，通过比例 = 过程分，且测试可 **harvest**（复用现成）；
3. **谓词满足率**（AgentBoard-PDDL/TravelPlanner）：目标可形式化成合取式/约束时，subgoal **auto-derive** 自动拆出、布尔判定、零人工。

**一条选任务载体的硬判据**：想要 hard 中间卡点，任务背后**必须有一个能随时导出结构化真值的环境**（仿真器/文件系统/数据库/可执行测试）。有 → 可 hard（agent 看皮肤、判分查账本）；只有自然语言输出、无结构化真值 → 只能 soft（LLM judge）。选 AI4S 任务时，第一个要问的就是这个。

**判定器的三种形态（解释为何空白存在）**：hard 中间检查目前只有三条路——可执行单元测试、正则痕迹匹配、人工硬编码谓词；而"语义级中间检查"（判断'这一步思路对不对'）目前几乎只能靠 **LLM judge（soft）**。一旦坚持 hard，就被逼回前三条。

**当前空白（本项目的 novelty 位置）**：对**开放式任务**自动产生"**语义级 + 路径无关 + hard 判定**"的中间 checkpoint——现有方案只能三选一（语义但 soft 的 LLM judge / hard 但需任务本身可测 / hard 且语义但靠人工），四者兼得者尚无。谁能填这个格子（哪怕限定某类任务），就是贡献。

---

## 附：25 个数据集速查表

| # | 数据集 | 类 | checkpoint 机制 | 判定 | 合需求 |
|---|---|---|---|---|---|
| 1 | SciCode | A① | 子问题单元测试 | hard | ★ |
| 2 | SUPER | A② | landmark 正则查日志 | hard | ★ |
| 3 | LongProc | A③ | 中间步确定性规则 | hard | ★ |
| 4 | TravelPlanner | A③ | 约束函数 Micro Pass Rate | hard | ★ |
| 5 | Commit0 | A① | 测试通过率（harvest） | hard | ★ |
| 6 | AgentBoard | A② | 仿真器谓词集布尔比对 | hard | ★ |
| 7 | AgentQuest | A③ | driver progress rate | hard | ★ |
| 8 | USACO | A① | 测试点通过比例 | hard | ★ |
| 9 | RE-Bench | A④ | 连续 scoring（随时可测） | hard | ★ |
| 10 | DevBench | A① | SDLC 分阶段（部分 hard） | 混合 | 部分 |
| 11 | PaperBench | B | 加权 rubric 树（489节点） | LLM judge | 参考 |
| 12 | ScienceAgentBench | B | VER/SR/CBS 多维 | 混合 | 参考 |
| 13 | SWE-bench Verified | C | FAIL/PASS_TO_PASS 终态测试 | hard | 对照 |
| 14 | SWE-bench Pro | C | 同上，长程强化 | hard | 对照 |
| 15 | MLE-bench | C | RMSE→奖牌，终态 | hard | 对照 |
| 16 | DSBench | C | 终态指标 | hard | 对照 |
| 17 | tau-bench | C | 终态数据库比对 + pass^k | hard | 对照(含交互) |
| 18 | ProcessBench | D | 标"最早错误步" | 人工 | schema |
| 19 | PRM800K | D | 步级人工评分 | 人工 | schema |
| 20 | Who&When | D | (agent,step,reason) | 人工 | schema |
| 21 | TRAIL | D | span 级错误 taxonomy | 人工 | schema |
| 22 | TheAgentCompany | D | checkpoint+evaluator.py | hard | 机制借鉴 |
| 23 | WebCanvas | D | key node URL 匹配 | hard | 机制借鉴 |
| 24 | OdysseyBench | D | 产物文件校验 | hard | 机制借鉴 |
| 25 | MultiAgentBench | D | milestone-KPI | LLM judge | 机制借鉴 |
