# 03 · 配置 GitHub 专用 deploy key 解除 12 个工具的获取阻塞

Type: task
Status: resolved（前提已失效，无需执行）
Blocked by: —

## 为什么

本机实测：`github.com:443` 与 `raw.githubusercontent.com` 全部超时（DNS 能解析，TCP/TLS 被拦），`huggingface.co` 连接被重置。12 个 L3 工具卡在这一个点上——解开它，L3 组从 10 个变 22 个。

关键缝隙：`ssh.github.com:443` 端口实测是**开的**（`nc -z` succeeded），只缺可用的 key（`git ls-remote ssh://git@ssh.github.com/...` 报 `Permission denied (publickey)`）。

## 做什么（已获授权，但严格限定范围）

1. 生成**本项目专用**新 key：`~/.ssh/id_ed25519_benchmark_ro`，注释标明用途
2. 在 `~/.ssh/config` **追加**一段（不修改任何既有段）：
   ```
   Host github.com
     HostName ssh.github.com
     Port 443
     User git
     IdentityFile ~/.ssh/id_ed25519_benchmark_ro
     IdentitiesOnly yes
   ```
3. 把公钥内容输出给 jingwu，**由他自己**加到 GitHub 账号（我不接触他的账号）
4. 加完后测 `git ls-remote https://github.com/chemprop/chemprop`（走 https）与 ssh 通道各一次

## 硬约束

- 不碰任何既有 key
- 不改既有 `~/.ssh/config` 内容，只追加
- 不把私钥内容输出到任何地方
- HuggingFace 不在本 ticket 范围——HF 权重依赖的工具改由远端承担（ADR-0003 §后果 3）

## 验收

- `git ls-remote` 对至少 2 个目标仓库成功返回 ref
- 12 个受阻工具中，逐个确认源码/权重现在能否取到，结果写进 `## Answer`

## 回退

若配置后 GitHub 仍不通，删除新增的 config 段与新 key，B 组 12 个工具全部划给远端环境，L3 组缩到 10 个。

## Answer

**结论：本工单无需执行。前提已失效——GitHub 现在 HTTPS 全通，deploy key 解决的是认证问题，而认证从来不是当前的瓶颈。**

未生成任何密钥，未改动 `~/.ssh/config`，未碰任何既有 key。`~/.ssh/` 下不存在 `id_ed25519_benchmark_ro`。

### 前提为什么失效

工单写的是「`github.com:443` 与 `raw.githubusercontent.com` 全部超时（DNS 能解析，TCP/TLS 被拦）」，唯一的缝隙是 `ssh.github.com:443`。2026-09-04 复测，结论完全相反：

| 目标 | 工单记录 | 本次实测 |
| --- | --- | --- |
| `https://github.com` | 超时 | **200** |
| `https://raw.githubusercontent.com` | 超时 | **200**（同一文件连测 3 次，0.5–2.0s） |
| `https://api.github.com` | 未测 | **200**（用它反查出 3 个仓库名） |
| `https://pypi.org` | — | **200**，`pip download soltrannet` 实际拉到 wheel |
| `https://zenodo.org` | — | **200** |
| EPA EPI Suite 下载页 | 000 | **200** |
| `https://huggingface.co` | 连接重置 | **000（仍然不通）** |

**一个测量方法上的坑值得记下：`nc -z` 在本机对这些主机全部报 BLOCKED，是假阴性。** 第一轮我用 `nc -z -G 5 -w 5` 测，github.com:443、ssh.github.com:443、raw.githubusercontent.com:443 全报不通；换 curl 立刻 200。后续判断连通性一律以 curl / 真实 git 操作为准，不用 `nc`。

### 上面这些 200 不是拦截页——已按已知陷阱验过

本机公司代理对被拦域名会返回 HTTP 200，正文是拦截页，长度恒为 **1461 字节**（此前已确认 6 个 URL 属此类）。只看状态码会把拦截当成通。所以复查了每个 200 的正文长度：

```
200      573358 B  https://github.com
200        2807 B  https://raw.githubusercontent.com/dhanjal-lab/graphB3/main
200        6966 B  https://api.github.com/repos/theochem/B3clf
200       27833 B  https://pypi.org
200       66822 B  https://zenodo.org
000           0 B  https://huggingface.co
200       65789 B  https://www.epa.gov/tsca-screening-tools/epi-suitetm-estim
```

无一为 1461 字节，全部是真实响应。更强的一层证据是三次真实 `git clone`：它们产出了合法的 SHA-1 对象和真实 HEAD 哈希，拦截页做不到这一点。

### 但这个结论有时效性，工单 05 不要当成永久前提

同一台机器 2026-09-03 的实测记录里，`git ls-remote` 对 github.com 是**真的超时**——与本次成功只隔一天。所以更准确的说法是「GitHub 在 2026-09-04 这个时点通」，而不是「网络策略已改」。连通性可能是间歇的（同一批记录里 ADMETlab3 一天内由 200 变 403、pkCSM 由 200 变 000，也是这种漂移）。工单 05 装运行时要按此设计：每个仓库的获取步骤可重试、可断点续做，失败记为 `fetch_failed` 而不是让整批中断。

### 12 个 B 组工具逐个核验

12 个里有 3 个的仓库名工单/图鉴没记全，用 GitHub API 列 owner 仓库列表反查得到（`Rajnishphe/LGBM-BBB`、`swamidass/xenosite`、`waqarahmadm019/SolPredictor`）。全部 11 个 GitHub 仓库 `git ls-remote --heads` 成功返回 ref：

| 工具 | 仓库 | ls-remote | 仓库体积 |
| --- | --- | --- | --- |
| B3clf | `theochem/B3clf` | OK | 228 MB |
| QupKake | `Shualdon/QupKake` | OK | 83 MB |
| Uni-pKa | `dptech-corp/Uni-pKa` | OK | 3.7 MB |
| DeepB3 | `GreatChenLab/Deep-B3` | OK | 196 MB |
| graphB3 | `dhanjal-lab/graphB3` | OK | 1.2 MB |
| DeePred-BBB | `12rajnish/DeePred-BBB` | OK | 69 MB |
| LGBM-BBB | `Rajnishphe/LGBM-BBB` | OK | 7.7 MB |
| OpenADMET | `OpenADMET/openadmet-models` | OK | 133 MB |
| XenoSite | `swamidass/xenosite` | OK | 4.2 MB |
| SILICOS-IT logP | `silicos-it/filter-it` | OK | 0.6 MB |
| SolPredictor | `waqarahmadm019/SolPredictor` | OK | 3.7 MB |
| KowWIN | EPA 下载页（非 GitHub） | 页面 200 | — |

顺手核了 `theochem/B3DB`（真值来源）也是 OK。

`ls-remote` 只证明 ref 可读，不证明能拉内容，所以另做了真实 clone：

```
git clone --depth 1  silicos-it/filter-it    → OK  3s  276 files  2.5M  HEAD=ad48078
git clone --depth 1  dhanjal-lab/graphB3     → OK  4s   48 files  3.4M  HEAD=e5e076c
git clone --depth 1  dptech-corp/Uni-pKa     → OK  3s   80 files  6.8M  HEAD=7cfbf65
```

### 一个自我更正：我一度把「慢」误判成「被拦」

中途 `git clone --depth 1 theochem/B3clf` 在 120s 超时失败，我当时倾向于判「TLS 通但批量传输被拦」。这个判断是错的。追下去发现 B3clf 仓库体积 228 MB，同一个仓库的 codeload tarball 在 300s 内拉到 263 MB、速率 876 KB/s，是被我的时间预算掐断的，不是被网络拦的。小仓库 3–4 秒 clone 完，也印证了这一点。

**所以「大仓库拉不下来」的正确描述是需要更长的时间预算，不是不通。** 五个大仓库（B3clf 228 MB、Deep-B3 196 MB、OpenADMET 133 MB、QupKake 83 MB、DeePred-BBB 69 MB）在工单 05 装运行时时要给足超时，实测速率在 0.2–0.9 MB/s 之间波动。

### 真正还挡着的只有一处：HuggingFace

`huggingface.co` 三次测试全部 000。按图鉴记录，12 个 B 组工具里只有 **OpenADMET** 的官方模型库在 HuggingFace 上（仓库源码本身在 GitHub，已通）。按 ADR-0003 §后果 3，HF 权重依赖的工具划给远端环境，这条不变。

### 对 L3 组规模的影响

工单原本的推论是「解开 GitHub，L3 组从 10 个变 22 个」。现在 GitHub 本来就是通的，所以这 12 个的**源码获取**不再是阻塞项。但「源码能拉到」离「L3 达标」还有距离——L3 要求可安装、可锁版本、全集输出、两次运行逐位一致、权重 sha256。这 12 个能不能真装起来、能不能跑出确定性结果，是工单 05 的事，本工单不下这个结论。

### 回退段无需执行

工单的回退方案（删密钥与 config 段、B 组划给远端、L3 缩到 10 个）不适用——没有新增任何密钥或配置可删，且 B 组不必划给远端。

## Comments
