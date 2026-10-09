---
name: nebula-a800-benchmark
version: 1.0.0
description: >
  在星云（Nebula）平台的 damo_genomic_gene 项目 A800 队列上跑 benchmark 仓库的代码。
  提供一个标准 CLI（tools/nebula/neb）覆盖体检、查队列、提交任务、看状态、取日志、诊断、停止。
  只在用户的 macOS 终端里操作，不需要登录任何远程机器。
  触发词：星云、Nebula、A800、A100、GPU 队列、damo_genomic_gene、远程跑批、远程执行、
  提交训练任务、run mdl、nebula-cli、cluster.json、algo_name、pytorch280、
  task_id、logview、OSS 挂载、/data/oss_bucket_0、ai4sci-develop-storage、
  jingwu_smoke、跑批上云、GPU 跑代码、远程服务器跑代码。
---

# 星云 A800 远程执行

在星云平台的 A800 队列上跑 `benchmark` 仓库的代码。**全部操作都在本机 macOS 终端完成**，没有 SSH，没有需要登录的远程机器——平台是任务式的：你提交，它拉起容器执行，跑完退出，结果落 OSS。

完整原理与坑见 [`docs/星云A800远程执行手册.md`](../../../docs/星云A800远程执行手册.md)。这个 skill 只讲**你该怎么行动**。

## 唯一入口

所有操作走仓库里的 CLI，不要直接调 `nebula-cli`：

```bash
cd /Users/jingwu/work/benchmark
tools/nebula/neb <子命令>
```

理由：`nebula-cli run mdl` 有 47 个参数，默认行为有三个会让任务静默跑错的陷阱（见下节），`neb` 把这些都封住了。直接用底层 CLI 等于每次重新踩一遍。

例外：需要 `neb` 没封装的只读命令时（`queue get`、`task history`、`task download-code`），可以直接调 `nebula-cli`，但**只调只读命令**。

## 硬约束（违反任何一条都算失败）

1. **提交前必须先跑 `neb doctor`，退出码非 0 就不许提交。** 它会查出 HEAD 没推到远端、algo_name 不在白名单这类会让任务白跑的问题。只有任务需要挂 OSS 时才加 `neb doctor --mount_oss`；普通 git-only 任务不应因为未配置 OSS 凭据被拦截。

2. **`neb submit` 不带 `--yes` 时是 dry-run，只打印计划不提交。** 先给用户看 dry-run 输出，**得到明确同意后才加 `--yes`**。这是写操作，绝不静默执行。

3. **`neb stop` 同样需要 `--yes`。** 停任务是不可逆的。

4. **绝不用底层 CLI 直接提交。** 特别是不要在仓库根目录裸跑 `nebula-cli run mdl`——它的默认代码来源模式会把当前目录打成 zip 上传，本仓库根目录是 4.9 GB（`archive/` 1.7 GB、`.venv/` 730 MB、`data/` 596 MB）。`neb` 用临时目录放 cluster.json，从根上避开。

5. **绝不加 `--force`。** 那是绕过 1 GB 体积闸门、把 4.9 GB 真的推上去。

6. **绝不把 AK/SK 写进命令行、日志或任何 git 跟踪的文件。** 凭证只放仓库根 `.env`（已 gitignore）。`neb` 打印命令时会把凭证值替换成 `***`。

7. **绝不执行 `nebula-cli config set` / `config init` / `project use` / `login`。** 这些改全局配置。需要指定项目时用 `-j damo_genomic_gene`（`neb` 已经默认这么做）。

8. **绝不删除业务 OSS 桶里别人的数据。** `ai4sci-develop-storage` 里有同事的 TB 级数据（`TransData/` 2.6 TB、`caixu/` 361 GB、`tianbian/` 556 GB）。本项目只动 `jingwu/` 前缀。

## 标准流程

### 提交一个任务

```bash
# 1. 体检（只读，退出码 0 才继续；挂 OSS 的任务用 `neb doctor --mount_oss`）
tools/nebula/neb doctor

# 2. 看队列有没有卡（只读）
tools/nebula/neb queues

# 3. dry-run，把输出给用户看
tools/nebula/neb submit \
  --job_name jingwu_<用途>_<序号> \
  --entry <仓库相对路径的入口脚本> \
  --user_params "<传给 entry 的参数>"

# 4. 用户确认后，同一条命令加 --yes
tools/nebula/neb submit ... --yes
```

记下输出里的 `task_id`（32 位十六进制）。

### 观察与取结果

```bash
tools/nebula/neb status <task_id>
tools/nebula/neb logs <task_id> --tail 200
tools/nebula/neb logs <task_id> -t stderr        # stdout 没线索时
tools/nebula/neb diagnose <task_id>              # 失败时
tools/nebula/neb list --status FAIL
```

结果从 OSS 的 `jingwu/` 前缀取。路径互转用 `tools/nebula/neb oss_path <oss://... 或 容器内路径>`。

## 参数怎么选

**`--workers` 和 `--gpus_per_worker`**：总卡数 = 两者相乘。

- 批量推理（49 工具 × 730 分子）用 `--workers N --gpus_per_worker 1`：N 个独立容器各跑一份分片，不需要写任何分布式代码。
- 真要单容器多卡才用 `--gpus_per_worker 8`，此时代码必须自己处理 `RANK`/`WORLD_SIZE`。
- **超过 8 卡会被 `neb` 拒绝**（单机上限 8 卡，`queue list` 的 `8*10+4*1` 可见），需要 `--allow_big`。跨机必须自己初始化分布式。

**资源配比**：默认每卡 15 核 / 240 GB，这是团队 9 个在跑的 Notebook 的实测值，验证过能调度成功。不用自己算，除非有明确理由。

**`--algo_name`**：默认 `pytorch280`（cuda 12.8 / python 3.10.13），与仓库 `requirements.txt` 钉的 py3.10 对得上。白名单外的值 `neb` 会直接拒绝。

**`--mount_oss`**：只有任务真要读 `/data/oss_bucket_0` 时才加。加了会校验 `.env` 里的凭据是否存在。

**`--wait`**：阻塞到任务结束。第一次冒烟建议加。

## 平台事实速查

这些是实测或从源码读出来的，不要凭印象改：

| 事项 | 事实 | 出处 |
|---|---|---|
| 卡型 | A800-SXM4-80GB，**不是 A100**（NVLink 带宽 400 vs 600 GB/s） | `queue list` 实测 |
| 单机卡数上限 | 8 | `queue list` 的 `8*10+4*1` |
| 代码来源默认模式 | 打包**当前工作目录**上传到平台桶 `ados-nebula` | CLI 源码 `detectCodeSourceMode` |
| git 模式 | 传 `--git_path` 即完全跳过本地打包；必须同时给 `--commit_id`/`--git_branch`/`--git_tag` | CLI 源码 + 实测报错 |
| 体积闸门 | 工作目录 > 1 GB 拒绝，除非 `--force` | CLI 源码 `checkWorkdirSize` |
| cluster.json | `cpu`/`gpu`/`memory` **三键必填**，单位百分之一/百分之一/MB，缺键得 NaN 不报错 | CLI 源码 `genXdlConfig` |
| algo_name 白名单 | pytorch201/210/220/240/260/280/1131/2100/2120/2100_py310_cu128，漏传报 `algo_name None` | 实测报错 |
| OSS 挂载点 | 固定 `/data/oss_bucket_0/`，多桶按填写顺序编号 | 平台文档 |
| OSS endpoint | **不能带 `http://`** | 平台文档 |
| `--env` | 这个 CLI **不支持**（老 nebulactl 的参数），传了不生效 | CLI 源码：选项表里没有 |
| `task <子命令> --help` | 不生效，会打印顶层帮助。用 `task help <子命令>` | 实测 |
| 状态值拼写 | `task list` 要 `FAILED`，`queue jobs` 要 `FAIL`；传错**不报错只返回空** | 实测 |
| `[AES] send fail` 堆栈 | 遥测被拦，不是错误，忽略（`neb` 自动滤除） | 实测 |
| `queue list` 偶发 `status: 13` | 张家口端点抖动，重试即可 | 实测 |

## 红线（平台级，与本项目无关也必须守）

- **容器里禁止跑 Agent 程序**（Codex、Claude Code 等），2026-03-25 起生产网规定。要 agent 参与就让它在本机调 `neb`，不要在容器里装 agent。
- **禁止 clone github / modelscope 等外网仓库**。只允许内网 GitLab（`gitlab.alibaba-inc.com`）和 pip 镜像 `yum.tbsite.net`。
- **禁止 conda 源**，生产网不通。依赖走 pip 镜像。
- **禁止开隧道、挂长驻服务**。
- Notebook 形态另有约束：页面 3 天不访问会被回收，`/home` 上限 200 GB + 40 万 inode。

## 已知未完成

接手时别假设这些已就绪：

1. **`.env` 里还没有 OSS 凭据**（`NEBULA_OSS_ACCESS_ID` / `NEBULA_OSS_ACCESS_KEY`），所以 `--mount_oss` 会被拦。
2. **还没有一次成功的真实提交。** 2026-09-21 两次探针提交因漏传 `algo_name` 在校验阶段被拒（1–2 秒，未起容器、未占卡），在项目任务列表留下两条 `failed`：`01a0c21057e2a190bbc8f9184e53b490`、`01a0c2100fec789f49340f424d60a773`。
3. **10 个工具的源码地址是 github**，在星云上装不了，需要逐个找内网替代或预先把源码传进 OSS。这是跑批的真卡点。
4. **conda 安装脚本未改写成 pip**（CardioTox 等）。
5. **entry 脚本未适配容器**：依赖安装、数据路径、结果落 OSS 三处都要改。第一次提交应该用一个只做环境自检的最小 entry。

### 清理那两条 failed 记录

删除是不可逆操作，**必须先征得用户同意**，然后生成命令让用户自己执行：

```bash
nebula-cli task delete 01a0c21057e2a190bbc8f9184e53b490 -j damo_genomic_gene
nebula-cli task delete 01a0c2100fec789f49340f424d60a773 -j damo_genomic_gene
```

这两条记录没有占用任何资源，留着也不影响使用，只是任务列表里难看。
