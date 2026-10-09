# task17 — 完成汇报

> ## ⚠️ 更正声明（2026-08-02，goal v3.0.1）
>
> 本报告中"**类型4 跨源发现（好结果在另一个源，只评终态 gt）**"的定义**已被更正**。正确定义承 [ADR-0012](../../governance/adr/0012-两类benchmark设计范式与query专家背书要求.md) §1 第二类：**B 完全不可查**，agent 须从 A 内线索**推理出 A 中不存在的分子**，用 `check_molecule_exists` 验证；判分为 `open_score = 𝟙[hit(G_B)] × 𝟙[grounding_pass]`，**不是"只评终态 gt"**。另：类型 4 已裁定**默认进第一版**，本报告"第二阶段"的标注作废。
>
> 实施一律以 [`goal.md`](../../goal.md) §1.5 + §2b 与 [`query类型分化设计.md`](query类型分化设计.md) §3b 为准。本报告正文保留原貌作历史记录。


## 一、结论先行

1. **按目标形态把 query 分 4 类，药物/材料对称**：类型1 约束满足（找唯一解）、类型2 约束最优（找最好的）、类型3 证据溯源（长文埋线索）、**类型4 跨源发现（好结果在另一个源，只评终态 gt）**。**本 task 落地前两类**，类型3、4 需扩判分引擎列第二阶段。
2. **每类配不同判分 DAG + 不同题面段结构**，不再是原来两类共用一句式：
   - 类型1：**5 个 cp（去 cp5 排序）**，题面 `A 动作 + D 交付`（对标 ScienceAgentBench）；
   - 类型2：**完整 6 cp**，题面 `A + M 评分标尺 + D`（对标 RE-Bench 把标尺写进题面）。
3. **约束外置**：三条性质阈值从题面口语移入独立 `domain_knowledge` 字段（对标 ScienceAgentBench，术语待 T-49 专家核），题面只留 A/D(/M) 段。
4. **4 个对称 case 全部穷举验证可解**——过程中可解性闸抓出并修掉两个真缺陷（见 §三）。

## 二、做了什么

| 子目标 | 完成 |
|---|---|
| 1 设计文档 | ✅ `query类型分化设计.md`（三类定义 + 段结构 + 对称矩阵 + 中英骨架 + 砍两类的理由） |
| 2 template 分化 | ✅ `constraint_satisfaction/`（5 cp）、`constrained_optimization/`（6 cp）各一份 template.json，query_template 改英文段结构、含 D 段字段名 |
| 3 约束外置 | ✅ 4 个 case 均新增 `domain_knowledge` 字段（英文骨架 + 数值占位） |
| 4 对称实例 | ✅ 类型1：`case_cs_cns_drug`(药)+`case_cs_supercon_material`(材)；类型2：`case_co_cns_drug`(药)+`case_co_supercon_material`(材)，ID 全部复用 mock_db 真实分子/晶体 |
| 5 账本 | ✅ TODO T-53、problems PR-28、本 report |

## 三、可解性闸抓出的两个真缺陷（已修）

出题时穷举 mock_db 验证合格集，抓出两个若不修就会成废题的问题：

| case | 初稿缺陷 | 修法 |
|---|---|---|
| `case_cs_cns_drug` | 约束 `caco2>-5.0` 下合格集 **\|G\|=8 而非唯一解**（mock 库扩容后新增 de/df/ea/ec/ee/eg/ei 都满足） | caco2 改窗口 `[-4.38,-4.30]`，穷举确认 **\|G\|=1（仅 aa）** |
| `case_co_cns_drug` | 合格集含 3 个 `caco2=None`（ba/bb/ca），目标量排序会崩 | 加约束 `caco2 != null`，合格集 8 个目标量均非空，最优唯一=ec |

**这印证了 task16 结论**：出题必须带生成时可解性校验（PR-27⑥ / T-50），否则"约束照 mock 字段凑"极易造出无解或多解题。

## 四、四 case 可解性终检（证据）

| case | 类型 | \|G\| | 判定 |
|---|---|---|---|
| case_cs_cns_drug | 约束满足 | 1（aa） | ✓ 唯一解 |
| case_cs_supercon_material | 约束满足 | 1（xb_exp_1, tc=92） | ✓ 唯一解 |
| case_co_cns_drug | 约束最优 | 8，最优 ec(caco2=-4.2) | ✓ 有合格集且目标量非空 |
| case_co_supercon_material | 约束最优 | 14，最优需排除高压陷阱 xd | ✓ 保留 xd(tc=250K/高压) 作核心区分点 |

## 五、遗留

| # | 遗留 | 处置 |
|---|---|---|
| 1 | **类型3 证据溯源未落** | 需扩 `evaluate.py` 前置证据 cp，第二阶段（承 T-33） |
| 1b | **类型4 跨源发现未落**（owner 2026-08-01 补充：高价值发现类，好结果在 B 源，只评终态 gt） | 需 `evaluate.py` 支持"只评终态"精简打分 + 可选 mock_db 多源标签；`predict` 接口返真实值无需合成环境（纠正原误判）。第二阶段 |
| 2 | **`domain_knowledge` 为英文骨架 + 占位** | 最终药化/材料学术语待 T-49 药化专家执笔 |
| 3 | **新 template 未接 evaluate.py 实跑** | 判分内核沿用 fixed/open 未改，但新目录未纳入评测入口的 case 清单；重跑属 T-43 范畴 |
| 4 | **旧 fixed_discovery/open_discovery 目录保留** | 未删除，避免影响 T-43 既有 trace；待类型体系稳定后再决定是否归并 |
