# 多步并行模式端到端不可用 v0.1

> **Block 文档 = 对外汇报的成品**(钉钉/邮件/上汇报)。详细过程与数据见 `experiment/`,内部收尾见 `results.md` / `CONCLUSION.md`,本文件只对外定调。

| 字段 | 值 |
|---|---|
| **Block ID** | TASK5-PARALLEL-MODE |
| **类型** | Investigation |
| **状态** | 🟡 Under Review(草拟,待平台方确认根因 / 排期修复) |
| **负责人** | jingwu |
| **审阅人** | (待补)|
| **创建 / 锁定** | 2026-06-04 / N/A |
| **上游需求** | `task5/goal.md`(测两种回答模式的质量差异);项目级汇总 `../../PROJECT_RESULT.md` |
| **下游依赖** | 评测设计(「多步组装应放 evaluator 端」结论);NobelClaw 平台方(并行模式修复) |

---

## 一句话结论

> 🔴 **NobelClaw 平台原生「分阶段(并行)」模式当前端到端不可用**:6 道多阶段题并行 **0/6 跑完**(全部在汇总前卡死,`synthesis_start` 事件 0/5 出现),且并行执行面板**切换会话即丢失**;同批题单步模式 **6/6 正常交付**。后端、前端两处根因均已定位到**具体代码行**并给出分层修复。

## 展开(5 点)

1. **后端卡死**:N 个子 agent 经 `asyncio.create_task` **并发复用同一个请求级 `AsyncSession`**,并发提交触发 `InvalidRequestError`,且抛错点在守护哨兵的 `try` 之外 → `DONE_SENTINEL` 永不入队 → 消费循环死等 → 永不汇总、SSE 不关闭。
2. **前端丢面板**:并行结构在**三层**被丢——前端状态是模块级单例且切换时被无条件覆盖、`loadHistory` 不重建 `subAgents`、后端 `/messages` 不返回 `sub_agent_id`。
3. **次要并行 bug**:子 agent 出现工具名翻倍 `crystal_generatecrystal_generate` 的无效调用,仅并行可见,是并行报错≈单步 2 倍的原因之一。
4. **对评测的启示**:评测中的多步分解/汇总应放在 **evaluator 端**实现,**不要依赖被测平台的「自动并行/汇总」**(本结论与 task1「分步错误传播未观察到」相互印证,已上卷至 `../../PROJECT_RESULT.md`)。
5. **前提(诚实标注)**:三领域预测工具停摆(测试环境前提,非并行缺陷),两模式都退化为记忆+文献,**科学精度维度本轮无法测量**;本 block 的差异结论落在**编排/交付可靠性**层面。

---

## 1. 背景与目标

### 1.1 为什么做
task5 要量化 NobelClaw「分阶段(并行执行) vs 不分阶段(单步顺序)」两种回答模式的效果差异(见 `goal.md`)。实测中并行模式始终交付不出成稿,遂转入卡点排查,定位阻断并行的根因并给平台方可执行的修复。

### 1.2 GOAL(可被验证)
- 给出两模式在同一批多阶段题上的**可复现差异结论**;
- 将阻断并行的卡点**根因定位到代码行**;
- 产出**分层修复建议**。
- **完成标准** = 上述三项齐备,且差异结论有 12 次运行数据支撑、根因有代码引用。

### 1.3 范围与非目标
- **范围内**:并行 vs 单步的完成率 / 交付 / 编排可靠性差异;阻断并行的后端 + 前端根因。
- **非目标**:① 两模式的**科学答案精度**对比(预测工具停摆,需先恢复 DiffCSP 等,见 §5);② **修改平台代码**(本 block 只给建议,不落实现)。

---

## 2. 方法 / 信息来源

- **实测**:API 驱动(curl + SSE,硬超时),6 题 × 2 模式 = 12 次运行,账号 sher,平台 `http://nobelclaw.aliyun-inc.com`,2026-06-04。运行器 `runner.py`,成稿 `runs/`,逐次记录 `experiment/`。
- **代码审计**:后端 `~/work/nobelclaw` 的 `server/routes/chat.py`、`db.py`、`server/routes/sessions.py`;前端 `frontend/src` 的 `composables/useChat.ts`、`App.vue`、`components/AppSidebar.vue`、`components/TaskExecCard.vue`、`components/MessageBubble.vue`。
- **核验**:每条结论用 SSE 事件流 + 代码 `文件:行号` + 截图三者交叉印证。

> 本节回答"读者要不要信结论";完整过程留痕在 `experiment/`,本文件不重述。

---

## 3. 关键发现

| # | 发现 | 证据 | 置信度 |
|---|---|---|---|
| 1 | 并行 **0/6 跑完**、单步 **6/6 跑完**;并行平均交付 2041c(多为子 agent 片段),单步 4157c 完整成稿;并行工具报错 24 ≈ 单步 13 的 2 倍 | `experiment/0003-single-vs-parallel-12-runs.md` | 高 |
| 2 | 并行卡死根因 = 多子 agent 共享同一 `AsyncSession` 并发提交;抛错点 `update_sub_agent_task_status` 在 `try` 外 → `DONE_SENTINEL` 不入队 → 消费循环 `while done_count<N` 死等 | `experiment/0004-parallel-hang-root-cause.md`;`chat.py:675 / 740 / 751`,`db.py:269` | 高 |
| 3 | 切换丢面板根因 = 三层丢 `subAgents`:前端单例状态被 `loadHistory` 无条件覆盖、`loadHistory` 不重建 `subAgents`、后端 `_message_dict` 不返回 `sub_agent_id` | `experiment/0005 §5`;`useChat.ts:9-13 / 623 / 533-618`,`sessions.py:57-66`,`TaskExecCard.vue:343` | 高 |
| 4 | 并行子 agent 工具名翻倍 `crystal_generatecrystal_generate` 无效调用,仅并行出现 | 见下图;`experiment/0005 §5` | 中 |
| 5 | 测试环境前提:三领域预测工具停摆(DiffCSP 离线 / 无 cif / `predict_admet` 是 mock / 无带隙工具),两模式都退化为记忆+文献 → 科学精度测不出模式差异 | `experiment/0002-platform-tools-availability.md` | 高 |

并行分解选择框 + 子任务执行中的工具报错 + 工具名翻倍(一图覆盖发现 2/4/5):

![并行分解选择框与子任务工具报错](assets/parallel-tools-error.png)

---

## 4. 决策与建议

### 4.1 判定
**🔴 No-Go:评测不得依赖平台原生「分阶段(并行)」模式** —— 它当前完不成(后端卡死)也看不见(前端丢面板)。评测中的多步分解/汇总改在 **evaluator 端**实现。对平台本身:**🟡 Conditional**,按下表修复后可重新评估。

### 4.2 行动建议(带优先级 + 时限)
| # | 动作 | 负责人 | 时限 | 优先级 |
|---|---|---|---|---|
| 1 | 后端:每个子 agent 用独立会话 `async with _get_session_factory()() as sub_db:`(解死锁,治本) | 平台方 | 待排期 | P0 |
| 2 | 后端:`DONE_SENTINEL` 放进 try/finally 兜底(止血) | 平台方 | 待排期 | P0 |
| 3 | 后端:`_message_dict` 补 `"sub_agent_id"`(sessions.py:57) | 平台方 | 待排期 | P1 |
| 4 | 前端:`loadHistory` 按 `sub_agent_id` 分组重建 `subAgents[]`;聊天状态按 `sessionId` 缓存 | 平台方 | 待排期 | P1 |
| 5 | 后端:排查子 agent tool-call 组装路径的工具名拼接(根因 4) | 平台方 | 待排期 | P2 |
| 6 | 评测侧:多步组装放 evaluator 端,不依赖平台并行(本项目可立即落地) | 本团队 | 阶段 1 | P0 |

---

## 5. 风险与未决问题

| 类型 | 描述 | 影响范围 | 缓解 / 待决方 |
|---|---|---|---|
| 已知风险 | 预测工具(DiffCSP 等)长期停摆,则两模式的「科学精度」差异始终测不出 | task5 目标的精度维度 | 需平台方恢复工具;在此之前以交付可靠性差异为准 |
| 已知风险 | 分解触发是随机的(初始发送 8/12 触发) | 复现并行卡点需重试到触发 | 实验时重试;evaluator 端不依赖此触发 |
| 未决问题 | 「刷新页面是否也丢面板」为代码推断(刷新同走 `loadHistory`),尚无专门复现图 | 前端结论的最后一格 | 需一次浏览器复现实测坐实 |
| 未决问题 | 平台方是否 / 何时修复并行后端 + 前端 | 平台并行模式可用性 | 需平台方决策排期 |

---

## 6. 触发重审条件

**本 block 锁定后,任一条件成立即需重审**:

1. 平台方修复并行后端(独立会话)或前端(重建 `subAgents`)→ 需重测完成率与面板留存。
2. DiffCSP / `crystal_predict` / `predict_admet` 等预测工具恢复 → 可补做「科学答案精度」对比,差异结论可能改写。
3. `task5/goal.md` 或上游 PRD 对"两模式差异"的定义变更。

---

## 7. 交付物指引

| 类别 | 路径 | 说明 |
|---|---|---|
| Task 收尾 | `task5/results.md` / `task5/CONCLUSION.md` | 内部数据账本 + 结论 |
| 详细排查 | `task5/experiment/0005-parallel-vs-single-and-block-investigation.md` | 套六节模板的综合排查报告 |
| 实验数据 | `task5/experiment/`(逐次)、`task5/runs/`(12 份成稿) | 原始过程与产物 |
| 截图 | `task5/assets/parallel-tools-error.{png,jpg}` | png 原图 / jpg 142KB 压缩版(便于拖入钉钉) |
| 上游 | `task5/goal.md`、`../../PROJECT_RESULT.md` | 任务目标 / 项目级汇总 |

---

## 附录 A · 变更日志

| 版本 | 日期 | 变更 | 作者 |
|---|---|---|---|
| v0.1 | 2026-06-04 | 初版(Draft),合并原 block/ 下 4 份拆碎文档为单份正规 block | jingwu |

## 附录 B · 术语

- **分阶段 / 不分阶段**:平台对复杂任务弹出的两种执行选择——并行多子 agent(`run_parallel_agents`)/ 单 agent 顺序(`run_agent_with_tasks`)。
- **`synthesis_start`**:并行模式各子 agent 跑完后,由 synthesizer 汇总的起始 SSE 事件;本轮 0/5 出现 = 从未走到汇总。
- **`subAgents` / `sub_agent_id`**:前端并行面板的渲染数据 / 后端区分子 agent 消息的分组键。
- **AsyncSession**:SQLAlchemy 异步数据库会话,禁止跨并发 asyncio 任务同时使用。
