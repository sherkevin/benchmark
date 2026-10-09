# E1 · 步 4 — 后端数据库构造 · 进度看板

> 认领人：E1-数据线（Qoder 工程组）｜认领日期：2026-08-06｜对应 `task.md` / `goal.md` v3.0 §6 步 4

## ✅ 已完成

1. **认领本 task**（2026-08-06）：`task.md` 表头已更新为「🚧 进行中」，其余内容未动。
2. **复跑入库门核验 --strict**（2026-08-06 当场实测）：
   - 命令：`python3 src/benchmark/verify_snapshot_identity.py --strict`
   - 结果：**退出码 0，合计身份问题 0 项**。快照 `dag_poc/data/mock_db.json`：40 分子 / 17 晶体 / 1 靶点 / 14 文献；ADMET 填充率 80.4%。
   - 环境：RDKit 2025.09.2 已安装，可正常运行。
   - **T-55 判定：已闭环**（16→0、退出码 1→0）。证据：`TODO.md` T-55 行（已完成 2026-08-06，监工执行，性质值零改动）、`task/task13/report.md` + `修复明细.json`。本次复跑与记录一致。
   - 按任务要求：**仅记录，未修复任何内容**。`task.md` §6 中「已实测退出码 1、报 16 项问题」的历史记录已被 T-55 修复覆盖，以本次实测（退出码 0）为准。
3. **阶段二 2a：现有 benchmark 后端库五问调研完成**（2026-08-06）：
   - 交付：`task/E1-后端数据库构造/现有后端数据库调研报告.md`（五问对照表覆盖 `reference/` 61 家已落盘 benchmark + 13 家重点深挖 + 9 契约工具决策表 + 实测汇总 + 结论段）。
   - 实测脚本：`task/E1-后端数据库构造/measure_local_samples.py`（可复跑；17 个本地样本的体积/条数/读取耗时全量落盘，均 <1 ms）。
   - 核心结论：61 家中自带后端库的重点候选 11 家（含仓库快照/服务态共 15 行），**无一家是可复用的双领域查询后端**（均缺匿名 ID/A-B 分层/provenance 三件硬前提）；决策汇总＝拿来用 0 / 改造 1（get_property ← TDC ADMET 成品数据）/ 自建 4（literature_search、search_candidates、get_entity、check_molecule_exists）/ 无需后端库 4（compare、submit_answer、edit_molecule、normalize_structure）。
   - 补充核查（当场执行）：tau-bench 官方库体积经 HTTP HEAD 实测 retail 2.31 MB + airline 5.04 MB；PoseBusters Zenodo 8278563 经 API 实测 54.96 MB；TravelPlanner 约 400 万条、TDC 66 集 1591.9 万数据点均标注「基于仓库文档/论文」。
   - 红线自查：未跳过调研（验收项 1 达成）；未混淆「用了哪些数据源」与「自带可查询后端库」；venue/权威性全部沿用 research 底稿已核准结论，未重断言。
4. **阶段二 2b：架构决策与后端库规格契约完成**（2026-08-06，任务板 Task #6）：
   - 交付：`task/E1-后端数据库构造/后端库规格契约.md` v1.0——喂给 E3/E5 的书面交接物：①存储格式唯一结论＝全部子库统一 JSONL 全载（不用 SQLite、不分档），逐条列出体积/读取耗时/多行 CIF/md5 冻结/依赖/A-B 分层六维依据（基于 17 样本全 <1 ms、拟引入资产 <0.5 GB 的实测）；②目录与 A/B 物理分层：`dag_poc/data/backend/<tool>/<tool>_A.jsonl` 与 `<tool>_B.jsonl` 各自独立文件，隔离靠文件系统读取层白名单而非查询过滤；③5 子库（literature_search / search_candidates / get_entity / get_property / check_molecule_exists）逐库字段清单/索引键/记录数目标/数据来源；④provenance 三元组规范（含 3DSC condition 人工标注写法，未标注记「未知」不得默认 ambient）；⑤三道入库校验门（RDKit 身份校验 / provenance 非空 / ChEMBL·SIDER 记录数=0）及与 `verify_snapshot_identity.py`（保持不动）的关系；⑥逐子库读取接口签名（含 A 侧全域枚举接口，仅造题/评分用、不暴露为 agent 工具）；⑦E3 清单修订时的增量修订流程。
   - 硬边界自查：仅 5 个需后端库 tool 出字段规格（compare/submit_answer/edit_molecule/normalize_structure 一句话说明）；契约只定义规格、不含实际数据与构造脚本（属 Task #7）；未写入任何禁写区文件。
   - **写作区报备（向监工）**：`dag_poc/data/backend/` 目录树（§2 定义）属 E1 prompt「后端库构造脚本」产出物的合理延伸——task.md §9 交付物 3 明定「实际的 mock 后端库（≤10 GB）+ 构造脚本 + 入库校验门」归 E1，后端库落盘位置是该交付物的组成部分；本阶段（2b）仅在契约中定义其规格，未实际写入任何数据文件。
5. **阶段二 2c：逐 tool 子库构造与入库校验门完成**（2026-08-06，任务板 Task #7）：
   - 交付：`src/data_build/` 构造脚本 9 个（共用模块 + 校验门 + 五子库构造 + 收官复核）；`dag_poc/data/backend/` 五子库全部落盘并通过全量复核；统计报告 `task/E1-后端数据库构造/build_stats.md`。
   - 规模实测：A 池 80,712 实体（PubChem 40 + ZINC 74,901 + 3DSC 5,771）；get_property 27,618 性质行（药物 6 性质按 TDC/AqSolDB 分源直取 + 材料 5,771×4）；文献冻结快照 321 条真实 DOI（CrossRef 采集，≥300 下限 ✓）；`du -sh` 总量 85 MB ≤ 10 GB 预算。
   - 校验门：三断言逐行过门 + 落盘后重放复核 0 拒收；回归自测通过（40 分子全过 + aa 旧 SMILES 注入必拒）；SA 红线 ChEMBL/SIDER 记录数=0；A/B 隔离断言通过。
   - condition 人工标注：16 条命中（PdH/ThH ambient、SiH4 high_pressure，DOI 逐条 CrossRef 核验）；5,755 条记“未知”（红线不默认 ambient）；LaH10/H3S 标注已预冻结待 gt 实体入池。
   - 意外发现（详见 build_stats.md §6）：①契约 §4.3 示例 LaH10 DOI 经核验为错误 DOI（实指白血病论文），已改用核验后的 `10.1038/s41586-019-1201-8`，**建议契约 v1.1 修订**；②ZINC 落盘文件截断（实存 4.66MB/100,044 行，仅 MANIFEST 声明的一半）；③ZINC 源第 986 行孤立引号脏行会吞 csv 后续 8 万行（已改逐行字面解析）；④落盘 3DSC_MP.csv 的 cif 列是文件路径非 CIF 正文（cif 字段如实置 null 待补）；⑤LaH10/H3S 等高压氢化物不在任何已落盘材料库（3DSC_MP 最高 Tc 132.9 K），open 类陷阱题 gt 需 E5 另行冻结入池。
   - 硬红线自查：未计算任何性质真值（ZINC filter_props 无落盘描述符列→置 null 待补，不计算）；未引 DFT/JARVIS 预测值；一侧一文件；每条记录 provenance 三元组齐全；未写入任何禁写区（verify_snapshot_identity.py 未动，mock_db.json 未动）。
6. **阶段二 2d：性能实测、A/B 隔离测试与 md5 冻结完成**（2026-08-06，任务板 Task #8）：
   - 交付：`src/data_build/backend_reader.py`（契约 §6 参考读取层，白名单守门）、`test_ab_isolation.py`（隔离测试）、`bench_backend.py`（性能基准）、`generate_checksums.py`（冻结清单，支持 --verify）；实测报告 `task/E1-后端数据库构造/性能与隔离实测报告.md`。
   - **隔离测试三断言全部通过**（退出码 0）：① A∩B 键集交集＝0（五子库逐个实测），并在临时沙箱注入重叠 B 键做负向自检——证明 B 非空后断言自动生效；② monkeypatch 句柄追踪实测 agent 读取路径 0 个非法 `_B.jsonl` 句柄（唯一触 B 为契约 §6.5 白名单例外）；③ check_molecule_exists 抽样 A 内 50 键＋库外 3 键全返 exists=false、纯 bool、返回仅三键。
   - **性能实测**（验收项 3）：最大子库全表扫描 0.755 s（search_candidates_A 80,712 条）/ 0.644 s（get_entity_A）/ 0.159 s（get_property_A 27,618 条）/ 0.002 s（literature 321 条）；点查均值 26–253 ns/次（抽样 200 次）；内存峰值 tracemalloc 最大 377 MB、进程级 ru_maxrss 860.5 MB（均远低于契约 §1.3 升档红线）。
   - **体积断言**（验收项 4）：du -sh 总量 85M（du -sk 精确 88,625,152 bytes）≤ 10 GB → 达标（仅占预算 0.8%）。
   - **md5 冻结**：`dag_poc/data/backend/CHECKSUMS.json` 落盘（14 个文件＝8 jsonl＋6 MANIFEST），与根 MANIFEST 的 md5 逐一互证一致；--verify 复跑退出码 0。既有 jsonl 零改动（仅新增 CHECKSUMS.json）。
   - **enumerate_A 落盘**：`src/data_build/_build_state/enumerate_A.json`（search_candidates/get_entity 各 80,712、get_property 9,198 个 id，耗时 ≤7.2 ms），供 E5 open 类穷举质量门。
   - 写作区纪律自查：未写任何禁写区；契约 LaH10 DOI 问题只在报告遗留段登记建议 v1.1 修订，未改契约。
7. **阶段二 2e：每 tool 配置报告与收口完成**（2026-08-06，任务板 Task #9）：
   - 交付：`后端库配置报告/` 共 9 份（以 tools_schema.json 9 契约工具命名，含 alias 说明）：5 份需后端库（literature_search/search_candidates/get_entity/get_property/check_molecule_exists，各含 A/B 侧行数实测、功能、契约 §6 接口签名、逐参数出入参含义、provenance 示例、性能实测引用、已知限制）+ 4 份纯计算/收卷类（compare/submit_answer/edit_molecule/normalize_structure，各一页说明无查询后端及理由）。
   - 契约 v1.1 小修订（仅限已登记勘误）：§4.3 示例 LaH10 DOI 更正为 `10.1038/s41586-019-1201-8`，表头版本行 bump，§7 修订日志追加 v1.1 一行；无任何其他契约改动（构造/实测期已按核验值施工，数据零返工）。
   - 收口：`report.md` 金字塔结构（核心结论三句 / 2a→2e 证据链 / 验收项 1–7 逐条达标对照 / 七项待补清单与断言②重跑交接 / 契约改动说明 + 已通知下游线 E3、E5 及书面载体）；task.md §5 验收项 1–7 全部达标，E1 数据线收口。
   - 行文口径备注（非规格问题，已登记 report.md §3.4）：literature_search 的 provenance source_db=CrossRef 为契约 §4.1 枚举外的合理延伸；edit_molecule 的 ANCHOR_NOT_IN_A 按会话已触达实体集实现，与契约 §3.6 不冲突。
8. **评审修复一：代码缺陷（Task #13，2026-08-06 当场修复并全量回归）**：
   - 六项原清单全部闭环：①【阻断】SA 闸绕过——`fetch_id_only_tier` 重写为端点白名单（status.json/单 ID 查询/molecule search）+ 内存 curl（原始响应不落盘，含异常路径）+ `filter_id_only_payload` 白名单键裁剪 + “SA 派生文件只写 chembl_ids.json”硬断言，文档明记“黑名单过滤不是安全边界”（禁词表降为二级告警层）；②【阻断】JSONL 严格载入——新增 `backend_reader.iter_jsonl`（逐行 json.loads，非法行带行号抛 ValueError，EOF 残留同口径拒收，空行跳过），`_load_all`/`test_ab_isolation`/`bench_backend`/`finalize_backend` 全部读库路径收敛到它（负向自检：中间非法行/EOF 残留均拒收）；③【重要】断言三改白名单判定——SA 源（ChEMBL/SIDER）只许 id_only 通道（data_file 通道恒 0 条），其余来源必须在 `DATA_FILE_ALLOWED_SOURCES`（7 源，依实测分布固化）内否则拒收，selftest 覆盖 SIDER/未知源/id_only 放行负正例；④【建议】UCI 双拉取入口合并——从 FULL 移除仅留 SUBSET 单一入口 `fetch_uci_retry`，新增快速通道（既有产物 complete=true 且 md5 与 MANIFEST 一致→不重下载不翻写，实测验证兼容）；⑤【建议】无参运行打印“默认档位：SUBSET+ID_ONLY+PUBCHEM_MANIFEST；FULL 需显式传参”；⑥【建议】`generate_checksums.py` 新增 handover_artifacts 附录，enumerate_A.json md5 入 CHECKSUMS.json 并参与 --verify（14+1 全绿）。
   - Ryan 追加报告闭环：A2 `verify_sources_integrity` 两条假阴性修复（旧式 MANIFEST 多数据文件记 mismatch 不静默；lines>0 且非 zip 参与行数核验）——重跑 --strict 11/11 绿，ZINC 行数声明与磁盘实存一致无需修正；A3 `finalize_backend` 过度宣称修复——分子重放改用记录自带 formula 作真值（式比对重放 74,941 条），晶体/掩码记录如实降级为仅断言二并在 docstring/输出中明示，终审重跑 0 拒收；B1 占位符 casefold 归一（拦 Todo/Tbd 变体与纯空白 note）；B2 TDC fallback 伪造链接改为缺 url 即拒收快速失败；B3 FULL 档 target=0 且非 zip 记 complete=false+note；B4 无主键记录不参与交集运算直接判违规；B5 PubChem SMILES 取值改候选字段名回落（get_pubchem_smiles）；B6 晶体判据加固——全量强制元素集相等会误杀 2,132/5,771（掺杂语义合法差异），按预案降级为仅对子校验 2 跳过（NM/空）的行强制元素集相等，实测 0 新增失败；B7 backend_reader 白名单路径加 realpath 前缀校验；B8 口径差异登记见阻塞段第 8 条。
   - 回归全绿：ingest_gate --selftest ✓ / test_ab_isolation 退出码 0 ✓ / verify_sources_integrity --strict 11/11 ✓ / CHECKSUMS --verify 14+1 ✓ / finalize 终审 0 拒收 ✓ / bench_backend 8 文件全过严格载入器 ✓；严格载入抽查 189,363 行 0 非法行。
   - **数据零改动证明**：8 个 jsonl + 5 个子库 MANIFEST 修复前后 md5 逐一一致；唯一变化为根 MANIFEST.json 的 generated_at 时间戳（A3 终审重跑的预期产物，其余内容不变，已随 CHECKSUMS 重新冻结）。
9. **E1-02 §2 family 拼接修错 + §7.3 CIF null 声明**（2026-08-06，任务板 Task #16）：
   - **口径定案**：多标签 family 用 `|` 分隔符字符串（如 `Heavy_fermion|Chevrel`），不用数组——理由：get_property 的 family 性质值受契约 §3.4 与 tools_schema `value: number|string` 形状约束（数组破坏出参）；JSONL 解析零特殊处理，E5 分层消费 split("|") 即得标签列表；单标签记录不含 `|` 完全向后兼容。口径常量 `FAMILY_LABEL_SEP`/`SC_FAMILY_VOCAB`（7 单标签词表）与 `parse_family_labels`/`normalize_family_labels` 已固化于 `src/data_build/common.py`；拆解失败直接中断构建，禁止猜测拆分。
   - **拼接源头定位**：3DSC 源数据 `3DSC_MP.csv` 的 `sc_class` 列本身即含 11 条无分隔符拼接值（源数据缺陷，非构建脚本引入）；修复点在构建链路透传层（build_search_candidates 接入 normalize，registry 同步新增 `sc_class_raw` 留痕）。
   - **11 条逐一回溯 3DSC 原始值确认拆分**（非猜测：拆解按词表唯一匹配，且经化学语义核验）：7 条 `Heavy_fermionChevrel` → `Heavy_fermion|Chevrel`（c03031/c04851/c04852/c04853/c04860/c04865/c04870，全为 Mo6X8 稀土 Chevrel 相，化学上自洽）；4 条 `OxideHeavy_fermion` → `Oxide|Heavy_fermion`（c02166/c02168/c05108/c05754，均含氧与 f 电子元素，自洽）。
   - **重跑自查全过**：search_candidates_A 与 get_entity_A 均 80,712/80,712 行不变；伪类别 `Heavy_fermionChevrel`/`OxideHeavy_fermion` 计数归零；新值 7+4 条 id 与修前逐一一致；get_entity_A 重跑后 md5 与修前完全相同（证明 family 外字段零改动）；两份 MANIFEST 已刷新（record_count/bytes/md5/generated_at 与磁盘实存逐一核验一致）。
   - **§7.3 声明已落盘**：get_entity/MANIFEST.json 新增 `null_field_declarations`（crystal_cif 与 crystal_atom_count：当前为 null，原因=3DSC 快照 cif 列为路径引用；补齐需重拉 aimat-lab/3DSC 的 cif 目录），原 known_gaps 保留。
   - **get_property 同样存在拼接问题（按纪律未修，交收口协调）**：get_property_A.jsonl 的 family 行有同样 11 条伪类别值，受影响 id＝c03031/c04851/c04852/c04853/c04860/c04865/c04870（Heavy_fermionChevrel）+ c02166/c02168/c05108/c05754（OxideHeavy_fermion）。注：entity_registry 已修（sc_class 已规范化），condition 那一路重跑 build_get_property 时将自动带出正确值。
   - **纪律自查**：未碰 get_property/**、check_molecule_exists/**、B 侧文件（重跑前实测三处 B 文件均为空，无截断风险）、CHECKSUMS.json（未跑 generate_checksums，待收口统一刷）、condition_annotations.py；无需立 ADR 事项。
10. **E1-02 §1：tc≥100K 的 23 个晶体 condition 分层标注**（2026-08-06，任务板 Task #15）：
    - **清单实测**：从 get_property_A.jsonl 筛出 tc≥100 K 晶体 **23 条**（与监工实测一致），标注前 condition 全部为"未知"。
    - **标注结果：21 条标注 `ambient`，2 条如实保留"未知"**。21 条均为常压铜氧化物（Hg-1212/1223、Tl-2212/2223/1212、Bi-2223、Tl-Sr 系的 Re/Pb/Ce/Nd/Y/Fe 掺杂与氧计量变体），逐条以 formula_sc 精确串挂到各母体族发现文献；2 条 CrCuO2（c03272 Tc=132 K / c03269 Tc=117.5 K）在落盘快照中无独立可核验文献 DOI，按红线留"未知"（意外发现见下）。
    - **DOI 核验纪律延续**：6 个母体族 DOI 全部经 CrossRef API 逐条核验（返回 200 且标题匹配，核验记录固化于 `condition_annotations.py` 的 `VERIFIED_DOIS_BATCH2`）；每条标注附 citation（作者+期刊+年+关键结论+掺杂变体归属说明）写入 provenance note。期间再抓一例错误 DOI：网传 Sunshine et al. Science Bi-2223 条目 `10.1126/science.241.4864.446` 在 CrossRef 返回 Resource not found，改用核验通过的 Maeda JJAP `10.1143/jjap.27.l209`。
    - **机制扩展**：`condition_annotations.py` 新增 `match_formula_exact` 精确串口径（掺杂/氧计量变体无法用元素集合口径匹配母体式，逐条人工核对归属后挂族级文献）；`lookup_condition` 优先精确串、其次化学式等价、最后前缀，上一轮 16 条标注回归无冲突（回归实测 PdH/ThH/SiH4/H3S/LaH10 命中不变）。
    - **重跑与自查**：仅重跑 `build_get_property.py`（唯一涉及本子库的最小路径）；总行数不变 **27,618**；condition 行已标注 16+21=**37**、未知 **5,734**；23 条高温段逐条复核：21 ambient+DOI / 2 未知；MANIFEST.json 已刷新（md5=`e1bbd9af2ce4609bd22b650756a35dcb`、bytes=9,700,415、record_count=27,618、generated_at=2026-08-06T12:45:35Z，与磁盘实存逐一核验一致）；`ingest_gate.py --selftest` 全绿；其余子库 md5 实测未变（search_candidates/get_entity/literature_search 逐一旁证）。
    - **附带成果**：本次重跑自动带出 get_property 的 11 条 family 拼接错误修复（第 9 条登记的遗留项）——伪类别计数归零，11 条 `|` 分隔多标签值的 id 与第 9 条清单逐一吻合。
    - **第二批决定**：tc∈[60,100) 的 224 条本轮**不做**——23 条已耗时于逐族文献回溯与 CrossRef 核验，224 条横跨 MgB2/铁基/重费米子/A15 等更多材料族且部分族存在加压测量记录，逐条核验成本与出错风险不可接受，如实报告剩余缺口不硬凑（待监工裁定是否另批立项）。
    - **纪律自查**：只写 condition_annotations.py、get_property/**（含其 MANIFEST）、progress.md 本段；未碰 search_candidates/get_entity/check_molecule_exists/B 侧任何文件、未跑 generate_checksums；无需立 ADR 事项。
11. **E1-02 §3：B 侧入库全链路演练完成**（2026-08-06，任务板 Task #17）：
    - **交付**：演练 fixture 生成器 `src/data_build/make_rehearsal_fixture.py` + fixture（`src/data_build/fixtures/rehearsal_open_gt.json`，per-case 结构、REHEARSAL- 前缀、显式演练标注）；正式入库管线 `src/data_build/ingest_open_gt.py`（三道闸 + MANIFEST 刷新 + --clean 清场通道，E5 真 gt 交付时直接可用）；`入库操作手册.md`（面向 E5：格式/过闸/入库/清场/教训）；可复跑证据落盘 `_build_state/rehearsal_logs/`。
    - **全链路实跑跑通**：fixture → ingest_gate 三断言 → 字段白名单 → 身份键空间隔离闸 → 写入 check_molecule_exists_B.jsonl（2 条）→ generate_checksums 刷新 → test_ab_isolation 重跑全绿（退出码 0）→ 语义反向验证（B 成员 exists=true、A 池分子 exists=false）。
    - **“恒真→实际检查”证明**（BEFORE/AFTER 实跑对照留痕）：新增断言①b（跨键空间：B 双键 ∩ A 池身份键，ADR-0015 §2.4）——B 空时输出“模式=B 空集（平凡成立）”，fixture 入库后输出“B 侧键 InChIKey 2/SMILES 2…模式=实际比对”；同时实测确认：check_molecule_exists 无 A 侧文件，旧口径断言①对它永远恒真，跨键空间断言是 B 非空后唯一能失败的检查。
    - **负例拦截行为记录**：与 A 重叠的 gt（A 池分子 aa，实测 InChIKey/SMILES 双命中）——ingest_gate 三断言全部放行（身份/provenance/来源都合法），被入库管线的身份键空间隔离闸拦下（拒收清单落盘 `rejections_check_molecule_exists.jsonl`）——三断言不覆盖 A 重叠，隔离闸不可省。
    - **演练暴露并修复的流程缝隙**（均如实记录入手册）：①`open_case_gt` 不在来源白名单，首跑被断言三整库冻结——已在 `common.py` DATA_FILE_ALLOWED_SOURCES 新增（selftest 回归全绿）；②契约 §3.5 三字段白名单与 ADR-0015 case_ref 要求的口径冲突——case_ref 承载于 provenance.note（不增字段不违约），建议契约 v1.2 澄清（见阻塞段 10，报监工）；③fixture 候选实测：aspirin/metformin/ibuprofen/acetaminophen/sucrose/citric_acid 全部在 ZINC A 池内——gt 选分子不得凭直觉假设“不在 A 池”，必须实测比对（手册 §1 已写死）。
    - **清场证明**：`--clean REHEARSAL-` 清出 2 条，B 文件/MANIFEST/CHECKSUMS 三件 md5 与演练前基线逐一相同（`d41d8…`/`c1ee9…`/`34b60…`，备份于 `_build_state/rehearsal_backup/PRE_STATE.md5`）；隔离测试重跑回基线全绿。红线自查：fixture 全程可识别为演练数据且已清出，最终 B 侧为空等 E5 真 gt；未碰 A 侧 jsonl、未碰并行路正在改的 get_property/search_candidates 目录、未自行立 ADR。
    - **旁证登记**：清场后 `generate_checksums --verify` 报 get_property_A/search_candidates_A 等 5 处 md5 与冻结清单不一致——确认为 Task #15/#16 并行路预期变更（阻塞段 9 已登记待收口统一刷新），非本演练引入；本路的 CHECKSUMS 恢复仅针对演练前态，不越位刷其他路的变更。
12. **裁定 7.1 修复：盐类 logS 方案 A**（2026-08-06，任务板 Task #20）：
    - **背景**：研究员盐类 logS 抽查（`盐类logS抽查记录.md`）认定系统性偏差：2,311 条 logS 中 74 条把盐/混合物溶解度赋给游离碱/单组分实体；监工裁定方案 A。
    - **逻辑改动**（只动 logS 路径）：`build_get_property.py` AqSolDB 段改为双桶择优——同 canonical 存在单组分记录时只在单组分桶内按 Occurrences/SD 择优；仅无任何单组分记录时才用多组分记录，命中行 provenance note 标 `salt_form=true`（源记录为盐/混合物形式，值存在形式差异风险）；原有可追溯要素（匹配方式/冲突裁决/所选记录 ID）保留；bbb/herg/caco2/logD/half_life 与材料线零改动。
    - **前后对照（修前 2,311 → 修后 2,311）**：换源 **15 条**（多组分→单组分，恰为抽查点名"本有同分子单组分记录被 Occurrences 弃用"的 15 条，无一例多组分内换源）；新加 salt_form 标记 **59 条**；因失去候选移除 **0 条**（新增亦 0）。
    - **典型实体抽查**：z002962 特比萘芬 A-5442（盐，−1.720）→A-2743（游离碱，−4.465，Δ2.75 修复）；z002936 肾上腺素 B-24（0.000）→B-25（−3.008，Δ3.01 修复）；dc 二甲双胍无单组分候选，保留 A-5261（0.258）但已标 salt_form=true；z065571 十八烷 Δ4.10、z006891 苯胺 Δ1.58 同步修复；抽查 §四 14 条复测锚点预期全部命中（含 #13 季铵盐保持不变）。
    - **回归自查全绿**：总行数 27,618 不变；condition 37 标注/5,734 未知回归不变（含 CrCuO2 两条未知）；family 11 条 `|` 口径不变；`ingest_gate --selftest` 全绿；其余四子库（search_candidates/get_entity/literature_search/check_molecule_exists）md5 逐一未变。
    - **MANIFEST 刷新**：get_property_A.jsonl 新 md5 `14ad99f043092d9f817d2ac1f9f15c14` / 9,814,540 bytes / 27,618 行 / generated_at 2026-08-06T12:56:21Z，新增 `logS_salt_form_fix` 声明；未跑 generate_checksums（CHECKSUMS 收口统一刷）；`盐类logS抽查记录.md` 尾部已追加"处置"节登记修复统计与遗留项。
    - **纪律自查**：只写 build_get_property.py、get_property/**（含 MANIFEST）、抽查记录处置节（仅追加）、progress.md 本段；未碰其他子库与 B 侧（重跑前后 B 文件均 0 bytes，无截断风险）。

13. **E1-02 收口：统一冻结、全量回归与收口报告完成**（2026-08-06，任务板 Task #18）：
    - **CHECKSUMS 统一刷新**：先跑 `finalize_backend.py`（重放终审 0 拒收、根 MANIFEST 重生成 189,363 条/87,789,922 bytes）再跑 `generate_checksums.py`，`--verify` 复跑全绿（主清单 14 + 附录 enumerate_A.json 1，退出码 0），合计 87,798,099 bytes。新冻结关键值：get_property_A=`14ad99f043092d9f817d2ac1f9f15c14`（9,814,540 B/27,618 行，含标注+盐类修复）、search_candidates_A=`4ff6f7aab6decc2af89449c3eb73270e`（39,977,343 B，含 family 修复）、根 MANIFEST=`86bb944a236647e7d6f05085b5c34d19`、附录 enumerate_A=`55ba749cf2a3dad680926a15ff387754`；四 B 文件均 `d41d8…`（空）。
    - **MANIFEST 交叉核验**：根 MANIFEST 与五子库 MANIFEST 的 md5/record_count/bytes 逐字段一致，且与磁盘实存（md5 复算/行数实测）一致；backend 下无游离文件。
    - **全量回归五项全绿**（均退出码 0）：finalize 终审 0 拒收 / test_ab_isolation 三断言+①b 全过（B 空态）/ ingest_gate --selftest 全过 / bench_backend 8 文件严格载入 du=86M / CHECKSUMS --verify 14+1。
    - **§4 六条交付标准逐条实跑自检达标**：21/23 标注（2 条 CrCuO2 未知）；family 11 条伪类别两库归零、`|` 多标签恰 11；演练 BEFORE/AFTER 日志（`_build_state/rehearsal_logs/`）证明"恒真→实际检查"；入库手册已交付；四 B 文件空态与演练前基线相同；收口报告 `task-02-report.md` 落盘。
    - **剩余 condition 缺口 5,734 条按 tc 分层实测**：≥100K 2（CrCuO2 待裁量）/ 60–100K 224 / 30–60K 352 / 10–30K 805 / <10K 4,351。
    - **裁定 7.1 盐类修复最终统计（以 leader 21:00 勘误为准）**：logS 总量 **2,311 不变**，换源 15 / 新打标 salt_form=true 59（实测一致）/ 移除 0 / 新增 0；新增遗留 z000009 水实体退化匹配待另立小项（已登记盐类抽查记录处置节与收口报告 §5）。
    - **两项报监工裁量事项已在收口报告 §3/§5 登记**：①CrCuO2 两条疑似 3DSC 上游脏数据是否排除出 E5 造题池；②契约 §3.5 三字段白名单 vs ADR-0015 case_ref 口径冲突（建议契约 v1.2 澄清）。均未自行立 ADR。
    - **纪律自查**：本收口只写 task/E1-*/（报告、progress、task-02 表头）与 backend 的 CHECKSUMS.json/根 MANIFEST（经既有脚本），未改任何子库数据与子库 MANIFEST。
14. **三维评审 E1 域内项修复（8 项一次修完）**（2026-08-06，任务板 Task #21）：
    - **代码修复 5 项**：①闸 0 键序敏感——`ingest_open_gt.py` 字段白名单改集合比对（`set(rec.keys()) != set(ALLOWED_FIELDS)`），消除对 JSON 键书写顺序的隐式要求；同步修 `build_check_molecule_exists.py` 同构守门；②dry-run 恒真返回值——改为“至少 1 条过闸→0，全部被拒→1”（实测：纯负例 gt 干跑退出码 1）；③B 侧去重键——SMILES 经同一 canon() 归一后再作去重键，与隔离闸同口径；④open_case_gt 通道限定——`assert_sa_redline` 新增 `ab_side` 参数（默认 "A" 从严），open_case_gt 仅限 `*_B.jsonl`，A 侧出现一律拒收；`ingest_open_gt`/`build_check_molecule_exists` 显式传 "B"，`finalize_backend` 按文件名判定，其余 A 侧构造脚本默认即受保护；selftest 新增对应负正例；设计理由注释留痕；⑤`condition_annotations.py` 注释精确化（CrCuO2 c03272 Tc=132 K 及其 Fe 掺杂变体 Cr0.95Fe0.05CuO2 c03269 Tc=117.5 K）。
    - **意外发现并顺手修复（域内）**：演练清场后目录 MANIFEST md5 未回基线——根因是 `clean()` 空态 note 措辞与 Task #7 冻结基线漂移 + 时间戳翻新；已修：note 对齐基线措辞 + `file_meta`/`write_manifest` 支持 generated_at 锚定（清回空态继承原值）；实测幂等清场 MANIFEST 字节级不变（c1ee9…）。入库改写过的 MANIFEST 与 CHECKSUMS 的时间戳翻新仍需备份范式恢复（手册 §7.3 已写死）。
    - **文档修复 3 项**：⑥task-02-report.md 模板符合性：§1 六条加 `[x]` 勾选、§2 置信度统一标“高”（逐项列证据）、DOI 粒度说明两处补入（§1 第 1 条与 §2.2）；⑦入库操作手册新增 §7 口径说明：`sc_class eq` 精确相等不命中 `|` 多标签（消费方须按成员包含判定，ADR-0015 读取器算子限制预登记）+ open_case_gt 仅限 B 侧 + 清场恢复范式；同步更新手册闸 0 “顺序固定”过时描述；⑧本条 + 阻塞段第 11/12 条两项影响面漂移报监工。
    - **回归全绿（均实跑）**：`ingest_gate --selftest` 退出码 0（含新增 open_case_gt 通道负例）；改序版 fixture（provenance 键置最前）干跑接受 2/拒收 1 退出码 0（旧代码此场景会全拒——键序修复生效）；演练入库（2 条）→隔离测试（断言①b 模式=实际比对）→清场→三件恢复：B=`d41d8…`、目录 MANIFEST=`c1ee9…`、CHECKSUMS=`aad37…`（收口冻结值）；`generate_checksums --verify` 退出码 0（14+1 全一致）；`test_ab_isolation` 基线态退出码 0。
    - **数据零改动证明**：本轮纯代码/文档修复；五个 A 侧数据文件与根 MANIFEST md5 逐一未变（get_property_A=`14ad99f0…`、search_candidates_A=`4ff6f7aa…`、get_entity_A=`8aaa523b…`、literature_search_A=`6e542c97…`、根 MANIFEST=`86bb944a…`），--verify 全绿。
    - **纪律自查**：只写 src/data_build/ 五个脚本（ingest_open_gt/ingest_gate/finalize_backend/build_check_molecule_exists/common/condition_annotations）、task/E1-*/ 三份文档、_build_state/ 演练产物（改序版 fixture/备份目录）；未碰 query_spec.json 与 E5 文档（两项影响面漂移只登记报监工，见阻塞段 11/12）；未自行立 ADR。

15. **E1-03 §2.1 匿名映射表建成（红线 5 修复第一步）**（2026-08-06，任务板 Task #22）：
    - **交付物**：`src/data_build/anonymization.py`（judge 侧唯一访问模块，含 guard_agent_path 代码级位置断言、anonymize_smiles/anonymize_formula）+ `build_anonymization_mapping.py`（生成脚本）+ `check_mapping_nonmonotonic.py`（R4 独立断言）；映射表落盘 `dag_poc/data/anonymization_mapping.json`（15,775,493 bytes，md5=`a77b6ef8ce3be434eba36e08a5bb1401`，连续两次重跑逐字节一致）。
    - **映射表结构**：载体写死 RDKit dummy atom `[N*]`（禁止元素→真实元素映射）；87 种真实元素（分子 formula ∪ 晶体 formula_sc）+ 保留标签 `*`（44 条 ZINC 源 SMILES 自带裸通配原子）共 88 label；seed=sha256(KEY_MATERIAL) 派生（KEY_MATERIAL 为脚本常量不落盘）；judge 段与映射同文件（单一判据源）：分子 74,941 id→真实 InChIKey/SMILES/formula/name，晶体 5,771 id→formula_sc/material_id_2/spacegroup_2；分界靠文件位置（backend/ 之外，backend_reader 白名单结构上不可达）+ 载入时代码断言复核，不靠“记得别返回”。
    - **验证全绿**：R4 断言退出码 0（周期序 ρ=+0.032 / 字母序 ρ=+0.003，均 <0.5，非严格单调，相邻 label 差正 44/负 42）；ADR-0017 事实二复跑五条全 ✓；dummy 上限实测：N≤65535 稳定、N=100000 被 16 位截断为 34464（本表 88 label 无风险）；全量 74,941 条匿名 SMILES 自 canonical 通过（非自 canonical=0 为阻断条件）。
    - **意外发现并修复**：①就地置换保留源 chiral tag，匿名域中 14,298 条手性中心失效但首次输出仍带 `@`/`@@`→ 改为迭代重解析 canonical 化至不动点；②其中 18 条配位化合物/方向双键结构单次不收敛，迭代 2 轮全部收敛；③手性信息在匿名域丢失属匿名化正确语义（元素身份消失后手性判定基础不再成立），§2.2 重建 A 侧时匿名域只保骨架图，如实报告。
    - **实跑留痕**：`task-03-映射表实跑留痕.md`（R4 输出/事实二/dummy 上限/自 canonical 修复链）。
    - **纪律自查**：只写 src/data_build/ 三新脚本、dag_poc/data/anonymization_mapping.json、task/E1-*/ 留痕与 progress；未重建 A 侧 jsonl（§2.2）、未碰 case_id/gateway/query_lint/共享账本；未自行立 ADR。

16. **E1-03 §2.2 A 侧库去真实身份重建完成（红线 5 修复第二步）**（2026-08-06，任务板 Task #23）：
    - **交付物与新冻结 md5（实跑）**：四库 A 侧重建完成——search_candidates_A=`2c071ee64b5cfc72d992646ad5b48075`（38,113,942 B/80,712 行）、get_entity_A=`02ba808c3d935e4029c580d98668e8cd`（27,052,303 B/80,712 行）、get_property_A=`c5d95bb360ae5feae139ed1a9f757b2a`（1,960,637 B/27,618 行）、literature_search_A=`993989f1e2aa6aaff581e5cd30940eff`（122,657 B/309 行）；根 MANIFEST 重生成=`9f9913eff1b02fd97c78bbb94689122d`（189,351 条/67,249,539 B）；`_build_state/enumerate_A.json` md5 不变（`55ba749c…`，匿名化不动 id 空间）。
    - **字段处置（agent/judge 分界显式落位）**：inchikey 从 A 侧删除（judge 段保留）；formula/formula_sc → 匿名域分子式（dummy `[N*]` 计数）；smiles → 匿名域自 canonical SMILES（74,941 条）；masked_structure 仅匿名域（原子计数/匿名骨架）；cif 保持 null，"将来返回须先匿名化（ADR-0017 §2）"契约注记写入 get_entity MANIFEST；name/真名删除；provenance 三元组全部转出 A 侧 → **judge 侧账本** `_build_state/provenance_ledger_<tool>.jsonl`（key+provenance 逐行，四账本行数与数据文件逐一对齐 80712/80712/27618/309）——不进映射表 judge 段，理由：judge 段是 id→身份单一判据源，provenance 是逐记录属性，另落避免膨胀与双源漂移；ADR-0017 §3 论文标题/DOI 保留给评审的语义由账本承载。
    - **entity id 语义不变**：aa..ej / z000001… / c00001… 全保留（id 本就是匿名占位符，匿名化只换身份字段值，历史 trace 兼容）。
    - **构建链路显式化**：四个 build 脚本在 SA 红线/校验门之后、写出之前插入匿名化层（两侧分界在代码里）；finalize_backend.py A 侧断言反转（provenance dict/inchikey 存在即违规）+ 匿名域口径（自 canonical+纯 dummy+label 计数子集交叉，H label 豁免）+ SA 从账本重放；重跑四库终审 0 拒收。
    - **自检断言脚本**：新增 `check_agent_visible_anonymity.py` 实跑 exit=0（四道断言：元素符号公式语境/InChIKey 正则/judge 段真名黑名单/注册表公式黑名单 token 级，四库全部 0 命中）。
    - **文献库身份闸**：题录（DOI/标题/年份/被引/关键词）为工具设计功能保留 agent 可见，但过三道身份闸——拒收 12 条真实身份题录（Tl2Ba2CaCu2O8/Bi2Sr2CaCu2O8/YBa2Cu3O7 系/Bupropion 真名等）→ 321→309（≥300 契约下限守住）；TOPICS 关键词消毒（lah10→superhydride 等）。
    - **回归全绿（均实跑）**：`ingest_gate --selftest` 四库退出码 0；`test_ab_isolation` 退出码 0（**断言①b/③ 修复**：匿名化后从 A 侧文件取身份键会退化为平凡成立，A 池真实键改从 judge 侧 `_build_state/entity_registry.jsonl` 载入，恢复 InChIKey 74,283/SMILES 74,941）；enumerate_A 80,712/80,712/9,198 与冻结一致；git 对比新旧 get_property 27,618 行 (entity_id,prop,value) 逐字节一致、search_candidates 非身份字段零差异（condition/family/logS 标注口径不丢失，值域不变仅身份字段匿名）；构建可复现（重跑 md5 一致）。
    - **CHECKSUMS 过期清单（暂不刷，收口统一刷）**：本轮过期项——①四个 A 侧 jsonl md5（旧值：get_property_A=`14ad99f0…`/search_candidates_A=`4ff6f7aa…`/get_entity_A=`8aaa523b…`/literature_search_A=`6e542c97…`）；②四个 A 侧目录 MANIFEST.json；③根 MANIFEST（旧=`86bb944a…`，新=`9f9913ef…`）；④CHECKSUMS.json 本体（旧=`aad374d…`，现 --verify 必红）；附录 enumerate_A=`55ba749c…` 未过期。B 侧文件与 check_molecule_exists 全部未动。
    - **意外发现**：①ZINC 池有 2 条 H2 分子实体（z001399/z058891），匿名 SMILES 显式氢记法 `[69*H2]` 与黑名单式 'H2' 字面重合（419 处误报）→ 黑名单/元素符号扫描前先剔除 dummy 括号记法；②元素符号扫描基因/蛋白缩写误报（IKr/CoV/CYP2B6/VP40）→ 口径收紧为五条件（token 整体可分解/单元素计数≥2/必含重元素 Z≥21/大小写敏感/必同时含数字与含小写尾双字母元素），代价 FeAs/YBaCuO 无数字式不判（由公式黑名单补拦）；③ZINC 源存在裸 `*` 位点分子式（'C7H6*BO3'）→ FORMULA_TOKEN 增 `*` 分支映射到保留标签；④ZINC 金属离子式（`[85*+3]` 单原子 vs `[85*]3`）为源数据 SMILES/公式固有语义差（非匿名化引入）→ label 交叉改子集口径并留降级声明。
    - **纪律自查**：只写 src/data_build/（四 build 脚本/anonymization.py/finalize_backend.py/test_ab_isolation.py/common.py/新增 check 脚本）与 backend/ 四 A 库及 MANIFEST、_build_state 账本；未量化/加噪性质值（监工裁量中）、未碰材料侧身份键（§2.3）/case_id（E5）/gateway（E3）、未刷 CHECKSUMS；未自行立 ADR。

17. **E1-03 §2.3 材料侧匿名域身份键建成**（2026-08-06，任务板 Task #24）：
    - **空间群可得性实测（完全可得，走主方案）**：3DSC 源 CSV `spacegroup_2` 列 5,771/5,771 非空（H-M 符号带空格记法）；映射表 judge.crystals 同字段同覆盖；get_entity_A 晶体 masked_structure 内 SPACEGROUP 段亦齐——E1-02 报过的 cif 列路径引用问题不波及空间群；去重 135 个符号取值。
    - **键方案**：`material_identity_key = 匿名域 reduced_formula + "_SG" + 空间群号`。**pymatgen 可用性声明：环境可用（import 成功），仅用于空间群符号→号转换**；reduced_formula 不走 pymatgen（实测三障碍：无法解析 dummy 记法/小数计量不整数化而 3,482/5,771 条含小数计量/排序依赖真实元素电负性），改用纯 Python Fraction 精确归约（分数向量归一到最小整数比、标签升序规范序）。空间群转换 134/135 符号 pymatgen 直解，唯一例外 'C m c a'（24 条，SG 64 旧记法，标准符号 Cmce）走显式别名表，归属经三源核验（UCL 空间群图集/Wikipedia/Grenier 2012）且 24 条均为正交晶系自洽。
    - **覆盖率与冲突率（全量实测）**：键覆盖 **5,771/5,771（100%），无缺口无替代键**；唯一键 5,740；冲突 **31 组 62 条（全为恰 2 条/组）**——逐组核验全部是 3DSC 上游同材料（formula_sc+material_id_2 全同，涉及 4 个 mp-id）重复录入，非“不同晶体同键”；处置口径：grounding `==` 判等正确，实体级唯一性由 entity id 承载，同键重复实体的造题去重责任在 E5（交接文档 §3 写明）。
    - **交付形态**：键列落盘 judge 侧 `src/data_build/_build_state/material_identity_keys.jsonl`（5,771 行，md5=`55922089536b0388c8e343fbc2cd2575`，重跑两次逐字节一致；位置理由：_build_state 为既有 judge 侧构建态目录、backend_reader 白名单结构不可达）+ 纯函数接口 `src/data_build/material_identity.py`（无 IO 无映射表依赖，E3/E5 可直接 import，入参即 A 侧可见字段）。**不在 get_entity_A 增列**：键可由 A 侧既有字段确定性复算（已全量实证），不动四库不新增冻结过期项；agent 可见面零增量（空间群符号本就在 masked_structure）。
    - **内置自检全量实跑通过**（S1–S5）：id 集三方一致；A 侧可见字段重算键与 judge 侧键 5,771 逐一相同；A 侧 formula 反演元素计数与 judge formula_sc 逐一一致；整数计量子集 2,288/2,289 与 pymatgen reduced_composition 语义等价（唯一例外 c05146 O2 分子晶体＝pymatgen 分子单质内置特殊处理，已定名白名单不得静默扩大）；空间群号值域 1..230。另：1 条零计量掺杂记录（c04294 Pb0）按 pymatgen 同口径剔除零 token。
    - **反查性一句话评估**：匿名域组合不含真实元素身份、无映射表不可反查；但真实域同口径组合在公开库足以定位具体材料，映射表与键列必须保持 judge 侧不进 agent 可见路径（交接文档 §5）。
    - **交付文档**：`task/E1-后端数据库构造/材料侧身份键交接说明.md`（E3 接口说明与冲突口径写明）；探测留痕 `probe_material_keys.py` 同目录。
    - **意外发现**：①3DSC 上游 62 条重复录入（31 对同 mp-id）；②pymatgen 符号表不含 SG 64 旧记法 Cmca/Bmab；③pymatgen 对 O2 分子单质 reduced 不除 gcd；④3,482 条小数计量使“先 pymatgen 再映射”路径必然失真。
    - **纪律自查**：只写 src/data_build/ 两脚本+键列文件（_build_state/）、task/E1-*/ 两文档、progress.md 本段；未改分子侧四库字段、未改 case_id/gateway/query_lint/共享账本；未刷 CHECKSUMS（键列是否纳入冻结清单由收口裁定）；未自行立 ADR；材料版 edit_op 不在本任务范围（ADR-0017 §4 另定）。

18. **E1-03 收口：自检、冻结、交接与 report 完成**（2026-08-07，任务板 Task #25）：
    - **停止准则四条全部实跑通过**：①四 A 侧 jsonl 全文 grep InChIKey 正则 0 命中 + 黑名单扫描（公式 37,565/名字 40）0 命中 + check_agent_visible_anonymity.py 复跑退出码 0；②R4 非单调断言复跑退出码 0（ρ 周期序 +0.0323/字母序 +0.0029）；③材料键覆盖复算 5,771/5,771 零缺口；④test_ab_isolation 四断言 + verify_sources_integrity 11/11 均退出码 0。
    - **CHECKSUMS 统一刷新（第 16 条过期清单全部解除）**：裁定 material_identity_keys.jsonl 纳入冻结附录（judge 侧交接物，与 enumerate_A 同例）；finalize_backend 终审 0 拒收重生成根 MANIFEST（189,351 条/67,249,539 B，md5=`9f276ae1f9e1f091b8e76053945b7027`，与第 16 条 9f9913… 的差异仅为 generated_at 时间戳）；CHECKSUMS 主清单 14（合计 67,259,817 B）+ 附录 2（enumerate_A=`55ba749c…` 未变 / material_identity_keys=`55922089…` 新增），--verify 全绿退出码 0；四 A 侧 jsonl md5 与第 16 条逐位一致；五子库 MANIFEST→根 MANIFEST 交叉核验 8/8 条目 md5/bytes/record_count 全一致。
    - **意外发现并修复（前置路遗漏）**：`ingest_open_gt.py` 的隔离闸 `load_a_identity_keyspace()` 仍从 A 侧 jsonl 读 inchikey/smiles——匿名重建后该处 inchikey 已删、smiles 已匿名，闸实为空转（任何 gt 都能过）。已改读 judge 侧 `_build_state/entity_registry.jsonl`（与 test_ab_isolation 断言①b 同口径），实跑验证 InChIKey 74,283/SMILES 74,941。
    - **交接说明落盘**：`task-03-交接说明.md`（§4 第 4 项交付）——给 E3：映射表接入口径（dummy 载体/路径/载入/guard_agent_path）+ CalcMolFormula 口径建议（推荐透传库内匿名域分子式，不得返真实式/不得拿匿名 SMILES 现算丢氢）+ 材料键双接口；给 E5：匿名域字段样例、entity id 语义不变声明（enumerate_A md5 未变）、材料键列取用、31 组 62 条重复晶体造题去重提醒、open gt 键形态（g_b 三字段白名单交真实键，隔离闸以 registry 为准）；给 E2：R4 转实判口径（check_mapping_nonmonotonic.py 三断言）。
    - **性质值指纹裁定写入 report**：风险实证成立（logS 单值唯一命中 72.7%、双值组合 ≥91.5%）；监工裁定本轮原样保留、风险如实登记、量化/加噪另待裁定（若做须立 ADR）。
    - **PR-31 E1 侧结论（报监工）**：ADR-0017 备案“§2、§3 落地并过断言后方可 CLOSED”——§2（A 侧硬约束，E1 责）已落地并断言通过；§3（case_id 去真名）归 E5 未落地，按备案字面口径 CLOSED 条件未齐备；另意外发现 problems.md PR-31（出题质量，OPEN）与 ADR-0017 所引 PR-31（红线匿名问题）编号漂移，均报监工裁定，E1 未改 problems.md。
    - **交付**：`task-03-report.md`（收口报告）+ `task-03-交接说明.md`；task-03 表头已为 ✅（监工验收后已更新，本轮确认无需改动）。
    - **纪律自查**：只写 task/E1-*/（两份新文档+progress）、src/data_build/ 两脚本（generate_checksums 附录增列/ingest_open_gt 闸修复）、backend/ 仅 CHECKSUMS.json 与根 MANIFEST（经既有脚本）；子库数据零改动；未改 case_id/gateway/query_lint/共享账本/problems.md；未自行立 ADR。

19. **E1-04 材料侧身份键核验对齐 + 两处收尾完成**（2026-08-07，任务板 Task #27）：
    - **§1 四条核验全对齐（零实质差距，主任务在 E1-03 §2.3 已实质交付）**：①位置对等——分子侧 registry/材料侧键列均 judge 侧按 id 索引，E3 同一套接口可取；②匿名域计算——重跑 S2 全量（A 侧可见字段重算 == 键列，5,771/5,771）+ 独立抽样 5 条（含零计量 c04294/O2 白名单 c05146/冲突组 c01137）不 import 映射表复算一致；③覆盖率复算 5,771/5,771（重跑键列 md5 逐字节不变 `55922089…`，幂等）；④交接补齐——`材料侧身份键交接说明.md` 新增 §7 边界情况处理（空间群缺失预案：修复源头 CIF 精解→降级替代键 reduced_formula+tc分档+元素种类数、不静默降级；五类已踩坑：小数计量/零计量/SG64 别名/O2 白名单/排序不依赖真实元素）并声明唯一权威文档，与 task-03-交接说明.md 互链。
    - **裁定二落地：契约 v1.2**——`后端库规格契约.md` §3.5 补注明 `case_ref` 承载于 `provenance.note`（格式 `case_ref=<case_id>；…`，评分侧元数据不进返回值不触单 bool 红线，不增字段免动冻结结构）；表头版本行 + §7 修订日志同步。
    - **z000009/裸离子退化清理（三要素）**：规则＝多组分 AqSolDB 记录匹配到 heavy atom≤1 实体（水/质子/裸单原子离子）时该行 logS 不入库（入码 `is_degenerate_logS_entity`，仅 salt_form 路径生效，单组分零影响）；实剔 **12 条**（z000009 水/z062253 质子/S2-/F-/Li+/Na+/NH4+/OH-/Cl-/Zn2+/K+/I-，含抽查点名全部样本，回归锚 #13 季铵保留）；**logS 2,311→2,299**，salt_form 口径不受影响（59→47 恰为剔除对应行）；多原子离子（柠檬酸三阴离子等 11 条）保留标记不剔除（最小破坏，扩大规则待裁定）。
    - **冻结刷新（动了数据，全链路重刷）**：get_property_A 新 md5=`27c0a56e70e63a8bd1932b54a3e6c6cf`（1,959,728 B/27,606 行，旧 c5d95bb3…）；finalize 终审 0 拒收重生成根 MANIFEST=`b493cc788cf57ae6d1ac4ba73287cfc0`（189,339 条/67,248,630 B）；enumerate_A 刷新=`6ecb47404adb9e72968d0d5d1205354d`（get_property 实体数 9,198→9,186）；CHECKSUMS 重刷=`0e3c1315d83e095d2cebfc815e6013c9`，--verify 退出码 0（主清单 14 + 附录 2 全一致）；其余四库与材料键列逐一未动。
    - **回归全绿（均实跑）**：test_ab_isolation 退出码 0；verify_sources_integrity --strict 退出码 0（complete=false 0 个）；ingest_gate --selftest 全过；check_agent_visible_anonymity 四库 0 命中（清理不引入身份信息）；condition 37/5,734、材料线 5,771×4、其余药物性质行数（bbb 757/herg 104/caco2 235/logD 604/half_life 523）逐一不变。
    - **交付**：`task-04-report.md`（六项自检表逐条达标）；task-04 表头状态行置 ✅；无需新立 ADR（契约 v1.2 属口径澄清、清理属数据质量修复）。
    - **报监工**：logS 容量实体 −12 均为退化实体，裁定三容量格数 155/153/92 若以 logS 实体数为基数需 E2 复核一次（预期格数不变，不越位改）。
    - **纪律自查**：只写 src/data_build/build_get_property.py、task/E1-*/ 四文档、重跑产生的 get_property 数据与各级 MANIFEST/CHECKSUMS/enumerate_A；未改 query_spec.json/case_id/gateway.py/共享账本；未量化加噪性质值。

20. **E1-04 三维评审域内修复完成（6 项全落地，数据零改动）**（2026-08-07，任务板 Task #28）：
    - **重要 1：task-04-report.md 按模板六节重排**——自创七节结构改为 `task/_templates/task_template/report.md` 六节（模板遵循方式对齐 task-02-report）：§1 对照 task-04 §5 六条交付标准逐条 [x] 勾验附实跑证据；§2 关键产出（并入原 §2 契约 v1.2 与 §3 清理三要素，新增 E5 知会/破窗归因/修复清单）；§3 判定 ✅ + 下一步行动表（含 E2 复核容量基数 −12、监工裁定扩口径）；§4 无受阻；§5 风险与未决问题表（含影响/待决方列）；§6 最小背景 + 交付物路径表。
    - **重要 2：冻结链瞬时破窗排查与静默窗约定**——排查结论：01:15–01:18 窗口三件 mtime 重写（CHECKSUMS 01:15:18/get_property_A 01:17:55/目录 MANIFEST 01:18:10）内容与收口冻结值逐字节相同；CHECKSUMS/目录 MANIFEST 的 generated_at 锚定未翻新（仍为 16:58:49Z/16:57:42Z）证明非脚本真实重跑而是**字节级恢复**；get_property 四件同秒 mtime 与构建脚本顺序写出指纹不符（符合批量恢复）；**E3-03 排除**（隔离演练日志实测落盘 01:21:50–53，写入面仅 mcp_mock_server/）；E1 自身该窗口无运行记录。归因：评审期非 E1 并行线误写后备份恢复（只登记归因、不越界处置）。`入库操作手册.md` 新增 §8 冻结静默窗约定（CHECKSUMS 收口后至验收完成前任何脚本不得写 backend/ 与附录产物，演练写入须在独立沙箱目录）。合入前 verify 三连全绿（每连"冻结校验通过 ✓：主清单 14 + 附录 2"，exit=0）。
    - **重要 3：交接文档旧值漂移修复**——①`性能与隔离实测报告.md` 三处 9,198 加注"（E1-04 清理后为 9,186，见 task-04-report §2.3）"（历史实测值不改写）；②E5 知会写入 task-04-report §2.4 与 task-03-交接说明 §二.5：enumerate_A(get_property) 基线 9,198→9,186、新冻结 md5=`6ecb4740…`、穷举质量门以 CHECKSUMS 附录为准；③E5 gt 池准入规则写入 task-03-交接说明.md §二.5：候选 gt 实体 heavy atom≤1（水/质子/单重原子离子）不得入 open 题 gt 池，口径复用 `is_degenerate_logS_entity`。
    - **建议 4/5/6 顺手修**——④证据指针 progress.md 第 17 条→第 19 条（即本条上方）；⑤材料侧交接说明 §7 总结行"四类坑"→"五类坑"；⑥build_get_property.py docstring"裸单原子离子（…[NH4+]…）"→"水（O）/质子（[H+]）/单重原子离子（[Na+]/[Cl-]/[OH-]/[NH4+] 等）"（纯文档，py_compile + 10 组样例行为对照零变化）。
    - **登记不修**：3 条无机小多原子阴离子（氰根/亚硝酸根/硫氰酸根）若监工日后裁定扩清理口径为首批候选——已登记进 task-04-report §3.2 下一步行动表。
    - **回归（均实跑）**：generate_checksums --verify 三连全绿；check_agent_visible_anonymity 退出码 0；**数据零改动证明**：修复前后对全部 13 个 backend 冻结文件 + 附录 2 件逐一复算 md5 逐位一致（get_property_A=`27c0a56e…`/CHECKSUMS=`0e3c1315…`/enumerate_A=`6ecb4740…`/material_identity_keys=`55922089…`），本轮纯文档/docstring 变更未动任何数据文件。
    - **纪律自查**：只写 task/E1-*/ 六份文档与 src/data_build/build_get_property.py 仅 docstring；未动数据文件、未越界处置 E3/评审方写入行为（只归因登记）；未自行立 ADR。

21. **E1-05 收口再冻结完成（Task #33，七项交付全达标，E1-05 全部关闭）**（2026-08-07，任务板 Task #33）：
    - **再冻结**：`generate_checksums.py` `EXCLUDED_DIRS` 移除 `per_case_views` 排除项，重跑刷新后 CHECKSUMS=**`73d6e845f43f8856bb628747d2270370`**（主清单 14→**15**，per_case_views/e105_example_001.json 入主清单 md5=`8b1876ea…`，合计 78,480,236 B），`--verify` exit=0；其余 14 件 md5 与 Task #31 冻结值逐一零漂移。新值已记入 task-05-report.md 与入库手册 §8 再冻结登记（含静默窗恢复约定：此后 E5 新增视图一律走入库流程并刷 CHECKSUMS）。
    - **临时产物清理**：`_build_state/tmp_test_view/e105_test_b_side_gt.json`（Task #34 修复轮 B 侧测试件）落盘副本已删除——实证 `assert_per_case_view.py::build_b_side_test_view` 运行期自行构造（删后复跑 exit=0 即重建），不依赖落盘副本；`_build_state/` 不在 backend/ 冻结扫描面内。
    - **契约升版 v1.3**：§6.7 漂移知会正式并入正文——§2.1 目录树新增 per_case_views/、§2.3 隔离机制新增第 4 条视图机制（指向 per-case视图交接说明.md）、§3.2/§3.4 材料容量 5,771→17,252 与性质行口径回写、降级键口径指向材料侧身份键交接说明 §7.1/§7.1.1；v1.2 内容（case_ref 等）保留；§7 修订日志追加 v1.3 行。
    - **七项交付标准逐条勾验全达标**（task-05-report.md §1，关键项本轮亲复跑）：视图机制五项硬要求（A1–A8 断言 exit=0）；扩数据 11,481 条/31 达标族/26 体系支撑 15 道（stat_material_families 亲复跑锚不变）；匿名红线四库 0 命中；两项监工裁定落地确认（黑名单归一、降级键 grounding 归 E3 接口 B 扩展，E3 完成前 E5 不得按降级键造材料题）。
    - **收尾自检五连全绿**（均本轮亲跑 exit=0）：check_agent_visible_anonymity / assert_per_case_view / generate_checksums --verify（主清单 15 + 附录 2）/ test_ab_isolation（四断言，重生成 enumerate_A 后 verify 仍绿）/ stat_material_families（31/26 锚）；输出留痕见 task-05-report.md §6.1。
    - **状态行与文档同步**：task-05 表头 🟡→✅（监工三轮提醒后本轮落实）；per-case视图交接说明.md §6 两项收口待办标注办结；入库手册 §8 再冻结登记。
    - **下一步知会**：E5（视图格式可写/穷举接口可调用/材料题硬前置条件——E3 接口 B 扩展完成前不得按降级键造材料题）、E3（接口 B 扩展裁定与测试修正归属，见交接说明 §7.1.2）、E2（容量数字涉 query 定数时复核：材料 17,252/get_property_A 73,530 行）；详见 task-05-report.md §3.2 行动表。
    - **纪律自查**：只写 src/data_build/generate_checksums.py（一行+注释）、CHECKSUMS.json（脚本重刷）、task/E1-*/ 六份文档；删除一件临时产物；未动 gateway.py/query_spec.json/共享账本/既有 14 件冻结文件/anonymization_mapping.json；未造 case、未自行立 ADR。
22. **E1-05 §6 追加项补做完成（Task #35：注册 atom_count/element_count 为可查性质，监工批准方向 A）**（2026-08-07，任务板 Task #35）：
    - **漏做归因**：§6 于收口前追加进任务文件，收口轮（Task #33）任务文件快照未含 §6，前轮漏做；本轮补齐，口径严格按 §6 原文两件不自行扩展。
    - **补行**：get_property_A.jsonl 73,530 → **165,723** 行（+92,193，存量行零漂移）：药物侧 atom_count **74,941** 行（值取实体记录原子总数，监工示例 aa=44 抽查吻合）、材料侧 element_count **17,252** 行（匿名组成元素种类数确定性计算）；行格式/键序与存量一致。入库脚本 `src/data_build/build_property_counts.py` 幂等，重跑两次逐字节一致（md5=`197ab7920334d8b548a81cf5d07ad4af`）。
    - **性质清单增键**：drug_admet_v1.json 增 atom_count、materials_supercon_v1.json 增 element_count（number/tol=0/`{value} >= {threshold}`，口径写明匿名前后不变、只给计数）；两文件不在 backend/ 下不入 CHECKSUMS，md5 已记入 task-05-report.md 备查。
    - **再冻结**：CHECKSUMS 刷新并 `--verify` exit=0，新 CHECKSUMS md5=**`56de5a045b06551433acaa5becc6fa7b`**（主 15 + 附录 2，85,044,974 B）；附录 enumerate_A get_property 枚举 20,667→92,193（每实体均有性质行后的自然连锁）；其余主清单文件与 material_identity_keys 零漂移。入库手册 §8 已登记解冻事由；契约升 v1.3.1（§3.4 性质键枚举/记录数回写 + §6.7 行 5 + §7 修订日志）；judge 侧 provenance 账本同步补 92,193 条（finalize 行数对齐断言）。
    - **自检六连全绿（均实跑 exit=0）**：build_property_counts 双跑字节一致 / check_agent_visible_anonymity（四道断言零命中）/ generate_checksums --verify / test_ab_isolation（四断言）/ assert_per_case_view（A1–A8，A3 新性质行纳入后仍全绿）/ finalize_backend（重放 0 拒收、账本对齐）。
    - **后续归属**：gateway 白名单同步增两键归 E3（§6 明说）；疑义上报：药物侧 element_count 可确定性计算但本轮未自行扩，如需请监工另批；30 道 fixed case 未重生成（方向 B 仍有效，本项用于后续放量批次）。详见 task-05-report.md §「§6 追加项补做批次」。

23. **E1-05 归 E3 三项待办已汇总至交接面（Task #36，纯文档收口）**（2026-08-07，任务板 Task #36）：
    - **汇总落点**：`材料侧身份键交接说明.md` 新增 §10「归 E3 待办汇总」作为 E3 接手第一入口（该文档是材料身份键唯一权威文档、E3 grounding 条件③必读）；`per-case视图交接说明.md` 新增 §7 指路条目，两处互指、内容以 §10 为准。
    - **三项待办**：①接口 B 扩展：gateway `_material_key_universe()` 纳入降级键（formula+tc 复算路径，监工已裁定降级键纳入 grounding `==` 口径，依据本文 §7.1.2）——硬前置，完成前 E5 不得按降级键造材料题；②`test_material_identity_grounding.py` 库规模魔法数（5771/5740/31）改从数据源动态取（扩数据至 17,252 的正常连锁，监工验收意见点名归 E3，依据 task-05-report.md 监工验收节）；③gateway `get_property` 性质白名单同步 atom_count/element_count 两新键（task-05 §6 方向 A 补做产物，性质行与清单 E1 侧均已就绪，依据 task-05-report.md §6 补做批次节）；每项含事项/依据节号/前置后置关系/E1 侧已就绪条件四要素。
    - **核对落盘**：task-05-report.md「§6 追加项补做批次」节存在且含 CHECKSUMS 新值 `56de5a045b06551433acaa5becc6fa7b`（✓，无需补）；progress.md 第 22 条存在（✓，无需补）。
    - **纪律自查**：纯文档任务，只写 task/E1-*/ 三份文档（材料侧身份键交接说明.md §10、per-case视图交接说明.md §7、progress.md 本条）；未碰任何代码、数据、CHECKSUMS、共享账本；未自行立 ADR。

24. **assert_per_case_view.py 字段硬编码参数化完成（Task #37，task-05 §7：23 份视图全绿，E5 direction 分流复核接管）**（2026-08-08，任务板 Task #37）：
    - **问题与基线**：E5 写入 22 份新视图后 per_case_views/ 共 23 份（1 材料示例 + 4 材料 + 15 药物 + 3 药物备用），脚本两处硬编码材料侧字段致药物线 18 份视图断言失败——BEFORE 实跑复现 36 项失败 exit=1（A6 阈值取 gt.tc：药物 gt 无 tc 性质行；A4.positive 扫查键取 sc_class：药物分子 filter_props 无该键）。门禁未失效，是脚本不通用。
    - **E5 direction 分流复核接管**：A6 按 view.target.direction 分流（higher→ge / lower→le）的改动来源 E5 Task #17（越区修改，监工于 commit d2384a8 追认）；逐字复核结论：maximize 路径与原版判据逐字等价、minimize 以 gt 值本身为阈值宽于造题侧窗口 gt∓δ（断言更严非放宽）、不削弱任何既有检查——追认接管，留痕写入脚本内注释与 per-case视图交接说明.md §5。
    - **两处动态化**：①A6 阈值键由 view.target.prop 动态取（材料 tc、药物 logS），gt 值取性质行同键，宽松对照阈值同步动态化（可见集内该 prop 最小值−1，替换硬编码 tc>-1）；②A4 扫查键按可见集 filter_props 实有键值型分派（字符串键 eq 穷尽扫查、数值键值域范围扫查），**断言侧实测关键事实：全库 74,941 分子 filter_props 仅 40 条非空**，药物线族序列无任何 filter_props 约束可触达——故新增 B1 分流：正例改沿 view.target.prop 性质轴以族序列值域约束穷举（非恒真：BEFORE 实证全库同值域有白名单外命中）。
    - **防回归自检**：新增 SUPPORTED_TARGET_PROPS={tc,logS} 支持集与 A6.target_prop_valid 断言——视图 target 缺目标性质键或键名越集报清晰错误并停掉依赖该键的性质类断言（非 KeyError 裸抛），三型错误输出逐一实测；格式要求（target 必含目标性质键）已补入 per-case视图交接说明.md §2 供后续视图作者。
    - **全量实跑（BEFORE/AFTER 对照）**：BEFORE 23 视图 36 项失败 exit=1 → AFTER 24 视图（23 生产 + 1 B 侧 gt 形态测试视图，B 侧测试视图机制无回归）× 445 项断言全过 exit=0；抽样：case_dh_drug_01 `enumerate(logS≤-6.14)`=0（direction=lower）、case_dh_material_01 `enumerate(tc≥11.23)`=0、e105_example_001 `enumerate(tc≥38.2)`=0。
    - **收尾自检三连（均实跑 exit=0）**：assert_per_case_view（23+1 全绿）/ check_agent_visible_anonymity（四道断言）/ generate_checksums --verify（主清单 37 + 附录 2）。**冻结清单缺口核查：无缺口**——E5 已在入库批次刷新 CHECKSUMS（generated_at=2026-08-07T14:14:37Z），23 份视图全部在列且 md5 逐一一致；未自行扩清单。
    - **文档更新清单**：per-case视图交接说明.md（§2 target 格式要求 + §5 动态取键口径/接管留痕/实跑证据更新）、task-05-report.md（新增「§7 字段硬编码参数化批次」节）、progress.md 本条。
    - **知会事项**：药物视图 gt.entity_ref 实测 ∈ A 全库实体集（A 侧演示形态，与示例视图同口径，A1 双口径断言照常生效），归 E5 造题口径、未动未改；另实测发现 case_dh_drug_01 target.note 写“logS<=-5.89”而 gt 实值 logS=-6.14（断言以性质表 gt 实值为准，note 为描述性文本），未动视图，如需统一口径归 E5 裁定。
    - **纪律自查**：只改 src/data_build/assert_per_case_view.py（含运行期重写 _build_state/ 下断言日志与测试件，均在冻结面外）与 task/E1-*/ 三份文档；未动 per_case_views/ 里 E5 任何视图、未动四个 *_A.jsonl 与 CHECKSUMS；backend/ 零写动作，无需入库手册 §8 登记；未自行立 ADR。

25. **药物侧 element_count 注册完成（Task #38，监工裁定落地，leader 裁定口径 a 全量入库）**（2026-08-08，任务板 Task #38）：
    - **背景与裁定**：task-05 §6 验收节监工裁定「批准加，但不急」（药物侧 74,941 分子元素种类数四档分布有判别力可作切格维度；材料侧不加 atom_count——非整计量无定义）。首轮实跑发现实测比监工四档多 1,045 条（落在 1/2/7/8 种），按任务约定「不一致即停报、不许自行解释」恢复冻结面上报；leader 裁定取口径 a：全量 74,941 行如实入库、值域 1~8、档外不剔除另册登记——关键证据：监工四档占比合计 18.0+42.0+30.7+7.9=98.6% 不足 100%，缺口 1.4% 恰等于 1,045/74,941=1.39%，系主档表非穷尽分布。
    - **补行**：get_property_A.jsonl 165,723 → **240,664** 行（+74,941 药物侧 element_count，存量零漂移；md5=`afa3e79eca25242edb8bc6c1744303a8`/17,266,248 B）；入库脚本 build_property_counts.py（Task #38 扩展）幂等双跑逐字节一致；judge 侧账本同步补 74,941 条血缘→共 240,664 条（md5=`0a4dbcba…`，finalize 对齐断言过）。
    - **分布销账（按 leader 口径）**：3~6 四档与监工实测逐数一致（3种=13,498/4种=31,458/5种=23,019/6种=5,921，合计 73,896 占 98.6%）+ 档外 1,045 条（1种=61/2种=416/7种=539/8种=29，占 1.4%）另册登记；抽查 3 条手工核对全吻合（aa→4、z000009→2、z000078→3）。
    - **性质清单增键**：drug_admet_v1.json 增 element_count（与材料侧同款写法，注明匿名域 label 种类数、匿名前后不变、值域 1~8；新 md5=`e0c53484…`）；properties/*.json 不入 CHECKSUMS，md5 记入 task-05-report.md §8 备查。
    - **再冻结**：CHECKSUMS 刷新并 `--verify` exit=0，新 CHECKSUMS md5=**`06232bd217a592dbe51621f5eee1c386`**（主 37 + 附录 2，90,565,407 B）；与动手前基线（`219d413d…`）逐文件比对仅 3 件变更（get_property_A + 目录/根 MANIFEST），其余 34 件与附录 2 件零漂移（enumerate_A=`5635cc18…` 不变——每实体本已有性质行，distinct id 仍 92,193）；入库手册 §8 已先登记解冻事由（日期 2026-08-08）；契约升 v1.3.2（§3.4 键枚举/记录数 + §6.7 行 6 + §7 修订日志）。
    - **自检六连全绿（均实跑 exit=0）**：build_property_counts 双跑字节一致 / finalize_backend（复核拒收 0、账本对齐）/ check_agent_visible_anonymity（四道断言零命中）/ assert_per_case_view（23+1 视图，Robin 参数化后只复跑不改）/ test_ab_isolation（四断言）/ generate_checksums --verify（主 37 + 附录 2）。
    - **文档收尾**：task-05-report.md 新增「§8 药物侧 element_count 注册批次（裁定落地）」节（含停报-裁定经过与分布销账表）；入库手册 §8 解冻登记；progress.md 本条；契约 v1.3.2。
    - **后续归属**：gateway 白名单同步现共三键（atom_count、element_count×双侧）归 E3；材料侧不加 atom_count（监工已认可，不自行扩）；30 道 fixed case 未重生成。详见 task-05-report.md §8。

26. **E1-06 收尾执行完成（Task #39：§2/§3 全绿交付；§1 实跑到尽头触发停报条款，待监工裁定）**（2026-08-08，任务板 Task #39）：
    - **§1 ingest_open_gt 执行销账（触发停报条款，未自行处置）**：警告产生机制定位 `dag_poc/gateway.py` L1078（B∩A 两层判交 + 机器占位放行一次性 warning，占位源为 b_partition.json 22 道 open case 登记的 A 身份键）。新写 `src/data_build/build_open_gt_task39.py`（只读 templates/ + judge 侧 registry，只写 _build_state/）构造真 gt 文件（15 case/17 条药物 gt；材料 4 道为晶体，分子存在性通道双键列结构上无法承载）；`ingest_open_gt.py --dry-run` 实跑**接受 0/拒收 17**（15 条身份键空间隔离闸 B∩A=∅ 命中——gt 真实 InChIKey/SMILES 双双命中 A 池 registry；2 条断言二 DOI 缺失）。**根因**：ADR-0018 §2.3 per-case 逻辑 B——dh 类 gt 物理上就在 get_entity_A（z064386 等实测全部 ∈ A 全集 92,193，仅被视图白名单排除，断言 2/5 反向锁定 gt 可查），隔离闸拒收是 goal §2b⑤ 正确行为；“执行后归零”的消解预期（PR-46）与现行设计矛盾，非操作故障。accepted=0 时正式写盘只会翻新目录 MANIFEST 造成 CHECKSUMS 漂移且警告照旧，故不写盘；演练 fixture 零混入；残留相交属实（全部机器占位放行中，非隔离破口）——按任务书“停下报我”已上报 leader，附 A（口径改判）/B（gt 迁移手术，不建议）/C（等库外真 B 分子）三案待裁。拒收清单留痕 `_build_state/rejections_check_molecule_exists.jsonl`。
    - **§2 药物侧 element_count 核验（Task #38 已交付，本轮只核验不返工）**：实跑统计药物侧 prop=element_count 行数 **74,941**（总 92,193=药物 74,941+材料 17,252）；值域 1~8，档内 3~6 种 73,896（13,498/31,458/23,019/5,921）+档外 1,045（61/416/539/29）与 Task #38 销账表逐数一致；`check_agent_visible_anonymity.py` 实跑 exit=0 四道断言零命中（新行真实元素符号 0 条）；drug_admet_v1.json element_count 增键在案。时间线说明：监工 task-06 落笔早于 Task #38 交付，故重叠，report 已写明。
    - **§3 CHECKSUMS**：`--verify` 实跑 exit=0（主 37+附录 2），值 `06232bd217a592dbe51621f5eee1c386` 零漂移——本轮纯核验+登记销账、backend/ 零写入，无需再冻结/无需 §8 解冻登记。
    - **文档收尾**：task-06-report.md（六节，Status=blocked 如实标 §1 待裁）；task-06 状态行 🟡→✅已执行（附停报说明）；progress.md 本条。
    - **纪律自查**：只写 task/E1-*/ 三份文档与 src/data_build/build_open_gt_task39.py（+ _build_state/ 两件产物，冻结面外）；未动 backend/、gateway.py、b_partition.json、query_spec.json、共享账本；全程无 LLM 调用；未自行立 ADR。

27. **gt 迁移手术备战完毕、待 E3 后执行（Task #41，监工裁定方案 B；只备战、未执行迁移）**（2026-08-08，任务板 Task #41）：
    - **三项裁定照收**：①7 题判废不重建（drug_03/04/05/06/11、material_01/02）；②104 条同组成候选（89 z+15 c，含 37 G_B）全量迁出，B 侧只返存在性；③E3 先行、零断裂窗——本批不执行、不碰冻结数据。
    - **手术脚本一键就绪**：新写 `src/data_build/migrate_gt_to_B.py`（--dry-run 默认/--execute 两档；后者前置闸读 `e3_surgery_greenlight.json` 六字段，缺件 exit 2 已实跳验证）。
    - **dry-run 全验证实跑（对 Sam 基线逐条断言全中）**：33 case/37 G_B/104 候选；三库删行清单逐行落盘（get_entity_A 104 + get_property_A 278 + search_candidates_A 104 = **486**）；B 侧四文件注册载荷（存在性 104 行/get_entity_B 37 行身份档/两库保持空）+ b_partition 清 33 道占位；血缘预案 **486** 行；判废 7 题后保留 **26** 道视图 entity_ref 销账清单；判废 7 题处置方案（templates 建议标记废弃不删除，归 E5 署名）。计划 12 件落盘 `_build_state/gt_migration_plan/`。
    - **自检双绿**：dry-run 连跑两次 12 件产物逐字节一致；前后 `generate_checksums.py --verify` 均 exit=0（主 48+附录 2），CHECKSUMS 本体 md5 `c013c62406b17b52d54c1e99f79aa87e` 零漂移（backend/ 零写入）。注：该基线为 Task #39 后新一轮再冻结值（主清单 37→48 文件，generated_at 2026-08-08T06:32:34Z），非 Task #39 时点的 06232bd2。
    - **两份交接清单落点**：E3 五项（resolve 扩 A∪B 头号阻塞附行位/材料通道 4 primary+11 降级承 §7.1.2/5 测试改写/L1078 警告语义归零验收/ADR-0018 §8 第三行修订建议）→ 新开 `task-06-E3手术交接说明.md` + 材料侧交接 §10 增④指路；E5 知会（判废 7 题断链明细/26 道视图改 entity_ref/执行时点）→ per-case视图交接说明.md §8。
    - **执行前置条件（双硬闸）**：E3 完工（greenlight 六字段置真）+ 监工执行通知；缺一 --execute 拒绝。
    - **文档收尾**：task-06-report.md 补 §7 备战节（Status 改备战完毕待执行）；task-06 状态行（不标 ✅）；progress.md 本条。
    - **纪律自查**：只写 src/data_build/migrate_gt_to_B.py、_build_state/gt_migration_plan/（冻结面外）与本 domain 文档；未动 backend/（CHECKSUMS 零漂移为证）、gateway.py、b_partition.json、templates/、E5 视图、共享账本；未执行迁移；全程无 LLM 调用；未自行立 ADR。

## 🚧 进行中（接下来要做）

- **gt 迁移手术待执行（Task #41 备战完毕）**：执行顺序 E3（判分侧五项，见 task-06-E3手术交接说明.md）→ E1（migrate_gt_to_B.py --execute，前置闸=e3_surgery_greenlight.json 六字段）→ E5（视图/模板，见 per-case视图交接说明.md §8）；E1 侧等 E3 完工 + 监工执行通知，当前不动。
- **其余无进行中任务**：E1-06 §2/§3（Task #39）已全绿交付；§1 警告归零经方案 B 手术后自然清零（归零验收口径见 E3 交接 §1④）；性质值量化/加噪另待监工裁定（若做须立 ADR）；多原子离子 salt_form 行是否扩大清理规则待监工裁定（若扩，氰根/亚硝酸根/硫氰酸根 3 条为首批候选）；冻结静默窗已恢复生效（Task #41 零写入未破窗，CHECKSUMS=`c013c624…` 主清单 48+附录 2），手术执行时按入库手册 §8 解冻登记；**待 E3**：gateway get_property 白名单同步增 atom_count（药物）/element_count（药物+材料双侧）三键 + gt 手术五项。

## ⛔️ 阻塞 / 待补依赖（2026-08-06 Task #18 收口后终态）

1. **E3 gateway 切换新后端后重跑隔离断言②**（Task #8/#9 登记，收口后保留）：E3 gateway 现仍读 mock_db.json 旧实现；实现真实 gateway 接入新后端后，须把 `test_ab_isolation.py` 断言②驱动对象切换为真实 gateway 会话路径重跑（已在 report.md §3.2 书面交接 E3）。
2. **ZINC 源文件截断**（Task #7 新发现）：`sources/ZINC/zinc_id_smiles.tsv` 磁盘实存 4,660,898 bytes / 100,044 数据行，仅其 MANIFEST 声明（9,340,459 / 200,394）的一半；本次以磁盘实存入库（74,901 条，满足契约下限 ≥10,000），**补齐需重拉后增量入库**（search_candidates MANIFEST 已记 source_file_check）。
3. **3DSC CIF 正文缺失**（Task #7 新发现）：落盘 3DSC_MP.csv 的 cif 列全部为文件路径引用（5,771/5,771），get_entity.cif 如实置 null；**补齐需重拉 aimat-lab/3DSC 仓库 cif 目录**（同时解锁晶体 atom_count）。
4. **B 侧 gt 集待 E5**：四个 B 侧文件当前为空占位（不得编造 gt）；随 E5 造题冻结增长后由 E1 过门入库。**入库链路已演练就绪（Task #17）**：`ingest_open_gt.py`（三道闸+隔离闸+--clean）与 `入库操作手册.md` 已交付，E5 按手册 §1 格式落盘 gt 即可立即入库；隔离断言①/①b 均为一般形式，gt 入库后自动生效（沙箱自检+真实入库路径双证明）；届时需同步重跑 `generate_checksums.py` 刷新 CHECKSUMS.json 冻结基准。
5. **binding_affinity 待另立项**：数据源未锁定（UniProt/PDB/BindingDB 已落盘但未绑定到实体粒度），本阶段 get_property 不出行。
6. **历史阻塞项状态**：TDC_ADMET 已拉取并全部消费（6 基准 18,459 行，阻塞解除）；PubChem MANIFEST 已补齐（阻塞解除）；UCI_SuperCon zip 已整拉成功（8,300,005 B，EOCD/CRC 通过，complete=true），阻塞解除（回链：`task/E1-数据集背书调研/数据集背书调研报告.md` §六 与 `dag_poc/data/sources/UCI_SuperCon/MANIFEST.json`）。
7. **调研阶段新增核查（2026-08-06，不阻塞但登记在案）**：①Materials Project API 403、OQMD 不可达（沿用底稿 task11 结论）——材料线成品性质数据无现成借用源，get_property 材料线确认自建（3DSC/SuperCon 快照）；②Zinc-250K/TDC ADMET 子集/MOSES 全量为大体积资产，按任务纪律本阶段未拉取，拉取窗口留待构造阶段。
8. **AqSolDB 匹配键口径差异——已闭环（裁定 7.1 → Task #19 抽查 → Task #20 方案 A 修复）**：监工裁定保持代码实现（canonical SMILES 最大片段）；抽查发现盐类系统性偏差后按方案 A 修复（单组分优先择优 + salt_form 标记，见已完成第 12 条）；task18 文档口径订正归监工。
9. ~~**CHECKSUMS.json 与根 MANIFEST 待收口刷新**~~ **已解除（2026-08-06 Task #18 收口）**：CHECKSUMS.json 与根 MANIFEST 已统一刷新（先 finalize_backend 重放终审 0 拒收后重生成根 MANIFEST，再 generate_checksums 落盘），`--verify` 复跑全绿：主清单 14 + 附录 enumerate_A.json 1 全部一致，退出码 0。新冻结关键值：get_property_A=`14ad99f043092d9f817d2ac1f9f15c14`（9,814,540 B/27,618 行，含标注+盐类修复）、search_candidates_A=`4ff6f7aab6decc2af89449c3eb73270e`（39,977,343 B，含 family 修复）、根 MANIFEST=`86bb944a236647e7d6f05085b5c34d19`、附录 enumerate_A=`55ba749cf2a3dad680926a15ff387754`，合计 87,798,099 bytes；详见已完成段第 13 条。本条剩余未闭事项转跟踪：①condition 第二批（tc∈[60,100) 224 条）与 CrCuO2 两条处置待监工裁定；②z000009 水实体/裸离子退化匹配实体侧清理待另立小项（见 `盐类logS抽查记录.md` 处置节与收口报告 §5）。
10. **契约 §3.5 与 ADR-0015 的 case_ref 口径冲突（2026-08-06 Task #17 演练登记，报监工）**：契约 §3.5 写死 check_molecule_exists_B 记录只许三字段（多一个即违约），而 ADR-0015 §2.3 要求 G_B per-case 带 case_ref。演练采用“case_ref 承载于 provenance.note（格式 `case_ref=<case_id>；…`）”的兼容解（不增字段、清场/索引可解析），但建议契约 v1.2 显式澄清此口径（或裁定 B 侧增字段）；不自行改契约，待监工裁定。
11. **【报监工·影响面漂移】logS 方案 A 使 query_spec.json 药物容量格数过期（2026-08-06 Task #21 登记）**：裁定 7.1 方案 A 修复后实算药物容量为 **155/153/92**，而 `query_spec.json` 冻结值仍为 156/154/91（旧口径）；该文件属 **E2 冻结区**，E1 不越位改。请监工协调 E2 更新 capacity_basis 注记（或裁定口径）。
12. **【报监工·影响面漂移】family `|` 口径下材料侧格数两种口径不符（2026-08-06 Task #21 登记）**：E1 声明口径（`split("|")` 成员消费）实算材料侧格数为 **70/66/58**；而 E5 进场文档与 query_spec 的 **73/69/59** 为精确串口径（`|` 多标签整串不计入单标签格）。两口径均真实，差异恰来自 11 条 `|` 多标签记录的归属判定。请监工裁定统一口径并同步交接物（E5 进场文档/query_spec）；E1 不越位改写作区外文件。
13. ~~**E1-03 §2.2 登记的 CHECKSUMS 过期清单（已完成段第 16 条）**~~ **已解除（2026-08-07 Task #25 收口）**：四 A 侧 jsonl、四 A 侧 MANIFEST、根 MANIFEST、CHECKSUMS 本体已统一刷新，--verify 全绿（主清单 14 + 附录 2，material_identity_keys.jsonl 按裁定纳入附录）；详见已完成段第 18 条。
14. **【报监工·PR-31 编号漂移与 CLOSED 条件（2026-08-07 Task #25 收口登记）**：①problems.md 现存 PR-31（query 出题侧四道关，OPEN）与 ADR-0017/task-03 所引 PR-31（红线级匿名身份泄漏）编号指向不同事项，请监工裁定编号归一；②ADR-0017 备案 CLOSED 条件：§2（E1 责）已落地并断言通过，§3 case_id 去真名归 E5 未落地，PR-31 状态变更归监工。另：性质值量化/加噪若做须立 ADR（本轮只登记不立项）。
