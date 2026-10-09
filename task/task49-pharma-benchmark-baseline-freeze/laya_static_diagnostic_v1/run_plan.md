# Laya 静态表示诊断：提交计划

当前状态：实验合同、输入清单和读取器已通过本地检查，尚未提交 Nebula。

输入清单 `inputs.json` 的 SHA-256 由提交前命令重新打印并写入日志。它只引用已经固定的 `fingerprint_diagnostic_v1/inputs.json`；提交前必须核对父输入 SHA 不变。

特征目录直接读取现有 OSS 前缀 `/data/oss_bucket_0/jingwu/benchmark/task49/laya_features_v3`。任务启动时会逐 shard 检查 summary 中的 SHA-256、shape、有限值和 raw 行映射，然后只把 722 个有 BBB 标签的分子送进训练；8 个无标签请求不进 loss。

提交固定为 1 worker × 1 A800，一条任务。入口不重新加载 Laya 权重，不调用外部模型，只读取已核验的 marker shard。输出只走 Nebula 日志，不写 OSS ZIP，避免重复触发上次的 FUSE seek 问题。

```bash
env -u HTTP_PROXY -u HTTPS_PROXY -u ALL_PROXY \
    -u http_proxy -u https_proxy -u all_proxy \
    tools/nebula/neb doctor --mount_oss

CODE_COMMIT=$(git rev-parse HEAD)

env -u HTTP_PROXY -u HTTPS_PROXY -u ALL_PROXY \
    -u http_proxy -u https_proxy -u all_proxy \
    tools/nebula/neb submit \
    --job_name jingwu_task49_laya_static_003 \
    --commit "$CODE_COMMIT" \
    --entry src/probe_headless/nebula_laya_static_diagnostic.py \
    --mount_oss --workers 1 --gpus_per_worker 1 \
    --user_params "--input-manifest task/task49-pharma-benchmark-baseline-freeze/laya_static_diagnostic_v1/inputs.json --features-dir /data/oss_bucket_0/jingwu/benchmark/task49/laya_features_v3 --input-sha256 ffde2911dbee66c2960a2d31eeab6690160b734b2b216ccb4e8ea0c978210fd2 --code-commit $CODE_COMMIT"
```

提交前把实际的 `CODE_COMMIT` 和输入清单哈希写入本次实验目录；不要直接复用历史任务里的 commit。

否定条件：父输入 hash 变化、Laya summary/shard 读不全、任何 shard digest/shape/finite check 失败、A800 不可用、训练出现非有限 loss、checkpoint/evaluation 事件不完整，均停止，不解释成负效果。完成后先用完整日志恢复 40 个 fit 和 5,776 条 evaluation，再判断预登记闸门。
