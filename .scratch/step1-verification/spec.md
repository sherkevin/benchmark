# Spec: Step 1 逐工具核验与数据集构建

Status: approved（grilling 15 问全部裁定，2026-09-03）

## 一句话

给 22 个本地可部署工具各跑一遍 730 个分子，产出「分子 × 工具 → 预测值 + 状态 + 失败原因」一张宽表；这张表既是工具功能验证的证据，也是 Proposal Step 1 要的数据集本体。

## 一个具体例子贯穿全文

拿分子 `z000282`（SMILES `C[N+](C)(C)CCO`，胆碱）走一遍完整链路，读者看完这一条就知道整套流程在干什么：

1. **输入**：它是 730 个分子池里的一条，`input_order_sha256` 锁死它在第几行
2. **调用**：`invoke_worker` 起一个子进程，命令行是固定的 `~/.local/share/benchmark-runtimes/admet_ai_2_0_1/bin/python .../admet_ai_bbb_worker.py`，stdin 灌进 `{"invocation":{"round":"discovery","nonce":"..."},"rows":[{"entity_id":"z000282","canonical_smiles":"C[N+](C)(C)CCO"}]}`
3. **输出**：worker 回 `{"entity_id":"z000282","raw_probability":0.375...,"status":"success"}` —— 只有原始概率，不做判定
4. **落表**：宽表该行写三列 `discovery__admet_ai_2_0_1__value = 0.375`、`__status = success`、`__failure_reason` 留空；label 由上层按 `probability >= 0.5` 算出 = 0（判不透过）
5. **留证**：`run_receipts/discovery--admet_ai_2_0_1.receipt.json` 记下完整命令行、起止时间、`input_sha256`、`stdout_sha256`、`receipt_self_sha256`
6. **复现**：replay1 / replay2 用同一命令重跑，三轮 `value` 必须逐位一致，否则该工具 L3 不通过
7. **对照真值**：`bbb_martins.tsv` 里胆碱的实验标签接上来，算出这个工具在这个分子上对没对
8. **冲突判定**：胆碱在既有普查里是已知冲突分子——ADMET-AI 1.4.0 三轮都判 0，DeepPurpose CNN 三轮都判 1。新加的工具会把这个冲突从 2 个工具体系扩到更多

## 目标与非目标

**目标**

- 22 个 L3 工具各出 730 条预测值，三轮逐位一致
- 9 个 W 档 web 工具各出一次取值 + 完整证据链 + 隔日重取
- 一张宽表合并新旧数据，新工具与既有 4 个模型同口径可比
- 既有 census 产物从 `archive/` 提升到 `data/bbb_census_v3/`

**非目标**

- 不做 Step 3–4 的 agent 评测（那条链用 qoder CLI，见 ADR-0003 §6）
- 不复现各工具论文的性能指标（L4，本轮不做）
- 不重跑既有 4 个模型（4 轮漂移 0 已证，只做资产完整性校验）
- 不碰 `src/` 下的材料科学旧代码与 26 个 `ws_mcp_server_*.py`

## 判据

见 [ADR-0003](../../docs/adr/0003-step1-verification-protocol.md)。摘要：L3 = 装得上 + 版本可锁 + 全量出数 + 两遍逐位一致 + 权重 sha256；W = 单次取值 + 全量存证 + 隔日重取；V = 只登记不测。

## 分工

设计、执行、监控、复核全部由 Claude（本会话）承担；跑批在本机进行。工程师侧已有产出（`tools/step1_survey_evidence/`、`docs/step1_tool_inventory_v2.csv`、ADR-0001/0002）作为输入复用，不重做。

## 工具分组与本机可达性（2026-09-03 实测）

**A 组 · 本机 PyPI/Zenodo 直达（10 个，可立即开工）**
ADMET-AI 1.4.0、ADMET-AI 2.0.1、DeepPurpose v0.1.5、PKSmart 3.0.1、SolTranNet 1.0.0、TPSA(RDKit)、QED(RDKit)、SA score(RDKit)、SAI BBB Predictor v3.0.0、Rowan 3.1.14（需 API key，缺则降 V 档）

**B 组 · 卡 GitHub（12 个，需先配 deploy key）**
B3clf、QupKake、Uni-pKa、DeepB3、graphB3、DeePred-BBB、LGBM-BBB、OpenADMET、XenoSite、SILICOS-IT logP、SolPredictor、KowWIN(EPA 页 000)

**C 组 · W 档 web（9 个）**
ADMETlab 3.0、admetSAR 3.0、pkCSM、SwissADME、DeepB3 server、LogBB_Pred、BBBper、VEGA、PreADMET

**D 组 · V 档商业（8 个）**：只登记不测

## 已知风险

| 风险 | 证据 | 处置 |
| --- | --- | --- |
| DeepPurpose 钉旧 torch，arm64 可能无轮子 | 既有 census 用 torch 2.4.0+cu121，本机是 arm64+MPS 无 CUDA | 01 号 ticket 先验证；失败则该工具改由远端跑 |
| web 工具状态半天内翻转 | ADMETlab3 200→403、pkCSM 200→000、graphB3 200→000（同日两次测量） | W 档存原始响应体全文 + 隔日重取 |
| 真值 11 处自相矛盾 | `truth_unresolved_bbb.json`，其中 7 个在冲突集内 | 判分时按 entity_id 排除，不删数据 |
| 改造 harness 破坏既有可复现性 | 现有 4 轮漂移 0 依赖 `input_order_sha256=87f7e9d4…` 与固定 command | 改造前跑通三个既有测试；改造后用既有 receipt 回归验证 |

## Ticket 索引

见 `issues/`，01–12 顺序执行，依赖关系写在各 ticket 的 `Blocked by:` 行。
