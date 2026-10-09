# Common-stratum full receipt：E5/B11/B12 分解

日期：2026-09-30  
状态：只读分析完成；不改原始回执、不打开 confirmation。

## 1. 这一步解决什么问题

生效的 Benchmark+Baseline 评价标准要求主表不能只有 pooled accuracy，还要说明：

- runtime failure 是否被计入分母；
- 返回有效时的化学正确率是多少；
- 33 个具体 provider 按 family 聚合后是否仍成立；
- 调用、更新和状态存储的实际成本是多少。

本报告从已经通过独立 action/state/evaluator recovery 的 full_v2 `events.jsonl` 做只读聚合。没有重新训练、没有读取 confirmation、没有使用未选 provider 的输出。

原始回执：`experiments/logs/task49_common_stratum_full_receipt_20260930/full_v2/events.jsonl`   
原始 SHA-256：`ee456b5dc5b93e20b1d75c5e1ba9409ce5376887a8a6a4f3a4a96d0c6229422e`  
规模：150 streams、21,660 decisions、64,980 events。  
family 来源：`task/task38-p01-provider-pool/external_qualification.json`，SHA-256 记录在机器结果中；`fpadmet` 和 `admetlab_3_0` 不再被错误记为 UNKNOWN。

## 2. 固定估计量

- **all-provider task success**：`system_success / decisions`。runtime failure 记为任务失败。
- **valid-return conditional correctness**：`chemical_correct / valid_return`。provider runtime failure 不进入这个分母。
- **family macro**：先在每个 family 内计算 valid-return conditional correctness，再对有至少一个 valid return 的 family 等权平均。它不是把同 family 的多个模型当成独立样本。
- **cost**：直接使用事件中的 `selection_ns`、`update_ns` 和 `state_bytes`；没有把本地 CPU 时间外推成 Nebula 或生产延迟。

逐方法和逐 family 的完整数字在 `experiments/logs/task49_common_stratum_metrics_20260930/full_v1/metrics.json`。

## 3. 主分解

| method | task success | valid-return correctness | runtime failure | family macro correctness | selection p50/p95 (µs) | update p50/p95 (µs) | state p95 (bytes) |
|---|---:|---:|---:|---:|---:|---:|---:|
| Frozen direct | 0.901662 | 0.908583 | 0.007618 | 0.873464 | 27.834 / 49.583 | 1.459 / 2.709 | 16,896 |
| Matched Online-SGD | 0.892198 | 0.899465 | 0.008079 | 0.756552 | 28.000 / 39.852 | 9.208 / 17.625 | 16,896 |
| Greedy online | 0.887350 | 0.892086 | 0.005309 | 0.886374 | 156.125 / 203.654 | 24.583 / 54.102 | 1,098,504 |
| Bayesian online | 0.865420 | 0.870443 | 0.005771 | 0.873853 | 154.083 / 179.375 | 22.917 / 35.561 | 1,098,504 |
| MIRA relative reliability | 0.902816 | 0.909746 | 0.007618 | 0.888708 | 79.667 / 97.560 | 42.459 / 61.098 | 17,721 |

这里的 MIRA pooled task-success 差值相对 Frozen 为 `+0.001154`，与已有 paired bootstrap 的区间跨 0 一致；这张表不能把它写成稳定提升。MIRA 的 selector p95 约为 Frozen 的 1.97 倍，update p95 约为 22.6 倍；这是实时预算表必须保留的代价。Greedy/Bayesian 的 state bytes 约 1.1 MB，不能只报准确率。

## 4. Family 分解的读法

各方法选择的 family 暴露不同，所以 family macro 不能直接做方法排名。比如 Frozen 主要选择 `b3clf`、`lgbm_bbb` 和 `admetlab`，MIRA 的暴露也集中在这三类；Matched Online-SGD 只选择了 2 次 `fpadmet`，其 family 条件准确率为 0/2，不能据此得出 fpadmet 的总体结论。

因此主文表格至少要同时保留：

1. all-provider task success；
2. valid-return conditional correctness；
3. family macro 和每个 family 的 exposure/valid-return 分母；
4. runtime failure、selection/update p50/p95、state bytes。

只报 family macro 会掩盖 policy 改变了暴露；只报 pooled accuracy 会掩盖 failure 和 family 相关性。

## 5. 对验收标准的影响

这一步补强了 B11、B12、B13 的证据入口，并为 E5 提供了真实事件账本中的成本和失败分解。它没有关闭以下硬缺口：

- Gate A：provider training membership、endpoint qualification 和 confirmation lineage；
- Gate B：Gaussian Greedy/Bayesian 的 reward-scale 语义，以及所有 baseline 的统一 feature/warm-start/budget 合同；
- Gate C：动态事件主诊断、统一 paired uncertainty 和冻结 confirmation。

下一项工作因此不是继续调 pooled accuracy，而是执行已经预注册的 **method-conditioned incumbent 动态事件恢复实验**：在同一 no-event prefix 上冻结每个方法实际选择的 incumbent，再注入 transient/persistent failure，按 delay `{0,1,4}` 分开报告反馈盲区、release 后恢复、target exposure、切换延迟和成本。这个实验直接命中 M10/M11/M13 与 E4；confirmation 仍保持关闭。
