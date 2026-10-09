# Raw-run 证据链审查清单（内部）

日期：2026-09-30  
用途：给当前 BBB development baseline 做 raw log/schema 审查；不修改生效 benchmark 文档，不是 AAMAS 官方规定的字段表。

依据：`docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md` 的 H5--H10、B5--B15、R1--R8，以及当前 `development_master_table_v1` 的真实运行收据。AAMAS 审稿人不会要求某个固定 JSON 键名，但会追问下面六件事：

1. 选择前看到了什么，是否看到了真值或未选 provider 的结果？
2. 选择了谁，候选菜单是否和其他 baseline 相同？
3. 结果何时产生，何时变成可用于更新的反馈？
4. 反馈到达后，状态到底更新了什么，是否只用了 selected-only 信息？
5. 主表数字能否由一份原始事件流和独立脚本重算？
6. 方法、数据、初始化、预算和失败规则是否公平且可追溯？

## 一、审稿人真正关心的最小证据链

### P0：没有这些字段，online / fair / reproducible 主张不成立

| 证据层 | 必须能恢复的字段或文件 | 审查用途 |
|---|---|---|
| 运行合同 | `run_id`、`method_id`、方法版本；代码 commit、source/input/menu/checkpoint hash；Python/依赖/硬件；命令、seed/slot、fold/scaffold、confirmation 标志 | 证明这次运行是什么，防止只报一个均值 |
| 任务输入 | `entity_id/query_id`、候选 `provider_id` 的完整菜单（或 menu hash 加可读 manifest）、feature/representation hash、provider/version/event scope | 证明所有方法面对同一输入和菜单，能按 molecule/provider 对齐 |
| 选择事件 | 唯一 `event_id`、stream、逻辑 `tick`、selected provider、候选菜单、policy scores/propensity、决策时可见 state/read 集合或 state hash | 重建动作；检查 propensity、菜单和 pre-call 信息边界 |
| 选择前隔离 | 决策事件中不出现当前 truth、未选 provider 输出、未来 feedback；如保存 scores，必须能说明它们只由决策前 feature/state 得到 | 直接回应 H5/H6，排除 full-panel 泄漏 |
| 调用终止 | `event_id`、provider output snapshot/version、`valid_return`、`system_success`、`chemical_correct`、`recovered_label`、error/unknown reason、terminal tick | 区分“调用失败”“有返回但错误”“标签未知”；不能只把缺失行当 0 |
| 时间与反馈 | `decision_tick`、`due_tick`、`terminal_tick`、`release_tick`、declared delay、pending/release order | 证明 delayed selected-only 不是静态重训；能重放异步到达顺序 |
| 状态转移 | `version_before/after`；最好有 `state_hash_before/after`、本次实际应用的 `released_feedback_ids`、update time；无更新策略也要显式记录 unchanged | 独立复核更新是否发生在 release 后，防止偷偷读未来反馈 |
| 反馈绑定 | `event_id`、selected provider、reward/label、feedback status、update result；一条反馈只能归属于一次已选调用 | 证明奖励和动作绑定，没有错配或重复更新 |
| 结果分母 | 每 stream 的 decisions、terminals、feedback、correct、failure、unknown、invalid、accuracy/utility，以及每个 delay/fold/seed 的分母 | 让主表能独立重算；R4 要求失败和 unknown 入账 |
| 恢复收据 | 原始 JSONL、config、hash、独立 verifier/replayer、验证输出、篡改拒绝或负例；训练型方法另存 checkpoint/epoch-selection receipt | 证明数字不是手工表格；重放器不能依赖原训练 runner 才能复核 |
| 公平合同 | 同一 menu、feature、selected-only feedback、delay、warm-start、预算、seed/slot、tuning 规则；Oracle/privileged full-panel 单独标注 | 回答“为什么这不是给方法额外信息/预算” |

其中“字段”不是要求每个方法都产生相同的值。固定策略可以把 `state_hash` 标为恒定或 `unchanged`，Random 可以显式声明不更新；不能静默省略语义。对大向量，保存确定性 artifact 的 hash 加可重算路径即可，没必要把每次完整权重塞进事件行。

### P1：决定证据强弱，但不应阻塞最小回放

- 逐事件 `decision_reads`、`released_feedback_ids` 和 state hash，优先于仅记录一个全局 selected-only 声明。
- provider lineage：源码 commit、checkpoint hash、训练来源/成员资格等级、失败语义和许可证；共享 family 要有 family id。
- 计算成本：selection/update p50/p95、memory、调用数、checkpoint/恢复成本；不能只报 GPU wall time。
- 对每个事件保存 provider output 的可审计快照或内容 hash；若 raw output 很大，manifest 必须能定位它。
- 负例和错误 trace：重复 event、错误 release tick、tampered propensity、unknown truth、provider crash 的 verifier 结果。
- 统计分层：fold/scaffold、provider family、delay、seed/slot；paired difference 的原始配对键。

## 二、当前运行的覆盖情况

这是按事件字段而非最终分数做的审查；“有”表示当前收据中可以找到，“部分”表示需要从旁边的 config/summary 或隐式规则补全，“缺”表示当前 raw event 流无法直接恢复。

| 运行 | 选择与菜单 | 终止/失败 | 延迟绑定 | 状态转移 | 独立恢复强度 | 主要问题 |
|---|---|---|---|---|---|---|
| Matched Online/Replay SGD | 部分：有 entity/provider/tick/version/reward，但 decision 没有 event_id、menu、propensity、feature hash、read set | 缺：无 terminal 事件 | 有 due/release/feedback event | 部分：version before/after，无 state hash | NumPy recovery 通过 | raw schema 不足以逐事件证明 pre-call menu 和 provider output 绑定 |
| PILOT-adapted | 有 candidates、scores、propensity、feature hash、event_id | 有 system/valid/chemical/recovered 字段 | 有 due/release | 部分：version before/after，无 state hash/read set | recovery 通过 | adapted 版本需要显式记录与原论文的差异、输入语义和代码身份 |
| BayesianRouter-adapted | 与 Matched 类似，缺 terminal/menu/read set | 缺：无 terminal 事件 | 有 due/release | 部分：version before/after，无 state hash | recovery 通过 | 不能仅凭 posterior 更新事件证明调用失败/未知语义已入账 |
| Greedy LinUCB | 强：menu、decision_reads、propensity、feature hash、state hash、method/run id 齐全 | 有完整 terminal 和 unknown/failure 字段 | 有 due/release/released ids | 强：before/after hash 和 applied ids | recovery + tamper 通过 | 当前最接近目标 schema；仍需与其他方法统一字段语义和成本表 |
| MIRA SafeSwitch v2 | 部分：event/provider/tick/version；selected-only 读集合在 stream summary | 有 terminal/feedback status、output label 和 failure block 信息 | 有 due/release/delay | 部分：version before/after，有 update/block state，但无 state hash | replay/tamper 通过 | 缺 menu/propensity/feature hash/逐事件 read set/state hash；应补 schema 或明确 deterministic fallback |
| Random | 有 candidates、scores、propensity、feature hash、event_id | 有 terminal output、failure/unknown 字段 | 有 due/release | 部分：version before/after，但策略不更新；config/summary 在事件流外 | 30/30 verifier + tamper 通过 | 需要显式 `method_id/run_id`、stream summary 或统一 manifest；scores 必须标明是 fixture/random policy 信号 |

这张表解释了当前 Gate B 为何仍是 HOLD：多数方法已有可复核数字，但还没有一份能覆盖所有主 baseline 的统一 raw-run 合同。Greedy 的强日志不等于其他方法自动满足相同证据标准。

## 三、优先修复顺序

1. **先统一语义，再统一键名。** 建立一个只读 crosswalk，规定 `event_id`、menu、selected output、terminal、release、reward、version、unknown/failure 的含义；允许历史日志用 alias，但不能把缺失字段填成猜测值。
2. **给每个主 baseline 补最小 P0。** Matched/Bayesian 先补 terminal 绑定和 menu/read 证据；MIRA 补逐事件 menu/read；Random 补显式 method/run manifest。没有这些，主表最多写 development adapter 结果。
3. **统一独立回放接口。** 输入 raw JSONL + frozen cache + config，输出 per-event action/feedback/metric；回放器拒绝重复 event、错误 release、propensity 不一致和 decision 中出现未来字段。
4. **再补 P1 的 state hash、lineage 和成本。** 状态 hash 不是装饰，它是比较不同 updater 时发现顺序错乱和隐式读取的最低成本证据；但可以在确定性状态序列已由独立 replayer 重建后再补。
5. **最后才做主表统计和 confirmation。** 统一 schema 前的负结果可以保留为开发证据，不能把不同日志强行拼接成“同一协议”并打开 confirmation。

## 四、审稿人不太会因缺少而拒稿的内容

- 每一步完整模型权重、所有 hidden vector 的明文副本；hash、checkpoint manifest 和独立重算路径通常足够。
- wall-clock timestamp 的纳秒级精度；逻辑 tick、due/release 顺序和 p50/p95 成本更关键。
- 只对固定策略强行制造 propensity 或 state update；应记录为 deterministic policy / no-update，而不是伪装成 online learner。
- 与主张无关的 API 调试日志。调试日志应另存，主 raw contract 只保留能解释动作、反馈、状态和指标的字段。

## 当前判断

当前证据已经能回答“若干方法在 BBB development fixed-pool delayed replay 上跑过且数字可复核”，还不能回答“所有主 baseline 在同一信息/成本合同下公平可比”。下一项工程任务应是统一 raw-run/recovery schema 的 crosswalk 和 validator；这比继续添加一个弱 baseline 更直接地提高 Gate B/C 分数。
