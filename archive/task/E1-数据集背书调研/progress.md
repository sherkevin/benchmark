# E1 · 步 1 — 数据集背书调研 · 进度看板

> 认领人：E1-数据线（Qoder 工程组）｜认领日期：2026-08-06｜对应 `task.md` / `goal.md` v3.0 §6 步 1

## ✅ 已完成

1. **认领本 task**（2026-08-06）：`task.md` 表头已更新为「🚧 进行中」，其余内容未动。
2. **复跑入库门核验 --strict**（2026-08-06 当场实测）：
   - 命令：`python3 src/benchmark/verify_snapshot_identity.py --strict`
   - 结果：**退出码 0，合计身份问题 0 项**（A 非法 SMILES 无 / B 分子式不符 0 / C canonical 重复 0 / D ADMET 填充率 80.4%）。
   - 环境：RDKit 2025.09.2 已安装，可正常运行。
   - **T-55 判定：已闭环**。证据：`TODO.md` L63 记载「已完成 2026-08-06，身份问题 16→0、退出码 1→0、性质值零改动」；收口报告 `task/task13/report.md`。本次复跑与记录一致。
   - 按任务要求：**仅记录，未修复任何内容**。
3. **背书核验主表与调研报告落盘**（2026-08-06，阶段一 1a+1b）：产出 `数据集背书调研报告.md`——主表 16 行（药物线 9 库＋材料线 6 库口径全覆盖，SuperCon 的 UCI #464 衍生快照单列）× 11 列（URL / 造 benchmark 论文 / venue+DOI / 权威性 / 可达性实测 / License（含 SA 特别标注）/ 成品还是 raw / A/B 分层三条件 / 服务 tool / 背书判定）。**背书判定：达标 8 / 未达标 8**（未达标：UniProt、PDBbind、3DSC、UCI #464、SuperCon(NIMS)、OQMD、MPDS、JARVIS，均缺口径①，如实标注不凑数）。抽查 reference/ 原文 **8 条（≥5）** 全部属实，清单见报告附录 A。
4. **可达性当场重测**（2026-08-06 15:12–15:16 CST，未沿用 task12 历史结论）：16 库 19 个端点逐条 curl，HTTP 码＋时间戳落盘 `reachability_retest_result.md`（脚本 `reachability_retest.sh`）。与 task12 的差异：**OQMD 由「000 不可达」翻转为「间歇 200/000 不稳定可达」**；**ChEMBL 旧式 REST 端点已迁移（404），主站/status 200**；task12 未测的 PDBbind、MPDS 基础端点、JARVIS 三库首测均可达；MP 403、SuperCon NIMS 000 与预期一致。重测环节未下载任何数据。
5. **A/B 分层逐库判定**（2026-08-06）：完全支持 7 库；有条件支持 2 库（PubChem/ZINC 身份库）；ChEMBL 因 SA 只存 ID 明确不可分层；MP/OQMD/MPDS/SuperCon NIMS 当前不支持；PDBbind 暂缓、JARVIS 弱支持。详见报告 §五。
6. **报告终稿审校（阶段一 1d）**（2026-08-06）：通读 `数据集背书调研报告.md` 逐项核验——主表 16 行 15 库口径全覆盖、三口径判定与正文一致、可达性列与 `reachability_retest_result.md` 一致、ChEMBL CC-BY-SA 标注齐全、每库"成品还是 raw＋理由"与 A/B 分层无遗漏、附录抽查 8 条 ≥5；**补写缺失的「数据落盘状态」一节（报告 §六）**：引 11 库 MANIFEST（六要素齐全、complete 全 true）与完整性自检全绿输出（`src/data_download/integrity_check_output.txt`），仅引用未改 `src/**` 与 `dag_poc/**`。版头已标注终稿。
7. **收口 report.md 落盘**（2026-08-06）：`task/E1-数据集背书调研/report.md`——金字塔结构（核心结论 3 句 → 做了什么 5 步带证据路径 → 达标/未达标清单、可达性 3 处差异、3DSC 背书属性界定 → 遗留与交接 → 交付物清单），task.md §8 三项交付物全部闭环。

## 🚧 进行中（本 task 已收口，以下为下一阶段衔接）

1. **交接 E1-后端数据库构造（步 4）**：以达标 8 库冻结快照为真值底座；注意遗留事项（见 `report.md` §四）：ChEMBL 取数前验证新 API 路径、BindingDB 弱背书论文显式声明、get_property 的 binding_affinity/logD 数据源注意事项、OQMD 使用前重测。
2. 本 task 无剩余进行中子项。

## ⛔️ 阻塞（2026-08-06 刷新）

1. **PubChem 缺 MANIFEST / TDC_ADMET 空目录 / UCI_SuperCon zip 不完整——三项均已解除**：11 库 MANIFEST 六要素齐全、complete 全 true（UCI_SuperCon 整拉 zip 8,300,005 B、EOCD/CRC 校验通过），完整性自检全绿。证据：`数据集背书调研报告.md` §六、`src/data_download/integrity_check_output.txt`。
2. **E3 的 `dag_poc/tools_schema.json` 桩——事实更正：该桩已交付**。`ls dag_poc/*.json` 显示文件存在（23,933 字节，version `0.1.0-stub`，owner AI-E3-Lee，冻结 9 个契约工具 + 旧名 alias，明确不以 23 个灵枢工具为基线）。**此项不构成阻塞**，后续以该桩为准；`dag_poc/data/TOOLS_INVENTORY.md` 的 23 tool 清单仅作存量参考。
3. **OQMD 可达性不稳定**（2026-08-06 重测）：3 连测 200/200/000，若后续纳入取数链路，必须先当场重测并准备降级方案，不得依赖本次结论。
4. **ChEMBL 旧 API 端点失效**（2026-08-06 重测）：`/api/data/molecule/<ID>/JSON` 返回 404（主站与 status.json 均 200），旧查询配方在下游使用前需重新验证；叠加 CC-BY-SA 3.0，仍维持「只存 ID＋查询配方」规则。
5. **--strict 实测结果**：退出码 0、0 项身份问题（见「已完成」第 2 条）。T-55 判定为**已闭环**，无因此产生的阻塞。
