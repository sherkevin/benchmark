# τ-bench 是怎么工作的

> 素材：`reference/tau-bench/REPO_README.md`、`case_示例.py`、`few_shot_sample.jsonl`。素材里查不到的项，逐条标"README 没写"。

**一句话：τ-bench 让一个 LLM 扮演顾客、另一个 LLM 当客服 agent，两者围着一个模拟业务数据库多轮对话；对话结束后，程序比对 agent 实际造成的数据库终态与标准答案 actions 造成的终态，一致即通过，不看对话过程。**

速读区，只看这几条也能带走答案：

- 两个域：airline（航空客服）和 retail（零售客服）。每个域有多少道题，README 没写。
- 一条题 = 给用户模拟器的 instruction（人设 + 目标）+ 一组 ground-truth 写操作 actions。
- 判分是确定性的程序比对（数据库终态），不用 LLM 打分。
- 指标叫 Pass^1 到 Pass^4，定义 README 没写；从榜单看 k 越大数字越小（最好成绩 claude-3-5-sonnet-20241022：retail Pass^1 = 0.692，Pass^4 = 0.462；airline Pass^1 = 0.460）。
- README 开头自己警告：本仓的 airline/retail 任务已过时有错，修复版在 τ³-bench。

## 一条题从进到出

拿一条真实 retail 题（逐字摘自 `case_示例.py`）走全程。

**进：一条 Task。** 两部分：

1. `instruction`——给用户模拟器的剧本："You are Yusuf Rossi in 19122. You received your order #W2378156 and wish to exchange the mechanical keyboard for a similar one but with clicky switches...（若没有 clicky + RGB 背光 + 全尺寸的键盘，就退而求其次选无背光）"。注意这段话 agent 看不到，agent 只拿到域的 API 工具和 policy guidelines（业务规则文档，README 只提了名字没给内容）。
2. `actions`——标准答案，一串写操作，例如 `Action(name="find_user_id_by_name_zip", kwargs={"first_name": "Yusuf", "last_name": "Rossi", "zip": "19122"})`，后续还有定位订单、调 `exchange_delivered_order_items(...)` 完成换货。

**站 1：用户模拟器开场。** 一个 LLM（默认 gpt-4o，`--user-strategy llm`）拿着 instruction 扮演 Yusuf，发出第一句话。为什么要有这站：题目的目标藏在用户脑子里，agent 必须靠追问逐步挖出来，这正是它和"一次性给全指令"类 benchmark 的区别。

**站 2：对话循环。** agent（另一个 LLM，策略可选 tool-calling / Act / ReAct）每轮二选一：对用户说话，或调工具读写数据库。从 `few_shot_sample.jsonl` 看真实轨迹长这样：用户问礼品卡余额 → agent 要邮箱做身份验证 → 调工具查用户，工具返回 JSON（含 `"gift_card_8245350": {"balance": 60}`）→ agent 回答"余额 $60"。写操作同理：改支付方式后，工具返回的订单 JSON 里 `payment_history` 多了一条 payment 和一条 refund——数据库真的被改了。循环直到用户模拟器认为目标达成，发 `###STOP###` 收尾。为什么要有这站：agent 的全部得分行为（该查什么、该改什么、改之前有没有跟用户确认）都发生在这里。

**站 3：判分。** 程序把这条题的 ground-truth actions 造成的数据库终态，与 agent 对话后实际的数据库终态比对，一致 = 通过（`case_示例.py` 注释原话）。为什么这样设计：多轮对话路径千差万别，没法比对话文本；但"换没换对货"在数据库里只有一个正确终态，比对终态既确定又便宜。比对的粒度（全库还是只看被改的表）README 没写。

**出：pass/fail，聚合成 Pass^k。** 单题结果进榜单。Pass^k 的确切定义 README 没写。

流程图就是：instruction → 用户模拟器 ⇄ agent ⇄ 工具/数据库 → ###STOP### → 终态比对 → pass/fail。

## 配套设施

- **用户模拟器的质量控制**：除默认 llm 外还有三种策略。react 让模拟器先写 Thought 再回复；verify 用第二次 LLM 调用检查回复是否合格，不合格就重新生成；reflection 在 verify 基础上加一步反思再重写。
- **自动错误定位**：轨迹长、约束复杂，人工找错误位置费时，仓库提供 `auto_error_identification.py`，用 LLM 给失败轨迹做两个标注：谁的锅（user / agent / environment）、什么类型的错（goal_partially_completed / used_wrong_tool / used_wrong_tool_argument / took_unintended_action）。README 明说这一步是 LLM 干的，可能标错。
- **历史轨迹**：跑一遍很贵（具体多少钱 README 没写），仓库在 `./historical_trajectories` 里附了现成轨迹。

## README 没写的（8 项必答缺口清单）

- 题量：airline / retail 各多少条任务，没写。
- Pass^k 的定义、单题耗时、跑一遍的成本，都没写（成本只有定性的 "expensive to run"）。
- 题目怎么造的：instruction、actions、数据库、policy 是人写的还是生成的，没写。
- policy guidelines 的具体内容，没写。
- 防作弊 / 防数据污染机制，没写。
- 终态比对的具体粒度和实现，没写。

## 作者自己承认的边界

- 本仓任务已过时：airline 和 retail 的任务有错误，修复版在 τ³-bench（README 顶部警告原话）。
- 自动错误定位依赖 LLM，"may lead to inaccurate error identifications"。
- 错误定位的责任方枚举里含 user——作者默认用户模拟器本身也会犯错，即评测结果混入了模拟器噪声。
