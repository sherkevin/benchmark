# 06 · L3 三轮跑批：22 个工具 × 730 分子

Type: task
Status: open
Blocked by: 05

## 输入契约（不可改）

- 分子池：`data/bbb_census_v3/` 里既有的 730 分子，**必须沿用 `input_order_sha256 = 87f7e9d4…` 的固定顺序**，否则与历史 receipt 无法比对
- 端点：BBB 类工具用 `BBB_Martins`，`action_rule = probability >= 0.5`；非 BBB 工具（SolTranNet logS、rdkit 三件套等）按各自端点注册，**不与 BBB 混判**
- worker 只吐原始数值，label 一律由上层算

## 做什么

三轮跑批：`discovery` → `replay1` → `replay2`，每轮对全部 enabled 工具执行。

每轮每工具落三个文件（既有格式）：
```
data/bbb_census_v3/run_receipts/<round>--<tool_id>.receipt.json   (19 字段)
data/bbb_census_v3/run_receipts/<round>--<tool_id>.stdout.json    (730 行预测)
data/bbb_census_v3/run_receipts/<round>--<tool_id>.stderr.txt
```

## L3 判定（逐工具）

三轮的 730 个 `value` **逐位一致** → L3 通过。任何一位不一致 → L3 不通过，记入 `nonreproducible_tools.json`，并查因：

- 模型内部有随机性未固定种子 → 修 worker 固定种子后重跑三轮
- 浮点非确定性（MPS 后端常见）→ 记录为该工具在本平台的固有属性，降级为"平台相关不可复现"，不进主判分组
- 权重加载路径漂移 → 修 runtime 后重跑

**不允许**为了让它通过而放宽比对精度。

## 监控（我执行，我盯）

跑批用后台任务，每个工具完成后立即：
1. 校验 receipt 自哈希
2. 检查 730 行是否齐全、`status` 分布
3. 与上一轮比对，一旦发现不一致立刻停下来查，不等整批跑完

## 验收

- 每个 enabled 工具都有 3 轮 × 3 文件
- 全部 receipt 自哈希校验通过
- `L3_verdict.json`：每个工具一条，`{tool_id, tier, rounds_completed, all_identical, first_mismatch(若有), verdict}`
- 失败工具在 `tool_registry.json` 里状态与失败原因完整

## Comments
