# ADR-0003: 工具核验分三档 L3/W/V，L3 以"同版本跑两遍逐位一致"为准入线（2026-09-04 修订为标签跨轮稳定，见文末与 ADR-0004）

- 状态：Accepted
- 日期：2026-09-03
- 决策人：jingwu（grilling 15 问逐项裁定）

## 背景

`docs/Pharma_Tools_Survey.md` v4 与 `docs/step1_tool_inventory_v2.csv` 完成了 42 个工具的文献核实与"入口存活"检查，但**功能实跑数为 0**：核过的只是"文献真实存在"与"获取入口返回 200"，没有任何工具被安装、运行，没有一条预测值产出。v4 曾用"实测可用"描述这一状态，属过度声称，已更正为"入口存活"。

Step 1 需要的是 Proposal §3.2 的记录表——每个分子逐行记下结构、各工具预测值与判定、实验测定值、是否给出相反结论。要判断一个工具能不能用，唯一办法是拿分子喂进去看它出不出数。**因此"核验工具"与"造数据集"是同一遍工作**，核验产物不是日志，是数据集本体。

本 ADR 定义这遍工作的判据、档位与记录格式。

## 决策

### 一、三档判据

工具按可锁版本能力分三档，档位由部署形态决定，不由重要性决定。

**L3 档（进数据集判分）**：要求同时满足
1. 装得上，且版本可锁（PyPI 版本号 / git commit / Zenodo DOI 三者之一）
2. 对全部目标分子出输出，逐分子留 `raw_probability` 或对应数值
3. **同输入同版本跑多遍，标签跨轮稳定**；概率漂移只记录不拦截（2026-09-04 修订，原为"逐位一致"，见文末修订节）
4. 权重/依赖留 sha256 证据（照 `authority_audit.json` 既有格式）

**W 档（存证但不进判分列）**：仅 web 服务、版本不可知的工具。要求
1. 单次取值，且存**完整证据链**：请求 URL、请求参数、原始响应体全文、HTTP 状态码、访问时间戳（UTC）、解析出的数值、解析器版本
2. 隔日全量重取一次，记录漂移
3. 数值单独成列，**绝不与 L3 值混入同一判分列**

**V 档（只登记不测）**：商业需 license 的工具。表里留行、注明不可测原因，不产出数值。

### 二、L3 不适用于 web 与商业工具

L3 第 3 条要求"同**版本**跑两遍"。web 工具无法得知服务端权重版本，且状态会漂移——2026-09-03 实测：ADMETlab 3.0 上午 HTTP 200、下午 403；pkCSM 上午 200、下午 000；graphB3 web 上午 200、下午 000；LightBBB 唯一入口已 404 下线。商业软件无 license 装不上。**故 L3 只在本地可部署工具上可执行**，按 v2 主表 42 个工具统计：L3 可执行 22 个（原计 24，扣除归类错误的 ADMETlab 3.0 与实为 web 的 1 项）、W 档 9 个、V 档 8 个、待定 2 个。

### 三、记录格式：值列 + 状态列 + 原因列三件套

每个工具每轮出三列，不用哨兵值：

| 列名模式 | 类型 | 取值 |
| --- | --- | --- |
| `<round>__<tool_id>__value` | float，可空 | 成功时为原始数值；失败留空 |
| `<round>__<tool_id>__status` | enum | `success` / `install_failed` / `runtime_error` / `timeout` / `not_attempted` |
| `<round>__<tool_id>__failure_reason` | text，可空 | 失败时的原始报错摘要 |

禁止用 `-999` 之类哨兵值填数值列：它与真实概率同列，判分脚本一旦漏过滤即静默污染结论。

判定阈值不写进 worker。worker 只吐原始数值，label 由上层按端点契约计算（BBB 端点为 `probability >= 0.5`，沿用 `authority_audit.json` 的 `action_rule`）。

### 四、失败重试边界

3 次指数退避（1s / 4s / 16s），仍失败记 `timeout` 并继续下一个工具，不中断整批。隔日全量重取一次。理由：无限重试会让一个挂掉的服务卡死整批；单次不重试会把网络抖动误记为工具下线（pkCSM 在两次相隔数小时的测量里 200 与 000 各出现一次，无法排除抖动）。

### 五、既有 census 产物提升出 archive

`archive/cases/_methodology/tool_conflict_census/` 与 `tool_conflict_candidate_pool/` 是本提案的实证地基（730 分子 × 4 模型 × 4 轮、漂移 0、receipt 自哈希已复核一致），但物理上位于 `archive/`，而 archive 的规矩是"可参考、不纳入当前进度"。**这批产物提升到 `data/bbb_census_v3/`**，用 `git mv` 保留历史，移动后重算全部 sha256 确认未损坏。archive 仅保留旧六格项目。

### 六、批量跑批与 agent 评测是两条链，不共用执行器

- **Step 1 批量出数**：确定性执行器（改造 `src/probe_headless/tool_conflict_census.py`），receipt 里的 `command` 必须是可重放的固定命令行。
- **Step 3–4 agent 评测**：qoder CLI 作 harness（取其上下文管理与记忆），工具以 MCP 暴露并做白名单隔离，只给领域工具、不给 Bash/Read/Write/WebSearch。

两条链诉求相反：前者要可重放，后者要 agent 自主决策。共用一个执行器必然牺牲一头——若用 agent 驱动批量跑数，receipt 记录的是"某次会话"而非可重放命令，L3 第 3 条自动失效，且执行层噪声会与工具分歧混淆，破坏提案要测的信号。

隔离强度的既有实践（`src/probe_headless/ws_isolation.py` 已验证）：`--mcp-config` 指定专用配置 + `--strict-mcp-config` 阻断全局 MCP 继承 + `--allowedTools` 只列领域工具名。注意该文件记录的坑：`--allowedTools` 只管免批准，不管工具是否存在，三者必须叠加。qoder CLI 侧的等效能力**尚未实测**，是 Step 3 的前置验证项。

## 理由

1. L2（"能出一个数"）作为准入线太松：装错权重的模型照样吐出合法概率值。项目已在"入口存活≠可用"上出过一次错，不应在"能出个数≠部署正确"上再出一次。
2. L3 与既有 census 的实际标准对齐（4 轮重跑逐位一致 + 权重哈希 + 预登记先冻结）。新旧工具若用不同标准，数值无法并入同一张表比较。
3. web 工具的不可复现本身是可发表的观测（提案动机之一即工具生态不可靠），故 W 档保留存证价值，但其数值不得进入判分列，否则冲突来源无法区分是模型分歧还是服务漂移。

## 后果

1. `docs/Pharma_Tools_Survey.md` 与 `step1_tool_inventory_v2.csv` 的"可用性"列语义收敛为"入口存活"，功能状态另立 L3/W/V 列。
2. 批量跑批器需改造三处：输出根路径（`cases/` 已移入 archive，现路径失效）、工具清单（现硬编码 4 个 model_id）、失败处理（现 `exit_code != 0` 直接 `raise RuntimeError`，与"失败也记录并继续"相反）。改造前须跑通 `src/probe_headless/tests/` 三个测试作安全网。
3. ~~本机（macOS arm64）GitHub 与 HuggingFace 全通道受阻，12 个 L3 工具卡在此点。已批准配置本项目专用只读 deploy key 走 `ssh.github.com:443` 解除~~ **2026-09-04 更正（工单 03 实测）**：GitHub 的 HTTPS 通道本来就通——`github.com`、`raw.githubusercontent.com`、`api.github.com`、`pypi.org`、`zenodo.org`、EPA 下载页全部返回真实响应（正文长度均不等于代理拦截页的 1461 字节），并以三次真实 `git clone` 产出合法 SHA-1 对象为证。**故未生成任何 deploy key，也未改动 `~/.ssh/config`**；12 个工具的源码获取不再是阻塞项，11 个 GitHub 仓库 `git ls-remote` 全部成功。仍然不通的只有 `huggingface.co`（三次全 000），其中只有 OpenADMET 的官方权重在 HF 上，这一项划给远端环境不变。两条测量纪律随之确立：`nc -z` 在本机对这些主机全报不通，是假阴性，判连通一律以 curl 或真实 git 操作为准；且此结论有时效性——同一台机器 2026-09-03 对 GitHub 确实超时，故工单 05 装运行时须可重试、可断点续做，失败记 `fetch_failed` 而不中断整批。
4. 工具运行环境从 `/tmp/<tool>-runtime` 改为 `~/.local/share/benchmark-runtimes/<tool_id>/`，避免重启后重装。

## 相关

- [ADR-0001](0001-tool-identity-version-lineages.md)：工具同一性=名称+权重+训练数据+端点定义；重训版本是不同工具（ADMET-AI 1.4.0 与 2.0.1 各占一行）
- [ADR-0002](0002-tool-table-schema.md)：一行一工具、输出性质类型必填、可用性作主表门槛

## 2026-09-04 修订：L3 第 3 条从"逐位一致"松绑为"标签跨轮稳定"

> 本修订已于 2026-09-13 独立成文为 [ADR-0004](0004-verification-label-level.md)。以下原文按台账规矩保留不改写，后续引用请指向 ADR-0004。

**改了什么**：L3 准入第 3 条原文是"同输入同版本跑两遍，结果逐位一致（漂移为 0）"，现改为"跑多遍，标签跨轮稳定；概率漂移只记录不拦截"。默认重复轮次同时从 2 提到 8。

**为什么**：B3clf 逼出来的。它的特征由 PaDEL（一个 Java 描述符程序）计算，而 PaDEL 对同一份固定几何重复算会给出不同描述符，导致概率在小数点后第四位抖动——咖啡因在八次里取 0.9317634105682373 与 0.9326184391975403 两个值。但这个抖动幅度（约 0.0009）从未跨过 0.5 判定线，标签始终不变。逐位一致这条线会把 B3clf 挡在打分列外，而它的判定其实是稳定的。

**为什么标签级是对的口径**：普查判分歧本来就只看标签——`tool_conflict_census.pair_is_stable` 逐轮比的是 label，不是概率。概率漂移没有被丢弃，而是由 `summarize_model_runs` 的 `maximum_probability_drift` 逐模型记账。所以"标签稳定即准入、概率漂移另表记录"与普查的原生口径一致，逐位一致反而比普查本身更严。

**旧口径为何一度显得成立**：两遍重复对抖动工具的检出率太低——B3clf 的低值八次里只出现两次，跑两遍有很大概率两次都抽到同一个值而误判为"逐位一致"。提到八轮后 B3clf 的抖动稳定暴露。

**证据落点**：`verify_runtime.py` 现同时记录 `deterministic_across_runs`（概率级，观测用）与 `label_stable_across_runs`（标签级，准入用），逐轮取值见 `data/bbb_census_v3/runtime_verification/<tool_id>.json` 的 `value_spread`。

**副发现（不改判据，仅记录）**：B3clf 的 24 个权重是 scikit-learn 0.24.2 序列化的，本机运行时装的是 1.0.1；两者对同一输入的 `scaler.transform` 与 18 个非 xgb 分类器 `predict_proba` 实测逐位一致，证据见 `data/bbb_census_v3/sklearn_cross_version_equivalence.json`，故跨版本反序列化在数值上无损。
