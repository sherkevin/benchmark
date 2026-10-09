# Task E5-06 — 补齐 open 剩余 11 道（题集最后一个口子）

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
| **Task ID / 路径** | E5-06 / `task/E5-template与case构造/` |
| **类型** | Implementation（题集收口） |
| **状态** | ✅ 已收口（Task #37，2026-08-08）：投 11 过 11 废 0 预检剔除 0，open 30/30 题集满 150；Task #35 十项复检全 PASS + 三维评审 + Task #38 加固；CHECKSUMS 37→48 verify 0；白名单七组原子提交；收口见 `task-06-report.md` |
| **负责人** | E5（template/case 线） |
| **预计起止** | 2026-08-08 ~ （约 1～2 天） |
| **上游来源** | E3-05 §9 接口 B 已交付（降级键 grounding 通道打开） |
| **优先级** | 🔴 最高 —— 题集唯一剩余缺口 |

---

## 一句话目标

> open 现在 19/30，把剩下 11 道补齐，题集就满 150 了。

---

## 0. 堵你的那条前置已解除

上批你只造了 4 道材料 open，因为「E3 接口 B 未完成前不得按降级键造材料题」——31 达标族里 26 个走 UCI 降级键，你只能用 3DSC 的 5 个。

**E3 的 §9 已交付并通过我的复验**：降级键全集复算通道落地，且口径从严——`DEGRADED_KEY_NOT_UNIQUE` 与 `PRIMARY_KEY_NOT_UNIQUE` 都 fail-closed。**UCI 那 26 个族现在可用了。**

---

## 1. 要造的 11 道

| 项 | 数 |
|---|---|
| 现有 open | 19（药物 15 + 材料 4） |
| 目标 | 30（药物 15 + 材料 15） |
| **缺口** | **11 道材料 open** |

沿用你 task-03/04 已验证的五步法与梯度三参数（n=3～5、d=1～2、K=20～30），三个坑照旧避（禁跨源混排、Tc=0 先排、降级键只可排除不可判唯一）。

---

## 2. 三件不要忘的

| # | 项 |
|---|---|
| 1 | 每道落 `grid_key`（E2 的 `check_grid_uniqueness` 会查跨批次撞格）与 `s_feasible_size` |
| 2 | `g_b` 注册进 `b_partition.cases`、`view_ref` 指向真实视图文件（E2 的 O3/O4 会查） |
| 3 | **gt 查重的分母是全语料** —— 这是你上批自己提炼的教训，本批同样适用 |

---

## 3. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | 11 道材料 open | open 达 30/30，**题集满 150** |
| 2 | 全量 lint | 30 道 open 跑 `--strict`，O1～O4 + M13 全过 |
| 3 | 判废记录 | 如实报，沿用"预检剔除单列、不冒充判废"的口径 |
| 4 | report 收口 + 状态行更新 | — |

---

## 4. 风险

| 风险 | 说明 |
|---|---|
| **UCI 族的降级键判别力弱** | E3 已让"降级键唯一命中也拦截"。若某 case 的判定必须依赖唯一性，**换主键族重造**，不要绕过拦截 |
| **11 道可能不够投造** | 26 个 UCI 族按 31.8% 判废率算够用；若实际判废偏高，**如实报**、不要降 K 或降 n |
