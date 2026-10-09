# task18 — 权威数据库拉取与 mock 构造方案

| 字段 | 值 |
|---|---|
| **状态** | 🚧 进行中 |
| **建立日期** | 2026-08-01 |
| **上游** | `task12/权威数据源与数据缺口指南.md`（数据源清单 + 9 步执行顺序 + 5 条硬规则）｜ 计划：`权威库拉取与mock构造方案` |

## 上游诉求（owner 原话）

> "下一步应该就是先把数据集 mock 出来，也就是我说的 mcp server，所以下一个 task：去看看我们之前调研到的权威的数据库，全量的下载下来，然后仔细阅读，看看怎么构造我们自己的查询 mock 值"

## 对齐 GOAL（ACTION_RULES §1.1）

- **最终目标**（`goal.md` §1）：为材料+药物发现造 long-horizon agent benchmark，评过程而非只评终答。
- **本 task 服务于**：GT-first **第 1 步「选定分子并反查性质」的数据底座**——mock server 返回的必须是权威库的真实值（`goal.md` §4 可信度支柱 1）。当前 mock_db 仅 40 分子/17 晶体、47 空格、15 SMILES 错配，数据底座不足。
- **红线关联**（`goal.md` §5）：判分零联网——权威库只在**造题阶段**用，拉下来冻结为本地快照。

## 边界（已与 owner 确认，2026-08-01）

1. **分类拉取**：开放许可中小库全量（3DSC/UCI SuperCon/AqSolDB/TDC）；SA/超大库只取子集+存查询配方（ChEMBL/ZINC）；不可达库跳过并标注（MP/OQMD）。
2. **原始库不入库**：下载存 `dag_poc/data/sources/<db>/`，已加入 `.gitignore`；仓库只留提炼后的数据字典与方案。
3. **只拉取+阅读+方案**：**本 task 不改 `mock_db.json`**。真正改数据按指南 §4.3 分步另开 task（含暂停中的 task13 修 SMILES）。

## 子目标与验收（三要素）

| # | 子目标 | 打勾通过标准 | 验证方式 | 红线 |
|---|---|---|---|---|
| 1 | 拉取脚本 | `fetch_authoritative_sources.py` 可复现，含退避重试 + 先干跑可达性 | 脚本存在且每库带 MANIFEST | 沿用历史可达结论不实测 |
| 2 | A 档全量落盘 | 3DSC/UCI/AqSolDB/TDC 至少 3 个成功落 sources/ + md5 | MANIFEST 记录行数/md5 | 未落盘就写方案 |
| 3 | B 档子集落盘 | PubChem 按 40 分子反查身份；其余按实体取样；ChEMBL 仅 ID | sources/ 有对应文件 | ChEMBL bulk dump |
| 4 | 数据字典 | 每拉到的库一节：字段名/含义/单位/取值范围/可映射性质 | `数据字典.md` 覆盖所有拉到的库 | 只列库不解字段 |
| 5 | 构造方案 | 一张字段映射表 `源库.列→mock.<段>.<字段>` + provenance 三元组 | `mock构造方案.md` 覆盖 §4.3 可支撑项 | 映射无来源标注 |
| 6 | 账本 | TODO 无重号、problems 关联、report 收口 | 脚本核验 | 重号或漏登 |

## 不做的

- 不改 `mock_db.json`（§4.3 分步另开 task）；不修 SMILES（task13）。
- 不 bulk dump ChEMBL（CC-BY-SA）；不拉 ZINC 全量（L3 留位）。
- 不引 MP/OQMD（不可达）、不引 JARVIS DFT 预测值当真值。

## 交付物

- `src/data_download/fetch_authoritative_sources.py`
- `dag_poc/data/sources/<db>/`（原始 + MANIFEST，gitignore 不入库）
- `task/task18/数据字典.md`、`task/task18/mock构造方案.md`、`task/task18/report.md`
