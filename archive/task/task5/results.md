# Task5 结果 — 分阶段(并行) vs 不分阶段(单步) 实测

实验时间:2026-06-04。全程 **API 驱动**(非浏览器),账号 sher。每题两种模式各新建独立 session。

## 方法

- 入口:`POST /api/v1/projects/{pid}/sessions/{sid}/chat`(SSE)。先发 `{message, deep_thinking:true}`。
- **分阶段**:复杂任务回 `event: decomposition` 后,再发 `{message:"", confirmed_tasks:[...]}` → 后端 `run_parallel_agents` 起多个子 agent 并行、最后 synthesizer 汇总。
- **不分阶段**:回发 `{message: 原题+" [请作为单一顺序步骤执行，不要拆分并行子任务]", deep_thinking:false}` → 后端 `run_agent_with_tasks` 单 agent 顺序执行。
- 每条流用 curl `--max-time` 硬超时(单步 200s / 并行 150s)。runner:`runs/`(原始最终报告)、`runner.py`。

## ⚠️ 平台工具现状(影响结论解读)

实测三领域的**预测工具当前都不可用**:`crystal_generate` 报 DiffCSP 服务离线;`crystal_predict` 找不到 cif(demo cif 已失效);`predict_admet` 是 mock 肽类过滤器(小分子只回 "Insufficient data",不给 logS/logD/BBB/hERG);半导体无带隙工具;只有 `literature_search` 正常。→ **两种模式都只能靠 LLM 记忆 + 文献作答**。因此本轮测到的差异主要在**编排/交付可靠性**,而非数值精度。

## 数据(12 次运行)

| 题 | 模式 | 触发分解 | 跑完(done) | 耗时s | 工具 ok/err | 最终报告 |
|---|---|---|---|---|---|---|
| SC-P1 | 单步 | 是 | ✅ | 115 | 3/4 | 2909c |
| SC-P1 | 并行 | 是 | ❌ 卡死 | 160 | 4/**9** | **122c**(无成稿) |
| SC-P2 | 单步 | 是 | ✅ | 132 | 4/2 | 3351c |
| SC-P2 | 并行 | 是 | ❌ 卡死 | 158 | 1/2 | 2454c(子agent片段) |
| SE-P1 | 单步 | 否* | ✅ | 109 | 2/0 | 4214c |
| SE-P1 | 并行 | 是 | ❌ 卡死 | 155 | 3/2 | 3096c(片段) |
| SE-P2 | 单步 | 是 | ✅ | 144 | 4/3 | 4590c |
| SE-P2 | 并行 | 是 | ❌ 卡死 | 156 | 3/2 | 3681c(片段) |
| AD-P1 | 单步 | 否* | ✅ | 158 | 5/2 | 4926c |
| AD-P1 | 并行 | 否* | ❌ | 85 | 7/3 | **0c** |
| AD-P2 | 单步 | 否* | ✅ | 135 | 3/2 | 4957c |
| AD-P2 | 并行 | 是 | ❌ 卡死 | 158 | 3/6 | 2898c(片段) |

\* 触发分解是**随机的**:同一题不同次发送有时拆、有时不拆。初始发送 12 次里 8 次触发;AD-P1 两次都没触发。

## 聚合

- **单步:6/6 跑完**,平均成稿 **4157c**,工具报错合计 **13**。
- **并行:0/6 跑完**(全部在 synthesizer 汇总前卡死;`synthesis_start` 事件 **0/5 次**出现),平均"交付物"仅 **2041c** 且多为子 agent 中途片段(SC-P1 仅 122c、AD-P1 为 0c),工具报错合计 **24**(约单步 2 倍)。
- **并行独有 bug**:工具名翻倍 `crystal_predictcrystal_predict`(无效工具),仅在并行子 agent 出现。

## GT 抽查(仅"单步"有成稿可判;数值来自模型记忆/文献,非工具)

- **SE-P1 单步**:GaN 3.40 直接 / GaAs 1.42 直接 / Si 1.12 间接;带隙差 2.28;蓝光 LED→GaN。**全中 GT** ✓
- **AD-P2 单步**:溶解度 caffeine 最高、口服溶液→caffeine。**中 GT** ✓
- **SC-P2 单步**:Tc 预测 Nb 9.3 / MgB2 37.5 / YBCO 91.2 / Bi-2223 108(近实验值),排序正确、>77K 命中 YBCO+Bi-2223。**中 GT** ✓
- **AD-P1 单步**:**有错**——把 M1(theophylline,GT=BBB−)判成 BBB+、把 M4(risperidone)误标为 lurasidone。说明工具缺位时靠记忆识别 SMILES 会出错。

→ 即"有成稿时"单步在易记事实上多数对 GT,但难题仍会错;**工具停摆主导了数值正确性,模式不是主因**。
