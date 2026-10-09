# 长程任务 Agent Benchmark 调研报告（合并版）

> 本文由原 v1（第一轮·范式普查）与 v2（第二轮·按需求边界筛选 + 榜单生态）合并重写，按"是否符合本项目边界"而非"第几轮"组织；原 `长程任务Benchmark_v1备份.md` 已失效删除。调研日期：2026-07。
> 所有数据均已实际下载到 `reference/`（每文件夹含采样 case + `README_采样说明.md`），每条结论可追溯到 arXiv 论文或官方仓库。

---

## 一、总结论（先读这里）

1. **checkpoint（判断 trace 有没有偏的中间卡点）有四种构建范式**：①出题时埋点、程序化判定（hard 主流）；②事后逐步人工标注（信号密但成本不可持续）；③事后对冻结 trace 做错误归因（是"阅卷"不是"考试"）；④纯终态对照（无法区分"差一步失败"与"完全跑偏"）。本项目只取①。
2. **按本项目三条硬边界**（单 agent + 一次性输入 + hard 中间 checkpoint），**直接可用 10 个**：SciCode、SUPER、LongProc、TravelPlanner、Commit0、AgentBoard、AgentQuest、DevBench（部分）、USACO、RE-Bench（后两个来自榜单/Kaggle 生态）。
3. **社区趋势印证本项目方向**：ProcessBench 专门收录"答案对但过程错"的样本；τ-bench 的 pass^8<25% 说明长程稳定性远低于单次成功率；TRAIL 显示 SOTA 模型定位 trace 错误的能力仅约 18%。但"**hard + 过程级**"的 benchmark 仍稀缺——这正是本项目的空白。

---

## 二、需求边界（筛选口径）

目标 benchmark 必须同时满足：**① 单 Agent**（非多智能体协同）；**② 一次性输入**（信息一次给全，执行中无模拟用户对话、无长期记忆检索）；**③ hard 中间 checkpoint**（执行中途有卡点判 trace 偏没偏，判定必须是可执行断言/规则匹配/状态比对/单元测试，**不能是 LLM judge 或人工**）。

---

## 三、符合边界的数据集（★ 直接可用，均已落盘）

### 匹配度总表

| # | 数据集 | 领域 | hard 中间 checkpoint 形态 |
|---|---|---|---|
| 1 | **SciCode** | 科研编程 | 每个子问题一组可执行断言（338 个） |
| 2 | **SUPER** | 科研仓库执行 | landmark 正则匹配执行日志关键行 |
| 3 | **LongProc** | 长程序化生成 | 中间步确定性规则逐步校验 |
| 4 | **TravelPlanner** | 规划 | hard/commonsense 约束谓词（Micro Pass Rate） |
| 5 | **Commit0** | 从零建库 | 分层单元测试通过率（随实现单调上升） |
| 6 | **AgentBoard** | 具身/游戏/工具 | subgoal 正则匹配环境反馈（progress rate） |
| 7 | **AgentQuest** | 推理/搜索 | driver 确定性计算 progress rate |
| 8 | **USACO** | 竞赛编程 | 每题 N 个测试点，通过比例部分给分 |
| 9 | **RE-Bench** | ML 研究工程 | 连续 scoring function，随时可测 |
| 10 | **DevBench**（部分） | 软件工程 | SDLC 分阶段：编译/测试/覆盖率（design 阶段非 hard，只取可执行阶段） |

### 逐个说明（case 与判分机制）

**1. SciCode ★最贴合**
- case：主问题 Berendsen_thermostat（分子动力学）被科学家分解为 12 个有依赖的子问题。
- checkpoint：每子问题带 `assert np.allclose(f(x), target)` 式断言；子问题通过率=中间卡点分，全过=主问题解决。
- 关键：checkpoint 由领域专家出题时定义、可执行断言表达，完全 hard。

**2. SUPER ★最贴合（coding/科研方向一号参考）**
- case：`colbert` 任务——"在我的数据上训练 ColBERT 并报告 last_loss"。
- checkpoint：任务标注 `landmarks`（正则），如"模型正确加载"的警告行、"开始打印 loss"的格式行；`evaluate_checkpoints()` 正则搜执行历史算命中率=过程分，终态数值精确比对=终态分。
- 关键：landmark=执行必经的确定性 stdout 谓词，纯正则，完全 hard。

**3. LongProc ★最贴合**
- case：countdown 凑数 `nums:[21,16,17,26] target:46`，标准解 `["21+16=37","26-17=9","37+9=46"]`。
- checkpoint：逐步校验——每步算术对不对、是否合法使用剩余数字、推理链自洽否，全确定性程序判定。

**4. TravelPlanner**
- case：3 天旅行规划、预算 $1400，产出逐日行程。
- checkpoint：`hard_constraint.py`（预算/房型/交通）+ `commonsense_constraint.py`（连贯/合理/完整），每条一个 True/False 函数；Micro Pass Rate=通过约束比例。

**5. Commit0**
- case：从 commit 0 把某真实 Python 库（57 个之一）从空实现补全到通过测试。
- checkpoint：库自带分层单元测试，每实现一部分即可跑对应测试，通过数随实现单调上升（pytest，无 LLM）。

**6. AgentBoard**
- case：ALFWorld "look at bowl under the desklamp"——subgoals=[看见碗/捡起碗/找到台灯]（正则）。
- checkpoint：正则匹配环境每轮观测文本，命中即 progress +1/n；Progress Rate=达成 subgoal 最大比例。
- 补充实证：全 134 个 alfworld 任务的 subgoal 仅约 6 种骨架——"任务族写一次模板、实例自动填槽"，非逐条人工。

**7. AgentQuest**
- case：Mastermind 猜 4 位密码（上千实例）。
- checkpoint：`driver.py` 确定性计算 progress=已猜对位数比例，每次猜测后更新。价值在"通用过程指标框架"。

**8. USACO**（Princeton，榜单系）
- case：竞赛编程题，每题 N 个隐藏测试点。
- checkpoint：`fraction_tests_passed`=通过测试点/总数（部分给分，hard）；被 HAL 主榜采用，是竞赛编程标准范式。

**9. RE-Bench**（METR，榜单系）★checkpoint 的高级形态
- case：开放式 ML 研究工程任务（如优化 kernel），agent 长时间自主迭代。
- checkpoint：**连续 scoring function**（相对 human expert 归一化），把"过程偏没偏"量化为"当前分离目标多远"，且 agent 可自查——最贴合"过程量评测"。

**10. DevBench（部分符合）**
- case：C++ 项目走 design→setup→implement→test 全流程。
- checkpoint：编译通过、单元/验收测试、覆盖率（实现与测试阶段 hard；design 阶段用 LLM 辅助，采用时只取可执行阶段）。

### 小结：hard checkpoint 的三条构建路线 + 一条正确性双检验

1. **单元测试即卡点**（SciCode/Commit0/DevBench/USACO）：出题或建库时天然带测试，checkpoint=测试项。
2. **执行痕迹谓词**（SUPER/AgentBoard）：从执行日志/环境反馈用正则捕获"必经的确定性输出"；可按任务族模板复用。
3. **约束/规则库**（TravelPlanner/LongProc/AgentQuest）：把过程正确性写成一组可独立判定的约束函数，度量命中比例；规模化最友好。

**正确性双检验**（三条路线共同遵守）：金标准轨迹回放必须全过（必要性）+ 非解/空转轨迹不应全过（区分度）——无需 LLM 或人做裁判。

---

## 四、不符合边界但值得借鉴的数据集（存档，均已落盘）

### 四-A：checkpoint 是 hard 的，但非"单 agent + 一次输入"

| 数据集 | 机制 | 不匹配原因 |
|---|---|---|
| **TheAgentCompany** | `checkpoints.md`+`evaluator.py` 程序化查环境状态，**工程最成熟** | 含模拟同事对话交互；但其"checkpoint=与路径无关的可判定中间状态谓词、验证信号取自环境状态而非 trace 文本"是最佳工程范式，强烈建议借鉴 |
| **WebCanvas / Mind2Web-Live** | key node URL/元素匹配 | 在线真实网站多步交互，需维护动态环境 |
| **τ-bench** | 终态数据库状态比对 + pass^k 稳定性（gpt-4o pass^8<25%） | 含 LLM 模拟用户多轮对话；仅终态无中间卡点 |
| **SWE-bench Verified / Pro** | fail_to_pass/pass_to_pass 单元测试 | 仅终态验证，无中间 checkpoint（Pro 的 requirements/interface 字段可作改造原料） |
| **OdysseyBench** | subtask 产物文件校验 | 核心考察跨日长期记忆检索，与"一次性输入"冲突（其 HOMERAGENTS 自动造题流水线可参考降成本） |

### 四-B：过程思想相关，但判定靠人工/LLM（非 hard）

| 数据集 | 机制 | 不匹配原因 |
|---|---|---|
| **ProcessBench** | 标注推理链"最早错误步"，专收"答案对但过程错" | 人工事后标注冻结解答；被测是"挑错模型"非"干活 agent" |
| **PRM800K** | 80 万步级人工评分（过程监督 78.2% > 结果监督 72.4%） | 纯人工标注，成本不可持续 |
| **Who&When** | (mistake_agent, mistake_step, mistake_reason) 三元组标注 | 多智能体失败轨迹事后归因；人工标注 |
| **TRAIL** | span 级错误 taxonomy（Reasoning/System/Planning 三类，SOTA 定位仅 ~18%） | 事后标注冻结 trace；其错误 taxonomy 可作我们偏差标注 schema |
| **MultiAgentBench (MARBLE)** | milestone-KPI（唯一多智能体卡点范式） | 多智能体 + LLM judge，双重不符 |
| **PaperBench** | 层次化 rubric 树（数百节点，加权）评论文复现 | judge 判定非 hard；但"长任务递归分解成加权 rubric 树"的组织范式可借鉴（叶子判定需换成可执行断言） |

### 四-C：思想参考（无本地采样）
- **OSWorld**（arXiv:2404.07972）：134 个 execution-based getter 验证终态（hard，但主要是终态；GUI 多步交互）。
- **METR 时间跨度**（arXiv:2503.14499）：长程能力度量方法学（任务时长上限），非 checkpoint 型。
- **MLE-bench / DSBench**：Kaggle/数据科学终态分级（奖牌/指标），无中间卡点——印证"过程级 hard"是空白。

---

## 五、榜单生态（HAL / Kaggle 系怎么测 long-horizon）

| 榜单 | 主办 | 评测内容 | 中间卡点 |
|---|---|---|---|
| **HAL** | Princeton | SWE-bench Verified、USACO、AppWorld、tau-bench 等，成本感知 | 取决于底层 benchmark（USACO 测试点部分给分） |
| **Artificial Analysis** | 商业 | Terminal-Bench Hard、SWE-bench、SciCode 等 | 底层决定 |
| **Epoch AI** | Epoch | METR 时间跨度、FrontierMath | 连续时间跨度 |
| **METR** | METR | RE-Bench、HCAST | **RE-Bench 连续 scoring** |

**结论**：榜单本身不定义卡点，卡点由底层 benchmark 决定；主流仍以"终态成功率+成本"为主轴，含 hard 中间卡点的底层 benchmark 只有 USACO（测试点）和 RE-Bench（连续 scoring）——均已收入本文第三节。

---

## 六、对本项目建 Benchmark 的启示（两轮统一结论）

1. **工程形态**：TheAgentCompany 式"里程碑 + 可执行验证器 + 部分给分"最成熟——验证信号取自**环境状态/产物**而非 trace 文本，客观可复现；checkpoint 本质是"与路径无关的可判定中间状态谓词"。
2. **偏差定义域**：TRAIL 三类错误 taxonomy + Who&When 的 (who, when, why) 三元组，可直接用作"trace 偏差"标注 schema。
3. **指标组合**：Progress Rate（过程健康度）× checkpoint 部分给分（里程碑达成）× pass^k（长程稳定性）× 连续 scoring（RE-Bench，离目标距离）——四者正交，可组合成完整过程量度量。
4. **构建成本**：AgentBoard 实证"任务族模板+实例填槽"（6 模板覆盖 134 任务）、OdysseyBench 自动生成流水线，证明长程任务可批量合成；PRM800K 教训是纯人工步级标注不可持续。
5. **待办**：AppWorld（arXiv:2407.18901）：state-based 单元测试 hard、单 agent，高度符合，数据需 `appworld` 包安装下载，建议下轮补采。

---

## 七、来源依据（全部经实际访问核验，2026-07）

- 符合边界：SciCode arXiv:2407.13168｜SUPER arXiv:2409.07440｜LongProc arXiv:2501.05414｜TravelPlanner arXiv:2402.01622｜Commit0 arXiv:2412.01769｜AgentBoard arXiv:2401.13178｜AgentQuest arXiv:2404.06411｜DevBench arXiv:2403.08604｜USACO（Princeton/HAL）｜RE-Bench arXiv:2411.15114
- 存档：TheAgentCompany arXiv:2412.14161｜WebCanvas arXiv:2406.12373｜τ-bench arXiv:2406.12045｜SWE-bench arXiv:2310.06770 / Pro arXiv:2509.16941｜OdysseyBench arXiv:2508.09124｜ProcessBench arXiv:2412.06559｜PRM800K arXiv:2305.20050｜Who&When arXiv:2505.00212｜TRAIL arXiv:2505.08638｜MultiAgentBench arXiv:2503.01935｜PaperBench arXiv:2504.01848｜MLE-bench (OpenAI)｜DSBench arXiv:2409.07703
- 方法学/综述：OSWorld arXiv:2404.07972｜METR arXiv:2503.14499｜RUC-NLPIR/Awesome-Long-Horizon-Agents（2026，多个候选项经其交叉确认）｜arXiv:2503.16416、arXiv:2507.21504
- 下载脚本（可复跑）：`download_samples.py`（11 个）/`download_samples_round2.py`（8 个）/`download_samples_round3.py`（6 个榜单系）
