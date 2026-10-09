# 测试 0004 — 并行模式卡死的后端根因

## 现象

[0003](0003-single-vs-parallel-12-runs.md) 中并行模式 0/6 跑完:多个子 agent 各自跑完后,后端在「汇总」前挂起,SSE 连接永不关闭,客户端永远等不到最终答案。证据:`synthesis_start` 事件 0/5 出现;SC-P1 并行只见到 agent_0 的事件;连接被一直占住直到客户端硬超时。

## 根因 — 多个子 agent 共享同一个 AsyncSession 并发提交

定位在 `~/work/nobelclaw/nobelclaw/server/routes/chat.py` 的 `run_parallel_agents`(L644+)。链条:

1. **一个请求只有一个 DB 会话。** `get_db()`(db.py L269)是 `async with factory() as session: yield session`,即每个请求注入**唯一一个** `AsyncSession`(SQLite + aiosqlite)。该 `db` 被原样传进 `run_parallel_agents`。

2. **N 个子 agent 并发复用这同一个 `db`。** L743 起为每个子任务 `asyncio.create_task(run_one(...))`,所有 `run_one` 拿的是**同一个 `db`**,且几乎同时启动。

3. **SQLAlchemy AsyncSession 不允许跨并发任务同时使用。** 多个 `run_one` 同时 `await db.commit()` 会抛 `InvalidRequestError: ... concurrent operations are not permitted`(`save_message`/`update_sub_agent_task_status`/`finish_sub_agent_task` 内部都有 commit)。

4. **致命点:抛错发生在保护 DONE_SENTINEL 的 try 之外。** `run_one` 结构:
   ```python
   async def run_one(agent_id, sub_task, record_id):
       await update_sub_agent_task_status(db, record_id, "running")  # L675 try 之外
       queue.put_nowait((agent_id, "agent_start", {...}))
       try:
           async for event in _graph.astream(...): ... await save_message(db, ...)  # try 之内
       except Exception as e:
           sub_status = "failed"; result_content = str(e)
       await finish_sub_agent_task(db, record_id, ...)              # L735 try 之外
       queue.put_nowait((agent_id, "agent_end", {...}))
       queue.put_nowait((agent_id, DONE_SENTINEL, None))            # L740 try 之外
   ```
   N 个 run_one 同时启动 → 几乎同时撞 L675 的 `update_sub_agent_task_status` 并发 commit → 抛错。这个抛错**不在 try 范围内**,直接逃逸出 run_one,L740 的 `DONE_SENTINEL` **永远不会入队**。

5. **消费循环死等。** 主流程:
   ```python
   done_count = 0
   while done_count < len(tasks_to_run):     # L751
       agent_id, event_type, data = await queue.get()
       if event_type is DONE_SENTINEL: done_count += 1
   ```
   少一个(或多个)`DONE_SENTINEL` → `done_count` 永远到不了 N → 永久阻塞在 `await queue.get()` → 走不到 `synthesis_start`/`done` → SSE 不关闭 → 客户端挂死。逃逸的异常被其后 `asyncio.gather(*runners, return_exceptions=True)` 吞掉,但那行根本执行不到,所以日志也安静。

## 证据吻合

`synthesis_start` 0/5(从没走到汇总)、并行报错≈单步 2 倍(并发 commit 撞库)、SC-P1 只见 agent_0 事件(其余子 agent 在 L675 就死)、连接长挂 —— 全部与该根因一致。

## 修复建议(任一即可解死锁)

1. **每个子 agent 用独立会话**:`run_one` 内 `async with _get_session_factory()() as sub_db:`,各写各的,从根上消除并发复用。(首选)
2. **把 DONE_SENTINEL 放进 try/finally**:即便子 agent 全程出错也保证哨兵入队,消费循环能收敛(止血,不治本)。
3. **加 `asyncio.Lock` 串行化 DB 访问**:能消错但牺牲并行度。

> 注:本根因即用户口中「为什么并行会卡死」的答案 —— **不是模型不会做,而是后端并发用同一个数据库会话把自己锁死在了汇总之前**。
