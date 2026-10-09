# Task Result · TASK-E1-02 收口 —— condition 分层标注 + family 修错 + B 侧入库演练：统一冻结与逐条自检

> **Status: `done`**

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E1-02 收口（任务板 Task #18）/ `task/E1-后端数据库构造/` |
| **类型** | Implementation（补缺 + 演练 + 统一冻结收口） |
| **负责人** | E1（数据线） |
| **起止** | 2026-08-06 ~ 2026-08-06（1 天，四路并行后收口） |
| **上游来源** | 监工验收意见（task-02 §0）· 容量判据裁定 7.1 · ADR-0015 |
| **下游消费方** | **E5**（材料侧造题依赖 family 修错与 condition 标注；open gt 入库依赖已演练的链路） |

---

## 一句话结论

> 四路前置（§1 标注 21/23 命中、§2 family 11 条修错、§3 B 侧入库全链路演练、裁定 7.1 盐类修复）全部完成后，本收口**统一刷新 CHECKSUMS 冻结基准（14 主清单 + 1 附录，`--verify` 全绿退出码 0）**、全量回归五项全过（finalize 终审 0 拒收 / test_ab_isolation 退出码 0 / ingest_gate --selftest 全绿 / bench_backend 8 文件严格载入 / CHECKSUMS --verify 14+1），task-02 §4 六条交付标准逐条实跑自检达标；剩余 condition 缺口 5,734 条按 tc 分层如实登记；两项报监工裁量事项（CrCuO2 疑似脏数据、契约 §3.5 与 case_ref 口径冲突）与一项新增遗留（z000009 水实体退化匹配）已登记，不自行立 ADR。

---

## 1. 任务执行情况（对照 task-02 §4 六条交付标准逐条自检）★

| # | 交付标准 | 状态 | 实跑命令与输出证据 |
|---|---|---|---|
| 1 | tc ≥ 100 K 的 23 个晶体 condition 标注完成，每条附核验 DOI + citation；未命中如实记「未知」 | [x] ✅ 21/23 | **命令**：聚合 `get_property_A.jsonl`（prop=condition/tc 交叉统计，python3 内联脚本实跑）。**输出**：condition 行合计 5,771 = `ambient` 36 + `high_pressure(~96-120GPa)` 1 + `未知` 5,734；其中 tc≥100K 段：21 条 ambient（c01801/c01802/c01806/c01810/c01813–c01821/c01824–c01828/c02154/c02712/c02740，tc 100.0–132.9 K）+ 2 条未知（**c03272 tc=132.0 / c03269 tc=117.5，均 CrCuO2**）。标注附 6 个母体族 CrossRef 核验 DOI（`VERIFIED_DOIS_BATCH2` 固化于 `condition_annotations.py`，含 citation）。未命中按红线记「未知」，无默认 ambient。**DOI 粒度说明**：21 条标注的 DOI 为母体族发现文献粒度（citation 内逐条注明掺杂变体归属），无独立文献可核验的 2 条按红线记未知 |
| 2 | family 11 条拼接错误修复，多标签口径明确，CHECKSUMS.json 已刷新 | [x] ✅ 11/11 | **命令**：聚合 `search_candidates_A.jsonl`（entity_type=crystal，filter_props.sc_class）与 `get_property_A.jsonl`（prop=family）。**输出**：伪类别 `Heavy_fermionChevrel`/`OxideHeavy_fermion` 两库计数均为 **0**；`\|` 分隔多标签各 **11** 条，id 与修前清单逐一吻合（c03031/c04851/c04852/c04853/c04860/c04865/c04870 → `Heavy_fermion\|Chevrel`；c02166/c02168/c05108/c05754 → `Oxide\|Heavy_fermion`）。口径=`\|` 分隔字符串（`FAMILY_LABEL_SEP` 固化于 common.py）。CHECKSUMS 刷新见 §2.1 |
| 3 | B 侧入库全链路演练跑通（fixture → 过门 → 入库 → 刷 CHECKSUMS → 隔离重跑），证明隔离断言由"恒真"转为"实际检查" | [x] ✅ | **证据（实跑日志留痕）**：`src/data_build/_build_state/rehearsal_logs/rehearsal_isolation_BEFORE.log`——断言①b 输出"B 侧键 InChIKey 0 / SMILES 0……**模式=B 空集（平凡成立）**"；`rehearsal_isolation_AFTER.log`——fixture 入库后输出"B 侧键 InChIKey 2 / SMILES 2；A 池键 InChIKey 74283 / canonical SMILES 74941；交集 0……**模式=实际比对**"。全链路（fixture→ingest_gate 三断言→字段白名单→身份键空间隔离闸→写入 check_molecule_exists_B 2 条→刷 CHECKSUMS→隔离重跑全绿→语义反向验证）实跑跑通；与 A 重叠的负例 gt（aa，双键命中）被隔离闸拦截（拒收清单 `rejections_check_molecule_exists.jsonl`） |
| 4 | 入库操作手册（面向 E5） | [x] ✅ | `task/E1-后端数据库构造/入库操作手册.md`（7,306 bytes，2026-08-06 20:50 落盘；含 gt 落盘格式、过闸失败常见原因、刷 CHECKSUMS 步骤、演练教训四条） |
| 5 | 演练 fixture 已清出 B 侧，B 侧恢复为空 | [x] ✅ | **命令**：`md5sum` 四个 B 文件。**输出**：四个 `*_B.jsonl` md5 均为 `d41d8cd98f00b204e9800998ecf8427e`（空文件标准 md5），record_count=0，与演练前基线（`rehearsal_backup/PRE_STATE.md5`）逐一相同；本次 CHECKSUMS 冻结值同样确认 4 个 B 文件为 0 bytes |
| 6 | report 收口：标注命中率、剩余 condition 缺口、演练流程问题 | [x] ✅ | 本报告 §2.2（标注命中率 21/23 + 剩余缺口 5,734 按 tc 分层）、§3（演练流程问题清单 + 两项报监工事项）、§2.1（新冻结值） |

**勾选符号**：`[x]` 完成——六条全部达成，无部分完成项。

---

## 2. 关键产出 / 发现（自包含）

> **置信度标注**（Task #21 评审补强）：本节各项产出均有实跑证据（命令/日志/md5 见各小节与 §2.5），置信度统一标 **高**：§2.1 冻结基准（--verify 全绿）= 高；§2.2 标注命中率（实跑聚合）= 高；§2.3 family 修错（两库计数实测归零）= 高；§2.4 盐类修复统计（前后对照实测）= 高；§2.5 回归清单（五项退出码 0）= 高。

### 2.1 CHECKSUMS 新冻结基准（本收口统一刷新，2026-08-06）

**命令**：`python3 src/data_build/generate_checksums.py` → 落盘后 `--verify` 复跑。
**结果**：`冻结校验通过 ✓：主清单 14 个文件 + 附录 1 个交接产物 md5 全部一致`，退出码 0。合计 **87,798,099 bytes**（较上轮冻结 87,680,846 增 117,253 bytes，全部来自 get_property_A 的标注/盐类修复与 search_candidates_A 的 family 修复）。

| 文件 | 新 md5 | bytes |
|---|---|---:|
| MANIFEST.json（根，finalize 重生成） | `86bb944a236647e7d6f05085b5c34d19` | 3,138 |
| check_molecule_exists/MANIFEST.json | `c1ee974af787d41b2ad60d2ec1ba98c1` | 667 |
| check_molecule_exists/check_molecule_exists_B.jsonl | `d41d8cd98f00b204e9800998ecf8427e`（空） | 0 |
| get_entity/MANIFEST.json | `d0edfcb8dce94efacb55584347655074` | 1,433 |
| get_entity/get_entity_A.jsonl | `8aaa523b03c58646a9237b7f2850db94` | 37,797,793 |
| get_entity/get_entity_B.jsonl | `d41d8…`（空） | 0 |
| get_property/MANIFEST.json | `4b5e3acb7d48dcb571a431ffa77fbaa7` | 1,180 |
| **get_property/get_property_A.jsonl** | **`14ad99f043092d9f817d2ac1f9f15c14`** | **9,814,540** |
| get_property/get_property_B.jsonl | `d41d8…`（空） | 0 |
| literature_search/MANIFEST.json | `5ed09fbae9748b0e425efe4989656fcd` | 535 |
| literature_search/literature_search_A.jsonl | `6e542c979f5461bc08496799cbfeda59` | 200,246 |
| search_candidates/MANIFEST.json | `bc6f83dc2a93501b3e9a11f67f8462ff` | 1,224 |
| **search_candidates/search_candidates_A.jsonl** | **`4ff6f7aab6decc2af89449c3eb73270e`** | **39,977,343** |
| search_candidates/search_candidates_B.jsonl | `d41d8…`（空） | 0 |
| **附录** `_build_state/enumerate_A.json`（E5 穷举质量门交接物） | `55ba749cf2a3dad680926a15ff387754` | 1,859,077 |

**MANIFEST 交叉核验**（收口实跑脚本）：根 MANIFEST（generated_at=2026-08-06T12:58:18Z，189,363 条 / 87,789,922 bytes）与五个子库 MANIFEST 的 md5/record_count/bytes **逐字段一致**，且全部与磁盘实存（md5 复算、行数实测）一致；backend 目录下除 CHECKSUMS.json 外无 MANIFEST 未覆盖的游离文件（6 份 MANIFEST 自身由 CHECKSUMS 冻结覆盖，属设计）。

### 2.2 condition 标注命中率与剩余缺口（按 tc 分层，实测聚合）

- **命中率**：tc≥100K 段 **21/23 标注 ambient**（逐条 CrossRef 核验 DOI + citation），**2 条 CrCuO2 记「未知」**（c03272 tc=132.0 / c03269 tc=117.5——落盘快照中无可核验的独立文献 DOI，疑似 3DSC 上游脏数据，见 §3 报监工事项 ①）。**DOI 粒度**：21 条标注的 DOI 为母体族发现文献粒度（citation 内逐条注明掺杂变体归属），无独立文献可核验的 2 条按红线记未知。
- **全库 condition 现状**（get_property_A 5,771 条晶体行）：已标注 **37**（ambient 36 + high_pressure(~96-120GPa) 1，即上轮 16 条 + 本批 21 条）；未知 **5,734**。
- **剩余缺口 5,734 条按 tc 分层分布**（实测）：

| tc 区间 | 未知条数 | 处置 |
|---|---:|---|
| ≥100 K | 2 | CrCuO2 两条，待监工裁量（§3 ①） |
| 60–100 K | 224 | 第二批，本轮如实不做（成本/出错风险不可接受，待监工裁定是否另批立项） |
| 30–60 K | 352 | 未立项 |
| 10–30 K | 805 | 未立项 |
| <10 K | 4,351 | 未立项 |

### 2.3 family 修错摘要（§2 + family 联动）

- **口径定案**：`|` 分隔字符串（不用数组，理由=契约 §3.4 / tools_schema `value: number|string` 形状约束；E5 分层 `split("|")` 即用；单标签向后兼容）。
- **源头**：3DSC `sc_class` 列上游缺陷（11 条无分隔符拼接值），修复留痕 `sc_class_raw`（registry）；拆解按词表唯一匹配 + 化学语义核验，非猜测。
- **影响面实跑确认**：`search_candidates_A` 80,712 行不变、仅 11 行 family 值变化；`get_entity_A` md5 不变（本就无 family 字段）；**get_property_A 的 11 条 family 行伪类别随重跑归零**（联动确认：`|` 多标签行恰 11，id 吻合）。
- **§7.3 声明**：get_entity/MANIFEST.json 已含 `null_field_declarations`（晶体 cif/atom_count 为 null 的原因与补齐路径），本次冻结值 `d0edfcb8…` 即含该声明。

### 2.4 裁定 7.1 盐类修复最终统计（以 leader 2026-08-06 21:00 勘误消息为准）

- **logS 总量 2,311 不变**（收口聚合实测一致）：**换源 15 条**（多组分→单组分，恰为抽查点名的 15 条）、**新加 `salt_form=true` 标记 59 条**（实测 59）、移除 0、新增 0。
- 典型实体：z002962 特比萘芬 A-5442→A-2743（Δ2.75 修复）、z002936 肾上腺素 B-24→B-25（Δ3.01 修复）、dc 二甲双胍保留但打标。
- get_property_A 新 md5 `14ad99f043092d9f817d2ac1f9f15c14` / 9,814,540 bytes / 27,618 行（与 §2.1 冻结值一致）。处置细节见 `盐类logS抽查记录.md` 处置节。

### 2.5 全量回归清单（收口实跑，全部退出码 0）

| # | 命令 | 结果摘要 |
|---|---|---|
| 1 | `python3 src/data_build/finalize_backend.py` | 重放终审：8 个 jsonl 逐文件复核**拒收 0**（search_candidates_A 断言一式比对重放 74,941；晶体 5,771 如实声明不可重放）；A/B 隔离断言 B 独有 id=0 / A 泄漏=0；根 MANIFEST 重生成（189,363 条） |
| 2 | `python3 src/data_build/test_ab_isolation.py` | 五子库断言①键集交集全 0 + 沙箱负向自检"注入重叠键 aa→被抓 ✓"；断言①b 模式=B 空集（平凡成立，B 侧为空态）；断言②无非法 _B 句柄；断言③ 53 键全 false 纯 bool；enumerate_A 三库枚举落盘；**总结论全部通过，退出码 0** |
| 3 | `python3 src/data_build/ingest_gate.py --selftest` | 40/40 分子正向过门；aa 旧 SMILES 负向拒收 ✓；占位符/大小写变体 provenance 拒收 ✓；断言三（SA 通道/未知源/白名单源/id_only）全过 ✓；退出码 0 |
| 4 | `python3 src/data_build/bench_backend.py` | 8 个文件全过严格载入器；全表扫描 0.004–0.759 s；合计 du=86M ≤10 GB ✓；退出码 0 |
| 5 | `python3 src/data_build/generate_checksums.py --verify` | `冻结校验通过 ✓：主清单 14 + 附录 1 md5 全部一致`，退出码 0（含 test_ab_isolation 重跑覆写 enumerate_A 后复验仍绿——内容确定性） |

---

## 3. 结论与建议

### 3.1 判定
**✅ 成立** —— 六条交付标准全部实跑达标（§1 表格），冻结基准已统一刷新并复验全绿（§2.1），回归五项全过（§2.5）；两项报监工裁量事项与遗留清单如实登记（§5），不阻塞 E5 开工。

### 3.2 演练发现的流程问题清单（task-02 §4 第 6 条）

| # | 问题 | 处置 |
|---|---|---|
| 1 | `open_case_gt` 不在来源白名单，演练首跑被断言三整库冻结 | **已修**：common.py `DATA_FILE_ALLOWED_SOURCES` 新增，selftest 回归全绿 |
| 2 | **契约 §3.5 三字段白名单 vs ADR-0015 case_ref 口径冲突** | 演练采用"case_ref 承载于 provenance.note（`case_ref=<case_id>；…`）"兼容解（不增字段）；**已登记 progress 第 10 条报监工，建议契约 v1.2 澄清，不自行立 ADR**（见 §5 报监工事项 ②） |
| 3 | fixture 候选（aspirin/metformin/ibuprofen/acetaminophen/sucrose/citric_acid）实测全部在 ZINC A 池内 | gt 选分子不得凭直觉假设"不在 A 池"，必须实测比对——已写死入手册 §1 |
| 4 | ingest_gate 三断言不覆盖"与 A 重叠"（负例 gt 三断言全放行） | 身份键空间隔离闸不可省；已在手册与演练日志留痕 |
| 5 | **CrCuO2 两条疑似 3DSC 上游脏数据**（tc=132/117.5 K 但快照无可核验独立文献 DOI，被 3DSC 归 Cuprate） | 按红线留「未知」；**是否排除出 E5 造题池待监工裁量**（见 §5 报监工事项 ①） |

### 3.3 下一步行动

| # | 动作 | 谁来做 | 何时 | 优先级 |
|---|---|---|---|---|
| 1 | 裁量 CrCuO2 两条是否排除出 E5 造题池 | 监工 | E5 造题冻结前 | P1 |
| 2 | 裁量契约 §3.5 三字段白名单 vs case_ref（建议契约 v1.2 澄清或裁定 B 侧增字段） | 监工 | E5 真 gt 入库前 | P1 |
| 3 | E5 按 `入库操作手册.md` §1 落盘真 gt → E1 过门入库 → 重跑 generate_checksums 刷新冻结基准 | E5 → E1 | E5 造题冻结后 | P1 |
| 4 | condition 第二批（60–100 K 224 条）是否另批立项 | 监工裁定 | 视 E5 题型需求 | P2 |
| 5 | z000009 水实体/裸离子退化匹配清理（另立小项，见 §5） | E1（待立项） | 第二版 | P2 |
| 6 | E3 gateway 切新后端后重跑隔离断言②（承上轮 report §3.2） | E3 | gateway 实现后 | P2 |

---

## 4. 受阻 / 失败说明

无，任务已完成（Status = done）。

---

## 5. 风险与未决问题

| 类型 | 描述 | 影响 | 缓解 / 待决方 |
|---|---|---|---|
| **报监工事项 ①** | CrCuO2 两条（c03272 tc=132.0 / c03269 tc=117.5）疑似 3DSC 上游脏数据：落盘快照中无可核验独立文献 DOI，却被 3DSC 归入 Cuprate；若入造题池，题面"常压超导"答案的可核验性存疑 | E5 材料侧高温段造题池（tc≥100K 段共 23 晶体） | 标注已如实记「未知」不误导；**是否排除出 E5 造题池待监工裁量** |
| **报监工事项 ②** | 契约 §3.5 写死 check_molecule_exists_B 记录只许三字段，与 ADR-0015 §2.3 要求 G_B per-case 带 case_ref 冲突；演练兼容解=case_ref 承载于 provenance.note | E5 真 gt 入库时的字段口径 | 已登记 progress 第 10 条；**建议契约 v1.2 显式澄清（或裁定 B 侧增字段），待监工裁定**，E1 不自行改契约 |
| 新增遗留 | **z000009 水实体与裸离子实体的退化匹配**属实体侧问题：AqSolDB 无水的单组分记录，方案 A 兜底打标（salt_form=true）是正确语义，但实体侧"水/裸离子作为可造题实体"本身待清理 | 个别低价值/无意义实体可能进入造题池 | 已在 `盐类logS抽查记录.md` 处置节登记，**待另立小项清理** |
| 已知风险 | condition 未知 5,734 条（tc<100K 段）未标注 | 低温段"常压超导"题面不可启用 | 红线记「未知」不默认 ambient；60–100K 224 条待监工裁定是否另批 |
| 已知风险 | ZINC 源文件截断 / 3DSC CIF 正文缺失（承上轮） | 分子池仅一半、cif/atom_count=null | 均已在 MANIFEST 声明、不阻塞（容量足够/MANIFEST 已声明），补齐待第二版 |

---

## 6. 交付物与背景（给外部读者的最小自包含上下文）

**最小背景**：本 benchmark 后端库为双领域（药物 74,941 分子 + 材料 5,771 晶体）查询后端，A/B 物理分层（A=造题池、B=open case 的 gt，由文件系统分层隔离）。上一轮（Task #6–#9）建成五子库并冻结；本轮 E1-02 补两处数据缺口（condition 高温段标注、family 拼接错误）、提前演练 B 侧入库全链路、落实裁定 7.1 盐类修复，最后由本收口统一刷新冻结基准并逐条自检。

**交付物**：

| 类别 | 路径 | 说明 |
|---|---|---|
| 本报告 | `task/E1-后端数据库构造/task-02-report.md` | 对外成品（本文件） |
| 冻结基准 | `dag_poc/data/backend/CHECKSUMS.json` + 根 `MANIFEST.json` | 本收口统一刷新，--verify 全绿（§2.1） |
| 标注机制 | `src/data_build/condition_annotations.py` | 37 条标注 + `VERIFIED_DOIS_BATCH2` CrossRef 核验记录 |
| 入库管线 | `src/data_build/ingest_open_gt.py` + `make_rehearsal_fixture.py` | 三道闸 + 隔离闸 + --clean，E5 真 gt 直接可用 |
| 入库手册 | `task/E1-后端数据库构造/入库操作手册.md` | 面向 E5 |
| 演练证据 | `src/data_build/_build_state/rehearsal_logs/` | BEFORE/AFTER 隔离日志（恒真→实际检查）、selftest、语义反向验证 |
| 盐类处置 | `task/E1-后端数据库构造/盐类logS抽查记录.md` | 含处置节（含 z000009 登记） |
| 进度账 | `task/E1-后端数据库构造/progress.md` | 第 12 条收口记录，阻塞段第 9 条解除 |
| 上轮收口 | `task/E1-后端数据库构造/report.md` | 上一轮（Task #6–#9）收口样例与监工验收意见 |

---

## 边界自查（task-02 §5 硬性）

- 本收口仅写：`task/E1-后端数据库构造/`（本报告、progress.md、task-02 表头）、`dag_poc/data/backend/CHECKSUMS.json` 与根 `MANIFEST.json`（经 generate_checksums/finalize_backend 既有脚本产出）；**未改任何子库数据与子库 MANIFEST**（只读核验）。
- 未碰：`dag_poc/templates/**`、`query_spec.json`/`query_lint.py`、`gateway.py`/`evaluate.py`/`tools_schema.json`/`answer_schema.json`、`TODO.md`/`problems.md`/`goal.md`/`governance/adr/**`、其他 `task/E*/` 目录。
- 需立 ADR 的两项（§3.2 第 2、5 条）只登记上报，未自行立。

---

## 监工验收意见（E1-02，2026-08-06，逐条实跑复核）

**结论：通过。** 三项全部达标，其中第 3 项的证据质量是目前所有交付里最好的。

| 项 | 实跑复核 |
|---|---|
| ① condition 标注 | ✅ tc≥100K **23 个中标 21 条 ambient**，全库 `ambient` 从 15 → **36**；剩 2 条（`c03272` tc=132.0 / `c03269` tc=117.5，均 CrCuO2）**按红线记「未知」并报监工**——理由是"落盘快照无可核验的独立文献 DOI，疑似 3DSC 上游脏数据"。**这是正确处置，不是缺口**：红线原文就是"未命中一律记「未知」，不得默认 ambient" |
| ② family 拼接错误 | ✅ 实测 family 取值 **9 个**，`Heavy_fermionChevrel` / `OxideHeavy_fermion` **无残留** |
| ③ B 侧入库演练 | ✅ **BEFORE/AFTER 双日志留痕**，见下 |
| 演练后清出 | ✅ 四个 `*_B.jsonl` 全部 0 行，B 侧已恢复空置 |

### 第 3 项特别记功：用日志证明了断言从"恒真"转为"实际检查"

这是我在派活时的核心要求，你用可复核的方式做到了：

```
rehearsal_isolation_BEFORE.log → "B 侧键 InChIKey 0 / SMILES 0…… 模式=B 空集（平凡成立）"
rehearsal_isolation_AFTER.log  → "B 侧键 InChIKey 2 / SMILES 2；A 池键 74283/74941；交集 0…… 模式=实际比对"
```

而且**做了负例验证**：与 A 重叠的 gt（`aa`，双键命中）被隔离闸拦截，落 `rejections_check_molecule_exists.jsonl`。**这比"跑通"强一个层级——它证明了闸门会拒收，而不只是证明合法数据能通过。**

对照：E3 的隔离测试至今是恒真断言（B 用合成串与 A 天然不同命名空间），同一个陷阱你做了两次防御（沙箱负向自检 + 本次 BEFORE/AFTER）。

### 诚实度记录两处

1. **DOI 粒度如实声明**：21 条标注用的是**母体族发现文献粒度**，citation 内逐条注明掺杂变体归属——没有把族级 DOI 冒充为逐条文献。
2. **主动报出上游数据疑点**：CrCuO2 两条疑似 3DSC 脏数据，报监工事项而非自行处置。

### 遗留（不影响本轮通过）

| # | 项 | 处置 |
|---|---|---|
| 1 | condition 未知 5,734 条（tc<100K 段） | 低温段"常压"题面不可启用。60–100K 的 224 条**是否另批标注需 owner 定**（已在 `USER_TODO.md` U-3 登记为选项 B） |
| 2 | CrCuO2 两条疑似上游脏数据 | 监工采纳你的处置（记未知）。若后续 E5 造题需要这两条，再回溯 3DSC 原始仓库核验 |

### 🔴 本轮之后新增的硬要求（ADR-0017，owner 直接决议）

**你的 A 侧库需要返工**——不是本次交付的问题，是 owner 新决议带来的：

实测 A 侧返给 agent 的数据带**真实化学身份**：`inchikey: IUBSYMUCCVWXPE-UHFFFAOYSA-N`（metoprolol 真实键）、`formula: C15H25NO3`（真实分子式）。加上匿名映射表不存在、`case_id` 带真名，**防污染红线 5 已被实际违反**（`problems.md` PR-31）。

**监工失职先认**：我上一轮验收把"数据真实（真 InChIKey）"写进了记功项——对数据集而言真实是优点，**对 benchmark 而言 agent 可见的真实化学身份就是漏洞**，我当时没转过这个弯。

具体要做什么见新任务 `task-03`（另发）。
