# ADR-0008: 环境清空后 /tmp 幸存数据的消毒纳入与防复发口径

- 状态：Accepted
- 日期：2026-09-16
- 决策人：jingwu（owner 指令：「你再去搜索本机器，尽量恢复，然后把这些都纳入 git，避免重复情况出现」）
- 性质：范围声明 + 不变量。承接环境清空事故（[PR-69](../../problems.md) / [T-278](../../TODO.md) / [U-9](../../USER_TODO.md)），把「恢复 + 纳入 + 防复发」三件事的口径钉死。
- 连带：把 U-9 裁定① 的选项 (c)（force-add `runs_naked` 逐样本 p_y1 快照）**证死**——全盘搜证确认该数据在本机任何位置都不存在（见下「task23 重放」一节）。

## 背景：事故与恢复的边界

2026-09-16 02:52 `benchmark/` 工作目录被外部清空。跟踪文件已从远程 HEAD 完整复原（T-278）；丢的是 gitignored 的本地数据（run 现场、`.bench_runs/`、`refbench/data`、`api_evidence/`），这些从未入库、clone 带不回来。

owner 指令分三段，本 ADR 逐段落口径：

1. **再搜索本机器**：全盘搜证（`*__n1__2026090*`、`runs_naked`、`.Trash-0`、`benchmarks_bak`、第二 checkout、`/root/.claude/projects`）确认——BBB `runs_naked` 的逐样本 p_y1 在本机**无第二副本、不可恢复**。唯一实质幸存是 `/tmp` 下的一批松散文件与目录（`/tmp` 不在被清空的 `benchmark/` 树内，故躲过一劫）。
2. **尽量恢复**：把 `/tmp` 幸存数据按确定性边界收进仓库（见下「纳入范围」）。
3. **纳入 git，避免重复情况出现**：纳入树落在**已跟踪**的 `data/` 下，使其随远程备份；未来 run 现场是否也入库属既有 `.gitignore` 口径的再修改，列停机条件第 4 条交 owner（见下「防复发」）。

## 纳入范围（确定性边界，可复跑）

纳入树根目录：`data/recovered_tmp_evidence_20260916/`。四类，边界全部确定性、不靠人工逐个挑：

| 类 | 子目录 | 纳入判据 |
| --- | --- | --- |
| A 冻结工具资产 | `frozen_assets/` | sha256 **锚定**在已跟踪证据 JSON（`preregistration.json` / `authority_audit.json` / `tool_registry.json` / `runtime_verification/*.json` / `run_receipts/*.json`）里记过的文件，外加它们的同目录伴生文件（license/config）。锚定 = 这些字节正是当年冻结输入的真实出处。 |
| B run 现场 | `run_scenes/` | `/tmp/bench_runs`（1041 run）、`/tmp/bench_verify`、`/tmp/vf2` 的全部 run 目录（合计 1044 run），**减去 `.cfg/`**（见消毒）。 |
| C 顶层松散文件 | `tmp_files/` | `/tmp` 顶层普通文件，mtime ≥ 2026-08-14、大小 ≤ 2MB，减去拒收名单（他人项目、pip/qoder 噪声、凭据形态文件、已入 A 的重复项）。 |
| D 小型目录 | `small_dirs/` | 明确列举的探针/脚本目录（epa_probe、concerto_ro、ctx、dpb-730、dilipred-research、dimorphite125 等），减去 `.git`/`.cfg`/`node_modules`/`__pycache__`。 |

**排除（不纳入）**：他人项目（lingshu / pack 17G / cowan / gf_* / pancreas / GSE* / paper_page / perez / en_labels 等）、pip 与 qoder 噪声、可重新下载的 clone（PaperSpine，带 `.git`）、可重装的巨型 venv/缓存（admet-ai-runtime 949M、deeppurpose-runtime、gf_clone 274M、openadmet_probe 177M、deepredbbb-src/.git 68M）、顶层 >2MB 且未被 sha256 锚定的文件、凭据形态文件。

边界由生成器脚本固化，连跑产同一份 manifest（见「确定性与自检」）。

## 消毒：密钥绝不入库

纳入前与纳入后各做一道，全部在内存中比对、不把任何凭据片段打印到记录：

1. **源头权限漏口先关**：`/tmp/bench_runs/*/.cfg/settings.json`（1041 份，含 `env.ANTHROPIC_AUTH_TOKEN` 活 token）原本是 `0644` 他人可读（`/mnt` 是 `drwxrwxrwx`），承 T-264 同类漏口——已 `chmod 600`、`.cfg` 目录 `0700`。
2. **`.cfg/` 整个排除**：每个 run 的 `.cfg/`（settings.json + projects 会话记录 + backups + telemetry + .claude.json，共 ~145MB/run 总量 343MB）一律不纳入。内存核验确认：`.cfg` 里**只有** settings.json 含活 token，projects/backups/telemetry 不含；但 `.cfg` 无证据价值（是 harness 配置不是 run 结果），整体排除最干净。纳入后实测：全树 0 个 `.cfg`、0 个 `settings.json`。
3. **多模式密钥扫描**：纳入树 10660 文件全扫，模式覆盖 anthropic/openai/github/gitlab/aws-AKIA/slack/jwt/private-key/bearer/api_key 键值对，外加内存里的活 token 与旧硬编码 AK（`677b…`，自 `9f2bb28` 起在 origin/master 历史里的那把，owner 已裁「不需要换」）。**命中 0**。
4. **一处第三方凭据删除**：扫出 `tmp_files/rangehdr.txt` 含 figshare 预签名 S3 下载 URL 里的 `AKIA…` 串。虽属第三方公共数据托管、`X-Amz-Expires=10` 早已过期、不是本项目凭据，但按「含 AKIA 形态的文件不入库」纪律，**提交前删除**。删后复扫仍 0 命中。
5. **第三方 git 魔法文件去毒**：copytree 带进来的 4 个第三方源码树自带 `.gitignore`/`.gitattributes`（unipka-src、llm4chem-main、epa_probe/test-app、solpred_probe）会篡改本仓 git 语义（实测拦掉同级 7 个 `.bat`/`.json`）——重命名为 `_thirdparty_gitignore.txt` / `_thirdparty_gitattributes.txt`，**保留字节、去掉 git 魔力**。复查：纳入树被任何 gitignore 拦截的文件数 = 0。

## task23 重放：选项 (c) 证死

生成器对纳入树做了一项客观判定（`run_scenes.task23_replayable_from_ingest`）：是否存在任何 run 现场带逐样本 `p_y1`、且带 BBB entity id。结果 **`p_y1 = False`** ⇒ 纳入的 1041 个 run 是**上一版项目的 QED / lead-optimization runs**（meta.round = `vf` 等），**不是** task23 `discover_runs()` 要读的 BBB `runs_naked` 现场。

⇒ **U-9 裁定① 的选项 (c)（force-add `runs_naked` 逐样本 p_y1 快照）客观不可行**：那份数据本机已无任何副本。`DeterminismTests::test_rerun_byte_identical` 的处置因此**收敛为 (a) 改 skip / (b) 保持红**二选一，仍归 owner（见 U-9）。task23 的全部结论不受影响——其数字可由三个已跟踪文件独立核对（`naked_difficulty_crossref_bbb.json`、归档 `results.json`、冻结 `screening_ledger.jsonl`），断的只是「逐字节重放原始 run」这一条出处链。

## 防复发：纳入即备份，未来 run 现场入库属停机条件第 4 条

- **本次纳入已实现「这些数据的防复发」**：纳入树在 `data/`（已跟踪）下，随每次 push 进远程，不再只活在会被清空的本地。
- **未来 run 现场是否也入库** = 修改既有 `.gitignore` 的「`cases/**/runs_*/` 一律不入库」口径。该口径有实测出处（全项目 7315 run、曾 964 个误入库淹没 review、run 目录出过 AK 风险），**再修改它属停机条件第 4 条，须 owner 裁，本 ADR 不静默改 `.gitignore`**。两个候选方向交 owner：
  - **甲（轻量，不破口径）**：加定时 `git push` 或每日 `git bundle` 备份（含 force-add 的关键 run 现场快照），run 目录仍 gitignore，但关键现场有离线备份。对应 U-9 裁定②。
  - **乙（重，破口径）**：缩小 `.gitignore` 的 run 排除范围，只放行「已被某 task/ADR 指定为证据出处」的 run 现场入库（白名单式），其余仍排除。这条改 `.gitignore` 既有口径，须 owner 显式授权。
- 生成器脚本 `tools/build_recovered_manifest.py` 进仓，纳入树的可复算清单与密钥自检由此固化（承 §5.5「聚合数脚本计算」与 R5.3 确定性）。

## 确定性与自检

- `python3 tools/build_recovered_manifest.py` 从盘上重算 `data/recovered_tmp_evidence_20260916/manifest.json`：分类文件数/字节、run 现场模型分布、sha256 锚定数、密钥扫描命中数。**无时间戳、无随机**，连跑两次 md5 一致（实测 `15739821626abc774d275fa07c7a40d7`）。
- 体量如实登记（取自 `manifest.json`，非估算）：纳入树 **378.5MB / 10660 文件**——`frozen_assets` 161.5M（663 文件，其中 80 个 sha256 锚定、583 个伴生）、`run_scenes` 112.4M（8833 文件，1044 run、1016 个 answer.json）、`small_dirs` 89.9M（530 文件）、`tmp_files` 14.8M（634 文件）。最大单文件：epi-estimators-cli.jar 41M、deepredbbb data.csv 33M、admet_ai wheel 18M、cdk jar 17M、DeepPurpose tarball 11M——均为 sha256 锚定的冻结资产或其伴生，是证据链的真实出处，保留有据。push 体量较大，已据 owner「纳入 git」指令执行。

## 不撞停机条件的部分 vs 交 owner 的部分

- **本 ADR 已自行执行**（纯加法、不破口径）：搜索、消毒、纳入到已跟踪的 `data/` 下、生成器进仓、密钥扫描、第三方凭据删除、git 魔法文件去毒。这些不改任何判据实现、不动冻结输入、不改 `.gitignore` 既有排除。
- **交 owner 裁**（停机条件第 4 条）：未来 run 现场入库口径（甲/乙）、转红测试处置（(a)/(b)，因 (c) 已证死）、备份机制做到什么程度。三者登记在 U-9。
