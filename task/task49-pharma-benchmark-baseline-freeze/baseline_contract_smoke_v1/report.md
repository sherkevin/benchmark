# C4–C8 baseline contract smoke v1

日期：2026-09-29  
状态：通过接口合同；不产生科学效果结论，不打开 confirmation，不提交 Nebula 任务。

## 目的

把主 baseline 合同中的五个在线方法放进同一个真实缓存回放入口，检查它们是否能在相同的 64 条 BBB cohort、33 个 provider 菜单和 selected-only delayed feedback 下运行：

- C4 `OnlineSGD`；
- C5 `ReplaySGD`；
- C6 `GreedyLinUCB`；
- C7 `PILOT-adapted`；
- C8 `BayesianRouter-adapted`。

每个方法都运行 delay `{0, 1, 4}`。决策前只得到冻结 Morgan-64 分子向量和菜单；终止后只把所选 provider 的二元 `system_success` 作为延迟反馈释放给方法。当前 smoke 的 warm start 是固定有限 fixture，明确不代表离线拟合，也不代表任何方法的质量。

## 结果

运行入口：`src/probe_headless/run_pharma_baseline_contract_smoke.py`。  
独立核验：`src/probe_headless/verify_pharma_baseline_contract_smoke.py`。

| 检查项 | 结果 |
|---|---:|
| 方法数 | 5 |
| delay 数 | 3 |
| stream 数 | 15 |
| decision 事件 | 960 |
| terminal 事件 | 960 |
| feedback 事件 | 960 |
| 每条 stream 的反馈守恒 | 通过 |
| selected-only 可见性检查 | 通过 |
| terminal 与冻结缓存绑定 | 通过 |
| 状态版本与释放时刻检查 | 通过 |
| `state_hash_before/after`、`update_ms` 合同字段 | 通过 |
| confirmation | closed |
| effectiveness estimate | `null` |

独立 verifier 没有导入任何 baseline kernel，只读取 raw JSONL、冻结缓存和输入文件；它逐 stream 检查了 cohort 顺序、provider 身份、terminal 输出、反馈延迟、版本递增、反馈可见字段和事件守恒。

## 可复核证据

主运行目录（统一合同字段版本）：

`experiments/logs/task49_baseline_contract_smoke_20260929/run_v5/`

- `events.jsonl`：原始逐事件日志；
- `config.json`：代码 HEAD、依赖、菜单、cohort、超参和 source SHA-256；
- `inputs.json`：64 条输入向量、provider 菜单和 fixture prior；
- `input_manifest.json`：缓存与 truth 文件哈希；
- `summary.json`：仅合同计数和状态，不含效果汇总；
- `verification.json`：独立核验结果及 raw/config/input 哈希；包含合同字段检查。
- `tamper_rejection.json`：把一条 feedback 人为加入 `system_success` 后，独立 verifier 以非零状态拒绝；原始 `events.jsonl` 未改动。

此前的 `run_v1/` 失败现场、`run_v2/` 首次修复运行和 `run_v3/` 旧字段版本和 `run_v4/` pre-commit 版本均保留；当前正式证据使用 `run_v5/`。第一次运行的失败现场保留在 `run_v1/`：source hash 配置将相对路径直接对绝对根目录求 `relative_to`，在写入配置前失败；没有开始 replay。修复后先完成 `run_v2`，再补充统一合同字段并重跑 `run_v4`，提交字段修复后以 `run_v5` 生成与代码匹配的正式证据，未覆盖失败现场。

`run_v5/config.json` 记录提交 commit `728216c200b06059ca1b60a6e5c44d403adcca83`，与当前代码一致；events SHA-256 为 `eed9e036dc7ff97c4ddbf4b5adefc2341c22de61ed80c2f5fb4cb43ffee215f5`。

## 边界

这项交付只说明 C4–C8 的输入、反馈和日志合同已经可以接到同一 runner。它没有完成：

1. fit-only provider prior 的正式复现实验；
2. C4–C8 的 accuracy、regret 或 recovery 排名；
3. provider/version 动态事件实验；
4. Gate B 的完整 baseline 效果资格。

因此该 smoke 可以作为 Gate B 的“统一接口与信息边界”证据，不能写成“先进 baseline 已公平比较”或“某方法有效”。
