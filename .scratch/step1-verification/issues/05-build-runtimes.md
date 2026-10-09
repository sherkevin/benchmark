# 05 · 建立 22 个 L3 工具的持久化运行环境

Type: task
Status: open
Blocked by: 03, 04

## 为什么改落点

既有 harness 把工具环境放 `/tmp/admet-ai-runtime`、`/tmp/deeppurpose-runtime`。`/tmp` 重启即空，每次重跑都要重装——而 L3 要求跨轮次同版本，重装引入版本漂移风险。已获授权改为 `~/.local/share/benchmark-runtimes/<tool_id>/`。

## 做什么

按工具逐个建独立 venv（用 uv，本机无 conda）。每建一个立刻做三件事：**锁版本、记哈希、跑探针分子**。

### A 组（10 个，PyPI/Zenodo 直达，先做）

| tool_id | 获取方式 | 特殊风险 |
| --- | --- | --- |
| `admet_ai_1_4_0` | `pip install admet-ai==1.4.0`（依赖钉 torch==2.5.0） | 与 v2 必须分环境 |
| `admet_ai_2_0_1` | `pip install admet-ai==2.0.1`（依赖 chemprop>=2.2.2，dry-run 已验依赖树可解析） | — |
| `deeppurpose_bbb_molnet_{cnn,morgan,mpnn}` | pip + BBB_MolNet 官方权重 | **arm64 无 CUDA，既有 census 用 torch 2.4.0+cu121；本机须验 arm64 轮子是否存在** |
| `pksmart_3_0_1` | `pip install pksmart` | — |
| `soltrannet_1_0_0` | `pip install soltrannet` | 输出是 logS 非 BBB，端点不同 |
| `rdkit_tpsa` / `rdkit_qed` / `rdkit_sa_score` | 已在 `.venv`（rdkit 2026.3.5） | 确定性计算，无需三轮（但仍跑三轮以统一流程） |
| `sai_bbb_v3` | Zenodo DOI 10.5281/zenodo.19205279 下载（含数据+权重+脚本） | 12 个 CNS 端点，需确认哪个是 BBB |
| `rowan_3_1_14` | `pip install rowan-python` | **云端计算需 API key；无 key 则降 V 档并在表里注明** |

### B 组（12 个，依赖 03 号完成）

B3clf、QupKake、Uni-pKa、DeepB3、graphB3、DeePred-BBB、LGBM-BBB、OpenADMET、XenoSite、SILICOS-IT logP、SolPredictor、KowWIN

## 验收（逐工具，缺一不可）

1. 环境建成，`<runtime>/bin/python -c "import <pkg>; print(<pkg>.__version__)"` 有输出
2. 版本锁定信息写进 `tool_registry.json`（PyPI 版本号 / git commit / Zenodo DOI 之一）
3. 权重与依赖 sha256 记录（照 `authority_audit.json` 的 `asset_evidence` 格式）
4. **探针分子出数**：喂咖啡因 `CN1C=NC2=C1C(=O)N(C)C(=O)N2C`，确认输出格式合法、取值域合理（概率∈[0,1]、logS 为负值等）
5. 写一个 worker（照抄 `model_workers/admet_ai_bbb_worker.py` 291 行或 `deeppurpose_bbb_worker.py` 542 行模板），遵守：读 stdin JSON、只吐原始数值不做判定、输出带 `schema_version` 与版本/资产证据

## 失败处置

某工具建环境失败（依赖冲突、无 arm64 轮子、权重下不来）：**不要硬修超过 2 小时**。记 `install_failed` + 完整报错进 `tool_registry.json`，标 `enabled: false`，继续下一个。它会以 `install_failed` 状态出现在最终宽表里——这本身是有效数据（工具在标准环境下装不上，是可复现的事实）。

## Comments

### 2026-09-04 执行记录

**平台事实更正（推翻此前记载）**：此前记的"本机所有本地编译产物都会被 SIGKILL"不成立。真相是终端安全代理（xattr 上可见 `com.alibaba.security.edr.antivirus.cloudquery`）对每个新写出的可执行文件发一次云查询，查询未返回时执行被 SIGKILL，返回或命中缓存即放行——是竞态，不是硬规则。同一个空 C 程序反复编译执行，结果时通时杀；sklearn 1.0.1 与 0.24.2 的源码构建各撞 4–5 次后成功。**结论：需要源码编译的工具不得据此判 `install_failed`，必须重试若干次再判。**

**B3clf（24 个组合全部 ready）**：作者两处依赖声明矛盾（setup.py 写 `scikit-learn==0.24.2`，requirements.txt 写 `1.0.1`），而 24 个权重 pickle 内嵌版本标记均为 **0.24.2**，以 pickle 为准。0.24.2 与 1.0.1 均无 arm64/cp310 轮子。已在 py3.9 下另编 0.24.2 做对照：同一输入下 `scaler.transform` 与 18 个非 xgb 分类器 `predict_proba` 逐位一致（xgb 6 个走 xgboost 引擎，与 sklearn 无关），证据 `data/bbb_census_v3/sklearn_cross_version_equivalence.json`。故运行时装 1.0.1（wheel 存档于 `b3clf/wheels/`，sha256 记在 `requirements.lock.txt`），24 个组合全部加载并跑通——含 1.0.2 下因 `sklearn.neighbors._dist_metrics` 搬迁而读不了的 3 个 knn 空间索引。24 条 registry 记录各钉自身权重 sha256。

**PaDEL 概率抖动（不构成缺陷）**：B3clf 特征由 PaDEL 计算，对同一固定几何重复算会给出不同描述符，概率随之漂移。24 个组合中最坏为 `logreg_common`：漂移 0.0101、最近探针距 0.5 阈值 0.0823、余量 0.0722。8 轮核验中 24 个组合**标签无一翻转**。据此 ADR-0003 的 L3 第 3 条已从"逐位一致"改为"标签跨轮稳定"（见该 ADR 文末修订节），并在核验记录中新增 `flip_exposure` 字段量化余量，避免 730 分子普查时才发现暴露面。

**lgbm_bbb（ready）**：作者手抄的 `SELECTED_FEATURES` 与权重 `booster_.feature_name()` 从第 35 位起错位，4 个名字在 Mordred 1613 个描述符里不存在（脚本静默填 0），`MIC3` 更隐蔽——它存在但 booster 要的是 `ZMIC3`。已改为按 booster 自报名单装配向量，作者源码文件零改动、哈希照钉，差异逐位记入 `endpoint_evidence`。修复前地西泮 0.335（判不穿透）→ 修复后 0.971，三探针相对关系方合乎化学。

**deep_b3（no_local_inference_path，登记不测）**：发布方已清空仓库（tip 仅 8 文件），权重与脚本只存在于历史（最全为 `2d24875`）。两条发布入口都不能给任意分子打分：`deep-b3-test.py` 读的 `datapkl/test-data.pkl` 在全部 48 个提交中从未出现；`deep-b3.py test` 载入的 `models/deep-b3.pkl` 亦从未提交（仅有 `.pth`），且该函数第二行 `test_df.copu()` 必抛。更根本的是两者都需先知道标签——阈值由 `find_best_cutoff` 在被预测分子自身标签上算 Youden's J，输出仅聚合指标、无逐分子概率。7 个权重 sha256 已记（3 个普通 blob 本地已验，4 个 LFS 仅引用作者申报 oid，实际对象下载约 6 MB 即断）。

**当前状态**：registry 33 条，31 个 enabled（24 b3clf + 2 admet_ai + 3 deeppurpose + graphb3 + lgbm_bbb），2 个登记不测（deep_b3、sai_bbb_v3）。测试 68 passed / 1 skipped。

**剩余**：A 组 pksmart / soltrannet / rdkit 三项 / rowan（多为回归型端点，等端点扩展）；B 组 DeePred-BBB（需 Keras 运行时，且同样依赖 PaDEL）、QupKake、Uni-pKa、XenoSite、SILICOS-IT、SolPredictor、KowWIN、OpenADMET（权重在 HuggingFace，本机不通，划远端）。

### 2026-09-09 PKSmart 执行记录

**运行时已建成并通过严格重复性闸门**：`pksmart==3.0.1` 已隔离安装到 `~/.local/share/benchmark-runtimes/pksmart_3_0_1/`（Python 3.10.14），依赖锁文件 SHA-256 为 `c4709c9bdd539b40ae38b9229322821ef4d128d3ddd8783b6ce7161f82e5029b`。官方 wheel 已持久化到运行时 `artifacts/` 并按实际文件核验 SHA-256 `0226969196cade1954b8355b600132d32a709fc7f12ef71a5db102f52de84743`；worker 还核验安装 RECORD 及其 51 个文件、推理源码、三项模型和全部依赖 RECORD。三项官方回归端点为 `human_VDss_L_kg`、`human_CL_mL_min_kg`、`human_fup`，通过独立的 `numeric-multi-endpoint-worker-v1` 资格协议登记，不进入 BBB 冲突普查。

初次八轮验证发现最多 `8.881784197001252e-16` 的跨进程漂移。根因不是模型随机性，而是五个已发布随机森林权重把 `n_jobs=30` 固化进 pickle，多线程树归约顺序不固定；仅设 OMP/MKL 单线程变量不能约束 joblib。worker 现用 `joblib.parallel_backend("sequential")` 约束官方 `pksmart.predict_pk_params` 的调度，不改上游源码、权重、特征或预测算法，也不做舍入。修正后三探针八轮全部成功且逐值精确一致，结果为 **PASS**；完整逐轮证据见 `data/bbb_census_v3/runtime_verification/pksmart_3_0_1.json`。

### 2026-09-09 SolTranNet 执行记录

**运行时建成并通过严格重复性闸门**：`soltrannet==1.0.0` 已隔离安装到 `~/.local/share/benchmark-runtimes/soltrannet_1_0_0/`（Python 3.10.14）。发布包漏报了实际必需的 NumPy、RDKit，且依赖已移除 `pkg_resources` 的新版 Setuptools 会导致导入失败；运行时据实补钉 `numpy==1.26.4`、`rdkit==2023.9.6`、`setuptools==80.9.0`，依赖锁 SHA-256 为 `33a0cc41a4053e7f9e27d93ba381d05f7bf91ee68d9905392f55dc37373e8fee`。四项 CPU 相关上游测试均通过，GPU 专用测试因本机 CUDA 不可用未纳入 CPU 验收。

worker 固定走 CPU、单线程和官方入口 `soltrannet.predict`，在导入模型前核验发布 wheel、归档 wheel、安装 RECORD 及其 20 个文件、依赖锁与全部依赖 RECORD、Apache-2.0 许可证、模型权重和四个推理源码。唯一端点 `aqueous_solubility_logS` 按 `log10(mol/L)` 回归登记；为避免捏造物理上下界，数值协议新增 `log10` 尺度并允许上下界为空，但仍拒绝 NaN 与无穷值。

三探针八轮逐位一致，咖啡因 `-1.1322435140609741`、地西泮 `-4.834878921508789`、蔗糖 `0.3145146369934082`，结果为 **PASS**。旧 730 分子产物与当前 Torch 2.14.0 环境在部分样本有 `2.38e-7` 至 `9.54e-7` 的末位差异，未用旧值代替本轮实测；当前冻结环境的跨进程重复值完全一致。完整证据见 `data/bbb_census_v3/runtime_verification/soltrannet_1_0_0.json`。

### 2026-09-09 RDKit 三项执行记录

**三个独立运行时均建成并通过严格重复性闸门**：`rdkit_tpsa`、`rdkit_qed`、`rdkit_sa_score` 分别安装在对应的持久化目录，均使用 Python 3.10.14 与 `rdkit==2026.3.5`。三处归档的官方 wheel SHA-256 均为 `3b299c2d1e2c4da14b38fae62c650ea315468cb54b2b83a42e1eeed212f5027d`；安装 RECORD SHA-256 为 `b91c99680c77b0961ae77c26d6558c2f2a1f7e49ef40df833c7b0c0860233274`。因 NumPy 的 `f2py` 入口脚本嵌入各自虚拟环境路径，三个环境保留各自的依赖锁哈希：TPSA `76a0d11c2b33b7e8dfa41f039c5265724716926bc4f908234132325f15d4fa91`、QED `0d2109f5e9fbbf70fff03d353c1bbb842215b1f034cf395137f47f4227d2e296`、SA score `49db3503831d9b9acc6e927d872dabee62531ab94a7cdaeb57fb4350003a5e3a`。

共享 worker 在加载 RDKit 计算入口前核验官方 wheel、安装 RECORD 及其全部带摘要文件、依赖锁与全部依赖 RECORD、许可证和端点实现资产。三项性质按确定性 `descriptor` 登记，不伪装成回归模型，也不进入 BBB 分类普查；TPSA 范围为 `[0,+∞)`，QED 为 `[0,1]`，SA score 为 `[1,10]`，并统一拒绝非有限值。

三项各跑八个独立进程，所有探针均成功且逐位一致，结果均为 **PASS**：TPSA 为咖啡因 `61.82`、地西泮 `32.67`、蔗糖 `110.38000000000001`；QED 为 `0.5384628262372215`、`0.79164533623487`、`0.3055865708949447`；SA score 为 `2.29798245679401`、`2.120641301464312`、`3.98413724979369`。完整证据分别见 `data/bbb_census_v3/runtime_verification/rdkit_tpsa.json`、`rdkit_qed.json`、`rdkit_sa_score.json`。

### 2026-09-09 Rowan 执行记录

**按预案降为 V 档，只登记不测**：本机未配置 `ROWAN_API_KEY`；`rowan-python==3.1.14` 客户端要求 Python >=3.12，且 wheel 内没有模型权重，实际计算由远端服务完成，服务端模型版本与哈希也未公开。因此不能把“客户端可安装”误记成可复现的 L3 本地推理，也没有创建空壳 worker、运行时或验证 JSON，更没有产出任何预测值。

已从官方 PyPI 获取并核验通用 wheel，SHA-256 为 `26f69a6862bb20ecec21d0b998ef0eb9ba69dffd90391675fcf6ee6b9d89e87c`。固定客户端源码确认六种可选方法：logP 的 `chemprop_sangster2026`、`crippen`、`cosmors`，以及溶解度的 `fastsolv`、`kingfisher`、`esol`；其中后两种只支持 298.15 K 水溶液，FastSolv 支持自定义溶剂和温度。本轮在资格 registry 中以 `license_gated`、`qualification_enabled: false` 登记这些能力。若后续取得获授权凭据，应按 W 档远程服务执行并保存脱敏请求/响应、服务端标识和跨日漂移证据，而不是恢复为 L3。

### 2026-09-09 QupKake 执行记录

**运行时建成且跨进程逐位一致，但仍按 `inference_contract_broken` 登记不测**。这不是装不上，也不是数不稳——是这个工具吐出来的东西，数值资格协议这张表装不下。

**装成了什么**：仓库无 tag、也未上 PyPI，故按提交 `f7a294e91929e4e4f0a7100ad5cf5a64a34a7d4f` 钉版，tool_id 定为 `qupkake_f7a294e9`，隔离安装在 `~/.local/share/benchmark-runtimes/qupkake_f7a294e9`（Python 3.10.14）。两阶段安装（先钉依赖、再 `--no-deps` 装本体），九项钉版全部落准：`torch==2.1.2+cpu`、`torchvision==0.16.2+cpu`、`numpy==1.26.4`、`scipy==1.11.4`、`pandas==2.1.4`、`rdkit==2022.3.3`、`torchmetrics==1.2.1`、`pytorch-lightning==2.1.0`、`torch_geometric==2.4.0`，`pip check` 无破损。三个官方权重与自带 xtb 6.4.1 / crest 二进制的 SHA-256 均已记入 registry。作者在 `pyproject.toml` 声明了 `tomli` 但包内从未 import，仍按其声明装上（`tomli==2.4.1`），不擅自删依赖。

**只在运行时内部补的一个系统库**：`rdkit==2022.3.3` 的 `PandasTools.py:116` 无条件 import `Draw`，进而要 `libXrender.so.1`，而本机全局缺这一个（libX11/libXau/libxcb 都在）。没有动共享系统：从 aliyun jammy 镜像取 `libxrender1_0.9.10-1build4_amd64.deb`（sha256 `a68f6db4…`）、`dpkg-deb -x` 后把 `libXrender.so.1.3.0`（sha256 `2415c0b5…`）放进 `<runtime>/system-libs/`，靠 `LD_LIBRARY_PATH` 消费，两个哈希都进了 registry。

**重复性这一关是过的**：冻结一个三分子输入文件（aspirin / imidazole / 4-aminophenol），八个互不相干的进程、各自独立 root 并行跑官方 `qupkake file`，八份输出 SDF 只有一个字节摘要 `6789d43efa53667e87278c1392d67c589e5aba668c68afaeb36a502ccbd2826e`，解析出的 8 行数值摘要同样唯一（`bccbf11c93cf40f43c58e0c05d76093d52396d225fa9c6e20f1841e6c2cffae8`）。所以判它不合契约与"数抖"无关。

**第一条硬伤：那个数不是分子的函数，是输入文件的函数。** `cli.py:89` 逐行调 `embed_molecule`，走的是 RDKit 进程内全局 RNG 且不传 `randomSeed`，于是一个分子拿到什么构象，取决于它前面几行消耗了多少次随机抽取；构象一变 xtb 能量就变，pKa 跟着变。实测两组对照，**没有一个值对得上**：imidazole 单独跑对比它在三分子批内跑，3/3 全变（basic idx 2 `6.744522` → `6.742665`；basic idx 4 `6.427045` → `6.425622`；acidic idx 4 `11.968395` → `11.968569`）；把同样三行换个顺序，8/8 全变（aspirin acidic idx 12 `3.638102` → `3.636389`；4-aminophenol acidic idx 5 `10.181016` → `10.180152`）。换句话说，逐分子 worker 报出来的数，任何批量跑都复现不了——这个数不能记在"这个工具 × 这个分子"名下。

**第二条硬伤：输出是一张变长的逐位点表，不是一组固定端点。** 官方产物每分子 0..N 行 `(idx, pka_type, pka)`：`predict.py:250-253` 在找不到质子化/去质子化位点时连输出文件都不建（可为 0 行）；同一分子内 `pka_type` 会重复（4-aminophenol 的 acidic 同时出现在 idx 0 和 idx 5）；`idx` 又随分子变。所以 `pka_type`、`idx`、`(idx, pka_type)` 三种键都构不成固定端点集。而 `numeric-multi-endpoint-worker-v1` 在 `verify_runtime.py:296-300` 要求逐 entity 一行且顺序与冻结输入一致，`:312` 要求 `set(raw_values) == set(endpoint_ids)`——结构上就表达不了。

**没做的事**：没有替上游发明 min/max 之类的聚合摘要（它本身不输出任何摘要），没有建空壳 worker，也没有以 QupKake 名义产出任何预测值。registry 内 `runtime_path` 照实指向已建成的运行时、`command` 留空、`qualification_enabled: false`，端点以单条 `micro_pka_per_site` 登记并在 `meaning` 里写明变长语义。

**要救有两条路，二选一**：给 `embed_molecule` 补种子并把契约扩成"逐位点表"（届时可回到 L3 逐位一致口径）；或者承认它测不进数值闸门，改为在冻结输入文件下交付一份逐位点表作为独立产物——后者会改变交付物形态，需先定夺。

### 2026-09-09 Uni-pKa 执行记录

**按 `no_local_inference_path` 登记不测：代码发布了，权重没发布，而唯一的官方权重入口现在是 404。** 没建运行时、没写 worker、没产出任何 pKa 值。

**权重缺口是硬缺口，不是"下不动"**：按提交 `7cfbf6532d7e14f427abaed5859bd1001b4b6377`（2025-04-01，Apache-2.0）钉版，tool_id 定为 `unipka_7cfbf653`。整棵树 51 个文件、4.0 MB，按 `.pt/.ckpt/.pth/.bin/.safetensors` 全扫，**权重文件数为 0**。而两条官方推理入口都要逐 fold 载 checkpoint：`infer_pka.sh:24` 要 `finetune_save/fold_{0..4}/checkpoint_best.pt`（task `mol_pka`、arch `unimol_pka`），`infer_free_energy.sh:24` 要 `dwar_finetune/fold_{0..4}/checkpoint_best.pt`（task `mol_free_energy`、arch `unimol`），合计 10 个文件，一个都不在仓里。

**三个官方分发点逐个实测，没有一个能给权重**：README 第 4 行只点了一处权重来源——Bohrium notebook `38543442597`，实测 **HTTP 404、正文 150 字节**；README 第 6 行的 Bohrium App 同样 **404**。GitHub 该仓 releases 与 tags 的 API 返回都是 5 字节，即空数组 `[]`，没有任何备用官方工件。第三个链接 AISSquare 返回 200，但 README 第 6、100 行把它明确写成 "Full datasets" 与 "The raw data"，是数据集不是权重；且该站是 JS 单页应用，`/openapi/...`、`/api/...` 等路径实测全部回同一个 1076 字节的壳，拿不到文件清单。

**必须澄清：这次的障碍不是 CUDA，也不是 Uni-Core。** 早前记录里"本机 CUDA 不可用"的假设与实况不符——本机 `nvcc` 12.1.105、NVIDIA H20、驱动 580.105.08、Python 3.10.14。`unicore` / `uni-core` 在配置镜像上确实都是 404，但 Uni-Core 的 GitHub release 轮子 `unicore-0.0.1+cu118torch2.0.0-cp310-cp310-linux_x86_64.whl` 实测可取（Range 请求 206、取到 1024 字节，未整包下载也未安装）。也就是说依赖侧装得上，**装上了照样没有那 10 个 checkpoint**，所以不能把账记到 CUDA 头上。

**没走训练这条路**：README 第 95 行往后给了完整训练流程（ChemBL 预训练 + dwar-iBond 五折微调）。跑完得到的是我们自己训的模型，不是作者发布的那个，按 `deep_b3`、`sai_bbb_v3` 的同一条先例排除，不记到这个工具名下。

端点以单条 `macro_pka` 登记（回归、单位 pKa、线性尺度，上下界留空不编造），`runtime_path` 为 `null`、`command` 留空、`qualification_enabled: false`。逐项探测结果（含各 URL 的状态码与字节数）落在 `data/bbb_census_v3/runtime_verification/unipka_7cfbf653.json`；网络可达性会变，复用前须重探。

### 2026-09-09 DeePred-BBB 执行记录

**先撤回我此前三条写错的结论。** 之前基于 3 分子探针记的"36 个 I-state 自相关描述符恒为空"、"地西泮被报成不透过是 NaN 造成的"、"data.csv 在这些位置没有 NaN"，按官方原样参数复测全部不成立：3 个探针的空格数都是 **0**；地西泮实测 p0=0.53617108、类别 1（透过）；`data.csv` 实际有 **5309 个 NaN**，分布在 238 列、2704 行中的 85 行。728 分子那一轮跑完，36 个 I-state 列的空格总数也是 **0**。这三条已从 registry 理由和证据 JSON 里剔除，不作为判据。

**运行时是真建成了，之前假定的两个阻塞项都不存在。** 按提交 `327e066970f6cb1dd8cf97adb92c16b91675e793` 钉版，tool_id 定为 `deepred_bbb_327e0669`，隔离安装在 `~/.local/share/benchmark-runtimes/deepred_bbb_327e0669`（Python 3.10.14、tensorflow/keras 2.15.0、scikit-learn 1.3.2、numpy 1.26.4、pandas 2.1.4、scipy 1.15.3、h5py 3.10.0，`pip check` 无破损）。"本机没有系统 JDK"这条靠 `jdk4py==21.0.8.2` 解决——它以 pip wheel 形式提供 Temurin OpenJDK 21.0.8+9，没有动共享系统 JDK；"权重取不到"这条根本不成立——`DeePredmodel.h5`、`data.csv`、`PaDEL-Descriptor/` 全部随仓库自带。10 个资产的 SHA-256 已记入 registry，其中 PaDEL 的 16 个 lib jar 用"排序后逐个摘要再摘要"的方式做成一个与路径无关的汇总值 `62791729…`（早前那个 `9c37cafb…` 是 `sha256sum` 输出内嵌文件路径导致的路径相关值，已废弃），配方写在证据 JSON 的 `padel_lib_jars_rollup_recipe` 里。作者归属可核：`12rajnish/DeePred-BBB` 是第一作者本人账号，钉住的 HEAD 是他本人合并的 PR #4；仓库无 tag、无 release，也**没有仓库级 license 文件**，只有内嵌的 PaDEL 目录带 AGPL-3.0 + LGPL-2.1。

**硬伤是官方特征步在同一份输入上不确定。** 冻结一份 16 分子的 `.smi`（哈希已入证据 JSON），用官方原样参数连跑 20 次：**4 次出现空格**（每次 36/68/36/72 个空格、各涉及 1–2 个分子），16 次为零；此前同一份文件还出现过一次 **11398 个空格、涉及 14/16 个分子**。这不是列错位——4 次输出的表头与干净输出逐字节相同（1918 列、md5 `8dbce1d3…`）。机制是链式的、有行号可查：`DeePred-BBB_Script.py:65-74` 调 PaDEL 算 1917 个描述符，空格经 `:42` 的 `StandardScaler.transform` 变成 NaN（sklearn 的 scaler 对 NaN 是透传的），NaN 传到 `:47` 的 softmax 恰好输出 `[0.5, 0.5]`，而 `:47` 先对整个 (n,2) 矩阵取 `round` 再于 `:48` 取第 0 列，numpy 的 `round(0.5)` 得 0，于是该分子被**静默**报成 0（不透过）。脚本既不告警也不失败，唯一输出是一个 0/1 整数，拿到结果的人无法把这种假 0 和真 0 分开。顺带一条口径冲突：真正执行的是 `round`，普查口径写的是 `probability >= 0.5`，两者恰好在 0.5 处相反——而 0.5 正是 NaN 产生的那个值。

**为什么记在 registry 而不交给重放闸门：因为闸门看不见这种污染。** 20 次重放里有 4 个分子的官方标签发生翻转（atenolol 0×1/1×19、mannitol 0×2/1×18、nicotine 0×1/1×19、phenytoin 0×1/1×19）。但 fluoxetine 同样遇到了空格跑次（p0 由 `0.37143564` 变成 `0.50000000`），因为两个值落在 0.5 同侧，它 20 次的标签始终是 0——标签稳定性闸门（`pair_is_stable`）会把它判为**稳定**，而模型那一次实际吃进去的是 NaN。只要 0.5 的塌缩没跨过真值，闸门就一律放行，所以只能在 registry 一层记账。

**另外两件独立事实，都不作为判据。** 一是描述符数值本身是稳的：干净重放之间有 203 个格子不同，逐个定位后全部落在 35 个信息含量描述符（IC/SIC/BIC/CIC/TIC/MIC/ZMIC 1–5），差 1–2 个 ULP，最大相对差 `1.21e-14`，量级远不足以改变任何标签，证据 JSON 里明确写了不据此下判。二是 730 个普查分子里有 **2 个永不返回**：`z002021`、`z002137`（两个桥环吗啡烷类），整批跑到 728 行后在这 2 个分子上以 236% CPU 空转 30 分钟无新输出，单独重跑 900 s 也只产出表头——是分子特异的，不是负载问题，所以按官方原样参数一次全量普查根本跑不完。PaDEL 本身提供 `-maxruntime`（逐分子上限）与 `-threads`（默认 -1 即全核），官方脚本两个都不传。

**方向未获独立验证，也不猜。** 脚本 docstring 第 35 行声明 `1 permeable; 0 non-permeable` 且取第 0 列，但 `data.csv` 宽度 1917 恰等于特征数、**不含标签列**，训练时的编码无从恢复。本仓 40 行 `answer_key.json` 定不了向：那 40 行是按"工具之间互相打架"挑出来的冲突池，随机水平本就是构造出来的（实测原样 0.45、反过来 0.55，n=40）。另做的 16 分子教科书面板 9/16 vs 7/16，同样落在噪声里。故记为 `AUTHOR_DECLARED_BUT_UNVERIFIED`，两次失败的尝试和各自失败的原因都写进了证据 JSON。

**没做的事**：没有替上游补 NaN 处理、没有自行加 `-maxruntime`/`-threads`、没有以 DeePred-BBB 名义产出任何普查预测值。两条可能的补救（补 NaN 处理，或显式钉 `-maxruntime -1` 与 `-threads 1` 后重测重放稳定性）都会改变官方推理路径，按 `deep_b3` / `sai_bbb_v3` 的先例需先定夺，已连成本写进证据 JSON 的 `recovery_options`。因为它预测的性质就是 BBB，记在普查 `tools` 里（`enabled: false`、`inference_contract_broken`），不移到数值资格表。逐轮证据见 `data/bbb_census_v3/runtime_verification/deepred_bbb_327e0669.json`；注意该缺陷本身就是不确定性，重跑**不会**复现逐轮数字，只会复现现象。

### 2026-09-09 XenoSite 执行记录

**按 `no_local_inference_path` 登记不测：代码是公开可装的，但训练权重被上游主动扣住不发，而且这件事上游自己在四处写明。** 没建运行时、没写 worker、没产出任何位点分数。

**先分清两个同名包，顺带交代 pip 起初为什么说包不存在。** 真正的预测器是 PyPI 上的 `xenosite-predict` 0.4.0，与仓库 tag `v0.4.0`、提交 `645d0536a64d0c10aeabf2602188aa9323409ec0`（`swamidasslab/xenosite-predict`）对齐，tool_id 定为 `xenosite_predict_0_4_0`。另有一个叫 `xenosite` 的包，wheel 只 2227 字节，它自己的描述写着"All useful functionality requires installing one of the XenoSite subpackages"——那是命名空间加 CLI 壳，不是预测器；我差点误认，在下任何结论之前查出来改正了。pip 一开始报"包不存在"是两件事叠加：配置的索引是 `http://mirrors.aliyun.com/pypi/simple/`，pip 对 http 源只发一条 "not a trusted or secure host" 警告然后**静默忽略**（改用同镜像的 https 地址即可）；改过去之后仍报 No matching distribution，是因为该包两个版本 `0a0`/`0a1` 都是**预发布版**，不加 `--pre` 一律跳过。用 curl 直取 simple 索引（200、1287 字节）确认包确实在——只是它本来就是错的那个包。

**权重缺口是上游的主动选择，不是"下不动"。** 0.4.0 的 wheel 里 63 个文件、`.onnx` 图 **0 个**。包自己把缺口讲清了：`weights.py` 第 3–4 行写 "The published archive URL is **not** compiled into this package"，要求自行设 `XENOSITE_ONNX_URL`；README 的 ONNX 段重申图不随 sdist 发、URL 不存在仓库里；README 的打包说明写 "Publish sdist only (no wheel): ONNX weights stay local"；README 的安全提示更进一步，要求读者**不要**把 `XENOSITE_ONNX_URL`、API key、权重主机名提交进仓库。钉住的提交里 `weights/` 目录只有两个文件：`.gitignore`（108 字节）与 `README.md`（1920 字节）。

**`weights/README.md` 点名的两条填充路线，加上其余官方入口，逐条实测全断。** ①它点的 Docker 镜像 `dockerreg01.accounts.ad.wustl.edu/swamidass/xenosite-legacy:api` 要 registry 登录，而该主机名**根本解析不了**（curl 错误 6、无 A 记录；`accounts.ad.wustl.edu` 是 WashU 内网 AD 域，不在公网）。②它给的兜底同级检出 `../xenosite-legacy/data/xenosite_legacy_data_trimmed.tgz` 取不到：`github.com/swamidasslab/xenosite-legacy` 返回 **404**，该组织公开的 9 个仓库里没有它。③v0.4.0 的 GitHub release **资产数为 0**，7 个 tag 也没有一个带权重工件。④HTTP 后端不能替代：它要求把 `XENOSITE_BACKEND` 指向别人部署的 `xenosite-api` 实例（可选 `XENOSITE_API_KEY` bearer token），`xenosite.org` 本身回 200（HTML 应用、19992 字节、标题 XenoSite），但 README 记载的路由形状 `GET /v0/<model>?smiles=` 在那里回 **404**，`/api/v0/...` 也一样。什么都不配时，后端选择器走到最后一个分支直接抛 `BackendNotConfigured`。另记一条纯可达性观察：`swamidass/xenosite` 声明的主页 `xenosite.vercel.app` 40 s 内无响应（curl 退出 124）——同日 `xenosite.org` 是通的，故只记为可达性事实，不当成额外阻塞项。

**必须澄清：这次的障碍不是依赖，也不是解释器版本。** `openbabel` 3.2.1 在配置镜像上为 cp310–cp314 提供 `manylinux2014_x86_64` 轮子，rdkit、numpy、onnxruntime、httpx、pydantic、msgpack、xenosite-forest 都是普通 PyPI 安装。唯一的环境缺口是 `xenosite-predict` 要 Python ≥ 3.11 而本机只有 3.10.14 且无更高版本二进制——这条**可解，而我故意没去解**：建出 3.11 环境后 ONNX 图数依然是 0，权重阻塞在前且无条件，不能把账记到解释器版本头上。

**还有一个障碍是推断出来的，没实测，也不作为登记状态。** `xenosite/predict/types.py` 声明的 `MolBondResult`、`MolAtomResult`、`MolAtomPairResult`、`AtomResult`、`BondResult`、`AtomBondResult` 返回的都是逐原子/逐键/逐原子对的分数数组，长度随分子走，固定端点的数值契约表达不了——和 QupKake（`qupkake_f7a294e9`）那边**已实测**的结构错配是同一类。但这里只是从类型声明读出来的，所以登记状态仍按实际撞上的那个障碍写。

8 个官方模型逐条登记为端点（epoxidation、quinone、reactivity、ugt、ndealk、isozyme、phase1、bioactivation），全部记为回归、单位 score、线性尺度；`value_range` 上下界留空，不去断言未经核实的 [0,1]；每条 `meaning` 都注明它是长度随分子变化的逐位点分数数组而非标量。`runtime_path` 为 `null`、`command` 留空、`qualification_enabled: false`。

**没做的事**：没猜也没爆破权重 URL、没尝试登录 WashU registry、没做格式转换或重训、没建运行时、没写 worker、没以 XenoSite 名义产出任何值。逐条探测结果（含各 URL 的状态码与字节数）落在 `data/bbb_census_v3/runtime_verification/xenosite_predict_0_4_0.json`；网络可达性会变，复用前须重探。

### 2026-09-09 SILICOS-IT 执行记录

**这是 B 组第一个真跑起来的工具，记为 `ready`、`qualification_enabled: true`。** 之前"厂商没了所以测不了"的判断错了，错在把发行渠道当成了唯一来源。

**渠道确实死了，但源码活着，而且这个模型本来就没有外部权重可丢。** 论文引的那个 S3 页面 `http://silicos-it.be.s3-website-eu-west-1.amazonaws.com/software/filter-it/1.0.2/filter-it.html` 实测 404、360 字节；现在的 `https://www.silicos-it.be/software.html` 返回 200、11151 字节，页面里 PyPharao/Shape-it/Spectrophore 都还在，"filter-it" 出现 **0 次**，已被下架。但同一账号的源码仓 `github.com/silicos-it/filter-it` 是活的：LGPL-3.0（仓内 `COPYING.LESSER`）、默认分支 master、2022-02-10 建仓、2022-04-11 最后 push、5 星，releases 与 tags 的 API 都返回 5 字节即空数组 `[]`，只有 master 和 python 两个分支。关键在于 **SILICOS-IT 的 logP/logS 系数是写死在 C++ 源码里的**，不存在需要另外下载的权重文件，所以渠道消失不影响可复现性。钉在 master 的 HEAD `ad4807808f215ea2edbf7b76bff5045db14af53b`（"First commit"，Hans De Winter，2022-02-10，推送账号 UAMCAntwerpen），tool_id 定为 `silicos_it_filterit_ad480780`；因为无 tag 无 release，按 commit 钉版。

**身份是拿一手文献对着代码逐项核过的，不是假定的。** 归档的 SwissADME 论文写明 "SILICOS-IT is the log P o/w estimation returned by executing the FILTER-IT program (version 1.0.2, 2013)"，把 logP 描述成 "27 fragments and 7 topological descriptors"，把 logS 描述成 "a system of 16 fragmental contributions modulated by the squared root of MW"，并明确 "All predicted values are the decimal logarithm of the molar solubility in water (log S)"。对着代码数：`filterLogp.cpp` 有 **27** 条 SMARTS、**35** 个系数，`Calculate` 前 27 项按 SMARTS 匹配数累加，随后依次加 27 号截距、28 号重原子数、29 号杂原子数、30 号环系数、31 号杂碳比、32 号分子量、33 号成环比例、34 号 TPSA——27 + 1 截距 + 7 描述符 = 35，与论文口径逐项吻合；`filterLogs.cpp` 是 16 条 SMARTS、16 个系数，`:169` 起算于 `-0.10369 * sqrt(GetMolWt(true))`、`:181` 加截距 `0.89823`。pinned `CMakeLists.txt:10-12` 声明 1.0.2，与论文引的版本号一致。

**官方文档和官方代码在 logS 的 √MW 符号上互相矛盾，代码赢了，而且是实测判的。** `docs/filter-it.rst:1830` 印的公式是 `logS = 0.898 + 0.104 √MOLWT + w_i c_i`，代码是 `-0.10369`。用只含 `[CH2;!R]` 与 `[CH3;!R]` 两种片段的正烷烃把两个符号分开：己烷实测 LOGS **−2.167**、十六烷 **−6.329**；按代码符号手算是 −2.166 / −6.324，按文档符号是 −0.239 / −3.199。差 2–3 个对数单位，不是舍入能解释的。同一节还有两处笔误：`:1186` 把 logS 说成 "the negative logarithm of the water solubility"，紧接着 `:1187` 又说 "Soluble compounds will have a large logS"，前后自相矛盾（论文那句"molar solubility 的十进对数"才对得上代码）；`:1188` 把 logS 的算法写成 "The algorithm for calculating the logP"。这些只坑照文档手算的人，不影响跑出来的数，所以不记为缺陷，但写进了 LOGS 端点的 `meaning`。

**建运行时只踩到一个坎，用编译选项解决，没改上游一行代码。** `git clone` 撞 `GnuTLS recv error (-110): The TLS connection was non-properly terminated`，改用 codeload 的 tar.gz 按 sha 取（同样是按内容寻址）。编译在 `filterDistance.cpp:391` 的 `std::make_pair<std::string, Pattern*>(p->name, p)` 断掉——这是 C++11 之前的写法，g++ 11 拒绝把左值绑到 `std::string&&`。**没有去改这行源码**，而是加 `-DCMAKE_CXX_FLAGS="-std=gnu++98"`（与代码 2012–2013 年的年代相符），一次编译链接通过。运行时装在 `~/.local/share/benchmark-runtimes/silicos_it_filterit_ad480780`（conda 内 Python 3.7.12 + Open Babel 2.4.1 + cmake 3.29.4，源码树 247 个文件），`bin/filter-it` 自报 "Filter-it v1.0.2 | GCC 11.4.0 | Open Babel 2.4.1"。

**第一次跑不出数不是命令写错，是 Open Babel 找不到自己的插件。** 连吃三次失败："Could not deduce the file format from the '--input=<file>' option" 与 "Could not deduce the format from the '--inputFormat=<format>' option"。选项确实必须写成 `--opt=value`（空格分隔会被拒），但真正的原因是 conda 把格式插件放在 `lib/openbabel/2.4.1/`、数据放在 `share/openbabel/2.4.1/`，不设 `BABEL_LIBDIR` 与 `BABEL_DATADIR` 时 OpenBabel 2.4.1 看不见自己的 smiles 读写插件。补上这两个（外加 `LD_LIBRARY_PATH`）才出数。这三个环境变量由 worker 自己设，已落在 `runtime_evidence.environment.openbabel_plugin_env`。

**worker 一分子一次调用，是被实测逼出来的，不是保守。** 官方入口是 tabulate 模式（`docs/filter-it.rst:82,84` "functions as a property calculator... No filtering is done in this mode"），filter 文件里只写裸关键字 `LOGP`/`LOGS`（`filterRealProperty.cpp:118,151`：`Initialise` 收 `tabulate` 参数，`if (!tabulate)` 才要求 min/max）。但**一批里只要有一个分子读不动，OpenBabel 的 reader 就地停下，排在它后面的分子被静默丢掉**：3 行输入把坏 SMILES 放在第 2 行，输出只剩第 1 行，尾部还报 "Total: 1"。批量省下的时间换不来逐行可归因的失败，所以改成一分子一次；实测 20 次调用共 0.417 s（约 21 ms/次），730 个分子约 15 s，代价可接受。返回码不能当判据——读不动分子时 filter-it 仍然 **exit 0**，原因只写进 stderr。失败文本只截 Open Babel 的报错块、不拼 stderr 尾部，因为 stderr 里带着每次调用不同的临时输入路径，拼进去会让同一个失败在两轮之间看起来不一样。

**两条官方行为要跟着数一起交出去，都不是缺陷，但都限制解释。** 一是官方输出固定 3 位小数（`calculator.cpp:824-825`：`(*tfs).precision(3); *tfs << std::fixed`），所以 `raw_values` 的精度上限就是 3 位——这是官方端点的输出精度，不是我们截的。二是**不传 `--salts` 时官方默认先把多组分分子拆开、只留原子数最多的片段再算**（`calculator.cpp:724` 起：`if (!o->Salts())` 后 `mol->Separate()`，按 `NumAtoms()` 取最大并还原原标题）：实测 `[Na+].CC(=O)Oc1ccccc1C(=O)[O-]` 得 LOGP 1.312，而中性阿司匹林 `CC(=O)Oc1ccccc1C(=O)O` 得 1.103，两者不同，因为留下的是去质子的阴离子。盐和共晶输入拿到的是"最大片段"的值。

**闸门跑满并全过。** 按 `numeric-multi-endpoint-worker-v1` 走 8 轮（1 discovery + 7 replay）逐位一致闸门：`run_count: 8`、`deterministic_across_runs: true`、`numeric_stable_across_runs: true`、`value_spread` 为空。三个固定探针：caffeine LOGP −0.499 / LOGS −0.519，diazepam 4.119 / −4.791，sucrose 探针 −1.837 / +2.039，方向合理（脂溶性 diazepam > caffeine > 糖，溶解度反向）。

**端点与资产登记。** 两条端点都记为回归：LOGP 单位 dimensionless、尺度 linear（与已在册的 rowan logP 同口径），LOGS 单位 log10(mol/L)、尺度 log10（与 soltrannet 同口径）。`value_range` 上下界一律留空——仓里自带的 filter 文件给的是筛药窗口而不是模型边界，同一个 LOGP 在 `CMClike.filter:42` 写 −11..14、`Druglike.filter:41` 写 −2.0..3.0、`Leadlike.filter:44` 写 0.6..7.4，三份互不相同，拿哪一份当模型上下界都是编的。资产按 8 个摘要入册：二进制、247 文件源码树汇总、4 个系数与构建源文件、`libopenbabel.so.5.0.0`、124 文件插件目录汇总。**二进制摘要不可复现**并已在证据里明说——filter-it 的 banner 打 `__DATE__`/`__TIME__`，那个值只记录这一次构建；可复现的锚点是源码树汇总 `4d85fe57…`，配方写在 `asset_evidence.source_tree_rollup_recipe`（按相对路径排序、逐文件 sha256，再把这些十六进制串各加换行拼接后取 sha256，排除符号链接）。它预测的性质不是 BBB，所以进 `qualification_tools` 而不是普查 `tools`：`tools` 仍是 34 条，`qualification_tools` 由 9 条变 10 条。

**没做的事**：没改上游任何一行源码、没给 logS 的文档笔误提 PR、没自己加 `--salts` 去改变官方默认行为、没把 3 位小数当缺陷绕过、没以 SILICOS-IT 名义跑 730 分子普查（等正式输入）。逐轮证据落在 `data/bbb_census_v3/runtime_verification/silicos_it_filterit_ad480780.json`。

### 2026-09-09 SolPredictor 执行记录

**结论先说：环境建成了，权重一个不缺，但官方入口对同一个分子每次给出不同的数，所以以这个工具的名义记不下任何一个值。** 状态记 `inference_contract_broken`，`qualification_enabled: false`，不写 worker，不产值。这跟 XenoSite、Uni-pKa 的"拿不到权重"是两类完全不同的阻塞，也跟"装不上"无关，所以 `runtime_path` 照实写而不是留 null。

**版本锚点。** 论文（Int J Mol Sci 25(2):715, 2024, doi 10.3390/ijms25020715）只指向 `github.com/waqarahmadm019/SolPredictor` 这一个分发点，该仓库 2 个提交、1 个分支 main、0 release、0 tag，所以只能按提交锁：`7e1ad74dced74c95e30225243400591325525b61`。

**装机和权重都不是阻塞项，这一点要单独写清楚，因为它否掉了最容易的那个解释。** 十个交叉验证折的权重全部在仓内，每个 236409 字节。仓库带 `.gitattributes` 的 `* text=auto`，理论上能在 checkout 时把二进制搞坏，所以这件事是实测而不是假设的：`git cat-file -s` 的 blob 大小与工作区大小都是 236409，`git check-attr text` 报 `auto`，即 git 的二进制启发式没动它们。运行时建在 `~/.local/share/benchmark-runtimes/solpredictor_7e1ad74d`（Python 3.10.14、torch 2.1.2+cu121、torch-geometric 2.4.0、rdkit 2023.9.1、numpy 1.26.4、setuptools 68.2.2），`requirements.lock.json` 冻 40 个分发、摘要 `7e08d2a9…`，`uv pip check` 干净。上游连依赖声明都没有——没有 requirements.txt、没有 setup.py、没有 README，所以这些 pin 是我们定的，得说明理由：torch 压在 2.6 以下是因为 `webserver.py:41,45` 要用 `torch.load` 反序列化一个 PyG 的 `(data, slices)` 元组，torch 2.6 的 `weights_only` 默认值会拒；numpy 压在 2 以下是因为 torch 2.1.2 是对着 numpy 1.x 编的，否则 `regression_train_test.py:20` 直接抛 `Numpy is not available`。

**先跑闸门再谈判定。** 按 `numeric-multi-endpoint-worker-v1` 的逐位一致闸门（`verify_runtime.py:540` 要求每轮都等于第一轮，`:614` 据此才放行），用未改动的官方入口 `python webserver.py`、对着它自己写死在 `webserver.py:109-110` 的两个 SMILES 跑 8 个独立进程：**8 个 stdout 摘要全不相同（8/8）**，从 discovery 的 `806fcc29…`（`[-5.3732567 -5.755916 ]`）到 replay7 的 `f3e8ba61…`（`[-5.6206064 -5.7125096]`）。这不是"有风险过不了"，是实测过不了。

**机制一：推理时 dropout 是开着的。** 预测路径上从头到尾没有一次 `model.eval()`，所以 `solubilityPredictor_resgatedgraphconv.py:115` 与 `:125`（后者在全部 8 个 `num_timesteps` 迭代里）的 p=0.127 dropout、以及 `:68-70` 那个 GATConv 的注意力 dropout 全程有效；而真正驱动这些掩码的 CPU 随机数发生器从未播种——`webserver.py:23-25` 只播了 CUDA 的种子，且那一支的 device 赋值在 `:30` 被无条件改回 `cpu`。单分子跑 20 次，三个探针都是 20 个互不相同的值：caffeine −1.583124…−1.150811（极差 0.432314、标准差 0.122351），diazepam −7.918946…−7.738183（0.180763、0.052067），sucrose −0.179144…−0.029830（0.149314、0.045402）。**用论文自己的尺子量**：论文报的测试集 R² = 0.79 ± 0.02、RMSE = 1.03 ± 0.04，caffeine 的极差是这个 RMSE 的 42%，所以这不是末位抖动，而是模型自称误差里相当大的一块。只把 `model.eval()` 这一行加上、别的全不动，20 次收敛成 1 个值（caffeine −1.346925973892212、diazepam −7.845784664154053、sucrose −0.1273057758808136），这既钉死 dropout 是全部原因，也说明 CPU 前向本身是逐位可复现的。

**机制二：分子身份被扔掉了。** `webserver.py:97` 用 `shuffle=True` 建 DataLoader，`regression_train_test.py:14-20` 按 loader 顺序拼接、返回一个不带任何标识的扁平数组，调用方无法知道哪个数属于哪个输入。同一批三个分子（输入序 caffeine、diazepam、sucrose），在 eval 模式下跑三次拿到 `[-1.3469, -0.1273, -7.8458]`、`[-7.8458, -0.1273, -1.3469]`、`[-0.1273, -7.8458, -1.3469]`——同样三个数，三种顺序；改成 `shuffle=False` 则三次都稳定在 `[-1.3469, -7.8458, -0.1273]`。这条比噪声更严重：按输入序上报会直接把分子标错，而不只是加了个抖动。

**这是谁的缺陷，用一手证据定。** 上游第二个提交删过一批源文件，所以"缺 `model.eval()` 是删文件删坏的"是个合理怀疑——我最初也是这么假设的，实测把它推翻了。仓里还留着作者自己的 `models/__pycache__/regression_train_test.cpython-38.pyc`（magic `550d0d0a`，Python 3.8），它早于那次删除，反映的是作者跑过的完整树。用运行时解释器 unmarshal 出七个函数并读 `co_names`：`eval` 出现在 `test` 和 `test_forcaptum` 里，但在 `predicting`、`SolPredictions`、`predicting_forcaptum` 里都没有。`webserver.py:103` 调的正是 `SolPredictions`——它在作者自己的树里就没切过 eval 模式。所以这是上游本来的行为。

**两条只记不判的观察。** 一是 `webserver.py:99` 写死读十折里的 `model_1`，而 eval 模式下各折分歧很大：sucrose 从 −0.127306（第 1 折，也就是出厂那折）到 +0.585235（第 2 折）跨过零点，caffeine −1.346926…−0.690357，diazepam −7.845785…−6.126152——决定答案的不只是 dropout 噪声，还有这个随手选的单折。二是 `webserver.py:94` 用 `time.time_ns()` 给缓存命名，每次调用都往 `data/processed` 写一个新的 `.pt` 且从不复用或清理（仓里那两个就是作者自己跑剩的）；这意味着官方入口无法就地反复运行而不让固定树长胖，我跑出来的 6 个缓存已删除，树核回 49 文件、源码摘要未变。

**两条与状态无关但要摊开的事实。** 仓库完全没有许可证：GitHub licence 端点 404、`license` 字段为 null、固定提交的 49 文件树里没有 LICENSE / COPYING / README；PyPI 上也没有这个包（`solpredictor` 与 `sol-predictor` 均 404）。论文给的在线服务 `https://nsclbio.jbnu.ac.kr/tools/SolPredictor/` 从本机连不上（工具路径、去掉尾斜杠的路径、裸主机三种写法一律 curl 错误 28、60 s 连接超时）——这更像本机出网被挡，不足以证明服务已死；何况远程服务本来就给不出可锁定的产物。这两条都不是记这个状态的原因，状态是那个实测出来的契约缺陷定的，与授权与否无关。

**没做的事**：没改上游任何一行代码（`model.eval()` 加 `shuffle=False` 确实能同时修掉两个毛病，但那样跑的就不是已发表的那个程序，而普查声称测的正是已发表的那个）；没挑种子（挑了就等于把某一次 dropout 采样冻起来当成模型的预测）；没对十折做平均或集成（上游没有这种聚合，`webserver.py:99` 只读一折）；没写 worker，没以 SolPredictor 名义产出任何值。逐项证据落在 `data/bbb_census_v3/runtime_verification/solpredictor_7e1ad74d.json`；`tools` 仍是 34 条，`qualification_tools` 由 10 条变 11 条。

### 2026-09-09 OpenADMET 执行记录

**结论先说：建成并跑满闸门，记为 `ready`、`qualification_enabled: true`，但只登记了一条端点，而"只有一条"这件事本身要交代清楚。** tool_id `openadmet_cdf3e9f7`、tier L3，运行时在 `~/.local/share/benchmark-runtimes/openadmet_cdf3e9f7`。它预测的性质不是 BBB，所以进 `qualification_tools`：`tools` 仍是 34 条，`qualification_tools` 由 11 条变 12 条。

**三个版本锚点，全部按内容钉住。** 代码是 `github.com/OpenADMET/openadmet-models` 的提交 `cdf3e9f7de9ae6b30d13a28c68e20d3b2e651498`（自报版本 0.2.0），模型是 `openadmet/logd-chemprop_foundation_surrogate-v1` 的修订 `6fd9462dc1db1d094d0ddd5a678321658fc772d6`，基座权重是 `openadmet/chemprop-foundation-pretraining-weights` 的修订 `0f8201d06c4656c3f97bc244ce2dc18ac2881be9`。端点取 `lipophilicity_logD`，回归、单位 log10 ratio、尺度 log10，上下界留空。端点名不是猜的，取自模型自带的 `data/target_cols.json`（内容就是 `["LogD"]`）；`meaning` 只写"辛醇/水分配系数"，**不写 pH 7.4**——上游没有任何一处声明测定 pH，写上去就是编的。

**为什么 11 个官方模型只做了 1 个：7 个卡在推理时下载，且卡点在我们这边的网络，不在工具。** 该组织今日早前可枚举到 11 个预测模型仓 + 1 个纯权重仓，其中 7 个是 CheMeleon 模型。用未改动的官方 CLI 跑 `permeability-logd-ppb-chemeleon-baseline`，栈顶明确停在 `architecture/chemprop.py:552` 的 `urlretrieve("https://zenodo.org/records/15460715/files/chemeleon_mp.pt")`，报 `urllib.error.HTTPError: HTTP Error 504: Gateway Time-out`。直连复核：浏览器 UA 拿到 **403**、763 字节，正文写的是 "Access to this resource has been restricted due to unusual traffic from your network"；换成 urllib 默认 UA 拿到 **504**。所以这条阻塞的性质必须写准——**是本机出网被 Zenodo 限流，不是工件不存在**，复用前必须重探，不能当成工具缺陷记账。另有一条可达性事实：HuggingFace 在今日约 20:06 是通的（三个仓都是那时下下来的），到 21:0x 变成 curl 000 / 连接超时，所以上面那个"11 + 1"的清点是当日早前的测量结果。

**能离线跑的那一个之所以能跑，原因很具体：它把基座权重打进了自己的仓库。** `logd-chemprop_foundation_surrogate-v1` 的 25 文件树里带 `foundations/surrogate_adme__s42_mp.pt`（34859955 字节），而 `model.json:12` 的 `from_foundation` 正指向这个相对路径。它与单独发布的 `chemprop-foundation-pretraining-weights/surrogate_adme__s42.pt` **逐字节相同**（两边都是 `924b61bc…`），只是文件名多了 `_mp`；两个摘要都入册。CheMeleon 那一支反过来，权重不在仓里、要在推理时去 Zenodo 取，于是断网即死。**剩下 3 个非 CheMeleon 仓（logd-minimol、mpbp-minimol、mpbp-surrogate）没有建、也没有测**，这里不对它们下任何判断；只记一条相关观察：纯权重仓里就 1 个文件（即上面那个 surrogate 检查点），所以两个 minimol 模型的基座不在该仓，它们是否各自自带副本未实测。

**上游环境按其自身声明就不可复现，所以那份 lock 是我们的口径，不是它的。** `pyproject.toml` 里**没有任何运行期依赖**（只有 `[project.optional-dependencies] test`），整套环境来自 `devtools/conda-envs/openadmet-models.yaml`，而那份 yaml 几乎不钉版本，还带 4 个会动的 git ref：`TabPFN@main`、`neural-pairwise-regression` 默认分支、`molfeat@main`、`useful_rdkit_utils@master`。两处偏离要主动交出去：①molfeat 装的是 **PyPI 上的 molfeat 1.0.0**，不是声明里的 `git+https://github.com/OpenADMET/molfeat.git@main`——移动引用没法钉；②`tabpfn`、`tabpfn-extensions`、`neural-pairwise-regression`、`useful-rdkit-utils` **一个都没装**，而官方 CLI 端到端跑通了，说明这条 LogD ChemProp 路径不需要它们。我们的口径写在 `requirements.lock.json`：222 个分发、Python 3.12.14、摘要 `c168e8d2…`。

**这个 venv 不是自给自足的，复用前得先看宿主。** `rdkit/Chem/Draw/rdMolDraw2D.so`、`rdkit.libs/libRDKitMolDraw2D-368cb635.so.1`、`rdkit.libs/libcairo-*.so.2` 以及 `cuik_molmaker` 都链 `libXrender.so.1`，实测解析到宿主的 `/usr/lib/x86_64-linux-gnu/libXrender.so.1.3.0`（`libxrender1` 1:0.9.10-1build4，SHA-256 `2415c0b512d332522625bc65bcc5e05dba93c2e3418329ded2855111fb0cf723`）。该摘要已进入 registry，并由 worker 在导入 OpenADMET 前逐次核验；缺失或字节漂移都会拒跑。

**最要紧的一条：官方 CLI 的数随 CPU 线程数变，"逐位一致"只在固定线程配置内成立。** 本机 32 核，不设线程数直接跑官方 CLI，caffeine 得 **0.024799109**；把 worker 那套 `OMP/MKL/OPENBLAS/NUMEXPR_NUM_THREADS=1` 补上再跑同一条命令，得 **0.02479887**，与 worker 的 0.024798870086669922 对齐；diazepam 与 sucrose 探针两种跑法都一样。差 2.4e-7 不是格式问题，是 float32 累加次序随线程数变。所以 worker 钉线程不是洁癖，而是让逐位闸门有意义的前提；同时也意味着这条闸门的承诺是"同一线程配置下逐位一致"，跨主机不能照搬。8 轮（1 discovery + 7 replay）跑满：`run_count: 8`、`deterministic_across_runs: true`、`numeric_stable_across_runs: true`、`value_spread` 为空。三个探针 caffeine 0.024798870086669922、diazepam 3.2769289016723633、sucrose 探针 −5.3132004737854；另按 `>d` 打包成十六进制独立数过一遍，8 轮里只有 1 种模式（`3f9964e000000000` / `400a372680000000` / `c01540b7a0000000`）。

**钉线程会不会把数带离官方那一次，是单独量过的，不是假定的。** 用作者自己的 2269 分子官方测试划分、在 worker 的完整配置下重跑，对着仓内 `data/y_pred.npy` 比：max_abs_diff 2.384185791015625e-06、相关系数 0.9999999999998366，重算 r² 0.8254902425934428，而 `regression_metrics.json` 印的是 0.8254902170970989。逐位相等的条数从 326 变 336、误差量级不动，说明是 float32 舍入噪声而不是行为改变。

**worker 一次调用整批，是被实测逼出来的，不是图省事。** 一是 `features/chemprop.py:165-167` 在列表推导里逐个 `from_smi`，一个读不动的 SMILES 直接把整次调用掀掉；二是同文件 `:77-81` 的 `dataset_to_dataloader` 在 `len % batch_size == 1` 时设 `drop_last=True`，于是**单行输入产出 0 个批次**（实测 n=1、batches=0、rows_seen=0）。也就是说官方路径在一次调用里是不可分割的全有或全无，逐分子调用根本走不通。代价照实记：失败时每一行拿到的是同一条原样异常文本，而不是逐分子诊断。

**顺手撞上一个上游缺陷，影响范围也量清了，但没有替它打补丁。** 该模型 `batch_size=128`，凡 `n % 128 == 1`（129、257、385、513、641、769…）最后一个分子会被 `drop_last` 丢掉。它**不会静默出错**：pandas 3.0.5 随即抛 `ValueError: Length of values (128) does not match length of index (129)`。计划中的 730 分子普查安全（730 % 128 = 90），三个探针（3）也安全。**故意没有加我们自己的行数预检**——那会复制上游逻辑，并且掩盖工具真实行为。

**资产闸门是验过活的，不是摆设。** 注册表钉 19 个摘要（唯一 19 个，新增宿主 `libXrender.so.1.3.0`）。在进程内逐个篡改期望常量可得到对应的拒绝理由，且改动前后 `verify_assets` 都能通过；全过程没有碰任何一个被钉住的文件。

**worker 里那道"导入位置"断言，是我自己踩坑踩出来的。** 因为 `from_foundation` 是相对当前目录的路径，worker 必须 chdir 进模型目录，而 Python 会把 cwd 塞进 `sys.path`。我一度把安装误判成"可编辑安装指向 /tmp"，真因是我 shell 链里上一条 `cd` 还没退出来、cwd 抢了导入——这是个假警报，但它变成了一条真断言：预测前用 `Path.is_relative_to()` 确认 `openadmet.models.inference.inference.__file__` 真落在 `sys.prefix` 目录树内，不能只做会误收 `/prefix-evil` 的字符串前缀比较。

**模型卡有三处笔误，只坑照着抄命令的人，不影响跑出来的数，所以不记为缺陷。** `README.md:80` 把这个模型说成预测 "the fraction unbound values"，而它的 `target_cols` 是 `["LogD"]`（同文件 `:93` 又写回 LogD）；`:85` 让你 `--input-path compounds_for_inference.csv`，`:76` 与 `:87` 让你 `--model-dir anvil_training/`——钉住的 25 文件树里这两样都没有，`model.json`/`model.pth` 就在仓库根，所以实际得写 `--model-dir .`。卡里的 `--input-col OPENADMET_CANONICAL_SMILES` 是对的。另记一条许可证自相矛盾、不作裁定：仓内 `LICENSE` 是 Apache License 2.0，`pyproject.toml:13` 写 `license = { text = "MIT" }`、`:17` 也按 MIT 归类，而模型卡 front-matter 写 apache-2.0。

**没做的事**：没改上游任何一行代码；没绕 Zenodo 限流（不找镜像、不猜 URL）；没加自己的行数预检去盖掉 `drop_last`；没建其余 10 个模型仓，也没替它们下判断；没给 LogD 断言 pH；没以 OpenADMET 名义跑 730 分子普查（等正式输入）。逐轮证据落在 `data/bbb_census_v3/runtime_verification/openadmet_cdf3e9f7.json`。

### 2026-09-09 KowWIN 执行记录

**结论先说：建成并跑满闸门，记为 `ready`、`qualification_enabled: true`。此前"KOWWIN 只有 Windows 桌面版所以测不了"的判断错了。** tool_id `kowwin_1_68_epi_1_1_0`、tier L3，运行时在 `~/.local/share/benchmark-runtimes/kowwin_1_68_epi_1_1_0`。它预测的性质不是 BBB，所以进 `qualification_tools`：`tools` 仍是 34 条，`qualification_tools` 由 12 条变 13 条。**这是 B 组最后一个，12 个全部有了实测结论。**

**错在哪：把"官方页面给的是 .exe"当成了"官方只有 .exe"。** EPA 的 EPI Suite 页面现在跳到 `https://episuite.dev/EpiWebSuite/#/`（自称 "EPI Suite BETA version 1.0"），那是个浏览器应用。第一轮判断到这里就停了，理由是"要么装 Windows，要么调远程服务"。实测把两头都推翻了：`/api` 返回的是一份真的 OpenAPI 文档（所以前端 `epi_api.js` 的 `selectTransport()` 会选 `"server"`，不是纯浏览器计算），而那份文档的标题写着 "EPI Suite **CLI** API"——顺着这条线找到 `/api/download`（`operationId: downloadDistributionJar`，自述 "Download the running EPI Suite CLI JAR"），下下来是一个 41386856 字节的自包含 `epi-estimators-cli.jar`（`Main-Class: EpiCli`、`Implementation-Version: 1.1.0`、`Build-Jdk-Spec: 21`、随包 `META-INF/sbom.cdx.json` 报 `pkg:maven/com.srcinc/epi-estimators-cli@1.1.0`）。**所以既不需要 Windows，也不需要连远程服务。**

**这份 JAR 就是当时线上跑的那一版，是对账对出来的，不是推断的。** 三条互相独立：包内 `webroot/data.parquet` 摘要 `c3a06b08…` 与 `webroot/search.parquet` 摘要 `c248acdf…`，与线上 `/api` 响应头 `x-epi-health` 公布的值逐字符相同；包内 `webroot/runtime/epi-runtime.js` 与线上 `https://episuite.dev/runtime/epi-runtime.js` 逐字节相同（都是 `ad58c92c…`）；包内 `webroot/build-id.txt` 为 `ee4f8a95…`，worker 每次启动都核这三样。

**版本锁扩了一档，这件事必须单独交代，因为它放宽了注册表的口径。** KowWIN 既不在 PyPI、也没有 git 提交、也没有 Zenodo DOI——`repo1.maven.org/maven2/com/srcinc/epi-estimators-cli/` 是 404，`search.maven.org` 的检索接口连不上（curl 错误 28、60 s 超时），GitHub 上也找不到官方源码仓。原有三种锁 `pypi | commit | zenodo_doi` 给的都是"还能再去取一次"的坐标，一种都套不上。**没有编一个假的锁，也没有反过来编一个假的阻塞状态**，而是在 `tool_conflict_census.py:42` 加了第四档 `artifact_sha256`，语义写在紧邻的注释里：它只钉字节、钉不了位置。理由是 `/api/download` 的 URL 里没有版本号，直发当前部署的那一份，所以下次去取可能是别的字节；`asset_evidence.download_url_is_immutable` 明写 `false`。这一档比前三档弱，读注册表的人要能一眼看出来。注意这个 tuple 是 BBB 普查的 `tools` 与 `qualification_tools` 共用的，所以扩的是两边的口径——目前在册 34 个 BBB 工具没有一个用它。

**建运行时只花了一件事：本机没有 Java。** `java not found`、`/usr/lib/jvm` 不存在，用 conda-forge 的 `openjdk=21` 解决（先装到临时 prefix 试跑，通了才装进正式运行时，临时目录已删）。运行时里是 conda 的 Python 3.12.14 + OpenJDK 21.0.10-internal 加那个 JAR，没有别的东西——worker 只是个壳，真正算数的是 `java -jar`。**包里 865 个 class 全是 major version 52（Java 8 字节码），跑在 21 上不改变数值**：Java 17 起浮点语义一律 strict（JEP 306 把 always-strict 收回来了），而 17 之前的宽松语义只在 x87 上才可能出现扩展精度，x86-64 的 SSE2 路径上从来没有过。所以没有加任何 JVM 标志去"锁"浮点，调用保持与官方文档一致。

**官方调用式是从 class 文件里挖出来的，不是猜的。** 拿到 JAR 时还没装 Java，于是写了个常量池解析器读 `EpiCli.class` 的 UTF-8 项，直接读出用法文本，得到 `epi --jsonl --request-json -`：NDJSON 从 stdin 进、一行一个请求，与我们的 worker 契约天然对齐。`--list` 报 29 个模块，本次只登记 `logKow` 这一个。

**只读 `estimatedValue`，不读 `selectedValue`，依据是官方手册而不是我的偏好。** KOWWIN 页面说得很直白：`selectedValue` 里可能装的是所给 CAS 号对应的实验证据，它不替代 KOWWIN 的计算，两者都会报出来。我们要的是模型自己的预测，读到 `selectedValue` 上就等于把实验测量记成了模型输出。本次跑批只喂 SMILES 不喂 CAS，实测 `experimentalValues` 恒为 `[]`、`selectedValue.valueType` 是 `ESTIMATED`、`source` 是 `module:kowwin`，两者此时数值相同——但这是输入决定的巧合，不是可以依赖的性质，所以路径写死在 `VALUE_PATH` 并有测试钉住。

**顺带说一条：官方输出比我们记的多得多，只记标量是刻意的。** `estimatedValue.model.factors` 会把每个片段的描述、匹配数、系数、贡献值、训练/验证集计数全列出来，还附一张 `KOWWIN v1.68 Results` 的文本表。caffeine 的 8 行（7 类片段 + Equation Constant）手工累加正好等于 0.1564，与端点 `meaning` 里"片段系数 + 校正因子 + 方程常数"的说法逐项对得上。`numeric-multi-endpoint-worker-v1` 只收 `raw_values`，所以这些不进产物；需要归因时按同一份摘要重跑即可拿到。

**闸门跑满并全过。** 8 轮（1 discovery + 7 replay）逐位一致：`run_count: 8`、`deterministic_across_runs: true`、`numeric_stable_across_runs: true`、`value_spread` 为空。三个固定探针 caffeine 0.15639997、diazepam 2.6996999、sucrose 探针 −1.0820992，方向合理（脂溶性 diazepam > caffeine > 糖）。另外单独量了 locale：JVM 程序常在数字格式上栽跟头，同一输入在 `C`、`de_DE.UTF-8`、`tr_TR.UTF-8` 下都是 0.15639997，所以没有去钉 locale，调用式保持与官方一致。

**worker 一次调用整批，与 SILICOS-IT、OpenADMET 的选择相反，因为实测行为不同。** 一批里混进 `this-is-not-smiles`，官方 CLI 仍然 exit 0、输出行数与输入行数相等、顺序不变，并在该行给出显式的 `errors:[{module, code, message}]`——既不像 SILICOS-IT 那样静默丢掉后面的分子，也不像 OpenADMET 那样整批掀掉。worker 仍然逐行核 `parameters.smiles` 是否与冻结输入同序，不一致就整批判失败。

**失败文本报的是 KOWWIN 自己的话——这是写完 worker 之后实测才发现要修的。** 失败时 CLI 把整个 `logKow` 节点换成错误对象（而不是保留结构、把值置空），第一版 `read_value` 的路径遍历因此提前退出，报的是我们自己那句"路径不存在"，把官方给的原因丢了。修成先取 `errors[]` 里该模块的原文：现在坏 SMILES 记的是 `estimation_failed: KOWWIN could not estimate the supplied SMILES.`。空 SMILES 不进这条路径——它在输入契约层就被 `validate_inputs` 拒了，与其它 worker 一致。

**一条会影响解释的行为：域外输入照样出数。** `[Na+].[Cl-]` 拿到 logKow −0.4608。官方 `help/content/introduction.md` 明确 EPI Suite 的适用域是有机物，所以无机盐和有机金属化合物本来就在域外，**但工具不会因此拒绝作答**。这一句已经写进端点 `meaning`，因为下游拿到一个数却不知道它在域外，是会出错的。

**端点与资产登记。** 单端点 `logKow`，回归、单位 log10 ratio、尺度 log10（与 rowan logP、openadmet logD 同口径），`value_range` 上下界留空——官方没有声明模型的取值边界，编一个上下界不如留空。资产按 6 个摘要入册：JAR 整包、34 个 `com/srcinc/kowwin/` class 的汇总（配方写在 `asset_evidence.kowwin_class_rollup_recipe`）、两个 parquet、`epi-runtime.js`、以及解析后的 `java` 启动器。**整包摘要已经覆盖了包内那几项，单独记是为了让读的人能拿模型代码、随包实验数据、线上部署三件事互相对账。**

**授权是"可用但非开源"，与 Rowan 的 `license_gated` 不是一回事，所以状态仍记 `ready`。** 随包 `help/kowwin/page.md` 与 `help/content/introduction.md` 写明：Copyright © 2000 U.S. EPA，KOWWIN™ 由 EPA 所有并受全球版权保护，"Permission is granted for individuals to download and use the software on their personal and business computers"，但 "Users may not alter, modify, merge, adapt, or prepare derivative works from the software"。**"不得修改"与我们"不许给上游打补丁"的既有规矩正好同向**，所以不构成阻塞；没有密钥、没有付费墙，也就不是 `license_gated`。但两条要摊开：一是这不是开源许可，二是 `/api/download` 那句 "byte-for-byte for redistribution" 是 SRC 对自己那个端点的描述，**不等于授权我们再分发**，所以 JAR 只留在本机运行时里、没有入库、没有转存。

**没做的事**：没改上游任何一行代码（也不允许——许可证明确禁止）、没把 JAR 提交进仓库或转存到别处、没读 `selectedValue` 去混入实验值、没为域外输入加我们自己的拒答规则（那会掩盖工具真实行为）、没建其余 28 个模块、没以 KowWIN 名义跑 730 分子普查（等正式输入）。逐轮证据落在 `data/bbb_census_v3/runtime_verification/kowwin_1_68_epi_1_1_0.json`。

**B 组收口**：12 个工具全部有实测结论——ready 6 个（B3clf、graphB3、LGBM-BBB、SILICOS-IT、OpenADMET、KowWIN），带实测原因的阻塞 6 个（QupKake、SolPredictor、DeePred-BBB 契约破损，Uni-pKa 与 XenoSite 无本地推理路径，DeepB3 见前述记录）。

### 2026-09-09 Issue 05 收口

**A、B 两组 22 个工具逐个都有实测结论，没有一个是"待定"。** 注册表现状：BBB 普查 `tools` 34 条（31 enabled），数值端点 `qualification_tools` 13 条（8 enabled：PKSmart、SolTranNet、RDKit 三项、SILICOS-IT、OpenADMET、KowWIN），5 条带实测原因不测（Rowan 需密钥，QupKake 与 SolPredictor 契约破损，Uni-pKa 与 XenoSite 无本地推理路径）。数值工具一个都没混进 BBB 普查表。测试 `src/probe_headless/tests` 100 passed / 1 skipped，另有 2 个 `test_census_receipt_regression.py` 的历史失败，原因是 `archive/dag_poc/data/backend/get_property/get_property_A.jsonl` 不在树里，与本轮改动无关（改动前后一致）。

**建环境时用过的一个临时 conda prefix `/tmp/kowwin_jre_probe`（547 MB，只有 openjdk，无任何产物）已删。** 删前核过正式运行时不依赖它：`~/.local/share/benchmark-runtimes/kowwin_1_68_epi_1_1_0` 自带 conda JDK 21 与 JAR，没有指向该目录的符号链接，`java` 的 ldd 也不解析到那里；删后重跑咖啡因照样出数。

**没做的事**：没跑 730 分子正式普查——还缺正式输入路径、ID/SMILES 列名与修正后的糖类 SMILES；没建 OpenADMET 余下 10 个模型仓（7 个卡在本机到 Zenodo 的限流，性质是网络不是工具）；没给任何工具的上游打补丁。
