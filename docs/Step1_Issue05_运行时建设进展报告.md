# Step 1 Issue 05 运行时建设与资格验证进展报告

日期：2026-09-10

## 速读结论

Step 1 Issue 05 的运行时建设已经收口：清单中的 22 个逻辑工具均有可复核结论，能按官方路径稳定运行的工具已建成持久化隔离环境，不能运行的工具也已按实测原因登记，没有通过修改上游模型、猜测权重地址、舍入数值或发明聚合规则来制造“可用”结果。

关键事实如下：

- 运行时统一落在 `~/.local/share/benchmark-runtimes/<tool_id>/`，不再依赖重启后会消失的 `/tmp` 环境。
- BBB 分类器仍在注册表的 `tools` 中；非 BBB 数值工具单独放在 `qualification_tools` 中，二者没有混用。
- `qualification_tools` 当前有 13 条记录，其中 8 条 `ready` 并启用，5 条因授权、权重或推理契约问题禁用。
- 数值工具使用 `numeric-multi-endpoint-worker-v1`，保存模型原始数值，不编造标签、阈值或跨端点聚合。
- `ready` 数值工具必须经过 1 次 discovery 加 7 次 replay，共 8 个独立进程；只有所有探针逐值精确相等才通过。
- 2026-09-10 再从公开 CLI 对 PKSmart、OpenADMET、SILICOS-IT 和 KowWIN 做了真实 stdin/stdout 表面验收，四项均通过。
- 工具矩阵已同步到 42 个工具、45 个性质族、55 行 × 43 列；注册表映射状态不一致为 0。
- 730 分子正式双轮跑批尚未开始，唯一前置阻塞是正式输入仍未冻结。

## 1. 本轮要解决什么

Issue 05 的目标不是“把包装上”这么简单，而是让后续跑批拿到一组可追溯、可重复、不会混淆性质语义的工具入口。每个工具至少要回答四个问题：

1. 实际运行的是哪个版本、提交或发布工件？
2. 模型、源码、依赖和必要宿主库是否能用 SHA-256 重新核对？
3. 输入一组固定分子后，工具能否通过 stdin/stdout 契约返回与输入逐行对应的原始值？
4. 在多个独立进程中重复运行时，这些原始值是否稳定？

因此，本轮交付不是一个统一环境，而是每个工具一个隔离运行时、一个注册表记录、一个薄 worker 和一份逐轮验证证据。对于不满足这些条件的工具，交付物是明确的失败分类和证据，而不是空壳入口。

## 2. 从输入到资格结论的完整路径

一条数值工具的资格验证按以下路径执行：

```text
注册表记录
  → 解析独立运行时与 worker 命令
  → 给 worker 发送带 nonce 的 stdin JSON
  → worker 先核版本、依赖与资产哈希
  → 调官方推理入口得到原始数值
  → stdout 返回固定协议 JSON
  → 验证器检查调用绑定、端点、行身份、有限值与范围
  → 1 次 discovery + 7 次 replay
  → 原始值逐项精确比较
  → 写入 runtime_verification/<tool_id>.json
```

### 2.1 输入

验证器固定使用三条探针记录，每条只有 `entity_id` 和 `canonical_smiles`。调用还带一个 64 位小写十六进制 nonce，防止 worker 返回缓存结果或与别轮输出串线。

请求形态如下：

```json
{
  "invocation": {
    "round": "qualification",
    "nonce": "<64 位小写十六进制字符串>"
  },
  "rows": [
    {
      "entity_id": "probe_caffeine",
      "canonical_smiles": "CN1C=NC2=C1C(=O)N(C)C(=O)N2C"
    }
  ]
}
```

### 2.2 worker 做什么

worker 只负责三件事：

1. 校验请求结构、nonce、实体 ID 和 SMILES；
2. 在调用模型前核对已登记的版本和资产；
3. 调用官方推理入口并返回原始数值。

worker 不把回归值转成标签，不替上游补阈值，也不对变长输出擅自做 min、max 或均值。某个工具的官方输出如果无法装进固定端点协议，就判为契约不兼容，而不是改变输出含义。

### 2.3 验证器做什么

`src/probe_headless/verify_runtime.py` 会核对：

- worker 协议版本；
- invocation 是否原样绑定本轮 round 和 nonce；
- 模型、家族、端点元数据是否在各轮保持不变；
- 输出行数、顺序、`entity_id`、SMILES 和输入摘要是否与请求一致；
- `raw_values` 是否恰好包含注册端点；
- 数值是否有限并落在已声明范围内；
- worker 报出的资产摘要是否覆盖注册表要求的摘要；
- 八个独立进程的原始数值是否逐项精确一致。

资格验证调用采用 `record_once`：一轮失败只对应一次真实进程调用，不在背后静默重试。CLI 会先写完所有请求工具的报告，只要任一工具未通过，最终退出码就是 1。

### 2.4 输出

每个工具的资格报告保存在 `data/bbb_census_v3/runtime_verification/`，记录命令、版本锁、资产证据、端点证据、逐轮数值、失败原因和最终稳定性判断。后续跑批只接受注册表中启用且资格报告通过的工具。

## 3. 已完成的运行时与协议建设

### 3.1 注册表分层

`data/bbb_census_v3/tool_registry.json` 现在同时承载两类记录：

- `tools`：BBB 分类器，共 34 条，其中 31 条启用；
- `qualification_tools`：非 BBB 数值端点，共 13 条，其中 8 条启用。

这项分层避免把 logP、logS、清除率、分布容积、游离分数、TPSA、QED 和 SA score 等数值性质误当成 BBB 分类器参加冲突普查。

### 3.2 数值协议

新增的 `numeric-multi-endpoint-worker-v1` 支持一个工具返回一个或多个固定数值端点。端点必须声明：

- ID 与含义；
- `regression` 或 `descriptor`；
- 单位；
- `linear` 或 `log10` 尺度；
- 可核实的数值范围，未知边界保留为 `null`。

严格重放闸门比较未舍入的原始 JSON 数值。展示层可以格式化，但判定层不允许用舍入掩盖漂移。

### 3.3 资产与运行时约束

不同工具按其发布方式核验以下资产的适用子集：

- 官方 wheel、JAR 或源代码提交；
- 安装后的 Python `RECORD`；
- 模型权重；
- 推理源码；
- 依赖锁及各依赖 `RECORD`；
- 原生二进制、插件目录和共享库；
- 必须由宿主提供的动态库。

OpenADMET 还会在推理前检查导入模块确实位于当前隔离运行时内，并核验宿主 `libXrender.so.1.3.0` 的摘要，避免当前目录或系统包抢占导入。

## 4. 通过资格闸门的数值工具

当前 8 条启用的数值资格记录如下。

| 工具 | 端点 | 资格结论 | 主要版本锚点 |
| --- | --- | --- | --- |
| PKSmart 3.0.1 | human VDss、CL、fup | 8 进程逐值一致 | PyPI 3.0.1 + wheel/RECORD/模型/依赖锁 |
| SolTranNet 1.0.0 | aqueous solubility logS | 8 进程逐值一致 | PyPI 1.0.0 + 权重/依赖锁 |
| RDKit TPSA | TPSA | 8 进程逐值一致 | RDKit 2026.3.5 |
| RDKit QED | QED | 8 进程逐值一致 | RDKit 2026.3.5 |
| RDKit SA score | SA score | 8 进程逐值一致 | RDKit 2026.3.5 |
| SILICOS-IT Filter-it | LOGP、LOGS | 8 进程逐值一致 | 提交 `ad480780…` + 源码树/二进制/Open Babel |
| OpenADMET | lipophilicity LogD | 8 进程逐值一致 | 代码、模型和基座权重三组固定修订 |
| KowWIN v1.68 / EPI CLI 1.1.0 | logKow | 8 进程逐值一致 | 自包含 JAR SHA-256 |

### 4.1 PKSmart：消除并行归约漂移，不改模型

PKSmart 初次八轮验证出现最大 `8.881784197001252e-16` 的跨进程差异。根因是发布的随机森林权重把 `n_jobs=30` 固化在 pickle 中，多线程树归约顺序不固定。

worker 使用 `joblib.parallel_backend("sequential")` 固定官方 `pksmart.predict_pk_params` 的调度。这个处理没有改源码、模型权重、特征或算法，也没有舍入输出。修正后八个独立进程逐值精确一致。

PKSmart 每次运行会核对 7 个注册资产，包括实际保存的官方 wheel、安装 RECORD、依赖锁、推理源码和三项端点模型。

### 4.2 SILICOS-IT：从固定提交构建，按官方三位小数输出

SILICOS-IT 的原发行网页已经下架，但源码仓仍可取得，而且 LOGP/LOGS 系数直接写在 C++ 源码中，不依赖外部权重。运行时按提交 `ad480780…` 构建，没有修改上游源码。

官方二进制把构建日期和时间写进 banner，因此二进制摘要只能标识当前构建，不能作为跨机器可复现锚点。报告同时保存 247 文件源码树汇总、系数源码、Open Babel 库和插件目录摘要，以源码树作为主要复现锚点。

官方 tabulate 输出固定为三位小数；worker 保存的就是这个官方精度，不做额外扩展。LOGP 使用 `log10 ratio`，LOGS 使用 `log10(mol/L)`。

### 4.3 OpenADMET：只确认一条离线 LogD 端点

OpenADMET 当前只把 `lipophilicity_logD` 登记为 ready。该模型仓自带所需 foundation 权重，因此能离线运行。其余官方模型没有据此推定为可用：7 个 CheMeleon 模型会在推理期访问 Zenodo，而本机当时受到 403/504 限制；另 3 个非 CheMeleon 模型本轮未建、未测。

该工具的输出随 CPU 线程归约顺序有末位差异，所以 worker 固定 OMP、MKL、OpenBLAS 和 NumExpr 为单线程。资格承诺是“在登记线程配置内逐值一致”，不是不加条件的跨机器位级一致。

运行时依赖宿主 `libXrender.so.1.3.0`；这个宿主库已作为第 19 个资产登记并在导入模型前核验。

### 4.4 KowWIN：锁定可下载 JAR，而非假装有不可变版本 URL

KowWIN 并非只能通过 Windows 桌面程序使用。EPI Web Suite 的官方接口提供自包含 EPI CLI JAR，内含 KowWIN v1.68，可通过 stdin NDJSON 批量调用。

该下载 URL 不含版本号，会随部署变化，因此注册表新增 `artifact_sha256` 锁类型：它锁定当前 JAR 字节，但不声称下载位置不可变。worker 读取模型的 `estimatedValue`，不读取可能混入实验测量的 `selectedValue`。

KowWIN 对无机盐等适用域外输入仍可能返回数值；端点说明已把这一限制写明，下游不能把“有数”误解成“在适用域内”。

## 5. 2026-09-10 真实 CLI 表面验收

在完成八轮资格报告后，又通过公开 CLI：

```bash
python src/probe_headless/verify_runtime.py \
  pksmart_3_0_1 \
  openadmet_cdf3e9f7 \
  silicos_it_filterit_ad480780 \
  kowwin_1_68_epi_1_1_0 \
  --replays 1 \
  --output-dir <临时目录>
```

这次调用对每个工具启动 discovery 和 replay1 两个真实子进程，没有直接 import 内部验证函数。四项均返回 `PASS`。

| 工具 | caffeine | diazepam | sucrose 探针 |
| --- | --- | --- | --- |
| PKSmart | VDss 0.6294321080；CL 1.5285126260；fup 0.6317354167 | VDss 1.0095260010；CL 0.4052115088；fup 0.0455632065 | VDss 0.3670175490；CL 2.2360455537；fup 0.7667076869 |
| OpenADMET | LogD 0.0247988701 | LogD 3.2769289017 | LogD -5.3132004738 |
| SILICOS-IT | LOGP -0.499；LOGS -0.519 | LOGP 4.119；LOGS -4.791 | LOGP -1.837；LOGS 2.039 |
| KowWIN | logKow 0.15639997 | logKow 2.6996999 | logKow -1.0820992 |

临时报告确认四项均满足：

- `status: success`；
- 所有探针行成功；
- 两轮原始值精确一致；
- `numeric_stable_across_runs: true`；
- `value_spread: {}`；
- 资产与端点证据完整返回。

持久化资格报告仍保留完整 8 轮证据，四项同样都是所有探针成功、逐值一致、无漂移。

另做了两个接口负例：未知 tool ID 被 CLI 以退出码 1 拒绝；畸形 nonce 被 PKSmart worker 在资产校验和推理前以退出码 1 拒绝。后者当前会输出完整 Python traceback，信息准确但对最终使用者略显冗长，不影响协议判定。

## 6. 被明确挡下的工具

“完成”不等于“全部 ready”。以下工具保留在注册表中，但不会进入正式数值跑批或 BBB 普查。

| 工具 | 状态 | 实测原因 | 本轮没有做什么 |
| --- | --- | --- | --- |
| Rowan 3.1.14 | `license_gated` | 无授权 API key；服务端模型版本和权重不可锁 | 没建空壳 worker，没伪造远程预测值 |
| QupKake | `inference_contract_broken` | 数值随输入批组成和行序变化；输出是变长逐位点表，固定端点协议无法表达 | 没发明 min/max/均值摘要，没改随机种子 |
| SolPredictor | `inference_contract_broken` | 推理未调用 `model.eval()`，dropout 保持开启；`shuffle=True` 且输出无身份，数值会错配到分子 | 没补 `eval()`，没关 shuffle，没挑种子 |
| Uni-pKa | `no_local_inference_path` | 代码需要 10 个 checkpoint，但仓库没有；唯一官方权重入口返回 404 | 没自行训练替代作者权重 |
| XenoSite | `no_local_inference_path` | 公共包和仓库没有 ONNX 权重，权重 URL 被上游刻意留空 | 没猜测或爆破权重地址 |
| DeePred-BBB | `inference_contract_broken` | PaDEL 偶发空描述符，经 NaN 传播后静默变成 `[0.5, 0.5]`；冻结面板有标签翻转，730 分子中另有 2 条长期不返回 | 没补 NaN 处理，没私加超时参数 |
| DeepB3 | `no_local_inference_path` | 发布仓缺数据和可用模型入口，且官方测试路径需要先知道标签 | 没用历史残缺脚本拼装新入口 |
| SAI BBB Predictor v3 | `no_local_inference_path` | 权重结构与代码不匹配，12 个声明端点中只有 7 个 checkpoint 有对应头 | 没把不匹配权重强行加载 |

这些结论把“依赖装不上”“权重没发布”“需要授权”“输出结构不兼容”“官方推理本身不稳定”分开登记。后续若上游重新发布权重或明确新契约，可以针对对应原因复测，而不需要重新猜测当初为什么被禁用。

## 7. BBB 工具与 B 组收口

B 组 12 个逻辑工具均已有实测结论：

- ready 6 个：B3clf、graphB3、LGBM-BBB、SILICOS-IT、OpenADMET、KowWIN；
- 有证据的阻塞 6 个：QupKake、SolPredictor、DeePred-BBB、Uni-pKa、XenoSite、DeepB3。

其中 B3clf 在注册表中展开为 24 个分类器与采样组合。BBB 分类工具继续使用标签跨轮稳定口径；概率漂移会记录 `flip_exposure`，但不会与数值工具的“原始值逐位一致”闸门混用。

## 8. 工具矩阵与审计同步

`data/tool_property_matrix/tools.json` 是矩阵事实源，`matrix.tsv` 由它确定性重建。本轮完成以下同步：

- PKSmart 从旧的“逐值闸门 FAIL”纠正为八轮逐值一致；
- OpenADMET、Uni-pKa、XenoSite、DeePred-BBB、KowWIN、SILICOS-IT、SolPredictor 的状态与实测事实对齐；
- KowWIN 名称纠正为 `KowWIN v1.68 / EPI CLI 1.1.0`；
- SILICOS-IT 名称纠正为实际运行的 `SILICOS-IT Filter-it`；
- OpenADMET 增加已实测的 lipophilicity LogD 覆盖；
- SILICOS-IT 除 LOGP 外增加同一官方入口的 LOGS；
- QupKake 分开记录冻结 SDF 字节摘要与解析值摘要，避免混为一谈。

当前审计结果：

- 42 个工具；
- 45 个性质族；
- 400 个已核实具体端点；
- `matrix.tsv` 为 55 行 × 43 列；
- 工具名和性质族无重复；
- 无全空性质行或全空工具列；
- 14 个可映射注册表列的状态不一致为 0。

覆盖面中的 33 个 survey 级工具列没有在本轮逐条重核，报告保留这一边界，不能把结构一致性误写成所有来源都达到运行时证据级别。

## 9. 代码与数据改动落点

本轮主要改动分为四组。

### 9.1 调度与验证

- `src/probe_headless/tool_conflict_census.py`
  - 增加 `record_once`，资格轮失败不再静默重试；
  - 扩展 `artifact_sha256` 版本锁，用于只能按字节固定的发布工件。
- `src/probe_headless/verify_runtime.py`
  - 加入数值多端点协议验证；
  - 固定 1 次 discovery + 可配置 replay；
  - 数值工具按原始值精确比较；
  - 任一报告未通过时 CLI 最终退出非零。

### 9.2 worker

新增或完善：

- `pksmart_worker.py`；
- `soltrannet_worker.py`；
- `rdkit_property_worker.py`；
- `silicos_it_worker.py`；
- `openadmet_worker.py`；
- `kowwin_worker.py`。

每个 worker 都通过 stdin/stdout JSON 工作，并把模型调用与资产核验放在同一个真实进程中。

### 9.3 注册与证据

- `data/bbb_census_v3/tool_registry.json`；
- `data/bbb_census_v3/runtime_verification/*.json`。

资格报告既包含 ready 工具的逐轮数值，也包含 blocked 工具的失败证据，避免注册表只有一个不可追溯的状态字符串。

### 9.4 矩阵与审计

- `data/tool_property_matrix/tools.json`；
- `data/tool_property_matrix/matrix.tsv`；
- `data/tool_property_matrix/AUDIT.json`；
- `.scratch/step1-verification/issues/05-build-runtimes.md`。

## 10. 验证与回归现状

### 10.1 已观察到的运行时行为

四个代表性 ready 工具已经从公开 CLI 端到端运行，真实子进程完成资产核验、模型调用、协议输出和跨轮比较，全部通过。持久化八轮报告与本次两轮表面验收一致。

`git diff --check` 通过，未发现空白错误。

### 10.2 自动化测试证据

最近一次变更相关定向测试结果为：

```text
66 passed, 1 skipped
```

此前 `src/probe_headless/tests` 的更广运行记录为：

```text
100 passed, 1 skipped
```

另有两个历史回归用例依赖已不在仓库中的 `archive/dag_poc/data/backend/get_property/get_property_A.jsonl`，在本轮改动前后都失败，不属于本次运行时变更。

全仓 `python -m pytest -q` 最近一次尝试在测试收集阶段停于 vendored `reference/tau-bench/retail_tasks_test.py`：当前环境没有安装其独立的 `tau_bench` 包。因此不能声称全仓测试通过；这项阻塞与四个运行时的实际 CLI 验收分开记录。

## 11. 已知风险与解释边界

1. **严格重复性依赖冻结环境。** PKSmart 和 OpenADMET 已分别固定 joblib 调度和 CPU 线程数；换机器或去掉约束后不承诺位级一致。
2. **发布位置不一定不可变。** KowWIN 的 JAR 下载 URL 会随部署更新，复用时必须以已登记 SHA-256 为准。
3. **构建摘要与源码摘要含义不同。** SILICOS-IT 二进制含构建时间，跨机器复现应先核源码树和构建参数，而不是期待二进制摘要相同。
4. **有数不等于在适用域内。** KowWIN 会给无机盐返回数值；OpenADMET 当前只验证了一条 LogD 模型。
5. **失败状态可能随上游变化。** Uni-pKa、XenoSite 和 Rowan 的阻塞分别依赖权重发布和授权状态，未来复测必须重新观察外部状态。
6. **矩阵可信度分层。** 运行时实测列证据最强，剩余 survey 级列仍受原调研材料完整性限制。

## 12. 下一步与当前阻塞

运行时建设完成后，下一步是 730 分子的正式两轮预测 campaign。但现在还不能启动，因为以下输入尚未由 owner 明确并冻结：

1. 正式 730 分子文件的唯一路径；
2. ID 列名；
3. SMILES 列名；
4. 修正后的糖类 SMILES。

拿到这四项后，应先保存输入文件摘要和列级契约，再启动两轮独立跑批。两轮结果应按实体与端点逐项比较，任何失败、缺行或数值差异都必须保留原始回执，不能用重试或舍入覆盖。

在正式输入冻结前，本轮不运行 730 分子 campaign，也不把探针验证结果冒充正式预测结果。

## 13. 证据索引

- 逐工具执行记录：`.scratch/step1-verification/issues/05-build-runtimes.md`
- 工具注册表：`data/bbb_census_v3/tool_registry.json`
- 逐轮资格报告：`data/bbb_census_v3/runtime_verification/`
- 数值验证器：`src/probe_headless/verify_runtime.py`
- worker 入口：`src/probe_headless/model_workers/`
- 工具事实源：`data/tool_property_matrix/tools.json`
- 派生矩阵：`data/tool_property_matrix/matrix.tsv`
- 矩阵审计：`data/tool_property_matrix/AUDIT.json`

## 14. 当前结论

Issue 05 已完成它应完成的部分：可复现的工具已经变成有版本、有资产、有协议、有逐轮证据的运行时；不可复现或无法取得权重的工具已经变成有明确原因、不会误入跑批的登记项。剩余工作不再是继续安装工具，而是等待正式输入冻结，然后运行 730 分子双轮 campaign 并交付预测回执。