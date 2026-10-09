# task11 — 完成汇报

## 一、结论先行

1. **最紧急的不是去找新数据库，而是修已有数据。** 缺口分三层，优先级与直觉相反：**数据错（15/40 SMILES）> 数据缺（填充率 80.4%）> 数据少（40 分子 vs 25 万）**。错数据比没数据危险——判分建立在错真值上，实验结论会全部作废，而 `aa` 恰好是 CNS 主打 case 的 gold answer。
2. **历史数据源结论已部分失效，必须以当场实测为准。** `ckpt_gt_binding.md` 标记 ✅实测的 OQMD **现已不可达**；标记「被墙」的 BindingDB **已恢复**。task10 建议的「材料线改锚 Materials Project」因 403 走不通。
3. **材料线的现实出路是 3DSC**（Scientific Data 2023, DOI 10.1038/s41597-023-02721-y）：它是 SuperCon × Materials Project 的匹配产物，自带 Tc ＋ 真实 CIF 结构，一次补上「Tc 无背书」与「structures 空壳」两个最大缺口，且走 GitHub raw 实测 200 可达。

## 二、做了什么

| 动作 | 方法 | 产出 |
|---|---|---|
| 盘点 MCP 23 个工具的数据需求与实现真伪 | 逐行读 `mock_server.py`，标注行号 | 指南 §一：6 个真查库 / 9 个硬编码假实现 / 8 个纯转发 |
| 精确统计 mock_db 覆盖率 | Python 按**非 null** 统计（非 key 存在） | 指南 §2.1：填充率 80.4%，逐性质缺失 ID |
| 核验分子身份正确性 | RDKit 算分子式 × PubChem 权威分子式交叉比对 | 指南 §2.2：15/40 不符 + 1 组重复 |
| 定位错配根因 | 回查 `证据_admet_gt.md` L91 | 搬运错配，非源数据错 |
| 实测数据源可达性 | 当场 curl 20 个端点取 HTTP 码 | 指南 §三：逐条标注 ✅/❌ |
| 固化入库校验门 | 把核验脚本工程化 | `src/benchmark/verify_snapshot_identity.py` |

## 三、结论是什么

### 3.1 三层缺口

| 层 | 问题 | 规模 | 证据 |
|---|---|---|---|
| 🔴 L1 数据**错** | 15/40 SMILES 分子式不符；`aa` 与 `bh` canonical SMILES 完全相同 | 15/40 | RDKit 核验，见 §3.2 |
| 🟠 L2 数据**缺** | ADMET 240 格空 47；`targets` 仅 1 条；`structures` 空壳无 CIF；`predict_binding` 恒返 −8.5；`generate_molecules` 返回 `C0CC` 非法 SMILES | 见指南 §二 | `mock_server.py` L149-L152、L171-L175 |
| 🟡 L3 数据**少** | 分子 40（vs Zinc-250K 25 万）；晶体 17（vs 3DSC 5.7k / UCI 21k）；文献 14 | — | — |

### 3.2 SMILES 错配的根因与范围

`aa` 用的 `CC(C)NCC(O)COc1cccc2ccccc12` 在 [`证据_admet_gt.md`](../../reference/NobelClaw/证据_admet_gt.md) L91 **正确归属 Propranolol（PubChem CID 4946）**，是搬进 mock_db 时错配给了 Metoprolol。

**关键区分：性质值是可信的，错的只有 SMILES 一列。** 证据文档 L130 载明 Metoprolol 的 Caco-2＝−4.35，与 `aa.caco2=-4.35` 一致。所以修复是重取身份，不必重做性质。

**范围**：证据文档只核验了 14＋6 个药，mock_db 的 40 个中约 20 个无血缘支撑，与「错配多在 db–ej 扩充批次」的分布吻合。

### 3.3 可达性实测（与历史结论对比）

| 数据源 | 历史文档 | 本次实测 | 影响 |
|---|---|---|---|
| Materials Project | 🔴 403 | ❌ **403** | task10 的「改锚 MP」走不通 |
| OQMD | ✅ 实测可用 | ❌ **000 不可达** | 历史推荐的 MP 替代**已失效** |
| BindingDB | 🔴 本环境墙 | ✅ **200** | `predict_binding` 缺口**现在可填** |
| 3DSC | 未记录 | ✅ **200**（9.4 MB） | 材料线新出路 |
| UCI #464 | 未记录 | ✅ 200 | 晶体池 17→21,263 |
| PubChem / ChEMBL / UniProt / PDB / ZINC / CrossRef / TDC | — | ✅ 全部 200 | 药物线通路完整 |

### 3.4 frozen snapshot 五条硬规则（指南 §4.1）

1. 判分零联网；在线库只在造题阶段用。
2. 每条记录带 `{source_db, source_url, source_paper_doi}` 三元组。
3. **身份与性质分离取源**——身份一律取 PubChem，性质取 TDC/AqSolDB/BindingDB。§3.2 的错配正因两者混在一处手工搬运。
4. 入库即校验（已固化为脚本）。
5. ShareAlike 库（ChEMBL CC-BY-SA 3.0、SIDER CC-BY-SA 4.0）**只存 ID + 查询配方**，不 bulk dump，避免快照被 SA 传染。

## 四、证据在哪

| 结论 | 路径 |
|---|---|
| 指南主文档 | [`research/数据缺口与权威数据源指南.md`](../task12/权威数据源与数据缺口指南.md) |
| 入库校验门（可复跑） | [`src/benchmark/verify_snapshot_identity.py`](../../src/benchmark/verify_snapshot_identity.py) — 实测输出 15 条不符、1 组重复、80.4% 填充率，`--strict` 返回 1 |
| 23 工具声明 | `dag_poc/data/TOOLS_INVENTORY.md` |
| 假实现行号 | `dag_poc/mcp_mock_server/mock_server.py` L149-L152（binding 硬编码）、L171-L175（非法 SMILES）、L178-L181（晶体全 0）、L101-L108（恒定 P6/mmm）、L66-L73（判定恒为真） |
| 真值血缘 | `reference/NobelClaw/证据_admet_gt.md`（L91 错配根因、L130 性质值）、`证据_superconductor_gt.md`（3DSC/UCI/JARVIS 四源） |
| 已有配料表（未重复） | `reference/NobelClaw/ckpt_gt_binding.md`（19 源 + license + 可靠性分级） |

## 五、遗留问题

| # | 问题 | 处置建议 |
|---|---|---|
| 1 | **15 条 SMILES 未修** | 本 task 只做诊断未改数据。修复需拉 PubChem 重取 40 条身份，属独立 todo（会改动 gold answer 所在记录，需先确认是否影响 T-43 重跑基线） |
| 2 | Materials Project 403 / OQMD 不可达 | 已给 3DSC 替代。若需 MP 原始数据，需 owner 侧解封或换网络环境 |
| 3 | 约 20 个分子无证据文档血缘 | 需补 provenance 或从快照剔除；与规则 2「三元组」冲突的记录不应进正式题集 |
| 4 | `crystals.bandgap` 全为 0.0 | 超导体带隙为 0 物理合理，但作判据则区分度为零。需 owner 确认是否用作 checkpoint 判据 |
| 5 | `crystal_generate` 返回 tc/bandgap＝0 | 建议改为 `null`/「待评估」而非 0，避免 agent 把 0 当真值。属工程修复 |
| 6 | 本 task 未改 `mock_db.json` 与 `mock_server.py` | 全部为诊断与文档产出；工程修复按指南 §4.2 的 9 步顺序另立 task |
| 7 | 工作区仍有 3 个 trace 文件未提交（承 task10 遗留 #4） | 待 owner 确认去留 |

## 六、过程中的一次自我纠错（记录以免重犯）

初次统计 ADMET 覆盖率时，我用「key 是否存在」判断，得出 **40/40 全满**的结论，并据此认为既有报告「缺 9–13 条」的说法已过期。复核发现 mock_db 中所有性质 **key 均存在、但值大量为 `null`**，按非 null 统计实为 **80.4%**——**既有报告的数字是对的，我的结论是错的**。已在指南 §2.1 写入「口径陷阱」警示，并在校验脚本 `check_coverage()` 中注释固定该口径，避免后续复现同一错误。
