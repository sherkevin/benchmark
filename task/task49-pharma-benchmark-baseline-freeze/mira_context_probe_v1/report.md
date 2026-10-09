# MIRA-ContextProbe contract report

日期：2026-10-02  
状态：**contract 与 full receipt 通过；候选停止，不进入 confirmation。**

## 先发现并修正的协议问题

第一版 schedule 在每条 stream 都重置 `seed=20261002`。在 145 个 evaluation 分子、
$\epsilon=.20$ 和 32 个非 fallback provider 下，每条 stream 只有约 29 次 probe，因而
部分 provider 会在所有重复 stream 中永远没有 probe。这不满足 selected-only feedback 的
coverage 条件。该版本没有用于效果解释。

现行版本保留 base seed `20261002`，按
`SeedSequence([base_seed, fold, seed, delay])` 派生每条 stream 的 seed。同一 stream 的
Frozen/Probe 对照使用同一个 seed；seed 派生不读取分数、标签、运行状态或未来结果。

## 静态 coverage

在不执行 policy、不读取 evaluation truth 的条件下，对 5 folds × 2 seeds × 3 delays 的
4,332 个 decision 做了 schedule 重算。收据为
`experiments/logs/task49_mira_context_probe_schedule_20261002/v2_all_streams.json`：
总 probe 数为 `895`，32 个非 fallback provider 均至少被 probe 一次；fallback 永不进入
probe 菜单。小流中的空臂只作为分母记录，完整 common-stratum receipt 才执行全覆盖停止门。

## 当前 CPU contract smoke

使用代码 commit `95039de2d514c91ea8759054d8d7e9bdb2a0ea45`、fold0/seed0、delay `{0,1,4}`、
每条 stream 32 个分子，生成 6 streams、576 个事件。Frozen 和 Probe 共用输入、菜单和
feedback clock；Probe 各 delay 的 probe 次数为 `9/9/8`，所有 selected feedback 均按
`decision_tick + 1 + delay` release。独立 verifier 通过：

`experiments/logs/task49_mira_context_probe_smoke_20261002/v4_current/verification.json`

该收据只证明 PRNG、菜单、propensity、shield 分支、局部 posterior/hazard、延迟队列和
state hash 可由独立实现重算。小流的 task success 为 Frozen `28/32`，Probe 为 `28/32`、
`30/32`、`27/32`（三个 delay）；样本太小且未完成预登记对照矩阵，不能写成效果收益。

## Full common-stratum candidate receipt

在 contract smoke 之后，使用同一 commit 做了 5 folds × 2 seeds × 3 delays 的 candidate full
receipt：60 streams、25,992 events，原始事件约 1.1 GB。独立 verifier 通过：
`experiments/logs/task49_mira_context_probe_full_20261002/v1/verification.json`。
机制分析收据为
`experiments/logs/task49_mira_context_probe_full_20261002/v1/mechanism_analysis_v2.json`。

ContextProbe 的 4,332 个 task 中正确 `3,905`（`0.901431`），Frozen direct 正确 `3,906`
（`0.901662`）；ContextProbe runtime failure=`6`，Frozen=`33`。ContextProbe 共有 `895`
次 forced probe 和 `3,437` 次 shield decision。probe 分支正确率为 `757/895=0.845810`，
shield 分支正确率为 `3,148/3,437=0.915915`。按 delay 的 ContextProbe accuracy 为
`0.901662/0.898199/0.904432`（delay `0/1/4`），这只是开发收据，未做 paired interval。

关键机制结果是：相对每个 fold 的 fit-only fallback，ContextProbe 的 `switch-away=895`，
`harmed=99`，`rescued=38`；所有离开 fallback 的动作都来自 forced probe，shield branch
没有产生额外的 provider switch。释放反馈更新 `4,332/4,332`，但 shield branch 没有出现非
fallback action change。现有 decision payload 没有保存 shield 的可行集合和预测分数，因此不能
区分“没有可行 challenger”与“可行但未超过 fallback”；收据只能证明 selected-only 反馈进入
状态，却不能证明实时学习后的 shield 选择已经工作。把 probe 造成的动作变化写成动态收益会违反
预登记的机制分层。

因此 ContextProbe v1 **停止作为当前方法候选**，不申请 A800，不进入 confirmation。失败定位为
“探索补上 coverage，但安全门仍没有把探索结果转化为 exploitation”，不是事件回放或 verifier
失败。下一候选必须先提出新的可识别 gate/目标，使 probe 后的非 probe 分支能在固定成本、harmed/
rescued 和 selected-only 约束下产生可复核的 action change；在数学和 CPU stop test 通过前不再
调 $ε$、$δ$ 或重复 full receipt。

## 下一步

先提出新的可识别 gate/目标，使 probe 后的非 probe 分支能在固定成本、harmed/rescued 和
selected-only 约束下产生可复核的 action change；完成数学审查和 CPU 静态 stop test 后，才
重新登记小流。当前不重复 full receipt，也不申请新的 GPU 任务。
