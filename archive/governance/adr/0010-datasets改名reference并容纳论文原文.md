# 0010 · `datasets/` 改名 `reference/` 并容纳会议论文原文（2026-07-31）

| 字段 | 值 |
|---|---|
| **状态** | Accepted |
| **日期** | 2026-07-31 |
| **类型** | 目录规范修订（修订 ADR-0007 §1） |
| **相关** | [ADR-0007](0007-仓库组织与文档规范.md)（§1 被本 ADR 修订）、[ADR-0001](0001-理解-goal-优先于工程精度.md)（历史快照不重写） |
| **来源** | owner 2026-07-31 指令："放在 reference（datasets 改名为 reference）下，每个论文写一个文件夹，里面存储原始 pdf 和对该 pdf 的阅读报告" |

## 上下文

ADR-0007 §1 把 `datasets/` 定义为**纯数据资产区，不存文档**。这条规范在"只调研公开 benchmark 的采样数据"阶段是够的。

进入投稿阶段后出现了新的资产类型：**目标会议（ICLR/ICRA/AAMAS/WWW）的相关论文原文 PDF 与逐篇阅读报告**。它既不是"数据集采样"（不进 `datasets/` 的旧定义），也不是"平铺调研报告"（`research/` 规定平铺、不设子目录，而一篇论文需要 PDF + 报告成对存放）。硬塞进任一侧都会破坏原规范。

## 决策

1. **目录改名**：`datasets/` → `reference/`，语义从"数据资产区"扩为**"参考资料区"**。
2. **两类资产共存，各自一个文件夹**：
   - 公开数据集采样：`reference/<benchmark_name>/`（沿用旧规范：采样数据 + 官方判分脚本 + `README_采样说明.md`）；
   - 会议论文：`reference/<CONF>-<slug>/`，**必须**含 ① 原始 PDF ② `阅读报告.md`。`CONF` 为会议名大写前缀（`ICLR` / `ICRA` / `AAMAS` / `WWW` / `NeurIPS` / …），`slug` 为论文短名。
3. **阅读报告的必答三问**（owner 指定的关注点，缺一不算完成）：
   - **数据量**：题目数 / 实例数 / 环境数，以及每题的规模量纲；
   - **数据集形态**：一条样本长什么样（字段、输入输出、附带资源）；
   - **"长"体现在哪**：该 benchmark 的 long-horizon 究竟来自步数、依赖深度、上下文长度、时间跨度还是状态累积。
   另加两节：**判分粒度**（终答/过程/部分分/是否 LLM judge）与**对我方的可借与可打点**。
4. **分析仍归 `research/`**：跨论文的横评、投稿策略、构造方案落 `research/` 平铺文档；`reference/<论文>/` 只放该论文自身的 PDF 与单篇报告。
5. **元数据只信一手**：论文 venue/year 以 DBLP 或官方 proceedings 为准（写入 `ACTION_RULES.md` §4）；取不到会议正式版 PDF 时可用 arXiv 版，但必须在报告中注明版本差异。

## 不动的部分

- ADR-0007 其余六条（`research/` 平铺、待办唯一、项目级文档居根、`governance/` 定位、`task/` 冻结）全部沿用。
- ADR-0007 正文保留 2026-07-26 的 `datasets/` 称谓（历史快照不重写，ADR-0001）；仅在其状态栏加 supersede 标记。
- `reference/NobelClaw/`（自造题集与 GT 证据底座）位置与定位不变，只随目录改名。

## 触发动作

| 动作 | 结果 |
|---|---|
| `git mv datasets reference` | 保留 git 改名血缘 |
| 全仓引用改写（仓内自指路径 121 处 + 目录内相对链接归一化 18 处） | 只改仓内自指；`huggingface.co/datasets/`、`hf://datasets/`、`site-packages/datasets/`、`data/datasets/`、上游仓库内的 `./datasets/` 与 `](datasets/)` 一律不动（已回退 3 处误命中） |
| 规范文档同步 | `ACTION_RULES.md` §2/§4、`CLAUDE.md`、`README.md`、`task/_templates/README.md` |

## 复核口径

- `grep -rn "datasets/"` 的剩余命中必须全部是外部路径（HF / pip 包 / 上游仓库），无一条指向本仓目录。
- `reference/` 下每个 `<CONF>-<slug>/` 必须同时存在 PDF 与 `阅读报告.md`，否则该论文视为未落盘。
