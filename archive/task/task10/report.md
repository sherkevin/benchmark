# task10 — 完成汇报

## 一、结论先行

1. **owner 的论证方式立住了**：数据源权威性不再靠自证，而是靠「被 N 家经同行评议的 benchmark 采用」这一可核查事实。**TDC 3＋1 家、PubChem 3 家、ChEMBL/ZINC/PDB 各 2 家**，全部 venue 经 DBLP 核准。其中最强一条：**TDC 本身就是 NeurIPS Datasets and Benchmarks track 论文**，与我方目标 track 同类型，数据源合理性已由该 track 同行评议完成背书。
2. **但发现一个此前未被识别的选型风险**：**SuperCon 零家 benchmark 使用**，而我方材料 case 的 17 个晶体目前锚在它上面。材料线整体背书强度远弱于药物线（材料线仅 Materials Project 一家，药物线 8 家）。
3. **顺带修正既有报告 4 处错误**，其中「Zinc-250K 250 万分子」错 10 倍，若按错数执行扩容会白做工。

## 二、做了什么

| 动作 | 产出 |
|---|---|
| 从 `reference/` 14 个 benchmark 源码提取数据库使用证据 | 见下「证据在哪」 |
| 用 DBLP search API 逐条核准 venue（ACTION_RULES §4） | 6 个支柱 venue 核准，3 个待补核 |
| 重写 `benchmark数据库调研报告.md` §一 为权威性背书章 | §1.1 含 venue 总表 / §1.2 按库聚合的交叉采用表 / §1.3 材料线缺口 / §1.4 论文可用句 |
| 就地订正 4 处错误（保留原行，加「⚠️ 订正」标注，不删历史） | §2.1 / §2.4 / §2.8 / §三 / §四 |
| 同步 `权威数据库调研报告.md` 的背书强度分层 | §四 警示 + §5.1 拆「机构权威」与「benchmark 采用背书」两列 |

## 三、结论是什么

### 3.1 venue 核准结果（DBLP）

| 论文 | 采样说明原写 | DBLP 实际 | 结论 |
|---|---|---|---|
| TDC | NeurIPS 2021 D&B | NeurIPS Datasets and Benchmarks 2021 | ✅ 一致 |
| PMO | NeurIPS 2022 | NeurIPS 2022 | ✅ 一致 |
| Tartarus | NeurIPS 2023 D&B | NeurIPS 2023 | ✅ 一致（DBLP 未区分 D&B track） |
| GuacaMol | JCIM 2019 | J. Chem. Inf. Model. 59 (2019) | ✅ 一致 |
| ChemCrow | Nat. Mach. Intell. 2024 | Nat. Mac. Intell. 6 (2024) | ✅ 一致 |
| **DiscoveryBench** | 仅写 arXiv:2407.01725 | **ICLR 2025** | ⚠️ **采样说明漏记 venue**，实为我方目标会议，是可加强的背书 |
| MatTools | arXiv:2505.10852 | CoRR 2025 | ✅ 确认仅预印本 |
| MOSES | Front. Pharmacol. 2020 | 仅 CoRR abs/1811.12823 | ⚠️ DBLP 不收该刊，待期刊页复核 |
| SciCode | NeurIPS 2024 D&B | 检索未命中 | ⚠️ 待复核 |
| PoseBench | Nat. Mach. Intell. 2024/25 | 无记录 | ⚠️ 待复核 |

### 3.2 修正的 4 处错误

| # | 位置 | 原写 | 订正为 | 影响 |
|---|---|---|---|---|
| ① | §2.1 及 §三/§四 共 4 处 | Zinc-250K「250 万真实分子」 | **约 25 万**（chemical_vae 249,456 条） | **错 10 倍**；若写进论文会被抓，按错数扩容会白做工 |
| ② | §2.1 | Zinc-250K 是「ZINC Clean Leads 子集」 | Clean Leads 是 **MOSES 的基底**；Zinc-250K 出自 chemical_vae(2018)，非 TOMG-Bench 自建 | 归属错误 |
| ③ | §2.4 / §4.2 | BindingDB「下载数据集替换 mock」 | Biomni 实际用 `DTI.model_pretrained(model=..._BindingDB)`，是**预训练模型**非原始记录 | 判分性质不同：预测器 ≠ 真值，作 oracle 须在论文声明 |
| ④ | §2.8 | SuperCon 置于「14 个 benchmark 使用的库」下 | **14 家无一使用**，属我方选型意愿非调研发现 | 分类错误，引用时不得声称他人也用 |

已核实无误、未改动的：MOSES 4,591,276 分子（逐字核实）、TDC 23 oracle、Materials Project 需免费注册。

### 3.3 一个方法论副产品

经源码核验，**这些 benchmark 判分时全部不联网**：GuacaMol 把目标分子 SMILES 硬编码进 `benchmark_suites.py` L64-L77，SciAgentArena 冻结为 `*_ground_truth.csv`，MatTools 冻结为 `case_properties.json`。**说明我方本地快照是领域标准做法，不是妥协——论文中应改称 frozen snapshot，不称 mock。**

## 四、证据在哪

| 结论 | 证据路径 |
|---|---|
| GuacaMol 用 ChEMBL 作分布基线 | `reference/GuacaMol/src/distribution_learning_benchmark.py` L206-L212 |
| GuacaMol 目标分子硬编码于源码 | `reference/GuacaMol/src/benchmark_suites.py` L64-L77 |
| Tartarus 受体结构为本地冻结资产 | `reference/Tartarus/oracles/docking.py` L56 |
| MADE 用 Materials Project 且需 API key、数据 RAW UNFILTERED | `reference/MADE/configs/dataset_phase_diagram.yaml`、`README_REPO.md` L33 |
| ChemCrow 实时调 PubChem REST | `reference/ChemCrow/tool_converters.py` L26 |
| Biomni 用 BindingDB 预训练 DTI 模型 | `reference/Biomni/tool_pharmacology_sample.py` L385 |
| MOSES 459 万分子 | `reference/MOSES/REPO_README.md` L25 |
| TDC oracle 分 A 类公式 / B 类 ML 预测器 | `reference/TDC_PMO/README_采样说明.md` |
| SuperCon 零家 benchmark 使用 | 对 `reference/` 全仓 `grep -ri supercon`，命中项全在 `reference/NobelClaw/` |
| venue 核准 | DBLP search API `https://dblp.org/search/publ/api`，核准于本 task 执行时 |

## 五、遗留问题

| # | 问题 | 建议处置 |
|---|---|---|
| 1 | SciCode / PoseBench / MOSES 三个 venue 未经 DBLP 确认（DBLP 不完整收录非 CS 刊） | 从官方 proceedings 或期刊页补核；报告中已显式标注「待复核」，未冒充已核准 |
| 2 | **材料 case 仍锚 SuperCon**，无 benchmark 背书 | 新开 todo：把 formation_energy / bandgap / 凸包稳定性改锚 Materials Project；Tc 保留 SuperCon 但 provenance 双标 |
| 3 | `各benchmark造题法详解.md` §1② 称「mock 库 20 个匿名分子」，实际 `mock_db.json` admet 已有 **40 条**（aa–ah, ba–bh, ca–cd, da–dj, ea–ej） | 该文「fallback 返回全库（20 条）」的结论随之失效，可能影响对命中率 4/15 的归因；建议并入 T-43 重跑时一并校正 |
| 4 | 工作区有 3 个 trace 文件未提交（`qodercli_*_trace.jsonl`），非本 task 改动 | ACTION_RULES §1.5 要求变更前先 commit；本 task 未代为提交，需 owner 确认这些 trace 的去留 |
| 5 | 本 task 未修改 `dag_poc/` 任何 case 的 provenance 字段 | 属 T-44 后续工程动作，未在本 task 范围内 |
