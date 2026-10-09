# toolcheck — 43 个 tool 的隔离实测台

这个文件夹干一件事：**开一个只能看到那 43 个 tool 的 claude code 实例，逐个调一遍，看哪些真能用。**

## 为什么要隔离

这台机器上平时有一堆东西会混进 claude 的工具面，不隔离就测不准：

| 干扰源 | 具体是什么 | 不管会怎样 |
|---|---|---|
| `~/.claude.json` 顶层 `mcpServers` | 11 个 server（computer-use、arxiv、unlimited-search、**lingshu-mock** 等）| 工具面直接多出几十个，还会和被测 server 撞名 |
| `~/.claude/settings.json` 的 `enabledPlugins` | 4 个插件，其中 `chrome-devtools-mcp` 本身就带一整套 MCP 工具 | 同上 |
| `~/.claude/settings.json` 的 `hooks` | 28 类生命周期钩子 | 钩子可能改写工具调用与结果 |
| `~/.claude/skills/` | 几十个 skill | skill 会被当成 slash command 注入 |
| 仓库根的 `CLAUDE.md` 与 `.claude/settings.local.json` | 项目指令 + 一长串 Bash 白名单 | 把内置工具放回来 |
| shell 里的 `claude()` 函数 | 注入 `BUN_OPTIONS` 加载 loongsuite-pilot 的 fetch 拦截 | 请求被中间层改写 |
| **端口撞车** | 我方 mock 与灵枢本地版**默认都用 9000** | 连上的可能不是你以为的那个 server |

## 隔离靠六道闸

全部写在 [run.sh](./run.sh) 里，缺一道就漏：

| 闸 | 挡住什么 |
|---|---|
| `--mcp-config mcp.json` | 只加载本目录这两个 server |
| `--strict-mcp-config` | 忽略其他一切 MCP 来源（挡掉那 11 个 + 插件带的） |
| `--tools ""` | 关掉**全部**内置工具（Bash/Read/Write/WebFetch/Task/…） |
| `--setting-sources ""` | 不读 user/project/local settings（挡掉 hooks 与 enabledPlugins） |
| `--disable-slash-commands` | 关掉全部 skill |
| `cwd=workspace/` | 空目录，避开仓库的 CLAUDE.md 与 settings.local.json |

外加：直接调 `$HOME/.local/bin/claude` 而不走 shell 函数，绕开 fetch 拦截；端口错开成灵枢 9000 / 我方 **9100**。

**认证不落盘**：`run.sh` 运行时从 `~/.claude/settings.json` 的 `env` 块里只取 `ANTHROPIC_*` 注入进程环境，本目录任何文件都不含密钥。

## 隔离已验证

[results/init_isolation_proof.jsonl](./results/init_isolation_proof.jsonl) 是 claude 启动时自报的 init 事件，实测：

```
mcp_servers: [{"name":"ours","status":"connected"},{"name":"lingshu","status":"connected"}]
tools 总数: 43        内置 tool: 0        MCP tool: 43        slash_commands: []
```

## 文件说明

| 文件 | 作用 |
|---|---|
| `mcp.json` | 被测的两个 server，只有这两个 |
| `settings.json` | 权限配置，deny 掉全部内置工具名（`--tools ""` 之外的第二道保险），无密钥 |
| `run.sh` | 隔离启动器。`./run.sh "prompt"`，或 `FORMAT=stream-json ./run.sh "..."` |
| `probe_all.py` | 不经过模型、直连 MCP 逐个调 43 个 tool 的确定性探针（判定基准）|
| `patches/` | **灵枢侧 bug 的待 PR 补丁**，见下 |
| `results/probe_raw.json` | 探针的完整原始返回 |
| `results/claude_run.jsonl` | claude 实例的完整事件流 |
| `results/REPORT.md` | **实测结论与修复方案看这里** |
| `results/schemas.json` | 43 个 tool 的完整 inputSchema |

## patches/ 里的两份补丁

它们修的是 **灵枢仓**（`gene-damo/damo-gene-lingshu-mcp`）的 bug。那是另一个团队的生产仓，**不应由本项目静默提交**，所以只出补丁、走 PR。

| 补丁 | 修什么 | 验证 |
|---|---|---|
| `A1-clinicaltrials-403-ua.diff` | `search_clinical_trials` 恒返 403。根因：ClinicalTrials.gov 的 WAF 查 TLS 指纹与 UA 是否自洽，httpx 指纹配自定义 UA 被当作伪装 | ClinicalTrials **403 → 200**；PubChem/Reactome/STRING 三个对照无影响 |
| `A3-validate-target-honest-fallback.diff` | `validate_target` 未命中时闷不作声返固定常数（ABL1 被说成“结构数据有限”）| 兜底改成带 `warning` + `SHOWCASE_MOCK_FALLBACK` + `score: null`；GLP-1R 策展真值不变 |

应用方式：

```bash
cd /Users/jingwu/work/damo-gene-lingshu-mcp
git apply --check /Users/jingwu/work/benchmark/toolcheck/patches/A1-clinicaltrials-403-ua.diff   # 先试跑
git apply         /Users/jingwu/work/benchmark/toolcheck/patches/A1-clinicaltrials-403-ua.diff
```

第三个 A 类 bug（`get_property` 错误信息自相矛盾）在**我方**仓，已直接修在 `dag_poc/gateway.py`，不需补丁。

## 怎么复现

```bash
# 1. 起灵枢（34 个 tool），端口 9000
cd /Users/jingwu/work/damo-gene-lingshu-mcp
LINGSHU_DIAMOND_DISABLED=true LINGSHU_STORAGE_PROVIDER=local PORT=9000 uv run server.py

# 2. 起我方 mock（9 个 tool），端口 9100 —— 必须错开，默认也是 9000
cd /Users/jingwu/work/benchmark
PORT=9100 .venv/bin/python dag_poc/mcp_mock_server/mock_server.py

# 3. 确定性探针（判定基准）
cd toolcheck && python3 probe_all.py

# 4. 开 claude 实例实跑
FORMAT=stream-json ./run.sh "把 43 个工具各调一次，报哪些能用" > results/claude_run.jsonl
```

## 一条必须记住的前提

本次实测跑的是**本地部署**（`LINGSHU_DIAMOND_DISABLED=true`，即没有凭证）。灵枢**预发环境当时是挂的**——`GET/POST https://damo-gene-lingshu-mcp.pre-fn.alibaba-inc.com/mcp` 返回 `HTTP 500 / x-error-code: 1108 / SYSTEM:fiber_data_is_not_health`，DNS、TLS、路由都正常，是 FC 函数实例不健康。

所以报告里判成"后端没配置"的那些 tool，**不等于它们在生产环境也坏**，只能说本地测不了。要下最终结论必须等预发恢复后重跑一遍。
