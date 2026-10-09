# task17 — query 类型分化：药物/材料对称 × 按目标形态分三类

| 字段 | 值 |
|---|---|
| **状态** | 🚧 进行中 |
| **建立日期** | 2026-08-01 |
| **上游** | `task16`（query 风格四维实测 + 段结构定案）｜ 计划：`query类型分化设计` |

## 上游诉求（owner 原话）

> "那我认为我们也要配置不同的 template 格式，和这些论文保持一致，使得 template 具备多样性，你认为我们设计什么样格式的 query 类型比较好，分为哪几类"
>
> 补充确认：药物和材料**分开、但对称**；分类主轴按选项 A（目标形态）。

## 对齐 GOAL（ACTION_RULES §1.1）

- **最终目标**（`goal.md` §1）：为材料+药物发现造 long-horizon agent benchmark，评过程而非只评终答。
- **本 task 服务于**：GT-first **第 2、3 步**（构造 query + 生成模板与 DAG）。当前两类模板共用同一句式、且任务类型与领域绑死（fixed 全药物、open 全材料），无多样性；本 task 按目标形态分化 query 类型并在双领域对称铺开。
- **依据**：task16 实测结论——题面主流是 `A 动作 → D 交付` 两段式、约束外置、题量越大句式越该收敛（故应先定少数主模板）。
- **红线关联**（`goal.md` §5）：不改判分"评过程"内核；答卷契约仍遵 ADR-0011（`screened_pool`/`ranked_candidates`）。

## 三种 query 类型（详见 `query类型分化设计.md`）

| 类型 | 本质 | 判分 DAG | 段结构 | 对标 |
|---|---|---|---|---|
| **类型1 约束满足** constraint_satisfaction | 硬约束下找唯一解（\|G\|=1） | 无 cp5 排序 | A + D | ScienceAgentBench |
| **类型2 约束最优** constrained_optimization | 硬约束 + 合格集内目标量最优 | 完整 6 cp | A + M + D | RE-Bench |
| **类型3 证据溯源** evidence_grounded | 约束埋进长上下文，先抽证据再解题 | 前置证据 cp | S + A + D | InfoMosaic + Toolathlon |
| **类型4 open 发现探索** discovery_haystack | **B 完全不可查**，须从 A 内线索**推理出 A 中不存在的分子**，存在性接口验证 | 终态命中 `G_B` **×** grounding | A + D | **ADR-0012** §1 第二类 |

类型 3 需扩判分引擎列**第二阶段**（承 T-33）；**类型 4 已按 goal v3.0.1 裁定为默认进第一版**（定义按 ADR-0012 更正，需 evaluate 支持 `open_score`=终态×grounding + `check_molecule_exists` + A/B 物理分层，归 E3/E1）。本 task 只落类型 1、2。

## 子目标与验收（三要素）

| # | 子目标 | 打勾通过标准 | 验证方式 | 红线 |
|---|---|---|---|---|
| 1 | 设计文档 | 三类定义 + 段结构 + 对称实例矩阵 + query 骨架（中英）齐全 | 文档存在且四部分完整 | 只给分类不给可落地骨架 |
| 2 | template 分化 | 类型1/2 各一份 `template.json`，`query_template` 按 A/D(/M) 段结构改写，非旧统一句式 | JSON 合法 + query_template 含 D 段字段名 | 沿用旧"输出你认为最优的答案" |
| 3 | 约束外置 | case 新增 `domain_knowledge` 字段承载约束，题面不含口语化性质翻译 | case JSON 有该字段 | 把阈值口语塞回题面 |
| 4 | 对称实例 | 类型1/2 各出 1 药物 + 1 材料 = 4 个 case，ID 复用 mock_db 真实分子/晶体 | ID 在 mock_db 中存在 | 编造 mock 里没有的 ID |
| 5 | 账本一致 | TODO 无重号、problems 记议题、report 收口 | 脚本核验编号 + 断链 | 重号或漏登 |

## 不做的（避免漂移）

- 不改 `evaluate.py` 判分逻辑与 checkpoint reward（本 task 只动 template/case 的题面与约束承载）。
- 不落类型 3（需扩证据 cp，第二阶段）。
- 不填 `domain_knowledge` 的最终药化术语（待 T-49 药化专家执笔，本 task 留占位 + 英文骨架）。
- 不动 mock_db 数据、不动 SMILES（那是 task13）。

## 交付物

- `task/task17/query类型分化设计.md`
- `dag_poc/templates/constraint_satisfaction/`、`constrained_optimization/` 两类模板 + 各 2 个 case（4 个）
- `task/task17/report.md`
- 账本：`problems.md`、`TODO.md`
