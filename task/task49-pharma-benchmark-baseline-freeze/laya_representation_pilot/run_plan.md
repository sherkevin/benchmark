# 单条 A800 表示提取：完成状态

2026-09-27。实验卡见同目录 `preregistration.md`。v3 已完成；完整结果见同目录 `report_v3.md`，旧失败见 `failure_v1.md`。

已准备：官方 Laya 权重 842,609,210 bytes，SHA-256 已与固定 revision 的 LFS 摘要匹配；模型本地目录 `.bench_runs/task49-assets/laya-1c5edc17/`；五个文件的清单见 `asset_manifest.json`。730 请求与 33 工具菜单经过字段/哈希检查，四项合同测试通过。提取脚本不读标签，CPU 不冒充 A800；原 scorer 对齐在真实运行时验证，尚未执行。

资源接入已完成。凭据只写入仓库根 `.env`（已忽略），没有进入命令、日志或 git。五个文件已上传到 `ai4sci-develop-storage` 的 `jingwu/benchmark/task49/laya-1c5edc17/` 前缀；本地大小和远端大小、OSS CRC64 逐项一致。`tools/nebula/neb doctor --mount_oss` 已通过。完整传输和核对记录在 `experiments/logs/task49_laya_asset_transfer_20260926/ossutil_upload_v2/`。

当前网络直连桶虚拟主机时 TLS 会被提前断开，path-style 又被 OSS 拒绝；上传仍由官方 ossutil 2.4.0 完成，只通过本机临时 Host 保留转发访问根 endpoint。这个网络绕行只影响本机传输，不写入 Nebula 任务配置。`ai4sci-develop-fast` 当前未使用。

第一次真实 Laya 任务已经进入 A800，但在首个 `.npz` shard 导出时触发 OSS FUSE 的 seek 限制，详见 `failure_v1.md` 和 `remote_v2/`。入口已改成“本地生成 ZIP、顺序流复制到 OSS、逐文件校验”。修复后的 v3 任务已成功完成：730 请求、33 provider、92 个 shard；OSS 内容对象的 CRC64 已逐项读取核对。任务和完整日志见 `remote_v3/`。

这批仍然只是无标签表示导出。它没有训练选择器，也没有给出正确率、累计 regret 或在线更新效果。下一步另行登记同一 scaffold 划分下的冻结表示 head/online baseline 诊断。

```bash
env -u HTTP_PROXY -u HTTPS_PROXY -u ALL_PROXY \
    -u http_proxy -u https_proxy -u all_proxy \
    tools/nebula/neb doctor --mount_oss

env -u HTTP_PROXY -u HTTPS_PROXY -u ALL_PROXY \
    -u http_proxy -u https_proxy -u all_proxy \
    tools/nebula/neb submit \
    --job_name jingwu_task49_laya_features_001 \
    --commit e9d394377862a2c873735f4cd29d5d72670d9d0a \
    --entry src/probe_headless/nebula_laya_representation.py \
    --mount_oss --workers 1 --gpus_per_worker 1 \
    --user_params '--model-dir /data/oss_bucket_0/jingwu/benchmark/task49/laya-1c5edc17 --out /data/oss_bucket_0/jingwu/benchmark/task49/laya_features_v3 --code-commit e9d394377862a2c873735f4cd29d5d72670d9d0a'
```

提交时将当前已推送代码固定为具体 commit，不使用漂移分支。返回 task id 后留存配置、stdout/stderr 和最终状态；只有全部 730 条数组、逐行日志和 scorer 一致性成功后，才开展单独登记的学习诊断。失败保留原目录，修复后改 run id，不覆盖。

代码与输入已在 `e9d394377862a2c873735f4cd29d5d72670d9d0a` 推送到 `origin/codex/progress-20260924`。上面命令固定该版本；之后只修改说明文档不改变该批的执行代码。

## 22:30 CST 入口修正：上面的旧提交命令停止使用

同项目的指纹诊断真实运行确认：Nebula 把 git 代码交付为没有 `.git` 的归档。Laya 入口也有直接 `git rev-parse` 的同类错误，已修为显式 `--code-commit` + 源码 SHA 记录；输入/模型/批大小和提取逻辑不变。原配置读取失败现在也进入 events 的 failure 记录。

因此上面的 c3849c4e 命令仅保留历史，不可直接提交。存储恢复后以包含此修复的已推送 full commit 为准，`--commit <SHA>` 和 user_params 中 `--code-commit <同一SHA>` 必须一致，并将更新命令/来源 hash 写入新的 run plan。没有在存储缺失时提交 Laya 任务。
