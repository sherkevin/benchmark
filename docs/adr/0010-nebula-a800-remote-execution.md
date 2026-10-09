# 0010 - 星云平台远程执行形态：训练任务 + git 代码模式

状态：Accepted，2026-09-21

## 背景

项目自 2026-09-13 整体移交远程执行，49 个工具乘 730 分子的跑批需要 GPU 算力。业务方本周把 mlflow 资源迁到星云平台，开了共用项目 damo_genomic_gene（themis_id 8801，集群 asi_zjk_pai_b，张家口），可用队列 A800-120 卡、L20Z-48 卡、H20-8 卡。

平台提供两种执行形态：Notebook（长期存活容器，交互式）与训练任务（run mdl，提交后拉起容器执行 entry 再退出）。需要定一种作为跑批的主形态，并定下代码如何进容器。

## 决策

一，跑批走训练任务（run mdl），Notebook 只用于装环境与调试。

二，代码一律走 git 模式：`--git_path` 指向内网 GitLab 加 `--commit_id` 钉住 sha。禁止使用默认的本地目录打包上传模式。

三，数据走 OSS 挂载，桶 `ai4sci-develop-storage`，本项目前缀 `jingwu/`，容器内路径 `/data/oss_bucket_0/jingwu/`。

四，所有提交经仓库内统一入口 `tools/nebula/neb`，不直接裸调 `nebula-cli run mdl`。`neb submit` 默认 dry-run，必须显式 `--yes` 才真提交。

五，资源配比取每卡 15 核 / 240 GB，镜像取 `pytorch280`。

## 依据

形态选择。Notebook 有 3 天页面不访问即回收的机制，判据是页面访问而非 CPU 占用，这与跑批「无人值守长流程」的本质直接冲突。训练任务不受该约束，且失败有 logview 可查、产物落 OSS。团队现状佐证了这个分工：`queue get damo_genomic_gene_a800` 显示 9 个在跑任务全部是 `job_type=notebook`、零训练任务，说明团队目前只用 Notebook 做交互，跑批这条路还没人走通。

git 模式而非打包上传。`nebula-cli` 源码 `detectCodeSourceMode()` 的兜底分支是 `"oss"`，即什么都不传时把当前工作目录打 zip 上传（`zipExcludingFiles(this.workDir, ...)`，`workDir` 默认 `./`）。本仓库根目录 4.9 GB：`archive/` 1.7 GB、`.venv/` 730 MB、`data/` 596 MB、`src/` 518 MB。源码里有一道 1 GB 闸门 `checkWorkdirSize()`，绕过它的 `--force` 会把 4.9 GB 真的推上去。

传 `--git_path` 则完全跳过本地打包与体积检查。附带收益是可复现性：任务跑的是哪个 commit 有明确记录。源码还强制 `--git_path` 必须搭配 `--commit_id` / `--git_branch` / `--git_tag` 之一；选 `--commit_id` 是因为分支会在提交后、容器启动前被人推动，钉 sha 不会。

2026-09-21 一次误提交实测到了完整上传链路：代码包传到平台自己的 `ados-nebula` 桶，传完即提交，无二次确认。这是第四条决策（统一入口加默认 dry-run）的直接由来。

配比与镜像。`queue get` 显示 A800 队列配额 CPU 2192 / GPU 120 / Mem 30960 GB，即每卡约 18.27 核、258 GB。团队 9 个在跑的 Notebook 一律是每卡 15 核 / 240 GB（1 卡 15/240、4 卡 60/960、8 卡 120/1875），是经验证能调度成功的配比，故取为默认。镜像 `pytorch280` 对应 cuda 12.8 / python 3.10.13，与 `requirements.txt` 钉的 py3.10 及其注释里「云端实测 py3.10.14 加 mcp 1.29.0 加 matplotlib 3.10.9 可用」一致。

统一入口。`nebula-cli run mdl` 有 47 个参数，三个默认行为会静默跑错：打包当前目录、`cluster.json` 缺键得 NaN 而非报错、漏传 `algo_name` 得服务端拒绝。封一层 CLI 比每次靠人记住可靠。回归测试 `tools/nebula/tests/test_neb_cli.py`（18 条）锁住 cluster.json 算术与凭证脱敏。写 CLI 过程中确实在内存总量上算错过一次（漏乘 `gpus_per_worker`，8 卡显示 240 GB 而非 1920 GB），以及一次 GB 换算口径与平台不一致（除以 1024 得 234，而平台按 `memory_m / 1000` 显示 240，两者对不上账）。

## 后果

正面：提交可复现（钉 commit）、不会误传大目录、凭证不进 argv 与日志（只放仓库根 `.env`，已 gitignore；打印时脱敏为 `***`）、资源数字与平台显示口径一致可对账。

代价与遗留：

1. git 模式要求 commit 已推到内网 GitLab，平台侧 clone 不到本地未推送的提交。`neb doctor` 检查这一条，但它把「未推送」变成了提交的硬前置，本地攒着不推就跑不了。

2. 10 个工具的源码地址是 github，在星云上装不了（生产网禁 clone 外网仓库）。这是跑批的真卡点，比提交命令本身重要，需要逐个找内网替代或预先把源码传进 OSS。本决策不解决它。

3. conda 安装脚本需要改写成 pip（生产网禁 conda 源），涉及 CardioTox 等按 `environment.yml` 建环境的工具。派单文档里的 conda 路子迁到星云会失败。

4. `neb` 不支持传环境变量：`--env` 是老 nebulactl 的参数，这个 CLI 的选项表里没有，透传也不生效。要设环境变量只能写进 entry 脚本或用 `--property_file`。

5. 尚无一次成功的真实提交。2026-09-21 两次探针提交因漏传 `algo_name` 在服务端校验阶段被拒（duration 1 到 2 秒，未起容器、未占卡），在项目任务列表留下两条 failed：`01a0c21057e2a190bbc8f9184e53b490`、`01a0c2100fec789f49340f424d60a773`。清理命令见 skill，属不可逆操作，须 owner 同意后自行执行。

6. entry 脚本未适配容器：依赖安装、数据路径、结果落 OSS 三处都要改。第一次提交应先跑一个只做环境自检的最小 entry。

## 相关

操作手册：[docs/星云A800远程执行手册.md](../星云A800远程执行手册.md)，每条平台断言标了出处（实测／源码／文档）。

CLI：`tools/nebula/neb`。agent skill：`tools/nebula/skill/SKILL.md`，已符号链接进 `~/.codex/skills`、`~/.agents/skills`、`~/.claude/skills` 三个根，真身在仓库内随代码走。

回归测试：`tools/nebula/tests/test_neb_cli.py`，18 条，独立可跑。仓库根 `pytest` 收集有约 103 个既有错误（来自 `archive/` 与 `reference/` 的第三方测试），故本测试不依赖根级收集。

派单文档：[docs/远程跑批任务_全部工具.md](../远程跑批任务_全部工具.md)，遗留第 2、3 条与它直接相关。

ADR-0005（端点级冒烟判据）：跑批的判据规格仍归它管，本 ADR 只定执行载体。
