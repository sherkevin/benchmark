# bbb_census_v3 溯源记录

本目录存放 BBB（血脑屏障通透性）工具冲突普查的**既有实测产物**。这些产物不是本次重新测量出来的，是从 `archive/` 下搬过来的历史资产。本文件记录搬迁前后的路径、搬迁时间、以及逐项哈希校验结果，用途是让任何人事后都能确认：搬迁没有改动任何一个字节，且产物内部声称的哈希与实际文件一致。

搬迁时间：2026-09-04。执行人：Qoder agent，工单 `.scratch/step1-verification/issues/02-promote-census-artifacts.md`。

## 1. 为什么要搬

`archive/` 的语义是「已封存、不再动的历史材料」。但这批普查产物是在役的：Proposal Step 1 要构造多工具数据集，冲突集、候选池、真值判定全都要从这里取数，后续工单 06–11 还要往里追加新工具的测量结果。放在 `archive/` 下会造成两个具体问题——一是代码里的默认路径指向一个语义上「不该被读」的目录，二是新旧产物混在同一棵封存树里，无法区分哪些还在用。

真值源文件不在搬迁范围内，理由见 §5。

## 2. 路径变更

三个目录整体搬迁，目录内部结构不变：

| 原路径 | 新路径 |
| --- | --- |
| `archive/cases/_methodology/tool_conflict_census/` | `data/bbb_census_v3/tool_conflict_census/` |
| `archive/cases/_methodology/tool_conflict_candidate_pool/` | `data/bbb_census_v3/tool_conflict_candidate_pool/` |
| `archive/cases/_methodology/tool_conflict_discovery/` | `data/bbb_census_v3/tool_conflict_discovery/` |

第三个目录（`tool_conflict_discovery`）原本不在工单列举范围内，是执行时发现 `probe_tool_conflicts.py` 的默认输出目录指向它、且它记录了历史 orchestrator 环境（`tool_manifest.json`）而一并搬迁的。这是对工单范围的扩大，在此明示。

搬迁用的是普通 `mv`，不是 `git mv`。原因是 `archive/` 下 git 跟踪文件数为 0——`git ls-files archive/cases/_methodology/ | wc -l` 返回 `0`，整个 archive 重组本身尚未提交。这带来一个后果，见 §7。

另有一个本次新建的文件：`tool_conflict_candidate_pool/bbb_v1/generator.at_seal.py`，不是搬来的，来历见 §6。

## 3. 搬迁无损的证明

搬迁前对 `archive/cases/_methodology/` 下三个目录的每个文件算 sha256，生成清单；搬迁后在新路径重算，逐条比对。结果：

- 文件总数 **123**，搬迁后同为 123
- 缺失 **0**，多出 **0**，哈希变化 **0**

当前目录下共 **126** 个文件，多出的 3 个都不是搬来的：本文件、§6 说明的 `tool_conflict_candidate_pool/bbb_v1/generator.at_seal.py`、以及工单 04 新建的 `tool_registry.json`。

## 4. 产物内部哈希自校验

搬迁无损只说明「文件没变」，不说明「文件本来是对的」。所以另做一轮内部一致性校验，全部复用代码库自带的校验函数（`tool_conflict_census.validate_receipt`、`validate_conflict_set_seal`、`tool_conflict_candidate_pool.validate_pool_seal`、`tree_manifest`），而不是另写一套哈希逻辑——自己重写会引入「我的实现和它的实现不一致」这种伪失败，也证明不了生产代码本身能通过。

### 4.1 运行凭据（run receipt）

凭据是每次调用模型工具留下的证据记录：命令行、PID、进程启动 ticks、起止时间、输入哈希、stdout/stderr 文件及其哈希、以及凭据自身的自哈希。

| 普查轮次 | 凭据数 | 通过 | 失败 | schema |
| --- | --- | --- | --- | --- |
| `bbb_authoritative_v1` | 12 | 12 | 0 | `bbb-tool-conflict-census-v1` |
| `bbb_authoritative_v2` | 12 | 12 | 0 | `bbb-tool-conflict-census-v2` |
| `live_reverify_20260902` | 4 | 4 | 0 | `bbb-tool-conflict-census-v2` |
| **合计** | **28** | **28** | **0** | |

v1 与 v2 是两套不同的 schema，不是「v1 损坏了」。v2 比 v1 多三个字段：

- `invocation_nonce`——父进程为每一轮生成的随机数，要求 worker 在输出里回显
- `process_start_ticks`——进程启动时刻的内核 ticks
- `request_sha256`——含 nonce 的完整请求体哈希

这三个字段合起来才能证明「每个模型每一轮都是新起的进程」。v1 缺这三项，所以 v1 无法排除「结果是复用同一个进程、甚至是回放缓存」这种可能。**这正是 v2 取代 v1 的原因，而不是因为两者结果不同**——见 §4.2。

直接用 v2 的 `validate_receipt` 校验 v1 凭据会报 `Receipt has missing or unrecognized fields.`，这是 schema 不匹配的预期行为。v1 凭据改按 v1 自身的 16 字段集校验（字段集、自哈希、`output_sha256== stdout_sha256`、stdout/stderr 落盘文件哈希四项），12 条全通过。

### 4.2 预登记契约与冲突集封条

`contract_sha256` 是对 `preregistration.json` 里 `contract` 子对象的规范化 JSON 求 sha256。两轮都重算并与文件声称值比对：

| 轮次 | `contract_sha256` | 比对 |
| --- | --- | --- |
| v1 | `e806ad6476d8faf1fc302431a22baf81fb7acc20bcec2f34991c00a62bb7b74d` | 一致 |
| v2 | `be642e413cdc7db9c4da11666b6b2cf85678797a267400f35b8170f7becda4f4` | 一致 |

冲突集封条 `conflict_set_seal.json`：

- v2：`validate_conflict_set_seal` 完整通过
- v1：仅 `schema_version` 一项与校验器内置常量不符（校验器写死期望 v2），其余全部通过——`preregistration_sha256` 与本轮 `contract_sha256` 相等、`seal_self_sha256` 与重算自哈希相等、封条 8 项检查逐一手工核验通过

两轮封条都声明 `experimental_truth_in_projection: false`，即冲突集的划定过程没有用到实验真值。这是预登记里那条排序约束的落地证据：真值只允许在 `conflict_set_seal.json` 写盘之后才 join 进来。

**一个值得记下的发现**：v1 与 v2 的冲突集完全相同——

| 字段 | v1 | v2 |
| --- | --- | --- |
| `initial_conflict_set_sha256` | `7758ee6f568ca566d29d1866ba67c9e6fd7a7113fbdbe97f5b9255a8421e64b5` | 同 |
| `conflicts.json` sha256 | `5e55fa1ad28a43ae8dadc1de377796a1a8e9b2727a2150ce9e945804e3f43734` | 同 |
| 实体数 / 配对数 | 400 / 1282 | 同 |

`summary.json` 与 `verification.json` 两轮不同（v2 分别为 `f8053e92…`、`a3d7bb8f…`），这是两次独立运行的汇总元数据差异。冲突集本体逐字节相同，说明这批冲突在两次独立普查中可复现；v2 的价值在证据强度（§4.1 的三个字段），不在结论本身。

### 4.3 候选池封条

`tool_conflict_candidate_pool/bbb_v1/pool_seal.json` 逐项校验：

| 字段 | 结果 |
| --- | --- |
| `candidate_pool_file_sha256` | `cdb207a94690455e9ce163539c9c7a2220bd3daa13406dcc6c143f36198679f7` 一致 |
| `candidate_pool_semantic_sha256` | `1607137d04ffcd1d7a6f81acc1fda397aef33894a3a41d8378dbf82c200af3ab` 一致 |
| `source_tree_manifest.tree_sha256` | `498d594bb581300a64a89c67b81947cbe4099d35c88455a85aa8451786b3cbcd` 一致，47 个文件逐条哈希无差异 |
| `counts` | 与 `candidate_pool.json` 的 `summary` 相等 |
| `seal_self_sha256` | 与重算自哈希相等 |
| `experimental_truth_used_for_tiering` | `false` |
| `generator_sha256` | 见 §6 |

`source_tree_manifest` 用的是相对 `bbb_authoritative_v2/` 的路径（`tree_manifest()` 对 `path.relative_to(root)` 取值），所以整目录搬迁不影响它。这一项是从磁盘重算后与封条比对的，不是把封条自己的清单喂回去比——后者是循环校验，证明不了任何事。

候选池规模：400 个实体、502 个配对归属，分三档 A=10 / B=82 / C=308。

候选池 `source_binding` 声称的 `contract_sha256`、`initial_conflict_set_sha256`、`conflicts_file_sha256`、`source_tree_sha256` 四项，与 §4.2 实测的 v2 值逐一对上，构成一条完整的封条链：预登记契约 → 冲突集封条 → 候选池封条。

## 5. 留在 archive/ 下的真值源

以下文件**不搬**，仍按原路径引用：

| 文件 | sha256 | 字节数 |
| --- | --- | --- |
| `archive/dag_poc/data/sources/TDC_ADMET/bbb_martins.tsv` | `eb32676c7bf6e2bcd98b7625186bbb2ba4e4644c1d1468dfa18b6be665bd5e98` | 137,899 |
| `archive/dag_poc/data/backend/get_property/get_property_A.jsonl` | `7bc908ee5a89c65f933eac2111c5dde52fd3992b3a3147c326384bfee6296f54` | 17,253,482 |

`bbb_martins.tsv` 的 md5 为 `4c7ddf7260f9573476ba2d4ca877957f`，与 `verify_bbb_truth.py` 内部作为冻结溯源记录写死的那份 path+md5 一致。该处代码里的路径字符串是**产物内容的一部分**（会被写进输出 payload），不是文件系统读取路径，因此本次没有跟着改。

另一个真值相关文件不在 archive 下，位置不变：`src/data_build/_build_state/entity_registry.jsonl`，sha256 `074d3d27831d593fabbd2affee8ac00b5182a1dd35cf1863663cf2b6024dd7c0`，56,919,870 字节。

**关于「为什么不搬」需要更正一处说法。** 起初的判断是「预登记契约按原路径+哈希锁定，搬动即破约」。核验后发现这个理由站不住：预登记 `contract.source` 里记的是

```
property_path: /mnt/ai4sci_develop_fast/jingwu/benchmark/dag_poc/data/backend/get_property/get_property_A.jsonl
registry_path: /mnt/ai4sci_develop_fast/jingwu/benchmark/src/data_build/_build_state/entity_registry.jsonl
```

这是**原运行机的绝对路径**，本机不存在。所以契约对真值源的绑定只靠内容哈希，搬不搬都不破约——而两个 `*_sha256` 与本机文件实测值完全相等，说明内容确实是同一份。真正的不搬理由要弱得多：这些文件只被校验脚本按路径读取，没有资产化需求，动它只增加改动面。

同时 `eligible_input_order_sha256 = 87f7e9d41ed7b3873779531391f25e805b4c0eb01dbbfced27079bb4ab06c4de`（730 条合格唯一分子的入参顺序哈希），与 `live_reverify_bbb.py` 里写死的 `EXPECTED_INPUT_ORDER_SHA256` 一致。

## 6. generator_sha256 校验失败：起因、影响范围、恢复证据

`tool_conflict_candidate_pool.py verify` 目前会在一项上报错：

```
RuntimeError: Candidate-pool seal mismatch: generator_sha256
```

**起因是本次搬迁改路径造成的，不是产物损坏。** `validate_pool_seal` 里这一项写作 `sha256_file(generator_path)`，`generator_path` 默认取 `__file__`，也就是工作树里那份**活的**生成器文件。为了让代码指向新路径，本次改了该文件 3 行常量，文件哈希随之变化：

| | sha256 |
| --- | --- |
| 封条声称 | `f8413d3618f78c865d9fe69e36f64ccf0b0e982e658735d60280722f39b4eda3` |
| 工作树现状 | `d09e35ac50e1f5db89252c033650ed0b1f45b00a00ca10170e2d638886f91cbb` |

改动的全部内容（`git diff`，3 insertions / 2 deletions，只有路径常量）：

```diff
-SOURCE_DIR = REPO_ROOT / "cases/_methodology/tool_conflict_census" / SOURCE_CENSUS_ID
-OUTPUT_DIR = REPO_ROOT / "cases/_methodology/tool_conflict_candidate_pool" / POOL_ID
+CENSUS_ROOT = REPO_ROOT / "data/bbb_census_v3"
+SOURCE_DIR = CENSUS_ROOT / "tool_conflict_census" / SOURCE_CENSUS_ID
+OUTPUT_DIR = CENSUS_ROOT / "tool_conflict_candidate_pool" / POOL_ID
```

**影响范围已确认收敛到这一项。** 封条 8 个字段中另外 7 个全部通过，其中包括 `seal_self_sha256`——封条记录本身没有被篡改。

**恢复证据。** git HEAD 里那份生成器的哈希正是 `f8413d36…`，即封条声称值。已把它提取到 `tool_conflict_candidate_pool/bbb_v1/generator.at_seal.py`（与它封的产物同目录）并验证哈希相等。把该文件作为 `generator_path` 传入后：

```
validate_pool_seal(seal, pool, 从磁盘重算的 manifest, candidate_pool.json,
                   generator_path=tool_conflict_candidate_pool/bbb_v1/generator.at_seal.py)
→ PASS
```

也就是说，封条链在封条生成时刻的代码基础上是完整的。

**没有重新封条。** 重新算一遍 `pool_seal.json` 能让 verify 变绿，但那会毁掉封条唯一的价值——它就是用来发现「生成器被改过」的。留着这个失败，比抹掉它更符合封条的设计意图。

**这暴露了校验器的一个设计缺陷**：`generator_sha256` 比对的是可变的工作树文件，把「封条记录完好」和「生成器从未被编辑过」这两件事混为一谈。正确做法是比对一个钉住的 blob。**该修复已在工单 04 落地**：`validate_pool_seal` 的 `generator_path` 默认解析为产物同目录的 `generator.at_seal.py` 快照，不再回落到 `__file__`；`pool_seal.json` 未被重新生成（`git diff` 对该文件为空）。

## 7. 未满足的验收项：git 历史

工单 02 的验收项之一是「`git log --follow` 能追到搬迁前的历史」。**这一项当前不满足**，且不是靠改文件能满足的。

原因链条是：`archive/` 下 0 个 git 跟踪文件，整个 archive 重组尚未提交（工作树 4209 D / 3 M / 9 ??）。所以本次只能用普通 `mv`；`--follow` 要成立，必须先有一次提交把重命名落进历史。是否提交、以什么粒度提交，属于 owner 决策，本次没有提交任何东西。

顺带核出 4 个文件**从未进过 git**，搬迁前后都只存在于工作树：

- `tool_conflict_census/bbb_evidence_raw.json`
- `tool_conflict_census/bbb_全量测试矩阵_带可用性标注.tsv`
- `tool_conflict_census/truth_manual_verification.json`
- `tool_conflict_census/truth_unresolved_bbb.json`

这 4 个文件没有任何历史版本可回溯，一旦丢失无法从 git 恢复。（核这一项时用 `git -c core.quotepath=false ls-files`；不加该选项时 git 会把非 ASCII 文件名转义引号，与磁盘名比对会产出假失配。）

## 8. 代码侧同步改动

8 个文件的默认路径常量从 `cases/_methodology/...` 或 `archive/cases/_methodology/...` 改指 `data/bbb_census_v3/...`：

`tool_conflict_census.py`、`tool_conflict_candidate_pool.py`、`probe_tool_conflicts.py`、`verify_bbb_truth.py`、`verify_bbb_matrix.py`、`render_conflict_report.py`、`live_reverify_bbb.py`、`fetch_bbb_evidence.py`

测试基线变化：搬迁与改路径前 31 passed / 13 errors，之后 **44 passed / 1 skipped / 8 subtests passed**。那 13 个 error 的报错信息是 `Frozen source hash mismatch: conflicts.json`，容易误导——真正的触发条件是同一个 `if` 的另一半 `not path.is_file()`，即路径失效。在archive 位置重算三个文件哈希确认与冻结值一致后，改的是路径而没有动断言。

剩下的 1 个 skip 是唯一一个真正加载模型权重的集成测试，闸门为环境变量 `RUN_BBB_MODEL_INTEGRATION=1`，在工单 05 把 22 个工具运行时装好之前无法解封。

## 9. 后续工单会往这里写什么

- 工单 05 建工具运行时。历史普查的运行时根在 `/tmp`（`/tmp/admet-ai-runtime`、`/tmp/deeppurpose-runtime`、`/tmp/deeppurpose-models` 等），已被系统清空，**新运行时不得再放 `/tmp`**。
- 工单 06 跑 L3 三轮，新增凭据写入本目录。
- 工单 09 做真值 join，必须在冲突集封条已写盘的前提下进行（§4.2）。
- 工单 11 重做冲突普查，届时会产生 `bbb_census_v4` 级别的新封条，本目录转为历史对照。
