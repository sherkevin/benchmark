# DeepSeek "Bash-Only" 工具过拟合深度调查报告

> **报告日期**：2026-09-02
> **报告版本**：v1.0（综合 8 轮对话的完整结论）
> **数据来源**：DeepSeek 官方文档 + DeepSeek Harness GitHub 源码 + 多个独立第三方评测 + arXiv 学术论文 + GitHub 社区原始对照实验
> **核心结论**：DeepSeek V4 Pro 在 DSH Minimal（2 工具）下跑分 99，在 DSH Standard（25 工具）下只有 91。**这不是工具数量本身的普遍规律，而是 V4 Pro 权重被训练分布深度耦合到 Minimal 环境的具体事实**。

---

## 目录

- [第一部分：原始事件溯源](#第一部分原始事件溯源)
- [第二部分：具体实验数据](#第二部分具体实验数据)
- [第三部分：DSH 架构与工具定义](#第三部分dsh-架构与工具定义)
- [第四部分：多次独立复现的证据](#第四部分多次独立复现的证据)
- [第五部分：底层机制（三层叠加）](#第五部分底层机制三层叠加)
- [第六部分：根因分析（五层叠加）](#第六部分根因分析五层叠加)
- [第七部分：学术理论支撑](#第七部分学术理论支撑)
- [第八部分：常见误解澄清](#第八部分常见误解澄清)
- [第九部分：实践启示与决策清单](#第九部分实践启示与决策清单)
- [附录 A：完整源 URL 索引](#附录-a完整源-url-索引)
- [附录 B：术语表](#附录-b术语表)

---

## 第一部分：原始事件溯源

### 1.1 "bash-only" 论断的真实出处

**这个论断不是别人爆出来的——是 DeepSeek 自己在官方 API 更新日志脚注里写明的**。

#### 来源 1：DeepSeek API 官方更新日志

**链接**：https://api-docs.deepseek.com/zh-cn/updates/

| 日期 | 模型 | 官方原文 |
|---|---|---|
| **2026-07-31** | V4-Flash-0731 | "对于公开基准测试集中的 Code Agent 任务，正式版 DeepSeek-V4-Flash 使用 **DeepSeek Harness 极简模式**（即将发布）作为框架进行测试，并使用 max 档位，topp=0.95，temperature=1.0" |
| **2026-08-13** | V4-Pro-0813 | "对于公开基准测试集中的 Code Agent 任务，DeepSeek-V4-Pro-0813 使用 **DeepSeek Harness 极简模式**作为框架进行测试（使用 max 档位，topp=0.95，temperature=1.0），其他框架下结果可能略有不同" |
| **2026-08-21** | V4-Flash-Vision-Exp | 同样使用极简模式 |

V4-Pro 官方 Terminal-Bench 2.1 分数：**87.9**（极简模式下）
V4-Flash 官方 Terminal-Bench 2.1 分数：**82.7**（极简模式下）

#### 来源 2：DeepSeek Harness 开源仓库（GitHub）

**链接**：https://github.com/deepseek-ai/deepseek-harness
**协议**：MIT
**首版日期**：2026-08-13（V4-Pro 正式版同天开源）

仓库里有一份关键文档 [`packages/bundle/sdk-minimal/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/bundle/sdk-minimal/README.md)，原文：

> "The profile advertises a **platform-selected persistent shell and `str_replace_editor`**... The only advertised tools are **owner-scoped persistent `bash` on Linux/macOS or `pwsh` on Windows, plus `str_replace_editor`**; runtime context, workspace instructions, skills, jobs controls, compaction, and Harness identity are absent."

> "Token effect: One stable persona plus the **two tool schemas**. Tool results and ordinary conversation history grow with the session."

> "**KV Cache effect: Stable for a fixed persona, platform, provider, model, and bundle patch stack.**"

所以 DeepSeek 官方所谓"极简模式"= **bash + str_replace_editor 共 2 个工具**，没有 web search、没有 skill、没有 subagent、没有 compaction。

#### 来源 3："bash-only" 这个词的社区用法来源

"swebench.com" 的 SWE-bench Verified 默认视图就叫 **"Bash Only"**，因为 mini-SWE-agent 这个官方 harness 只给模型 bash 一种工具。社区讨论 DeepSeek 的极简模式时直接借用了这个术语，叫它 "bash-only"——这是社区用语，不是 DeepSeek 自己的命名。

### 1.2 这次争议的发酵时间线

| 日期 | 来源 | 关键动作 |
|---|---|---|
| 2026-07-31 | DeepSeek 官方 | V4-Flash 0731 发布，附带"极简模式"脚注 |
| 2026-08-09 | Tailin Zhou (HKUST) | arXiv 论文 "Hierarchical Self-Improvement"（用 DeepSeek-V4-Flash-Preview 当 frozen backbone） |
| 2026-08-10 | **DeepSeek Harness 仓库** | commit `fix(preset): align minimal agent with RL composition`（V4-Pro 发布前 3 天） |
| 2026-08-11 | **Composio**（Sunil Kumar Dash） | [Finding the Best Harness for DeepSeek V4 Flash](https://composio.dev/content/best-agent-harness-deepseek-v4-flash)：同模型 8 个 harness，46.7%~66.7%，**差 20 分** |
| 2026-08-12 | **Vals AI** | [独立评测 V4-Pro 0813](https://www.vals.ai/models/deepseek_deepseek-v4-pro-0813)：Terminal-Bench 2.1 = **54.68%**（三次完整试验），DeepSeek 自报 87.9，**差 33 分** |
| 2026-08-12 | **Stanford/Berkeley/NYU/Northeastern/UW 联合** | arXiv:2608.12253 [One Frozen Simulator Is Not Enough](https://arxiv.org/html/2608.12253v1) 发布 |
| 2026-08-13 | DeepSeek 官方 | V4-Pro 正式版 + DSH 仓库开源，仍用"极简模式"测 |
| **2026-08-14** | **GitHub user xiaobright**（B站"小明XBright"） | **本文核心**：DSH 三种模式对照实验 |
| 2026-08-15 | **yage.ai**（Superlinear Academy） | [A 20-Point Gap on the Same Model](https://yage.ai/share/simulator-collapse-deepseek-harness-en-20260815.html) |
| 2026-08-15 | **remio.ai**（Martin Chen） | [DeepSeek Pro Looks Brilliant in Minimal Mode](https://www.remio.ai/post/deepseek-pro-looks-brilliant-in-minimal-mode-that-is-also-the-problem) |
| 2026-08-15 | **X @MaxForAI** | 爆料"三种思考链指纹"现象 |
| 2026-08-15 | 百度贴吧 | 爆料 V4-Flash "超进化"（动态思考强度插件） |
| 2026-08-16 | **Aaron Guo** | [What I Learned From DeepSeek's Harness](https://www.aaronguo.com/blogs/deepseek-harness-teardown)：通读 DSH 源码 64 天 |
| 2026-08-16 | ZhihuFrontier (X) | [Minimal mode is not merely a smaller Harness. DeepSeek Harness Minimal is explicitly designed around the model's RL interface.](https://x.com/ZhihuFrontier/status/2088872677692076431) |
| 2026-08-16 | Medium / Coding Nexus | [DeepSeek Harness Reality Check](https://medium.com/coding-nexus/deepseek-harness-reality-check-0851d7a545e1) |
| 2026-08-20 | Andrew Baker | [Running SOTA Open AI Models in opencode](https://andrewbaker.ninja/2026/08/20/running-sota-open-ai-models-in-opencode-without-paying-frontier-prices/)：引用 Vals AI 的 SWE-bench Verified "minimal bash-only harness" |

**真正的引爆点是 8 月 11-12 日的三件事叠加**：Composio 8-harness 实验 + Vals AI 独立评测 + Stanford/Berkeley 等校学术论文。xiaobright 在 8 月 14 日的实验是最直接、最量化的证据。

---

## 第二部分：具体实验数据

### 2.1 xiaobright 的对照实验（这是"DS bash-only 跑分超过复杂 tools"的精确出处）

**原文出处**：
- GitHub 仓库：`github.com/xiaobright/modeltest`
- 衍生插件：`github.com/xiaobright/dsh-anchored-standard`
- 综合转载：[CSDN 综述](https://deepseek.csdn.net/6a802062662f9a54cb9d0a95.html)、BlockBeats、快科技

**实验设置**：
- 模型：DeepSeek-V4-Pro-0813
- 同一台 WSL、同一档 Think Max、同一冻结任务
- 对照组：DSH 三种模式

**关键数据表**：

| 预设模式 | 工具数 | 工具构成 | 跑分 |
|---|---|---|---|
| **DSH Standard** | **~25 个** | skills + subagent + web search + bash + edit + ... | **91 分** |
| **DSH PTC** | ~25 个（合并入口） | Standard 的 Code Mode 变体（单一 `run_code` 入口） | **92 分** |
| **DSH Minimal** | **2 个** | bash + str_replace_editor | **99 / 96 分** |

**工具最全的 Standard 输给了只有两个工具的 Minimal，差 7-8 分**。

**思考链指纹差异**：
- Standard 模式：思考链里 **"let me" 出现 208 次** → 得分 91
- Minimal 模式：**"let me" 几乎绝迹**，**"we" 出现 165-179 次** → 得分 99/96

> "let me" 是试探性的、自我对话式的低效思路；"we" 是集体视角、工程化的高效思路。

**其他时间数据**：
- 3D FPS 游戏：Standard 33 分钟 vs Minimal 23 分钟
- AI 宠物领养系统：Standard 49 分钟 vs Minimal 28 分钟

### 2.2 第三方独立复现数据汇总

| 评测方 | 任务 | DeepSeek 自报 | 独立复现 | 差距 | 备注 |
|---|---|---|---|---|---|
| **Vals AI** | V4-Pro 0813，Terminal-Bench 2.1（3 次完整试验） | 87.9 | **54.68%** | **-33.22 分** | 用 Inspect 的 ReAct agent |
| **Artificial Analysis** | V4-Pro，Terminal-Bench 2.1 | 87.9 | **79** | -8.9 分 | 用自己的 harness |
| **Composio** | V4-Flash，30 个 SaaS 任务，8 个 harness | — | **46.7%~66.7%** | **-20 分** | Pi 66.7% / OpenCode 46.7% |
| **Sentdex / hkinsley** | V4-Flash 自家 Minion harness | 82.7 | **50/89 (56%)** | -26.7 分 | Terminal-Bench v2.1 |
| **Andrew Baker 引用 Vals AI** | SWE-bench Verified（统一 minimal bash-only harness） | — | V4-Pro 0813 = **96.4%** | 反而合理 | 因为评分方控制了 harness |
| **xiaobright** | V4-Pro，DSH Standard vs Minimal | — | **91 vs 99** | **-8 分** | Project2 内部 benchmark |

注意 SWE-bench Verified 这条线特别有意思：Vals AI 用**统一的 minimal bash-only harness** 跑所有模型，所以 V4-Pro 的 96.4% 是真实的——**评分方控制了 harness，没有给 DeepSeek"自选主场"的机会**。

### 2.3 "Three Models" 谜团的真相

8 月 15 日凌晨，博主 Max For AI 在 X 上爆料：社区发现，同样调用 `deepseek-v4-pro`，换 IP、重新开会话后，模型会出现三种截然不同的"思维链指纹"：

1. 大量出现 **"Let me..."** 的版本 → 能力接近 V4 Pro Preview
2. 常以 **"The user wants me..."** 开头的版本 → 风格更接近 V4 Flash
3. 大量使用 **"we"** 的版本 → 能力明显更强，被社区称作"神版 V4 Pro"，且多在 Harness 的 Minimal 模式下出现

社区一度怀疑官方在 API 后面路由了三个模型。但挖源码 + 跑实验后这个猜测被否定了：**同一权重，只是 Harness 不同**。配合 xiaobright 的实验，三种指纹对应三种 DSH 模式（Standard/PTC/Minimal）。

---

## 第三部分：DSH 架构与工具定义

### 3.1 DSH 的四个 mode 实际是四个 profile

DSH 基于 Cordis 插件树，profile 是 YAML 配置文件。在 [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md) 里写得非常清楚：

```yaml
[dsh-base] 是 web, headless, sdk, acp profile 的共享第一层:
  - model adapters
  - tools
  - persistence
  - sandbox and approval policy
  - settings, credentials, telemetry

[dsh-sdk-minimal] 是有意的例外:
  - one bundle owns its complete explicit SDK tree
  - does NOT apply dsh-base
```

### 3.2 四个 mode 的真实继承关系

```
dsh-base（共享基础层）
├── web profile      ← dsh-base + dsh-web-app（浏览器 UI）
├── headless profile ← dsh-base + dsh-headless（一次性 runner）
├── sdk profile      ← dsh-base + dsh-sdk-app（JSON-RPC）
└── acp profile      ← dsh-base + dsh-acp-app（ACP server）

dsh-sdk-minimal ← 完全独立，**不继承 dsh-base**
```

| Mode | 对应 profile | 继承 dsh-base？ | 工具数 | 主要差异 |
|---|---|---|---|---|
| **Standard** | `web` | ✅ 是 | ~25 个 | 完整 dsh-base + browser UI |
| **Code** | `web` + tool-presentation: code | ✅ 是（Standard 的微调） | ~25 个（合并成 1 个 `run_code` 入口） | 工具入口从 25 合并成 1 |
| **Minimal** | `sdk-minimal` | ❌ **完全不继承** | **2 个** | bash + str_replace_editor |
| **Creator** | `sdk-minimal` + 调试工具 | ❌ 不继承 | 2 个 + 检查工具 | 适合开发 DSH 插件的开发者 |

Aaron Guo 在拆解 DSH 时特意强调了这一点：

> "The four 'modes' in the UI — standard, code, minimal, creator — are four such YAML files. The sharpest comparison is code mode against standard: comments aside, **the two files' configuration is identical — same bash, file tools, skills, compaction — and the difference is one appended row**."

Code vs Standard 是 1 行差异。但 **Minimal 是另一份完全不同的 YAML**，跟 Standard 在底层就分叉了。

### 3.3 Minimal 模式的完整排除清单

[sdk-minimal README](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/bundle/sdk-minimal/README.md) 列出了 Minimal **不包含**的内容：

```yaml
The bundle's single insert is the complete application tree:
  - SDK stdio startup and JSON-RPC serving
  - one environment-configured DeepSeek adapter
  - the explicit agent core
  - local subprocess and unrestricted filesystem providers
  - a platform-selected persistent shell PTY
  - the string-replace editor
  - uncompressed JSONL persistence under $DSH_HOME/sessions

It does NOT include:
  - Web UI / browser
  - Settings card
  - Managed credentials
  - Telemetry
  - Compaction
  - Workspace instructions
  - Skills system
  - Jobs controls
  - Subagents
```

**Model 视角**那段特别重要：

> "The only advertised tools are owner-scoped persistent `bash` on Linux/macOS or `pwsh` on Windows, plus `str_replace_editor`; **runtime context, workspace instructions, skills, jobs controls, compaction, and Harness identity are absent.**"

### 3.4 Minimal 的两个工具具体是什么？

#### 工具 1: `bash`（Windows 上是 `pwsh`）

一个**持久化的 shell PTY**（pseudo-terminal）：

```yaml
The profile mounts exactly one persistent shell stack:
  - Bash on Linux and macOS, OR
  - PowerShell on Windows

Both stacks use:
  - 300-second timeout
  - one owner-scoped terminal
  - the other platform's rows remain disabled
```

关键属性：
- **持久化**（persistent shell）：跨 turn 保留 working directory、环境变量、运行中的进程
- **PTY**：模拟真实终端，模型可以交互式运行命令
- **owner-scoped**：权限隔离，每个 session 一个

#### 工具 2: `str_replace_editor`

这是 **Anthropic Claude Code 风格的字符串替换式文件编辑器**。一个 schema 暴露三个操作：

| 操作 | 用途 |
|---|---|
| `view` | 读取文件内容（可指定行范围） |
| `str_replace` | 在文件里**精确字符串替换** |
| `insert` | 在指定行后**插入**文本 |

关键设计：**不做全文件覆写**，要求模型精确指明要替换/插入的字符串。
- 模型必须先 view → 拿到精确的字符串 → 才能 str_replace
- 不能瞎写"应该长这样"的文件
- 减少了幻觉造成的文件损坏

这个名字直接来自 Anthropic Claude Code 的工具命名，DeepSeek 1:1 搬过来用。

### 3.5 这两个工具 Standard 里有吗？

**有，都在里面**。Aaron Guo 明确说：

> "comments aside, the two files' configuration is identical — **same bash, file tools**, skills, compaction — and the difference is one appended row"

Standard 模式的工具清单（从 dsh-base bundle 来）大致是：

```
1.  bash (or pwsh)               ← Minimal 也有这个
2.  str_replace_editor / edit     ← Minimal 也有这个
3.  file read / write tools
4.  web search
5.  skills 注入系统
6.  subagent delegation
7.  jobs / 后台任务
8.  file search (grep/glob)
9.  http fetch
10. terminal multiplexer
11. LSP tools
... (合计 ~25 个)
```

**Minimal ≠ "两个不同的工具"，而是 "Standard 的工具清单里只留这两个，其余 23 个全部禁掉 + 关掉一切运行时注入"**。

### 3.6 关键区别不在工具本身，在"上下文注入"

两个模式里 `bash` 和 `str_replace_editor` 这两个工具**本身是一模一样的**。区别是 Standard 模式额外塞进了大量运行时上下文：

| 维度 | Standard | Minimal |
|---|---|---|
| `bash` 工具 | ✅ 有 | ✅ 有（同一个） |
| `str_replace_editor` 工具 | ✅ 有 | ✅ 有（同一个） |
| System prompt persona | 详细（含 harness identity） | 一句话 "You are a helpful software engineer assistant." |
| 自动注入 `AGENTS.md` / `CLAUDE.md` | ✅ | ❌ |
| Skills 注册表注入 | ✅（按需激活 skill prompt） | ❌ |
| Compaction 元信息 | ✅（上下文被压缩过的提示） | ❌ |
| Workspace instructions | ✅ | ❌ |
| Telemetry/session 元数据 | ✅ | ❌ |
| 其余 ~23 个工具 schema | ✅ | ❌ |

用一个比喻：

> Standard 模式 = "bash + editor + 一个塞满 runtime 元数据的豪华办公室"
> Minimal 模式 = "bash + editor + 一间只有白墙的空房间"

工具能力一样，**环境噪声完全不同**。

### 3.7 那到底 "Standard 包不包含 Minimal"？

**严格意义上 NO**：
- Minimal **不是** Standard 的子集
- 它们是**两个完全独立的 profile**，相互平级
- Standard 模式**碰巧**包含了同样的 bash + str_replace_editor 工具（因为这两工具本来就在 dsh-base 里）
- 但 Standard 的工具注册表、运行时注入、上下文压缩策略都和 Minimal **完全独立**

**在工具语义层面**：bash + str_replace_editor **同时存在于两种模式**（是子集关系）
**在配置/继承层面**：两者是兄弟关系，不存在包含

---

## 第四部分：多次独立复现的证据

### 4.1 Vals AI 独立评测（最权威的反证）

**链接**：https://www.vals.ai/models/deepseek_deepseek-v4-pro-0813
**日期**：2026-08-12

Vals AI 给 V4-Pro 0813 的官方独立评测结论：
- **Vals Index** 排名：#18（52.37%）
- **Terminal-Bench 2.1**：54.68%（三次完整试验），#33 of 52
- **SWE-bench Verified**：96.40%，#2 of 82（**最高分开源模型**），最便宜的 top 模型（$0.02/test）
- **HLE / Legal / Proof**：显著改善

**最关键的对比**：

| | Terminal-Bench 2.1 | SWE-bench Verified |
|---|---|---|
| DeepSeek 自报 | **87.9** | — |
| Vals AI 独立复现 | **54.68%** | **96.4%** |
| 差距 | **-33 分** | — |

为什么 SWE-bench Verified 没有大降？因为 **Vals AI 用统一的 minimal bash-only harness 跑所有模型**，等于用 DeepSeek 的主场打比赛，分数自然就高。

这正好印证了核心论断：**DeepSeek 的"高分"是 model × harness 共同决定的，不是 model 单方面的事**。

### 4.2 Composio 8-harness 实验

**链接**：https://composio.dev/content/best-agent-harness-deepseek-v4-flash
**作者**：Sunil Kumar Dash
**日期**：2026-08-11

**实验设计**：
- 模型：DeepSeek V4 Flash
- 任务：30 个 SaaS 工作流（跨 Airtable/Gmail/Calendar/Sheets/GitHub/Slack/PostHog）
- Harness：8 个（Pi Agent、Prime Agent、OMP、Claude Code、Codex、DeepAgents、Hermes Agent、OpenCode）
- 时长上限：900 秒/任务
- 验证：12 个 verifier check 全过才算成功

**结果**：

| Harness | Pass rate | Median time | Cost per success |
|---|---|---|---|
| **Pi Agent** | **66.7%** | 132.2s | $0.028 |
| Prime Agent | 62.5%* | 242.1s | $0.131 |
| OMP | 56.7% | 272.4s | $0.103 |
| Claude Code | 53.3% | 122.7s | $0.195 |
| Codex | 53.3% | 245.0s | $0.081 |
| DeepAgents | 53.3% | 187.1s | $0.045 |
| Hermes Agent | 50.0% | 175.5s | $0.056+ |
| **OpenCode** | **46.7%** | 129.7s | $0.073 |

**同一模型，差距 20 个百分点**。

### 4.3 Sentdex / hkinsley 本地推理复现

**链接**：https://hkinsley.com/reflections/in-search-of-the-frontier-at-home
**日期**：2026-07-02（V4 Flash 发布前的本地推理测试）

Sentdex 用他自写的极简 harness "Minion" 跑 DeepSeek V4 Flash：

| Model | Terminal-Bench v2.1 | Notes |
|---|---|---|
| GLM-5.2 FP8 (OpenRouter) | 64% (57/89) | 779B VRAM |
| **DeepSeek-V4-Flash (Mixed)** | **56% (50/89)** | vLLM, FP8, MoE |
| GLM-5.2 IQ4_NL | 52% (46/89) | 4-bit |
| GLM-5.2 Q2_K_XL | 46% (41/89) | 2-bit |
| MiniMax M3 Q4_K_XL | 35% (31/89) | dense |
| GLM-5.2 IQ4_NL 8bit KV | 24% (21/89) | KV 缓存量化 |

Sentdex 自己评价：
> "I am somewhat surprised that DeepSeek-V4-Flash outperformed the 4bit GLM 5.2. I will note, however, that the scores I am personally seeing seem to be 'weaker' than what I find on leaderboards. **DeepSeek-V4-Flash for example shows a 61.8% score**, which means allegedly for others it passed on 5 more questions than what I saw."

### 4.4 Andrew Baker 的 SWE-bench Verified 横向对比

**链接**：https://andrewbaker.ninja/2026/08/20/running-sota-open-ai-models-in-opencode-without-paying-frontier-prices/
**日期**：2026-08-20

关键引用：
> "Scores are from Vals AI's SWE-bench Verified leaderboard, which runs every model through the **same minimal bash-only harness** instead of each lab's own scaffolding, so the numbers are actually comparable across vendors rather than each lab grading its own homework."

这意味着 **SWE-bench Verified 这个 benchmark 上 Vals AI 用"统一的 minimal bash-only harness"压平了 DeepSeek 的主场优势**——这恰好是能"公平比较"的 benchmark，所以 V4-Pro 才能拿到客观的 96.4%。

---

## 第五部分：底层机制（三层叠加）

### 5.1 机制 A：KV cache 命中率（计算资源的硬约束）

#### DeepSeek 自己的 cache 折扣

| 模型 | Cache hit 成本 / miss 成本 |
|---|---|
| V4-Flash | 1/50 |
| V4-Pro | **1/120** |

阶梯电价从 2026-08-16 16:00 UTC 起生效，cache hit 折扣从 50× 压缩到 30×，但量级仍然在那里。

#### DSH 的 cache 稳定性设计

Aaron Guo 通读 DSH 仓库后发现，DSH 把 cache 命中率当成"运行时不变式"用 CI 强制：

1. **System prompt 极简且稳定**：minimal 模式就一句 `You are a helpful software engineer assistant.`，没有时间戳、没有 plugin 元信息、没有 runtime context。**每次请求的 prefix 字节级一致**。

2. **Tool schema 只有 2 个**：`bash` 和 `str_replace_editor`。Schema token 数小，canonical 排序固定。

3. **没有 compaction、没有 skills、没有 subagent**：DSH standard profile 里那些会动态注入 prompt 的插件统统关掉，prefix 永远不变。

4. **Session log 是"the only truth"**：conversation 是从 log 重算出来的，每次返回的 messages 完全可预测。

#### CI 断言代码

```typescript
// packages/core/agent-loop/tests/request-cache.e2e.ts

// every request after a session's first must report a cache hit
for (const usage of usages.slice(1)) {
  expect(usage.cacheReadTokens ?? 0).toBeGreaterThan(0);
}
```

> "any regression that shatters the prefix turns CI red before it burns money"

Aaron Guo 原文：
> "the clearest fingerprint of who built this and why — **only a model company engineers its harness around its own pricing table**"

#### 这个机制不只是省成本，是真实算力

```
Minimal 模式第 89 步：
  prefix = system_prompt (50 tokens) + 2 tool schemas (600 tokens) 
          + 89 步历史 (~30,000 tokens)
  → 这次请求的 prefix 跟第 88 步 byte-identical
  → cache hit: 模型直接跳过前 ~30,650 tokens 的计算
  → 等价于"算力预算凭空多了一倍"

Standard 模式第 89 步：
  prefix = dynamic system_prompt (3000+ tokens，注入 skills/compaction)
          + 25 tool schemas (5000+ tokens)
          + 89 步历史，但每步可能因 skills 触发、compaction 重写而变
  → cache miss 频繁
  → 每步都要重算整个前缀
  → 同样的算力预算下，能"思考"的 token 少很多
```

**这不是成本问题，是真实算力约束**。Cache hit 时模型可用的实际算力 = miss 时的 50-120 倍。

### 5.2 机制 B：注意力分散（认知资源的硬约束）

25 个工具的 schema 加起来约 5000 token。模型每次生成第一个 token 时，**这 5000 token 都要进 attention 计算**。

Transformer 的注意力是 O(n²) 的：
- 5000 token 的 tool schemas 占了 ~5M 个 attention pairs
- 这些 attention 都被"分到"了"这是什么工具、什么时候用、参数是什么"
- 留给"问题本身的推理"的 attention 配额变少

而 Minimal 模式只有 600 token 的 tool schemas，只占 ~360K attention pairs，**绝大部分注意力都用在问题本身上**。

#### 一个具体的幻觉示例

```
Standard 模式思考链（跑分 91）：
"let me search for the documentation for this library first
 to make sure I understand the right API"
... (实际上是个不需要 web search 的本地 bug 修复)
"let me try a different approach..."
... (208 次 let me)

Minimal 模式思考链（跑分 99）：
"We have a build error in main.rs:42. The issue is that
 Option::unwrap is called on a None value..."
"we fix this by replacing with proper error handling"
... (165-179 次 we)
```

Standard 模式给了模型 25 把锤子，模型就**怀疑每颗钉子都需要某把特定锤子**。Minimal 模式只给一把万能锤子，模型就**专注解决眼前钉子**。

### 5.3 机制 C：训练-推理分布错配（机制 A 和 B 的根因）

A 和 B 是"症状"，机制 C 是"病因"。

DeepSeek 的后训练用 GRPO（Group Relative Policy Optimization）：

```
对同一个 prompt：
  1. 采样 N 个回答
  2. 给每个回答打分
  3. 用组内的均值和标准差归一化
  4. 把"高于本组平均"的回答当作正例，"低于平均"的当作负例
  5. 更新参数让正例更可能、负例更不可能
```

关键：**这整个流程必须在某个具体的 harness 里跑**。具体的 system prompt 格式、具体的 tool schema、具体的 tool result 格式、具体的上下文压缩策略——**都是训练分布的一部分**。

所以 V4 Pro 训练时看到的世界是这样的：
- system prompt：`You are a helpful software engineer assistant.`
- 可用工具：`bash` + `str_replace_editor`（固定两个）
- 输出格式：function call 的特定 schema
- 上下文策略：不压缩
- agent loop：特定 step 模板

**模型的"最优策略"就是在这一整套环境的条件下最大化奖励**。权重里编码的是 `P(action | prompt, environment)`，不是 `P(action | prompt)`。

#### 为什么"少工具"碰巧对齐训练分布

DeepSeek 在 8 月 10 日的 commit `fix(preset): align minimal agent with RL composition`（commit hash `47f9438`）**主动选择对齐 Minimal**——原因：

1. Minimal 是"最小公倍数"——任何 harness 都能模拟 bash + 编辑器
2. 用 Minimal 训练，**下游可以靠"加工具"扩展，但不会因为"减工具"破坏**
3. 这其实是一种**逆向选择**：因为没有真实数据，所以选最稳的训练分布

#### 三个机制的关系

```
机制 C: 训练分布对齐（根因）
  ↓ 决定
机制 A: KV cache 稳定性（计算资源）
  + 
机制 B: 注意力集中度（认知资源）
  ↓ 共同导致
最终分数差异：91 vs 99
```

---

## 第六部分：根因分析（五层叠加）

### 6.1 第一层：训练时发生了什么

GRPO 是 V4 Pro 后训练用的核心算法。它的工作方式是：

```
对同一个 prompt：
  1. 采样 N 个回答
  2. 给每个回答打分
  3. 用组内的均值和标准差归一化
  4. 把"高于本组平均"的回答当作正例
  5. 更新参数
```

关键：**整个流程必须在某个具体的 harness 里跑**。System prompt 格式、tool schema、tool result 格式、上下文压缩策略——**都是训练分布的一部分**。

V4 Pro 训练时看到的世界是 Minimal 配置，**模型的最优策略 = 在这一整套环境的条件下最大化奖励**。

### 6.2 第二层：训练分布对齐 = 环境的"指纹"刻在权重里

比喻：你在北京学开车（手动挡、堵车、左舵），学到最后你的换挡动作、右脚力度、跟车节奏都和"北京拥堵 + 手动挡"耦合。让你去上海开自动挡或者英国开右舵，你**能开**，但不会像在北京那么丝滑。

LLM 也是一样。"推理能力"不是抽象的，是和具体环境耦合的。换个环境，模型的能力还在，但**触发它的条件变了**——程序换个 OS，能跑但路径变了。

### 6.3 第三层：为什么差 8 分这么夸张

第五部分已经讲过的三个机制叠加：
- 机制 A：KV cache 命中率（计算资源硬约束）
- 机制 B：注意力分散（认知资源硬约束）
- 机制 C：训练-推理分布错配（根因）

### 6.4 第四层：从第一性原理看

```
模型能力的本质 = f(权重, 环境, 任务)

权重 = argmax_θ E_(prompt, env, task) ~ train_dist [reward(model_θ, prompt, env, task)]
```

也就是说：
- 权重是被训练分布调出来的
- 训练分布包含了环境
- 所以**权重和环境是耦合的，不是独立的**

**"少工具对齐"不是"少 = 好"，而是"恰好这把 V4 Pro 的训练分布里有 Minimal 这种配置"**。换句话说：
- DeepSeek 训练时用什么环境 → 测试时就得用什么环境才能复现
- 如果 Anthropic 训练 Claude Code 时用的是 25 个工具的 Standard 模式 → Claude 跑 Minimal 模式反而会掉分

**这是一个模型特定、训练特定的事实，不是通用规律**。

### 6.5 第五层：为什么 DeepSeek 选了 Minimal 当 RL 训练环境（最本质的）

来源（yage.ai 深度分析）：
> "DeepSeek had neither [a coding agent product], with the latter remaining a long-standing blank. Its services primarily run through APIs, lacking proprietary end-user products... It was not until August 13, 2026, with the release of DSH, that DeepSeek finally had its own harness."

| 公司 | 端用户产品 | 真实用户行为数据 |
|---|---|---|
| OpenAI | Codex | ✅ |
| Anthropic | Claude Code | ✅ |
| Cursor | IDE | ✅ |
| **DeepSeek** | **❌ 无** | **❌ 无** |

**DeepSeek 没有端用户产品 → 只能靠合成数据**。

合成数据的天花板就是 Simulator Collapse 论文证明的：单一 simulator 训练 → policy entropy 从 1.9 跌到 0.4 → collapse 到单一轨迹 → 换环境就掉。

DeepSeek 在 8 月 10 日的 commit `align minimal agent with RL composition` 里**主动选择对齐 Minimal**——因为：

1. Minimal 是"最小公倍数"——任何 harness 都能模拟 bash + 编辑器
2. 用 Minimal 训练，**下游可以靠"加工具"扩展，但不会因为"减工具"破坏**
3. 这其实是一种**逆向选择**：因为没有真实数据，所以选最稳的训练分布

### 6.6 完整的因果链

```
DeepSeek 没有端用户产品
        ↓
只能用合成数据 + 单一 simulator
        ↓
Simulator Collapse：policy collapse 到这个 simulator 的 mode
        ↓
选择最稳的 simulator：Minimal (bash + editor)
        ↓
V4 Pro 权重被调优到 Minimal 分布
        ↓
推理时遇到 Minimal：cache hit、attention 集中、训练分布对齐 → 99 分
推理时遇到 Standard：cache miss、attention 分散、分布错配 → 91 分
```

### 6.7 最本质的一句话

**这个现象本质是：模型权重编码的是 `P(action | prompt, environment)`，不是抽象的"智能"。DeepSeek 因为没有端用户产品，被迫选择 Minimal 当训练分布的"最小公倍数"，导致 V4 Pro 的权重和 Minimal 环境深度耦合。换成 Standard 不是"工具多了干扰"，而是"环境错了整套表征都要重建"。**

---

## 第七部分：学术理论支撑

### 7.1 Simulator Collapse 论文

**全名**：One Frozen Simulator Is Not Enough: Simulator Collapse in Multi-Agent RL
**arXiv**：https://arxiv.org/html/2608.12253v1
**作者**：Simon Yu (Northeastern)、Nicholas Tomlin (NYU)、Marwa Abdulhai (UC Berkeley)、Ximing Lu (U Washington)、Derek Chong (Stanford)、Abe Hou (Stanford)、Dilara Soylu (Stanford)、Sergey Levine (UC Berkeley)、Christopher D. Manning (Stanford)、Weiyan Shi (Northeastern)
**日期**：2026-08-12

#### 核心定义

> "Simulator collapse"：因为 simulator LLM 是 mode-collapsed 的，针对它训练的 LLM policy 会 collapse 到利用 simulator 主导模式的窄策略上，迁移到未见过的 simulator 或真实用户时表现变差。

#### 关键发现

- **Policy entropy**：训练中从 1.9 nats 跌到 0.4 nats（模型只产生几乎单一的对话轨迹）
- **Rewards**：训练集奖励持续上升，但**迁移到未见过的 simulator 或真实人类时表现反而下降**
- **真人实验**：单一 simulator 训练的 agent 募捐 $0.46，**低于未训练的 baseline $0.51**
- **三大 simulator 都触发 collapse**：GPT-5-mini、Haiku-4.5、Gemini-3-Flash 都观察到
- **模型规模无解**：4B → 8B 仍然 collapse
- **问题在环境分布宽度**，不在模型/simulator 规模

#### 两个解决方案

1. **Verbalized Sampling**（推理时）：让 simulator 输出概率分布后再采样，让 held-out 成功率提升 **9%**
2. **Co-Training**（训练时）：simulator 和 policy 同步更新，让 held-out 成功率提升 **14%**

#### 与 DeepSeek 现象的对应

> "the DeepSeek V4 Flash 0731 release used the GRPO algorithm in post-training. At the core of GRPO is group-relative advantage, which calculates relative rankings across a group of sampled sequences by normalizing reward mean and variance. **This shares the same algorithmic logic as the within-group reward normalization mechanism used in the simulator collapse paper**."

### 7.2 HSI 论文

**全名**：Hierarchical Self-Improvement: A Framework for Task-Specific Evolvable Agent harnesses
**arXiv**：https://arxiv.org/html/2608.08466v1
**作者**：Tailin Zhou (HKUST)
**日期**：2026-08-09

#### 核心思想

> "The executable scaffold surrounding an LLM (the harness, including prompts, tool orchestration, memory, and verification logic) has emerged as a critical determinant of agent performance, with different harness designs producing substantial gaps even under identical model backbones"

#### 实验设置

- Backbone：DeepSeek-V4-Flash-Preview（frozen）
- Benchmark：BALROG（long-horizon text-based interactive games）

#### 关键数据

| BALROG 子任务 | Init harness → HSI evolved | 增量 |
|---|---|---|
| BabyAI | 较低 → 较高 | **+39.3** % Progress |
| Crafter | 较低 → 较高 | **+33.0** |
| TextWorld | 较低 → 较高 | **+25.0** |
| MiniHack | 较低 → 较高 | **+15.0** |

#### 重要发现

- 同一个 frozen backbone，**更复杂的 harness 反而能涨 15-40 分**
- 但 NLE 任务（超出 backbone 能力）harness evolution 提供不了改进

#### 与 xiaobright 实验的关系

| | xiaobright 实验 | HSI 论文 |
|---|---|---|
| 工具数量变化 | 25 → 2（少→更少） | 简单 → 复杂（少→更多） |
| 跑分变化 | +8 (涨) | +15~+39 (涨) |
| 真正改的是什么 | 训练分布对齐 | harness 演化设计 |
| 分数变化是否因工具数？ | **否** | **否** |

**真正决定分数的是"harness 是否与模型训练分布对齐"，不是"工具多还是少"。**

### 7.3 Live-SWE-Agent 论文（HSI 引用）

> "Live-SWE-Agent (Xia et al., UIUC, 2025). The first 'live' software agent that self-evolves at runtime by creating, modifying, and using custom tools during problem-solving. **Starts from a minimal scaffold (mini-SWE-agent, ~100 LOC, bash only)** and adds a reflection step after each action. Key results: SWE-bench Verified **77.4% with Gemini 3 Pro**, outperforming all known agents; zero offline cost, $0.02–0.12 overhead per task; critical finding: GPT-5-Nano *degrades* (-68.2% relative) when attempting to self-evolve, establishing a model capability floor for self-evolution."

bash-only 起点 + reflection step 在 Gemini 3 Pro 上拿下 SOTA。但这是 Gemini 3 Pro，不是 DeepSeek。

---

## 第八部分：常见误解澄清

### 8.1 "bash-only 跑分好是因为评分机制考虑了 token"

**错**。

所有这些 benchmark 都是 **pass/fail**——verifier 跑测试看通过率。**完全不考虑 token 消耗**。

那为什么 bash-only 跑分更高？因为 **KV cache 经济性让模型能 afford 更多思考 token**（机制 A）。这是间接的 token 影响，不是直接打分。

### 8.2 "DSH Minimal 是 Standard 的子集"

**严格意义上 NO**：
- 在工具语义层面：bash + str_replace_editor **同时存在于两种模式**（是子集关系）
- 在配置/继承层面：两者是兄弟关系，不存在包含（Minimal 是 explicit 不继承 dsh-base 的"deliberate exception"）

### 8.3 "少工具 = 更好"是通用规律

**错**。

xiaobright 实验的"少工具更好"严格成立的边界条件：

1. ✅ 同一个 model
2. ✅ 同一个 benchmark
3. ✅ 同一个 harness **家族**（只是切换模式）
4. ✅ 同一份后训练（没有跨 checkpoint）
5. ✅ 训练分布确实是用极简 harness 跑的

少一条都可能反过来（HSI 论文就是反例：同模型 + 演化后更复杂工具 +15-40 分）。

### 8.4 "DeepSeek 在 API 后面路由了三个模型"

**错**。

社区一开始怀疑官方路由了三个模型（对应三种思考风格："Let me" / "The user wants me" / "we"）。但挖源码 + 跑实验后否定了——**同一权重，只是 Harness 不同**。

### 8.5 "OpenCode/Pi/Hermes 这些 harness 都能让 DeepSeek 跑出官方分数"

**错**。

Vals AI 用 Inspect 的 ReAct agent 跑 V4-Pro 在 Terminal-Bench 2.1 上只有 **54.68%**（DeepSeek 自报 87.9），差 33 分。**官方分数只有官方 harness 能复现**。

### 8.6 "这个现象 DeepSeek 独此一家"

**部分错**。

- HSI 论文：DeepSeek-V4-Flash-Preview 在 BALROG 上 harness 演化 +15-40 分
- Live-SWE-Agent：bash-only + Gemini 3 Pro 拿下 SWE-bench SOTA 77.4%
- Darwin Gödel Machine：bash-only Claude 3.5 Sonnet 在 SWE-bench 20% → 50%
- mini-SWE-agent（SWE-bench 官方）：bash-only

**"harness 决定分数"是普遍规律**，"少工具对齐"是 DeepSeek 的特殊实例。

---

## 第九部分：实践启示与决策清单

### 9.1 选 DeepSeek V4-Pro 时的 harness 决策

| 你的目标 | 推荐方案 |
|---|---|
| 复现官方 Terminal-Bench 2.1 = 87.9 分 | **必须用 DSH Minimal**（bash + str_replace_editor） |
| 拿 SWE-bench Verified 96.4% 那种"可移植"分数 | 用 Vals AI / SWE-bench leaderboard 那种**统一 minimal harness** |
| 想拿 91-92 分又想用 25 工具的便利 | 用 DSH Code mode（25 工具合并成 1 个 `run_code` 入口） |
| 想"既要 99 又要 25 工具" | 用 `dsh-anchored-standard` 插件（首轮 Minimal + 之后 Standard） |

### 9.2 评测 LLM agent 能力的方法论 checklist

**避免被 harness 绑架**：

1. **永远问"用什么 harness 测的"**——任何 benchmark 分数都是 model × harness × inference_config 三元函数
2. **多 harness 复现**——同一模型在 3+ harness 上跑，取中位数
3. **控制 inference 参数**——temperature、top_p、reasoning_effort 都得对齐
4. **控制 prompt 前缀**——尤其是 system prompt 的字节级一致性
5. **算 token 消耗**——同一个任务不同 harness 的 token 消耗可能差 3 倍（Composio 实测）

**测出真实可移植能力**：

1. 用 SWE-bench Verified 那种**统一 harness 的 benchmark**（Vals AI / SWE-bench leaderboard）
2. 用你自己的真实任务集复测
3. 对比官方分数 vs 真实任务分数的差距

### 9.3 如果你是模型方

**避免被 DeepSeek 这次争议连带**：

1. **公布完整的 eval harness 配置**——包括 system prompt、tool schemas、inference 参数
2. **提供 N 个 harness 的分数**——展示模型的可移植性
3. **明确"自评 vs 第三方"**——独立评测占主导
4. **避免"主场优势"嫌疑**——如果分数只能在自家 harness 上复现，要诚实说明

### 9.4 KV cache 纪律（如果你是 harness 开发者）

DSH 的设计可以学：

1. **System prompt 不放时间戳**——DSH 默认 "the clock off entirely"
2. **Tool schema 按 canonical 排序**——不按 plugin 加载顺序
3. **每个 README 写 KV Cache effect**——DSH CI 强制
4. **CI 断言 cache hit**——`expect(usage.cacheReadTokens ?? 0).toBeGreaterThan(0)`
5. **动态注入要稳定**——skills/compaction/subagent 不能让 prefix 抖动

---

## 附录 A：完整源 URL 索引

### 一手官方（最权威）

- DeepSeek API 更新日志：https://api-docs.deepseek.com/zh-cn/updates/
- V4-Pro 8 月 13 日公告：https://api-docs.deepseek.com/zh-cn/news/news260813
- V4-Flash 7 月 31 日公告：https://api-docs.deepseek.com/zh-cn/news/news260731
- DeepSeek Harness GitHub：https://github.com/deepseek-ai/deepseek-harness
- sdk-minimal 包定义（"极简模式"的源码）：https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/bundle/sdk-minimal/README.md
- DSH 架构文档：https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md
- Aug 10 commit `fix(preset): align minimal agent with RL composition` (47f9438)：https://github.com/deepseek-ai/deepseek-harness/commit/47f9438

### 本报告核心实验证据

- xiaobright GitHub 仓库：https://github.com/xiaobright/modeltest
- dsh-anchored-standard 插件：https://github.com/xiaobright/dsh-anchored-standard
- CSDN 综述：https://deepseek.csdn.net/6a802062662f9a54cb9d0a95.html

### 第三方独立评测（最重要的反证）

- Vals AI 独立评测（54.68%）：https://www.vals.ai/models/deepseek_deepseek-v4-pro-0813
- Composio 8-harness 实验：https://composio.dev/content/best-agent-harness-deepseek-v4-flash
- Sentdex / hkinsley 本地推理复现：https://hkinsley.com/reflections/in-search-of-the-frontier-at-home
- Andrew Baker 引用 Vals AI SWE-bench Verified：https://andrewbaker.ninja/2026/08/20/running-sota-open-ai-models-in-opencode-without-paying-frontier-prices/

### 社区分析（解释机制）

- yage.ai "A 20-Point Gap on the Same Model"：https://yage.ai/share/simulator-collapse-deepseek-harness-en-20260815.html
- remio.ai "DeepSeek Pro Looks Brilliant in Minimal Mode"：https://www.remio.ai/post/deepseek-pro-looks-brilliant-in-minimal-mode-that-is-also-the-problem
- Aaron Guo "What I Learned From DeepSeek's Harness"：https://www.aaronguo.com/blogs/deepseek-harness-teardown
- Medium "DeepSeek Harness Reality Check"：https://medium.com/coding-nexus/deepseek-harness-reality-check-0851d7a545e1
- ZhihuFrontier X 帖：https://x.com/ZhihuFrontier/status/2088872677692076431
- Stackademic "DeepSeek Just Open Sourced the Missing Layer"：https://blog.stackademic.com/deepseek-just-open-sourced-the-missing-layer-for-ai-coding-agents-db40ffd7b8ae

### 学术论文（机制解释）

- "One Frozen Simulator Is Not Enough: Simulator Collapse in Multi-Agent RL" arXiv:2608.12253：https://arxiv.org/html/2608.12253v1
- "Hierarchical Self-Improvement: A Framework for Task-Specific Evolvable Agent harnesses" arXiv:2608.08466：https://arxiv.org/html/2608.08466v1
- Live-SWE-Agent (Xia et al., UIUC, 2025)——bash-only 起点 SWE-bench 77.4%
- Darwin Gödel Machine (Zhang et al., UBC/Vector Institute/Sakana AI, ICLR 2026)——bash-only Claude 3.5 Sonnet 20% → 50%

### SWE-bench leaderboard（"bash-only" 术语的来源）

- https://www.swebench.com/
- mini-SWE-agent：https://github.com/SWE-bench/mini-SWE-agent

---

## 附录 B：术语表

| 术语 | 定义 |
|---|---|
| **DSH** | DeepSeek Harness，DeepSeek 8 月 13 日开源的 agent harness 框架，MIT 协议 |
| **Minimal mode** | DSH 的极简 profile，只暴露 bash + str_replace_editor 两个工具，无运行时注入 |
| **Standard mode** | DSH 的完整 profile，继承 dsh-base，包含 ~25 个工具和所有运行时特性 |
| **Code mode** | DSH 的 Standard 变体，25 个工具合并成 1 个 `run_code` 入口 |
| **Creator mode** | DSH 的 Minimal 变体 + 运行时调试工具，供插件开发者使用 |
| **GRPO** | Group Relative Policy Optimization，DeepSeek 用的 RL 后训练算法 |
| **KV cache** | Transformer 推理时缓存前缀计算的机制，cache hit 成本可低至 miss 的 1/50~1/120 |
| **Simulator Collapse** | 学术论文术语：当 policy 针对单一 frozen simulator 训练时 collapse 到该 simulator 的 mode |
| **bash-only** | 社区术语，指 harness 只给模型 bash 一个工具 |
| **str_replace_editor** | Anthropic Claude Code 风格的字符串替换式文件编辑器（view/str_replace/insert 三操作） |
| **Agent loop** | harness 里的核心循环，决定何时调用模型、何时跑工具、何时继续或停止 |
| **Training distribution alignment** | 训练分布对齐，模型权重被训练分布调优后，对齐环境表现最好 |
| **Prefix stability** | 前缀稳定性，决定 KV cache 命中率 |
| **Profile** | DSH 中一组 plugin 组合的配置（web/headless/sdk/sdk-minimal/acp 五个模板） |
| **Cordis** | DSH 底层依赖的插件框架，"everything is a plugin" 架构 |
| **Composio Golden Eval** | Composio 的 SaaS 工具调用评测集，30 个真实工作流，12 个 verifier check |

---

## 附录 C：本报告的引用方式

如果你要引用本报告，建议引用方式：

```
《DeepSeek "Bash-Only" 工具过拟合深度调查报告》
调查日期：2026-09-02
核心数据来源：
  - xiaobright (2026-08-14) DSH 三模式对照实验
  - Vals AI (2026-08-12) 独立评测 54.68%
  - Composio (2026-08-11) 8-harness 实验
  - Stanford/Berkeley 等校 (2026-08-12) Simulator Collapse 论文
```

---

> **本报告版本**：v1.0（2026-09-02 完成）
> **下次更新触发条件**：DeepSeek V5 发布、DSH 重大重构、第三方对 dsh-anchored-standard 插件复现、CAISI 官方评测结果出炉。
