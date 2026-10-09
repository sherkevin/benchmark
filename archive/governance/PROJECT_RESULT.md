# AI4S(药物 + 材料)Agent 过程量(中间 checkpoint)评测 Benchmark v0.2

> ⚠️ **核心轴已对齐 `goal.md` v1.0(2026-06-08)**:主轴由「人类介入程度」**还原为更一般的「中间 checkpoint / 过程量度量」**;介入度降为过程量的一种操作化、不再是唯一形态。本文 §1 背景 / §3 关键发现的措辞已局部修订;findings 与建议本身不变(原结论仍成立,只是用更一般的框架表述)。

| 字段 | 值 |
|---|---|
| **项目 ID** | AI4S-PROCESS-METRICS(原 AI4S-AUTONOMY,2026-06-08 重命名) |
| **文档类型** | 项目级汇总(聚合各 task result;非单个 task)|
| **状态** | 🟡 进行中 · 预 MVP(锁定时机 = MVP 核心集发布) |
| **负责人** | jingwu |
| **审阅人** | (待补)|
| **创建 / 锁定** | 2026-06-02 / N/A(预 MVP)|
| **项目级 GOAL** | `goal.md` —— **三层 GOAL(权威定义)**;后续所有 task 优先级以此为准 |
| **上游需求** | `src/prd/AI4S_agent_autonomy_metrics.md` —— PRD(范围 §1 保留;§2 介入度主轴已被 `goal.md` 还原为工具箱之一)|
| **下游依赖** | (即将开启)阶段 1 核心集构建 task / 阶段 2 评测回路 task |

---

## 一句话结论

> 🟢 **建带过程量 + 人类介入度量的 AI4S agent benchmark 这条路成立、有差异化、有现成杠杆**:全景调研无正面竞争,最相关的 MolBench 不可直接用但其脚手架可大幅降本,测量仪器原型在真实数据上跑通,粒度策略实测无错误传播——**推荐进入阶段 1 MVP 核心集构建,8–11 周可出 30–50 题**。

## 展开(4 点)

1. **没人做了我们要做的**(task4 全景调研):医疗 / AI4S / 通用 三大方向均未发布 L0–L3 graded 人类介入 GT;最相关的 TheAgentCompany 评分模板可借,但任务非科学。**差异化清晰,无正面竞争。**
2. **MolBench 不能直接用但其仓库脚手架是高价值杠杆**(task2):释放数据是 190 条单轮 final-answer QA;但 14 个 L2 工作流 + 26 条 L3 原则 + 60 个 L1 工具 = 一份"金标准轨迹/介入卡"草稿,**专家从"创作者"降为"审核者"**——这是项目最大的成本杠杆。
3. **测量仪器在收敛 checkpoint 上已被原型验证**(task3):分级评分 + 偏离检测 + 分级介入回路 + 介入-成功率曲线,在真实 MS-3 数据上跑通且对模拟 agent 强弱单调区分;**开放式 L0 路径偏离判定仍是研究级难点**——主指标先建在收敛 checkpoint,开放任务用主动求助 + 过程 rubric + κ 公开作过渡。
4. **粒度策略实测可放心**(task1 + task5):NobelClaw 上"分步错误传播"假设在 9 步数值依赖 / 工具混合 / 纯叙述 三种压力链路下均未观察到(task1);**平台原生"分阶段"模式 0/6 能跑完**(synthesis 卡死,task5)—— 两条独立证据指向同一结论:**评测里的多步组装应放在 evaluator 端,不要依赖被测平台的"自动并行/汇总"**。

---

## 1. 背景与目标

### 1.1 为什么做这个项目

PRD 提出:现有 agent benchmark **只测最终结果**(成功率),回答不了一个对真实部署至关重要的问题——**"agent 走的过程是否可信、是否在该停的地方停、需要多少人类介入才能完成?"**。我们要建一个以**「中间 checkpoint / 过程量度量」**为核心轴的 AI4S 长程任务 benchmark,产出可被任意 agent 公平测、产品化后无需真人现场参与、能定位失败发生在哪个阶段的多维度指标。

> 历史说明:PRD 写作时把核心轴具体写成「人类介入程度」;`goal.md` v1.0 已把主轴**还原**为更一般的「过程量度量」,介入度作为它的一种核心操作化保留(因为有了 checkpoint / GT,才谈得上"在哪里、什么深度的介入能救回")。

差异化关键:其他 benchmark 测"能力 / 成功率"输出一个标量分,我们**在中间状态插桩**输出一条函数曲线 + 过程级诊断——**这是 agent 系统报告里最缺、对部署决策最有用的那根轴**。"自主性 / 介入度"是该轴上最具部署意义的切面之一。

### 1.2 本项目的 GOAL

**本项目完成的标准 = 出一个 30–50 题的 MVP 核心集**,含:
- 分层任务集(L0–L3 prompt 分级)
- 分级介入协议(每阶段 4 档介入卡)
- 对照裁判(收敛 checkpoint 上手握 ground truth 的廉价裁判)
- 介入-成功率曲线 / autonomy@0 / 达标介入数 / 许可校准度等多维指标
- 容器化 harness、负对照 agent、版本化 + 污染审计

### 1.3 范围与非目标

- **范围内**:可发表的 MVP 核心集(收敛型 checkpoint 主导);全套评测协议与 harness;对外汇报(本汇总锁定后定稿)。
- **非目标**:对**任意开放任务自动判路径偏离**(研究级难点,不承诺);超大规模题库(>200 题)(MVP 阶段不追求规模,追求质量)。

---

## 2. 方法 / 信息来源

调研 + 实证两条线并行:

- **task1**:对 NobelClaw 平台做控制变量实验,8 组配对 + 多轮压力测试,验证粒度策略的合理性和"错误传播"假设是否成立。
- **task2**:实查 MolBench 数据(190 条 record 级)+ 仓库源码(grep 验证)+ 论文逐节核对,得到"不可直接改造但脚手架高价值"的结论。
- **task3**:用真实 CK2α 种子搭原型,在 25 个真实 MS-3 任务上跑参考排序器 + 模拟 agent 介入回路,数值与解析期望吻合(`random hits@3=0.40`、`auroc≈0.5`)证明实现正确。
- **task4**:deep-research workflow——5 角度并发搜索 → 19 一手源 → 94 声明 → 25 条 3 投票对抗式核验(23 通过、2 被驳),对抗式核验直接挽救了报告里的 2 处过度声明。

---

## 3. 关键发现(从 4 份 task result 聚合)

| # | 发现 | 证据 | 置信度 |
|---|---|---|---|
| 1 | 现成 benchmark 全景中**无 L0–L3 graded 人类介入 GT** —— 我们差异化清晰、无正面竞争 | `task/task4/benchmark_landscape_report.md`、`task/task4/result.md` §2 | 中-高(综合判断,基于 23 条 high-confidence 声明)|
| 2 | TheAgentCompany 的 checkpoint+rubric+`Collaboration` 类别+部分得分公式可作为评分模板**直接借** | arXiv:2412.14161,task4 §2.1 | 高(3-0)|
| 3 | MolBench 释放数据 = 190 条单轮 final-answer QA,**无任何过程量字段** | `task/task2/result.md` §1–§2 | 高(已锁定)|
| 4 | MolClaw 仓库的 L1(60)+L2(14)+L3(2)= 高质量"金标准轨迹/介入卡"草稿,专家从创作者降为审核者 | task2 §3,`src/molbench_repo/` | 高(已锁定)|
| 5 | 收敛 checkpoint 上的"测量仪器"在真实 MS-3 数据上跑通且对模拟 agent 单调区分 | `task/task3/B_ms3_screening_prototype/results.md`、`task/task3/C_feasibility_demo/results.md` | 高(数值正确性自检通过)|
| 6 | per-instance 金标准轨迹 + 分级介入卡可由 L2 模板"填空"产出(以 CK2α 为例)| `task/task3/A_worked_example_CK2alpha.md` | 中(已写出,**未对真实 LLM 实测**)|
| 7 | 开放式 L0 设计任务的"被动偏离判定"廉价裁判前提**不成立** —— 限定收敛核心 | `task/task3/C_feasibility_demo/C_feasibility_assessment.md` §3 | 高(方法论分析)|
| 8 | NobelClaw 上"分步错误传播"假设在 3 种压力链路下**未观察到** —— 细粒度评测可行 | `task/task1/analysis.md` §3–§7 | 高(8 组实测同向)|
| 9 | 数据可追溯到一手源(CARA→ChEMBL、ACNet、ChemCoTBench)—— 可绕过 MolBench 直接采 | `task/task2/result.md` §1.1 + §3.3 | 高 |
| 10 | NobelClaw 平台**原生"分阶段(并行)"模式 0/6 完成**(synthesis 卡死);单步 6/6 完成 —— 评测器**不能依赖被测平台的并行/汇总能力**,多步组装应在 evaluator 端 | `task/task5/CONCLUSION.md`、`task/task5/results.md` | 高(6 道题实测,2026-06-04)|

---

## 4. 决策与建议

### 4.1 项目当前判定

**🟢 Go(进入阶段 1 MVP 核心集构建)** —— 调研证据指向"差异化无竞争、杠杆充足、原型已验证";风险已识别且有应对(收敛核心 + 开放任务过渡方案)。

### 4.2 行动建议(带优先级 + 时限)

| # | 动作 | 负责人 | 时限 | 优先级 |
|---|---|---|---|---|
| 1 | **真实 LLM 实测最小实验**(CK2α,Claude Code 技能版 vs vanilla,k 次重复)+ 收敛 checkpoint 上裁判与人工标注 κ 校准 | 维护者 + 1 名审核者 | 阶段 0(1 周内)| **P0** |
| 2 | 联系 MolClaw 作者**索取 MolBench-E2E 数据/rubric**(论文承诺"will be released")| 维护者 | 阶段 0(1 周内)| **P0** |
| 3 | 抽 MS-2 / MS-3 / MO seed 清单(33 + 25 + 78 个),叠加从 ChEMBL / ACNet 直采,落库为 seed pool | 维护者 | 阶段 1 第 1 周 | P0 |
| 4 | 用 L2-05 模板 + TheAgentCompany 评分契约,生成 30–50 个任务实例的 L0–L3 + 4 阶段 checkpoint + 分级介入卡草稿,**专家审核** | 维护者 + 专家 | 阶段 1(4–6 周)| P0 |
| 5 | 评测器实现(checkpoint 插桩 → 偏离/求助触发 → 喂介入卡 → 续跑;借 AppWorld 的 collateral damage 思路;`<finish>/<answer>` 契约复用 MolClaw)| 维护者 | 阶段 2(3–4 周)| P1 |
| 6 | 容器化 harness、负对照 agent、版本化 + 污染审计 | 维护者 | 阶段 2 末 | P1 |
| 7 | 补充调研(1 轮)回答 task4 的 4 个开放问题(尤其 BioProt license + 2025–2026 是否有未 surface 的 medical benchmark)| 维护者 | 阶段 1 前 | P1 |

---

## 5. 风险与未决问题

| 类型 | 描述 | 影响范围 | 缓解 / 待决方 |
|---|---|---|---|
| 已知风险 | E2E 数据要不到 | 阶段 1 长程任务集需自建,~+2 周工作量 | 行动 #2;若拿不到,从 MS/MO seed 重组为多阶段任务(`task/task2/result.md` 已有方案)|
| 已知风险 | 真实 LLM 实测可能 invalidate 模拟 agent 上的强区分度结果 | task3 的"已证明"档可能降为"待证明" | 行动 #1 是首个验证关卡;通过则升为"真实 agent 上有区分力";不通过则暴露要修的具体地方 |
| 已知风险 | 专家审核带宽不足 | 阶段 1 进度 | 用强模型生成草稿,专家只校验修订(PRD §5.1);先 30 题不强求 50 题 |
| 已知风险 | 开放式任务的偏离判定研究级难 | 影响 benchmark 通用性 | 限定边界发布(收敛核心)+ 开放任务用主动求助 + 过程 rubric + 公开 κ 作为过渡 |
| 已知风险 | 污染(MolBench 全是已知分子)| 主指标 autonomy@0 可能虚高 | 反事实扰动 + 污染探针(原始 vs 扰动成对题)兜底 |
| 未决问题 | BioProt 数据 license 是否可重发布 | 是否能把 BioProt 伪代码对纳入我们的 seed pool | 联系 Samuel Rodriques / FutureHouse(行动 #7)|

---

## 6. 触发重审条件

本汇总进入"locked"状态后,**任一条件成立即需重审**:

1. **真实 LLM 实测**(行动 #1)结果与 task3 模拟 agent 强区分度结论矛盾 → 影响 §3 发现 5/6
2. **MolBench-E2E 数据释放**(行动 #2)→ 影响 §3 发现 3/4 与阶段 1 工作量估算
3. **PRD 重大修订**(三层过程量定义、L0–L3 分级方案、介入轴定义)→ 影响 §1.2 GOAL
4. **新 medical / AI4S benchmark 发布,带 checkpoint 或 L0–L3 GT** → 影响 §3 发现 1 的差异化判断
5. **阶段 1 完成 30 题核心集** → 本汇总升级到 v0.2,锁定时机已到

---

## 7. 交付物指引

| 类别 | 路径 | 说明 |
|---|---|---|
| **task result(5 份)** | `task/task1/result.md` / `task/task2/result.md` / `task/task3/result.md` / `task/task4/result.md` / `task/task5/` | 各 task 的对外成品 + 证据指针 |
| 详细分析 / 主报告 | `task/task1/analysis.md` / `task/task4/benchmark_landscape_report.md` / `task/task5/results.md` | 各 task 的 long-form 分析 |
| 子产物 / experiment 文档 | `task/task1/` 各脚本 / `task/task3/{A,B,C}_*` / `task/task5/{runs,experiment}/` | 实验数据、可运行代码、运行结果 |
| 上游 PRD | `src/prd/AI4S_agent_autonomy_metrics.md` | 项目级需求 |
| 一手原始拉取 | `src/molbench_repo/` `research/素材_task4_*` `wechat_article.md`（根目录） | 论文 / 仓库 / 通稿 |
| 调研笔记 / 事实底座 | `reference/NobelClaw/证据_molbench_dataset.md` `research/素材_task4_deep_research_raw.json` | 中间证据 / 核查留痕 |
| 模板与约定 | `task/_templates/` | goal / experiment / result 模板 + 索引 |

---

## 附录 A · 变更日志

| 版本 | 日期 | 变更 | 作者 |
|---|---|---|---|
| v0.1 | 2026-06-04 | 初版,聚合 archive/task1/2/3/4 result 形成项目级汇总 | jingwu |
| v0.2 | 2026-06-04 | 文件由 `BLOCK.md` 改名为 `PROJECT_RESULT.md`,内部"block"措辞统一为"项目级汇总"(对齐"不再有单独 block 文档"的约定)| jingwu |

## 附录 B · 术语 / 缩略语

- **PROJECT_RESULT(本文件)**:项目级汇总,**全项目仅 1 份**;聚合各 task result、给项目级判定与路线。与 task 级 `result.md` 是同一思想的不同尺度。
- **Task Result**(`taskN/result.md`):每个 task 的**对外成品**,自包含,顶部带 `Status: done/blocked/abandoned`。
- **Experiment 文档**:运行细节 / 原始数据 / 可运行代码(内部),放 `task/taskN/experiment/`。
- **L0–L3**:prompt 详细度分级(L0 仅给目标,L3 详细步骤),用于刻画 agent 自主性曲线(见 PRD §2.3)。
- **autonomy@0**:零介入下的端到端成功率(主指标)。
- **达标介入数**:让成功率达到阈值(如 80%)所需的最小深度加权介入量。
- **κ(Cohen's Kappa)**:对照裁判与人工标注一致率的标准度量。
- **收敛型 checkpoint**:存在唯一可判定正确路径的阶段(如筛选/过滤/确定性编辑/锁定 box 对接);开放任务相对。
