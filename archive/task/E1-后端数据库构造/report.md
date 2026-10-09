# E1 · 步 4 — 后端 mock 数据库调研与构造 · 收口报告（report）

| 字段 | 值 |
|---|---|
| 任务 | `task/E1-后端数据库构造/task.md`（goal.md v3.0 §6 步 4）｜任务板 Task #6/#7/#8/#9 |
| 执行人 | E1-数据线（Qoder 工程组）｜收口日期：2026-08-06 |
| 状态 | ✅ 完成（7 项验收全部达标，遗留 7 项待补清单已登记交接） |

---

## §0 核心结论（三句话）

1. **五问调研结论：61 家 benchmark 无一家可复用的双领域查询后端，退回自建，但借用 4 类成品组件**（TDC ADMET 成品数据、Zinc-250K 分子池、tau-bench dict 内存库范式、PoseBusters 规则套件）——决策汇总：拿来用 0 / 改造 1 / 自建 4 / 无需后端库 4。
2. **五子库已建成并 md5 冻结**：`dag_poc/data/backend/` 共 **189,363 条记录 / 85 MB**（≤10 GB 预算仅占 0.8%），JSONL 全载、一侧一文件、每条记录 provenance 三元组齐全、入库三断言零漏检、隔离三断言全过、CHECKSUMS 14 文件冻结基准已落盘并复跑校验通过。
3. **task.md §5 验收项 1–7 硬指标全达标**，逐条证据见 §2；契约 v1.0→v1.1 仅 1 处 DOI 勘误（无规格变更），已按 §7 修订流程留痕并向 E3/E5 书面交接。

---

## §1 做了什么（2a→2e 全过程，各带证据路径）

| 阶段 | 内容 | 证据路径 |
|---|---|---|
| **2a 调研** | 61 家 benchmark 五问逐家回答 + 13 家深挖 + 逐 tool 决策表 + 17 样本本地实测 | `现有后端数据库调研报告.md`、`measure_local_samples.py` |
| **2b 契约** | 存储格式定案（JSONL 全载）、目录与 A/B 物理分层、5 子库字段规格、provenance 规范、三道入库校验门、读取接口契约 | `后端库规格契约.md` v1.1 |
| **2c 构造** | 构造脚本 9 个 + 五子库全部落盘 + 全量复核 0 拒收 + condition 人工标注（16 条命中，DOI 逐条 CrossRef 核验） | `src/data_build/`（9 脚本）、`dag_poc/data/backend/`、`build_stats.md` |
| **2d 实测冻结** | 性能实测（最大子库全扫 0.76 s、点查 ≤253 ns、内存峰值 860 MB）+ 隔离三断言（退出码 0，含沙箱负向自检）+ md5 冻结（CHECKSUMS.json 14 文件，--verify 通过）+ enumerate_A 落盘 | `性能与隔离实测报告.md`、`src/data_build/{backend_reader,test_ab_isolation,bench_backend,generate_checksums}.py`、`dag_poc/data/backend/CHECKSUMS.json` |
| **2e 报告收口** | 9 份每 tool 配置报告 + 契约 v1.1 勘误 + 本报告 + progress.md 终态 | `后端库配置报告/`（9 份）、本报告 |

---

## §2 硬指标逐条对照（task.md §5 验收项 1–7）

| # | 验收项 | 达标证据 |
|---|---|---|
| 1 | 现有后端库调研覆盖 35+ benchmark 五问 | 调研报告 §1 覆盖 **61 家**（超 35+ 下限）逐家五问对照表 + §2 十三家深挖，未跳过调研 |
| 2 | 每 tool 后端来源明确决策与理由 | 调研报告 §3 决策表：拿来用 0 / 改造 1（get_property←TDC ADMET）/ 自建 4 / 无需后端库 4，逐行附理由 |
| 3 | 每子库条数/体积/读取耗时/内存峰值实测 | 实测报告 §3 逐文件数字表：全表扫描 0.002–0.755 s、点查 26–253 ns、tracemalloc 峰值 1.5–377 MB、进程级 ru_maxrss 860.5 MB（可复跑 `bench_backend.py`） |
| 4 | 总体积 ≤10 GB | `du -sh` 实测 **85M**（du -sk 精确 88,625,152 bytes），仅占预算 0.8%（实测报告 §4） |
| 5 | 每 tool 一份配置报告（数据数量/功能/调用方式/出入参含义） | `后端库配置报告/` 共 **9 份**（5 份需后端库：数据数量/功能/契约 §6 接口签名/逐参数含义/provenance 示例/性能数字/已知限制；4 份纯计算收卷类：无后端理由） |
| 6 | A/B 源物理分层 + 隔离测试 | 一侧一文件 + 读取层白名单（非查询过滤）；隔离三断言全过退出码 0：①A∩B 键集=0（沙箱负向自检证明 B 非空后自动生效）②句柄追踪 0 个非法 `_B` 句柄 ③check_molecule_exists 53 键抽样全返 false 纯 bool 三键（实测报告 §2） |
| 7 | 入库即校验，不一致拒收 | 三道断言逐行过门 + 落盘重放复核 0 拒收；回归自测（40 分子全过 + aa 旧 SMILES 注入必拒）；实际拒收 232 条全部留痕 `rejections_search_candidates.jsonl`；SA 红线 ChEMBL/SIDER=0（build_stats.md §2） |

**task.md §3 硬指标**同步达标：总体积 ≤10 GB ✓；本地可解析（读取/内存实测已记录）✓；每 tool 一个子库（5 目录独立，不共用）✓；provenance 三元组每条记录必带（入库断言二强制，缺一即拒收）✓。

---

## §3 遗留与交接

### 3.1 七项待补清单（承 `build_stats.md` §4，不伪造、按契约如实留白）

| # | 待补项 | 补齐路径 |
|---|---|---|
| 1 | `binding_affinity` 性质行 | 数据源未锁定（UniProt/PDB/BindingDB），另立项 |
| 2 | ZINC 分子 `filter_props`（mw/logp/tpsa） | 重拉 Zinc-250K 含属性列版本（红线禁止计算补齐） |
| 3 | 晶体 `cif` 正文 | 重拉 aimat-lab/3DSC 仓库 cif 目录（落盘 3DSC_MP.csv 的 cif 列是文件路径引用） |
| 4 | 晶体 `atom_count` | 随 #3 一并补齐 |
| 5 | 四个 B 侧文件（gt 集） | 待 E5 造题冻结后由 E1 过门入库（每 case ≥1，不得编造）；入库后隔离断言自动生效、须重跑 `generate_checksums.py` 刷新 CHECKSUMS |
| 6 | 5,755 条晶体 `condition`="未知" | 按材料族 + 文献补标（红线不默认 ambient） |
| 7 | ZINC 源文件截断 | 磁盘实存 100,044 行仅 MANIFEST 声明一半，重拉后增量入库 |

### 3.2 下游须重跑项

- **E3 gateway 切换新后端后须重跑隔离测试断言②**：断言②当前被测对象是契约 §6 参考读取层 `backend_reader.BackendReader`（E3 gateway 现仍读 mock_db.json 旧实现）；E3 实现真实 gateway 后，把 `test_ab_isolation.py` 断言②的驱动对象切换为真实 gateway 会话路径重跑一次（实测报告 §7 遗留 2 已登记）。

### 3.3 契约 v1.1 勘误说明

§4.3 示例 LaH10 DOI `10.1038/s41586-019-1410-1` 经 CrossRef 核验指向无关白血病论文，更正为 `10.1038/s41586-019-1201-8`（Nature 572, 2019）。**构造期与实测期均已按核验值施工，数据零返工**；详见 §4。

### 3.4 行文口径备注（非规格问题，登记在案）

- literature_search 子库的 provenance `source_db` 取值 `CrossRef`，为契约 §4.1 示例枚举之外的合理延伸（自建冻结快照的采集源），三元组齐全、过断言二；
- edit_molecule 的 `ANCHOR_NOT_IN_A` 前置校验按「会话内已触达实体集合」实现（承 compare 同口径），不触后端库，与契约 §3.6「不触达任何子库」无冲突；E3 实现时如改用 A 侧成员关系查询，须在 gateway 层记账并知会 E1。

### 3.5 TODO 账本同步触发

- 本线已收口，请监工将 TODO.md T-63（后端数据库构造）置 ✅ 并回链本 report。

---

## §4 对已冻结契约的改动说明 + 已通知的下游线（DoD 硬性项）

### 4.1 契约改动说明

- **改动对象**：`后端库规格契约.md`（E1 交付的冻结规格契约，基线锚定 tools_schema.json v0.1.0-stub）。
- **v1.0 → v1.1：仅 1 处勘误，无任何规格变更**——§4.3 示例的 LaH10 DOI 更正（见 §3.3）；字段规格、目录结构、校验门、读取接口签名全部未动。已按契约 §7 修订流程执行：表头版本行 bump + 修订日志追加一行 + progress.md 记一笔。
- **对 tools_schema.json（E3 的冻结契约）：零改动**——本 task 全程只读引用，未写任何 E3 交付物。

### 4.2 已通知的下游线（通知方式均为书面契约/报告路径，随本收口报告一并交接）

| 下游线 | 交接内容 | 书面载体（通知方式） |
|---|---|---|
| **E3（gateway 实现）** | 读取接口契约（五查询接口 + enumerate_A 的白名单与预算口径）、`dag_poc/data/backend/` 目录树与 MANIFEST/CHECKSUMS 冻结基准、A 侧文件白名单规则、断言②重跑义务 | `后端库规格契约.md` v1.1 §6 + §2.3（书面契约）；`性能与隔离实测报告.md` §7 遗留 2（书面登记）；`后端库配置报告/` 9 份逐 tool 调用方式节 |
| **E5（template/case 构造）** | enumerate_A 清单（open 类穷举质量门）、condition 人工标注表（LaH10/H3S 已预冻结，gt 实体入池即自动命中）、B 侧 gt 冻结入库流程（E5 落盘 → E1 过门 → 刷新 CHECKSUMS） | `src/data_build/_build_state/enumerate_A.json`（落盘数据）；`src/data_build/condition_annotations.py`（标注表，书面代码载体）；`build_stats.md` §3–§4 与契约 §3.2/§3.5 B 侧条款（书面契约） |

---

## §5 交付物清单（全部路径）

| 类别 | 路径 |
|---|---|
| 调研报告 | `task/E1-后端数据库构造/现有后端数据库调研报告.md`、`measure_local_samples.py` |
| 规格契约 | `task/E1-后端数据库构造/后端库规格契约.md` v1.1 |
| 构造统计 | `task/E1-后端数据库构造/build_stats.md` |
| 实测报告 | `task/E1-后端数据库构造/性能与隔离实测报告.md` |
| **每 tool 配置报告（9 份）** | `task/E1-后端数据库构造/后端库配置报告/{literature_search, search_candidates, get_entity, get_property, check_molecule_exists, compare, submit_answer, edit_molecule, normalize_structure}.md` |
| 后端库本体（冻结） | `dag_poc/data/backend/`（8 jsonl + 6 MANIFEST + CHECKSUMS.json，189,363 条 / 85 MB） |
| 构造与验证脚本 | `src/data_build/`：common / ingest_gate / condition_annotations / build_*（5）/ finalize_backend / backend_reader / test_ab_isolation / bench_backend / generate_checksums |
| 供 E5 的枚举清单 | `src/data_build/_build_state/enumerate_A.json` |
| 收口 | 本报告（`report.md`）+ `progress.md` 终态 |

---

## 监工验收意见（2026-08-06，实跑复核）

**结论：A 侧交付通过，质量为目前三条线最高；但查出一个跨线问题（不是你的过失），须由监工协调 E2 处置。**

全部结论均实跑验证：`python3 src/data_build/test_ab_isolation.py` 退出码 0；数据统计直接读 jsonl 聚合。

### 通过项（实跑证据）

| 项 | 实测 |
|---|---|
| A 侧规模 | `search_candidates_A` / `get_entity_A` 各 **80,712 行**（molecule 74,941 + crystal **5,771**）；`get_property_A` 27,618 行；`literature_search_A` 321 行 |
| 数据真实性 | 抽样首行含真实 InChIKey（`IUBSYMUCCVWXPE-…`）、真实 SMILES（`COCCc1ccc(OCC(O)CNC(C)C)cc1`）、真实 DOI（`10.1002/047002142x`）；**`provenance` 覆盖 27,618/27,618 = 100%**（含 `TDC_ADMET:BBB_Martins` + Harvard Dataverse URL） |
| A/B 物理隔离 | 三断言实跑全过：①键集交集 0 ②句柄追踪无非法 `_B` 句柄（`check_molecule_exists_B` 为契约 §6.5 白名单例外，符合"唯一允许触 B 的通道"）③`check_molecule_exists` 抽样 A 内 50 + 库外 3 **全返 false 且为纯 bool** |
| 存在性接口语义 | 与 `goal.md` §2b ⑤ "A 内与库外一律返不存在、只返单 bool" **逐条吻合** |
| enumerate_A | 落盘可用，80,712 id 枚举耗时 7.5 ms —— 为 open 类质量门的 A 全域穷举提供了可执行基础 |

### 特别记功：沙箱负向自检

B 侧当前为空（0 行），这会让"A∩B=0"变成恒真断言。**你主动识别并处理了这个问题**——在临时沙箱注入与 A 键重叠的 B 记录，实跑输出"注入重叠键 aa → 被抓 ✓"，证明断言在 B 非空后真实生效。

**对比价值**：E3 的 `test_a_source_exhaustive_isolation` 恰恰栽在这一点上（B 用合成串与 A 天然不同命名空间，断言恒真、零信息量，且未自觉）。同一个陷阱，你做了负向自检，它没有。

### B 侧为空：是正确的依赖声明，不计为缺陷

report §5 第 5 条已写明"四个 B 侧文件待 E5 造题冻结后由 E1 过门入库（每 case ≥1，**不得编造**）"，§7 交接项也给了完整流程（E5 落盘 → E1 过门 → 刷新 CHECKSUMS）。**B 侧装的是 open case 的 gt，而 open case 尚未存在，不编造是对的。**

---

## 🔴 跨线问题（非 E1 过失）：实体数达标，但多性质交集不足以支撑药物侧配额

**这是所有线此前都没发现的问题，由监工在本次验收中实测发现。**

### 事实

| 判据 | 数值 | 按 E2 §4.8 的结论 |
|---|---|---|
| 药物实体数 | **74,941** ≥ 10⁴ | ✅ 达标 |
| 材料实体数 | **5,771** = 契约值 | ✅ 达标 |

**但性质覆盖没跟上**（实测 `get_property_A` 聚合）：

| 药物性质 | 有值的分子数 |
|---|---|
| logS | 2,311 |
| bbb | 757 |
| logD | 604 |
| half_life | 523 |
| caco2 | **235** |
| herg | **104** |

**多性质交集（类型1/2 题的真实基数）**：

| 组合 | 同时有值的分子数 |
|---|---|
| bbb + herg | **50** |
| bbb + herg + caco2 | **14** |
| bbb + herg + caco2 + logS | **10** |
| 按现有 case 的约束 `bbb==1 AND herg==0` | **19** |
| 再加 caco2 有值 | **7** |

### 为什么这是问题

E2 冻结的配额是**类型1 药物 30 + 类型2 药物 30 = 60 道**，每道需 `|G|=1` 或多约束交集。**三性质交集只有 14 个分子，不足以支撑 60 道互不雷同的题**——会重演我此前批评 E2 的"材料 17 晶体出 60 题"，只是瓶颈从材料侧转移到了药物侧。

**判据失效的根源**：E2 §4.8 的下限判据看的是**实体数**，而真正的约束是**多性质交集数**。E1 交付 74,941 个分子在实体数上超额 7 倍，但可造题容量反而是全链路最紧的一环。

### 材料侧反而充足（且优于预期）

5,771 个晶体 100% 有 tc，另有 `family` 字段提供 **9 个族**的分层维度（Other 3,536 / Cuprate 874 / Ferrite 517 / Heavy_fermion 409 / Oxide 320 …）——**可按族分组出题，缓解了"tc 单目标导致排序答案雷同"的问题**。E1 建的 `family` 也顺带让 E3 gateway 里那条"材料线有 family"的注释从错误变为正确。

### 处置（归监工，不占用 E1 工时）

1. **E2 的 §4.8 判据须改**：下限从"实体数 ≥10⁴"改为"**满足题目约束模式的多性质交集实体数 ≥ 该格题量 × k**"（k 为雷同容忍系数）；
2. **E1 的后续可选动作**（待监工与 owner 定优先级，不在本轮验收范围）：为 A 侧补齐 ADMET 多性质覆盖（当前 caco2 235 / herg 104 是瓶颈），或由 E2/E5 改用"单性质 + 结构约束"的题型以绕开交集稀疏；
3. **不因此推翻本轮 A 侧交付**——数据真实、隔离有效、provenance 100%，问题在配额判据而非数据质量。
