# task13 — 修复 15 条 SMILES 错配（真值身份修复）

## 上游诉求

owner 已确认为下一子任务：修 task11 查出的 15 条 SMILES 错配。

## 对齐 GOAL

- **最终目标**（`goal.md` §1）：为材料+药物发现造 long-horizon agent benchmark，评过程而非只评终答。
- **本 task 服务于**：GT-first 第 1 步「选定分子并反查性质」的**真值可信度**。`goal.md` §4 可信度支柱 1 要求「答案锚定科学数据库的已知分子与性质」——`aa` 的 SMILES 指向错误分子，该支柱在这条记录上不成立。
- **红线关联**（`goal.md` §5）：真值错误会使「过程级 hard checkpoint」判在错基准上，等价于指标可信度受损。

## 范围已通过实测精确锁定

**只改 `molecule_search[].smiles` 一列，不动任何性质值。** 依据：抽取证据文档 `证据_admet_gt.md` §2.2/§2.3 载明的真值，与 mock_db 逐值比对 6 个分子共 24 项，结果全部一致（仅 1 项例外，见下）。**性质值没有串，错的只有 SMILES。**

| 校验分子 | 结果 |
|---|---|
| aa (Metoprolol) | herg/bbb/caco2 三项全 OK |
| ab (Theophylline) | caco2/bbb/herg/half_life 四项 OK，logD 为 null（属缺口非错误） |
| bd (Caffeine) | logS/logD/caco2/bbb/half_life 五项全 OK |
| ac (Imipramine) | 六项全 OK |
| bg (Warfarin) | logS/bbb 两项 OK |
| bh (Propranolol) | logS/half_life OK；**bbb 证据=0 而 mock=1 —— 非错误，见下** |

### bh.bbb 的差异是有意为之，不改

`证据_admet_gt.md` 脚注 † 载明：Martins BBBP 数据集里 propranolol 出现互相矛盾的两行（盐型 Y=1 / 原型 Y=0），是**已知标签噪声**；propranolol 实际能过血脑屏障。mock_db 取 1 是采用了正确生物学事实而非噪声标签，**正确，不改**。

但该脚注同时建议「出 BBB 题时避开 propranolol」——需登记为遗留风险（bh 不应出现在 BBB 约束型 case 中）。

## 子目标（做完即验收）

1. 从 **PubChem PUG REST**（已实测 200）按药名逐个拉取 `CanonicalSMILES` + `InChIKey` + `CID`，替换 `molecule_search[].smiles`。
2. **新增 `inchikey` 与 `pubchem_cid` 两个字段**，作为身份锚（比 SMILES 更稳定，SMILES 有多种等价写法）。这是落实指南 §4.1 规则 3「身份与性质分离取源」。
3. 消除 `aa`/`bh` 的 canonical SMILES 重复：`aa` 取 Metoprolol（CID 4171），`bh` 保持 Propranolol（CID 4946）。
4. **验收硬标准**：`python src/benchmark/verify_snapshot_identity.py --strict` **退出码为 0**（当前为 1，报 15 条不符 + 1 组重复）。
5. 不改 `admet` 任何性质值；不改 `crystals`/`structures`/`literature`/`targets`。

## 不做的（避免漂移）

- 不补 47 个空 ADMET 格子（属指南 §4.3 第 2 步，另立 task）。
- 不扩容分子池、不动材料线、不改 mock_server 的假实现。
- 不改 case/template 文件。

## 已知风险（需 owner 确认）

`aa` 是 CNS case 的 gold answer。修 SMILES **不改变判分逻辑**（判分走匿名 ID 与性质值，二者均不变），但若 agent 在解题中用 SMILES 检索，返回内容会变。因此需确认：**本 task 与 T-43（重跑 5 case × 3 模型）的先后顺序**——建议先修再跑，否则重跑基线仍建立在错身份上。

## 交付物

- `dag_poc/data/mock_db.json`（仅 `molecule_search` 段变更 + 新增两字段）
- `src/data_download/fetch_pubchem_identity.py`（拉取脚本，记录来源与抓取时间）
- `reference/NobelClaw/证据_admet_gt.md` 补 26 个扩充分子的身份出处（若 PubChem 可全部覆盖）
- `task/task13/report.md`
