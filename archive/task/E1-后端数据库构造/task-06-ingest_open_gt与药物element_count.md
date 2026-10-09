# Task E1-06 — 执行 ingest_open_gt + 药物侧 element_count

> ## 📌 跑 LLM 必读：中间结果必须完整落盘（owner 要求，2026-08-07）
>
> **凡本 task 涉及调用 LLM，每次调用都必须留完整 JSON log。** 必留字段见 [`ACTION_RULES.md` §7](../../ACTION_RULES.md)：`request`/`response` 完整原样（含 `usage`、`finish_reason`、完整 `message`）+ `latency_ms`/`attempt`/`seq`/`timestamp`/`code_version`；落盘在自己 task 目录下；temperature 一律调 `agent_harness.temperature_for(model)`。


> ## 🟢 debug 阶段用免费模型（owner 指定 2026-08-08，见 [ADR-0019](../../governance/adr/0019-debug阶段用dogfooding免费模型.md)）
>
> **做逻辑 debug 与联调时一律用 DogFooding 免费模型；只有正式跑实验矩阵时才换回付费模型。**
>
> - 开关：`.env` 里已置 `LLM_PROFILE=dogfooding`，`agent_harness` 会自动整组替换 key/model/endpoint，**你的调用代码不用改**
> - 模型名：**`Peach-07-17-DogFooding`**（必须精确，带 `[1M]` 或小写都报 `CE-001 模型不存在`）
> - 已下架：`Qwen3.7-Max-DogFooding`（`CE-012`），不要再用
> - 老 key 对本模型无权（`PL-001 平台业务限制`），必须用 `DOGFOODING_API_KEY`
> - 这是**推理模型**，真题跑测超 120s，超时已改为配置驱动（`LLM_REQUEST_TIMEOUT`，dogfooding 档默认 600s）
> - 跑 MCP server 与端到端要用 `./.venv/bin/python`（系统 python3.9 装不了 mcp）；可执行入口是 **`mock_server.py`**，不是 `server.py`（后者缺 `src.context`，跑不起来）

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E1-06 / `task/E1-后端数据库构造/` |
| **类型** | Implementation（收尾） |
| **状态** | §1 手术备战完毕、待 E3 后执行（Task #41：监工裁定方案 B，手术脚本一键就绪 + dry-run 全验证留盘 + E3/E5 交接清单落位；**未执行迁移、未碰冻结数据**；执行前置=E3 判分侧完工+监工通知。§2/§3 已由 Task #39 全绿交付，见 task-06-report.md §7） |
| **负责人** | E1（数据线） |
| **预计起止** | 2026-08-08 ~ （约半天） |
| **优先级** | 🟡 中 —— 不阻塞任何人，但那条警告该清掉 |

---

## 一句话目标

> 清掉 `b_partition 与 A 全集相交 1 键` 那条警告，并把之前批准的药物侧 `element_count` 补上。

---

## 1. 执行 `ingest_open_gt`

每次构造 Gateway 都会打这条警告：

```
b_partition 与 A 全集相交 1 键（E1 交接登记占位，ingest_open_gt 未执行；
机器识别放行，非快照错误口径；执行 ingest_open_gt 后应归零）
```

**我按结构串复算过，B 成员与 A 实际相交 0 个** —— 所以这不是隔离破口，是登记占位没销账。但它每次都刷屏，且会让真正的隔离告警被忽略（狼来了效应）。

**要做**：执行 `ingest_open_gt`，让该警告归零。若执行后仍有相交，**那才是真问题，报我**。

---

## 2. 药物侧 `element_count`（之前已批准）

你在 §6 上报过"药物侧同样可确定性计算 `element_count`，请监工另批"，我批了但排在你三项待办之后。现在那三项都清了，可以做。

判别力我实测过：药物侧 74,941 个分子的元素种类数分布 3 种 18.0% / **4 种 42.0%** / 5 种 30.7% / 6 种 7.9%，最大档 42% 未挤在一档，**作为切格维度可用**。

**注意**：值必须在匿名域自洽（元素种类数匿名前后不变），且**只给计数、不给元素名**——这条红线上批你守住了（92,193 条新性质真实元素符号 0 条），本批同样。

---

## 3. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | `ingest_open_gt` 已执行 | 构造 Gateway 不再打该警告；若仍相交则报监工 |
| 2 | 药物侧 `element_count` | 全覆盖；红线扫描真实元素符号 0 条 |
| 3 | CHECKSUMS 刷新 | `--verify` 退出码 0 |
| 4 | report 收口 + 状态行更新 | — |
