# 第三批 JPlogP / XLogP3 / SSL-GCN 独立验收裁定

> 日期：2026-09-13。执行人：本项目负责人（远程与本地两线已合并归一）。
> 范围：交接文档 §3.5 与 P4 点名、未纳入首批十工具验收的第三批三个工具。
> 本裁定**不删除任何旧现场**，**不回填、不改变首批 8/38 与 2/12 的统计**。

## 速读结论

| 工具 | campaign | verify | 终态 | 是否进根 manifest |
| --- | --- | --- | --- | --- |
| JPlogP | `jplogp_cdk_formal_20260911`（v1） | exit 0，但 `full-r1` = `failed_protocol`（730 全 `tool_error`） | **闭账**，非事实源 | 否 |
| JPlogP | `jplogp_cdk_formal_v2_20260911`（v2） | exit 0，smoke 3/3、两轮各 729 ok + 1 `tool_error`、逐值一致 | **验收通过，事实源** | **是**（第 9 条 formal_delivery） |
| XLogP3 | `xlogp3_pug_formal_20260911`（v1） | **exit 1**（`full-r2` 退避违约） | **闭账**，非事实源 | 否 |
| XLogP3 | `xlogp3_pug_formal_v2_20260911`（v2） | exit 0，但 smoke = `failed_protocol`（PubChem 503 黑名单），无全量轮 | **记为未取得结果**（口径 15） | 否 |
| SSL-GCN | `.bench_runs/ssl_gcn_20260911/attempt-001` | 无 campaign，远程已出 `adjudication.json` | **blocked_not_runnable，不产出预测值** | 否 |

三个工具中只有 JPlogP v2 产出可交付的双轮全量结果，已独立验签并收进结果库。XLogP3 两个 campaign 均不可用。SSL-GCN 从未形成 campaign，远程裁定为不可运行。

## 一、为什么有 v2，哪个是事实源

任务清单要求「对 `formal_20260911` 与 `formal_v2_20260911` 的关系做书面裁定，说明为什么有 v2」。两个工具各有一个 v1→v2，**起因完全不同**，逐工具说明。

### JPlogP：v1 是 worker 构建缺陷，v2 重建 worker 修正

v1 的 smoke 通过（3/3 ok），但 `full-r1` 整轮 `failed_protocol`，730 个分子全记 `tool_error`。根因写在 v1 的 receipt 里，不是空 stderr 那种悬案：

```
.bench_runs/drug_prediction/jplogp_cdk_formal_20260911/runs/full-r1/receipt.json
  "returncode": 0,
  "protocol_error": "ValueError: Native JPlogP row count mismatch: expected 730, got 731.",
  "status_counts": { "tool_error": 730 }
```

v1 worker 对 730 行输入吐出了 **731 行数据**（`native.stdout.tsv` 实测 732 行 = 1 行 META + 731 行数据），多出一行触发行数合同失败，整轮判废。v2 把 worker 重建为 `jplogp_cdk_2_13_worker_v2`（见 v2 `runs/full-r1/invocation.json` 的 `command` 路径），同输入吐出 730 行数据（731 行 = META + 730），合同通过。

仓库里的 `src/probe_headless/model_workers/JPlogPBatch.java` 是 **v2 的修正版**：`main()` 对每一行输入恰好调用一次 `emit()`（成功或 `tool_error` 各一次），因此输出行数恒等于输入行数，不可能再多出一行。这与 v2 的 stdout 行数一致，与 v1 的 731 行不一致——v1 用的是更早的、有 off-by-one 的 worker 构建。

**裁定：JPlogP 事实源是 `jplogp_cdk_formal_v2_20260911`。v1 因 worker 构建缺陷闭账，现场保留不删。**

### XLogP3：v1 是退避完整性违约，v2 是 PubChem 端黑名单

XLogP3 走 PubChem PUG REST 接口（`POST .../property/XLogP/JSON`），两个 campaign 的失败性质不同：

**v1 `full-r1` 干净**（730 ok，730 次请求），但 **`full-r2` 崩溃**：

```
.bench_runs/drug_prediction/xlogp3_pug_formal_20260911/runs/full-r2/receipt.json
  "status_counts": { "ok": 300, "tool_error": 429, "invalid_input": 1 },
  "http_attempts": 2080,          # r1 只有 730，r2 暴涨到 2080
  "seconds": 12170                # 3.4 小时
```

v1 的 `verify` **硬失败 exit 1**，停在退避完整性断言上：

```
RuntimeError: Retry-After was not observed:
  .../xlogp3_pug_formal_20260911/runs/full-r2/requests/000227/attempt-02
```

核对 `requests/000227`（分子 z007025）的两次 attempt：attempt-01 收到 PubChem `503` 且响应头带 `Retry-After: 30`、`X-Throttling-Control: Service status: Black (150%), too many requests per second or blacklisted`，finished 于 `05:29:01.04`；attempt-02 在 `05:29:02.06` 就发出——**仅隔约 1 秒，没有等满 30 秒**。`verify` 的断言（`xlogp3_pug_campaign.py:807-810`）正确地拦下了这次违约。也就是说 v1 的 runner 在被服务器要求退避 30 秒时仍按 1 秒重试，把 host 推进了 PubChem 黑名单，进而整轮崩溃。

**v2 的代码是对的**：smoke 三个分子各 4 次 attempt，间隔严格约 31 秒（遵守了 `Retry-After: 30`），但 PubChem 在 v2 的时间窗里对**全部 12 次请求**返回 `503 Service status: Black`（host 已被拉黑）：

```
smoke_caffeine  att1..4  st 503  RA 30  start 08:59:24 / 08:59:55 / 09:00:26 / 09:00:58
smoke_diazepam  att1..4  st 503  RA 30  start 09:01:29 / ...
smoke_sugar     att1..4  st 503  RA 30  start 09:03:35 / ...
→ runs/smoke/receipt.json: "status": "failed_protocol", "status_counts": {"tool_error": 3}
```

v2 smoke 因此 `failed_protocol`，没有进入任何全量轮，`delivery` 为 null。

值得记一笔：仓库当前的 `xlogp3_pug_campaign.py` 在 `:397-401` 与 `:500-501` 已实现 `cooldown_until = monotonic() + retry_after` 的退避，v2 的实测时间戳证明它生效。**v1 的退避违约是更早的 runner 代码留下的，当前代码不会再犯**；v1 的 `verify exit 1` 是审计闸正确工作的证据，不是脚本 bug。

**裁定：XLogP3 两个 campaign 都不是事实源。v1 退避违约（`verify exit 1`）闭账；v2 退避正确但撞上 PubChem 黑名单，按口径 15 记为「未取得结果」。两个现场都保留不删。**

## 二、逐工具验收判据核对

任务清单判据：「各有 smoke、双轮全量、差异报告与三件套，缺什么明确列缺口」。逐项核：

### JPlogP v2 —— 全部齐备，验收通过

| 判据项 | 证据 | 结论 |
| --- | --- | --- |
| smoke | `runs/smoke/receipt.json` status `succeeded`，3/3 ok | 齐 |
| 双轮全量 | `runs/full-r1`、`runs/full-r2` 各 730 终态，729 ok + 1 `tool_error` | 齐 |
| 差异报告 | `delivery/.../verification.json` 的 `comparison.jplogp`：pairs 730、`status_mismatches []`、`numeric_pairs 729`、`exact_numeric_pairs 729`、`max_absolute_difference 0.0` | 齐（两轮逐值一致） |
| 三件套 | predictions.csv / verification.json / manifest.json，sha256 与 delivery manifest 内登记逐一相符 | 齐 |
| 唯一失败分子 | z034476：`java.lang.IllegalStateException: JPLogP returned a non-finite value.`，两轮同因同分子 | 已记录，不阻塞 |
| 分子集合 | delivery 730 行 == 冻结分子池 730（集合相等，无缺无多） | 一致 |

独立验签命令与结果：

```
python src/probe_headless/jplogp_cdk_campaign.py verify \
  --campaign .bench_runs/drug_prediction/jplogp_cdk_formal_v2_20260911
→ exit 0；delivery rows 730
  predictions_sha256 c62300dbdbc66f4251c8586df9ab6866254df44f905a72307ef39055fac943fa
```

`predictions.csv`（10 列，75590 字节）、`verification.json`（1815 字节）、`manifest.json`（6375 字节）已**逐字节复制**进
`data/tool_results/formal_campaigns/jplogp_cdk_formal_v2_20260911/jplogp_cdk_2_13/`，并在根 `data/tool_results/manifest.json` 的 `formal_deliveries[]` 新增第 9 条记录（sha256 与字节数与磁盘文件复算相符）。这条记录是**独立追加**，首批 8 条 formal_delivery 与 8/38 端点统计一字未动。

### JPlogP v1 —— 闭账，缺口即「整轮无有效输出」

smoke 齐（3/3），但 `full-r1` 730 全 `tool_error`、无 `full-r2`、`delivery` null。缺口：无双轮全量、无差异报告、无三件套。原因见上节（worker off-by-one）。**保留现场，不删，不重跑**（ADR-0005：正式阶段一次性，重跑须新建 campaign，而 v2 已经是那个新 campaign）。

### XLogP3 v1 / v2 —— 均无交付

- v1：smoke 齐、`full-r1` 730 ok，但 `full-r2` 崩溃且 `verify exit 1`（退避违约）。缺口：第二轮不可信、无差异报告、无三件套。
- v2：smoke `failed_protocol`（PubChem 503 黑名单），无 `full-r1`/`full-r2`、无三件套。缺口：全部全量轮缺失。

两者均**不进根 manifest**。现场保留。

### SSL-GCN —— 无 campaign，远程已裁定不可运行

SSL-GCN 从未形成 smoke/全量 campaign，只有安装与裁定现场 `.bench_runs/ssl_gcn_20260911/attempt-001/`。远程 `adjudication.json` 给出 `status: blocked_not_runnable`、`release_decision: no_smoke_or_full_prediction_campaign`，四条阻断理由：

1. 文档指定的 data 与 model 两个 Google Drive 资产下载超时、零产物（`download_receipt.json` 两条 `returncode 1`，stderr 为 `drive.google.com ... ConnectTimeoutError`）。
2. README 锁定的旧依赖栈无法 import PyTorch：`libtorch_cpu.so: undefined symbol: iJIT_IsProfilingActive`（`import_probe.stderr.txt`）。
3. 冻结源码只暴露「在带标签 task CSV 与固定 test 索引上做评测」，**没有任意 SMILES 的推理入口**——要出数就得改官方程序，撞停机条件第 1 条。
4. 冻结源码内**无许可文件**，即便日后修通也不得发布预测值。

**裁定：SSL-GCN 不产出预测值，归缺陷/阻断记录。现场与裁定保留。**

代码与现场下落（交接文档 P4 问的「SSL-GCN 代码与现场下落」）：本仓**没有** SSL-GCN 的 campaign 脚本（不同于 JPlogP/XLogP3 各有 `*_campaign.py`），只有远程安装尝试与其产出的 `adjudication.json`。`LEDGER.tsv` 里它原是一行「安装线 待远程安装（已派发）」，现按裁定改为阻断记录。

## 三、与既定口径和停机条件的关系

- **口径 15（接口失败归属）**：XLogP3 的 PubChem 503「too many requests per second or blacklisted」属服务器侧频率限制，是传输问题；两个 campaign 的退避重试预算均已用尽，故记为「未取得结果」。这是**应用**既有口径，不是修改口径，不触发停机条件第 4 条。
- **ADR-0005（正式阶段一次性，不重试）**：JPlogP v1、XLogP3 v1 的失败现场一律保留，不在原 campaign 内补跑。任何重跑都须新建 campaign。
- **停机条件第 1 条（改官方程序才能出数）**：SSL-GCN 因「无任意 SMILES 推理入口」命中此条，远程已据此判 `blocked_not_runnable`，本裁定确认，不试图改官方源码。
- **停机条件第 3 条（追加预算）**：见下节 XLogP3 待决项——是否再花预算新建一个 XLogP3 campaign 归 owner，本裁定不自行启动。
- **不回填首批统计**：根 manifest 只**追加**第 9 条 formal_delivery；`formal_deliveries` 计数 8→9，首批 8 条记录、8/38 端点数、2/12 失败闭账数全部未动。

## 四、XLogP3 待 owner 决的一件事

XLogP3 已按口径 15 记为「未取得结果」，这是 criterion-faithful 的默认处置，本裁定据此闭账。但存在一个**可选项需 owner 拍板**，本裁定不自行执行：

- 本机此刻探测 PubChem 可达（轻量探针 HTTP 200，咖啡因 XLogP −0.1，2.2 秒返回）。
- 若要拿到 XLogP3 的双轮全量，须**新建一个 campaign**（ADR-0005 禁止在 v1/v2 原地重跑），并面对一个已知风险：v1 的 `full-r2` 证明 PubChem 在 `minimum_interval_seconds = 0.25`（约 4 req/s）的持续负载下会把 host 拉黑（`Service status: Black`）。再跑 730×2 次请求很可能再次触发黑名单，除非把请求间隔显著调大、或改用 PubChem 的批量/分页端点。
- 调大间隔或改端点形态属于改 campaign 合同参数，且要再花接口预算，**归 owner 决定是否追加**（停机条件第 3 条精神）。
- 另一面：logP 这一性质族在矩阵里已有多个来源（ALOGPS、ADMET-AI、FPADMET、KowWIN 等），XLogP3 的边际价值有限。

**默认处置：维持「未取得结果」闭账。是否新建 XLogP3 campaign 重跑，等 owner 裁定，本裁定不启动。**

## 五、本裁定改动了哪些登记

只动第三批三个工具自己的状态格，**不动任何首批数字、不动矩阵维度与归属计数**（仍为 55 行 × 50 列、227 个工具—性质族归属）：

1. 根 `data/tool_results/manifest.json`：`formal_deliveries[]` 追加 JPlogP v2 一条（第 9 条）。
2. 新增 `data/tool_results/formal_campaigns/jplogp_cdk_formal_v2_20260911/jplogp_cdk_2_13/` 三件套（逐字节复制自 `.bench_runs`）。
3. `data/tool_property_matrix/matrix.tsv` 第 8 行「实测状态」的 JPlogP / XLogP3 / SSL-GCN 三格，改为验收后的真实状态。
4. `data/tool_property_matrix/LEDGER.tsv` 的 JPlogP / XLogP3 / SSL-GCN 三行「远程状态」与「依据」改为验收后的真实状态。
5. 第三批的 5 个代码文件（`jplogp_cdk_campaign.py`、`xlogp3_pug_campaign.py`、`model_workers/JPlogPBatch.java`、两个测试）经验证（22 项测试全过）后入库，与首批 24 件交付物分开提交。

矩阵的三个 family 归属格（JPlogP→亲脂性 logP、XLogP3→亲脂性 logP、SSL-GCN→核受体/应激通路 Tox21）**不改**：那是设计层的性质归属，与是否已交付无关，改了会动 227 这个归属计数。owner Step 1/3 在飞的 QupKake/DeePred-BBB/SolPredictor 覆盖计数三格也**不碰**（AUDIT 已记，归 owner 收口）。

## 六、复现实验签的命令

```bash
# JPlogP（v1 闭账 / v2 事实源）
python src/probe_headless/jplogp_cdk_campaign.py verify \
  --campaign .bench_runs/drug_prediction/jplogp_cdk_formal_20260911      # exit 0, full-r1 failed_protocol
python src/probe_headless/jplogp_cdk_campaign.py verify \
  --campaign .bench_runs/drug_prediction/jplogp_cdk_formal_v2_20260911    # exit 0, delivery 730

# XLogP3（v1 退避违约 / v2 PubChem 黑名单）
python src/probe_headless/xlogp3_pug_campaign.py verify \
  --campaign .bench_runs/drug_prediction/xlogp3_pug_formal_20260911       # exit 1, Retry-After not observed
python src/probe_headless/xlogp3_pug_campaign.py verify \
  --campaign .bench_runs/drug_prediction/xlogp3_pug_formal_v2_20260911    # exit 0, smoke failed_protocol

# 第三批代码测试
python -m pytest -q \
  src/probe_headless/tests/test_jplogp_cdk_campaign.py \
  src/probe_headless/tests/test_xlogp3_pug_campaign.py                    # 22 passed

# SSL-GCN 裁定现场
cat .bench_runs/ssl_gcn_20260911/attempt-001/adjudication.json            # blocked_not_runnable
```

## 相关

- 交接文档 §3.5、§4 第 9 条、P4：第三批独立接手的要求出处。
- [../docs/adr/0005-endpoint-level-smoke.md](../docs/adr/0005-endpoint-level-smoke.md)：正式阶段一次性不重试。
- [../docs/计划_全工具远程跑批与结果库.md](../docs/计划_全工具远程跑批与结果库.md) 口径 15：接口失败归属与退避预算。
- [../docs/任务清单.md](../docs/任务清单.md)「第三批工具：单独接手」：本裁定对应的清单条目。
