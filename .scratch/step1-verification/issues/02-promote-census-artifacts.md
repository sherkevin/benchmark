# 02 · 提升既有 census 产物出 archive 并校验完整性

Type: task
Status: resolved
Blocked by: —

## 为什么

`archive/cases/_methodology/tool_conflict_census/` 与 `tool_conflict_candidate_pool/` 是本提案的实证地基（730 分子 × 4 模型 × 4 轮、漂移 0），但 archive 的规矩是"可参考、不纳入当前进度"。放在那里，下一个接手的人会当废弃资产（本会话就差点漏掉）。

## 做什么

1. `git mv` 两个目录到 `data/bbb_census_v3/`，保留 git 历史
2. 移动后**重算全部 sha256**：每份 receipt 的 `receipt_self_sha256`、`stdout_sha256`、`output_sha256`，以及 `preregistration.json` 的 `contract_sha256`、`conflict_set_seal.json` 的三个哈希
3. 逐项与文件内声称值比对，写一份 `data/bbb_census_v3/PROVENANCE.md` 记录：原路径、新路径、移动时间、全部哈希校验结果
4. 真值源文件（`archive/dag_poc/data/sources/TDC_ADMET/bbb_martins.tsv`、`get_property_A.jsonl`）**不移动**——它们被预登记契约按原路径+sha256 锁定，移动会破坏契约。在 PROVENANCE.md 里写清引用路径

## 验收

- 全部哈希重算一致（任何一项不一致必须停下来查，不得覆盖）
- `data/bbb_census_v3/PROVENANCE.md` 存在且记录完整
- `git log --follow` 能追到移动前历史

## 备注

移动前先 `git status` 确认工作区干净。当前仓库有 4218 个未提交变更（重组遗留），移动大目录前须确认不会与之冲突——必要时先就当前状态做一次固化提交。

## Answer

已完成，但有 1 项验收未满足、1 项校验仍失败，两者都不是产物问题，详见下文。完整记录落在 `data/bbb_census_v3/PROVENANCE.md`。

### 做了什么

三个目录搬到 `data/bbb_census_v3/`：`tool_conflict_census/`、`tool_conflict_candidate_pool/`、`tool_conflict_discovery/`。**第三个超出了工单列举范围**——执行时发现 `probe_tool_conflicts.py` 默认输出目录指向它、且它保存着历史 orchestrator 环境记录（`tool_manifest.json` 里的 python 3.10.14 / rdkit 2025.03.6 / numpy 1.26.4 / scikit_learn 1.7.2 与 admet_ai 权重逐文件哈希），留在 archive 会和另两个目录割裂，故一并搬迁。

搬迁无损：搬前搬后各算一遍 123 个文件的 sha256，**缺失 0、多出 0、哈希变化 0**。

8 个代码文件的默认路径常量同步改指新路径。测试基线从 31 passed / 13 errors 变为 **44 passed / 1 skipped / 8 subtests passed**。

### 哈希校验结果

全部复用代码库自带校验函数，不另写哈希逻辑。

- **运行凭据 28/28 通过**（v1 12 条 + v2 12 条 + live_reverify 4 条，失败 0）
- **`contract_sha256` 两轮都一致**：v1 `e806ad64…`、v2 `be642e41…`
- **冲突集封条**：v2 `validate_conflict_set_seal` 完整通过；v1 仅 `schema_version` 与校验器内置常量不符，8 项检查手工逐一核验通过
- **候选池封条 7/8 项通过**，含 `seal_self_sha256`（记录未被篡改）、`source_tree_manifest.tree_sha256`（47 文件从磁盘重算，无差异）
- 封条链闭合：预登记 `contract_sha256` → 冲突集 `initial_conflict_set_sha256` → 候选池 `source_binding` 四项，逐一对上

### 两个偏离工单的地方

**1. 用了 `mv` 而非 `git mv`。** `git ls-files archive/cases/_methodology/` 返回 0——archive 下没有任何 git 跟踪文件，整个重组本身未提交，`git mv` 无从执行。改用普通 `mv` + 123 文件哈希清单证明无损。

**2. 工单第 4 条给的「不移动真值源」的理由不成立，但结论仍然保留。** 工单说真值源「被预登记契约按原路径+sha256 锁定，移动会破坏契约」。核验后发现预登记记的是原运行机绝对路径 `/mnt/ai4sci_develop_fast/jingwu/benchmark/...`，本机根本不存在，契约对真值源的绑定**只靠内容哈希**，搬动不破约。两个 `*_sha256` 与本机文件实测值完全相等。真实的不搬理由弱得多：这些文件只被校验脚本按路径读，没有资产化需求。已据此更正 `verify_bbb_truth.py` 里我先前写下的同一处错误注释。

### 仍失败的一项：generator_sha256

`tool_conflict_candidate_pool.py verify` 报 `Candidate-pool seal mismatch: generator_sha256`。**这是我改路径造成的**：该项比对 `sha256_file(__file__)`，即工作树里活的生成器文件，我改了 3 行路径常量，哈希从封条声称的 `f8413d36…` 变成 `d09e35ac…`。

影响已收敛到这一项，另 7 项（含 `seal_self_sha256`）全通过。已把 git HEAD 里那份封条时刻的生成器提取到 `data/bbb_census_v3/sealed_code/tool_conflict_candidate_pool.at_seal.py`（哈希验证等于 `f8413d36…`），以它作 `generator_path` 传入后 `validate_pool_seal` **完整 PASS**。

**没有重新封条。** 重算 `pool_seal.json` 能让 verify 变绿，但封条的全部价值就是发现「生成器被改过」，抹掉这个失败等于毁掉它。校验器本身有设计缺陷——把「封条记录完好」和「生成器从未被编辑」混为一谈，应比对钉住的 blob（`validate_pool_seal` 已有 `generator_path` 形参）。该修复已追加到工单 04。

### 未满足的验收项：git log --follow

追不到搬迁前历史，且改文件无法满足。前提是 archive 重组本身尚未提交（工作树 4209 D / 9 M / 10 ??），`--follow` 要成立必须先有一次把重命名落进历史的提交。

**owner 已裁定（2026-09-04）：本次不提交，owner 自行选时机提交。** 该验收项状态为**挂起**，不是放弃——owner 提交后需补验 `git log --follow data/bbb_census_v3/...` 能否追到 `cases/_methodology/...` 时期的历史。本次未提交任何东西。

顺带核出 4 个文件从未进过 git，只存在于工作树，丢失无法恢复：`bbb_evidence_raw.json`、`bbb_全量测试矩阵_带可用性标注.tsv`、`truth_manual_verification.json`、`truth_unresolved_bbb.json`。

### 给工单 05 的前置警告

历史普查的模型运行时根在 `/tmp`（`/tmp/admet-ai-runtime`、`/tmp/deeppurpose-runtime`、`/tmp/deeppurpose-models`、`/tmp/deeppurpose-github`），已被系统清空。新运行时不得再放 `/tmp`。这也是当前那 1 个 skip 的解封条件（闸门 `RUN_BBB_MODEL_INTEGRATION=1`）。

## Comments
