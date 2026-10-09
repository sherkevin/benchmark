# Baseline 下一步审查

日期：2026-09-27
审查范围：`pharma_online_baselines.py`、`pharma_selection_stream.py`、`baseline_adapters_v0.md`、`next_plan.md`，以及它们直接使用的测试和诊断入口。这里只审查适配状态和协议，不提交 Nebula、不修改数据、不解释实验效果。

## 结论先说

现在能拿出来运行的是“在线递推接口”，还不是一组可以直接进论文主表的完整 baseline。

| 对照 | 当前状态 | 还缺的最小东西 |
|---|---|---|
| Best Fixed / Oracle | 在指纹和 Laya 静态诊断中作为 fit-only 控制和 evaluator 上界计算过；没有统一直接选择 runner | 固定 train/validation/test 口径，输出统一逐事件记录；Oracle 永远留在 evaluator |
| Greedy LinUCB | 在线公式、selected-only 更新、延迟队列和真实缓存冒烟可运行；当前是论文算法的 H=1 单步适配 | 固定公开的 feature、候选菜单和超参配置，接入统一 runner 后再做正式轨迹 |
| PILOT | `PilotOnlineKernel` 的 cosine/UCB 和 selected-arm ridge 可运行；真实冒烟使用的是随机 fixture prior | 实现并核验论文的离线 query projection、provider preference fit 和 $λ_a$ 初始化；随机 prior 不能称 PILOT |
| BayesianRouter | 在线 Gaussian posterior/Thompson 部分可运行；已有 Morgan + BT+CLS prior 的 A800 诊断入口，但还没有这条诊断的结果证据 | 先完成并恢复一条同反馈合同的 frozen-vs-online 诊断；再决定是否进入主表 |
| GraphRouter | 只有源码、论文和许可证审计，没有 BBB 适配 runner 或结果 | 把分子/provider 图、训练边权限、epoch 选择、平票规则单独适配并验证 |
| RouterDC | 论文审计，仓库没有声明许可证；没有实现 | 仅在资源足够且按论文独立重写时再做，不能写成已经运行 |
| Frozen / Online-SGD / Replay-SGD | 当前 Task49 目录中没有统一实现和正式日志 | 先定义与候选方法完全相同的 backbone、初始化、更新次数和 replay 大小，再实现三个 matched controls |

因此，当前最合适的下一步是先跑已有的 **Morgan BT+CLS prior + BayesianRouter selected-only 更新**诊断。它的离线 fit、延迟流和 Frozen 对照已经在同一个入口里，能用一条小的 A800 任务回答“标准在线后验是否已经解释了实时更新收益”。PILOT 的离线 fit 在这之后补；GraphRouter 和 matched SGD 再接入共同 runner。

## 已核对的协议边界

1. `OnlineKernel.decide()` 只接收一个冻结 query 向量和候选 provider 名单，保存决策时的特征副本；`observe()` 只接收已选 provider 的 `0/1` reward。未选输出、真值和未来反馈没有进入 kernel。
2. `TerminalFeedbackQueue` 先检查 episode 已终止、query/provider 身份和评分状态，再把 `system_success` 放入到期队列。队列释放时才调用 `observe()`；失败记为 0，化学正确性和不可恢复真值仍为 null。
3. 反馈时间是 `release_tick = decision_tick + 1 + delay`。代码覆盖了 delay 0、1、4；延迟期间外部改变 feature 不会改变待更新样本。23 个在线接口测试和指纹在线测试当前通过（本地命令输出 `23 passed`）。
4. `run_stream()` 的 reward 从 evaluator-side `rows` 在作出选择之后生成，policy 在下一个 release tick 才收到它。因此按当前可信执行进程没有决策前泄漏；但它仍是同进程接口，不是恶意代码隔离沙箱。正式 runner 应继续保持这个边界，并让日志明确区分 evaluator 行和 learner 行。

## 需要修正或明确的公平性问题

### 1. 候选菜单还没有贯穿 runner

`OnlineKernel.decide()` 支持每题 `candidates`，但 `run_bbb_stream()` 总是用 `policy.providers`，也总是用完整 provider 集创建 episode。若最终 benchmark 冻结为每题同一个 33-provider 菜单，应在实验卡明确写死这个事实；若菜单是输入的一部分，最小修复是给 runner 传入按 `entity_id` 索引的冻结菜单，并把同一个菜单传给所有方法。不能只在某个 baseline 里删候选。

### 2. 随机 prior 冒烟不构成 PILOT 或 BayesianRouter 效果

`run_pharma_online_adapter_smoke.py` 给 PILOT 和 BayesianRouter 使用随机 fixture prior，目的是覆盖数值路径。这个做法没有标签泄漏，但不能用于排序、准确率或论文对照。PILOT 的 preference prior、BayesianRouter 的 BT/CLS prior 必须由同一 fit 区训练，并把投影、先验方差、noise variance 和 checkpoint 选择写入配置。

### 3. 反馈预算和初始化权限要按方法对齐

完整离线先验可以使用 fit 区 full panel；在线阶段只能使用所选且已释放的反馈。零目标标签 cold-start 必须另列一组，不能让 Greedy 使用零先验而让 PILOT/BayesianRouter 使用训练标签，然后把差异归因于在线更新。所有方法须共享分区、流顺序、delay、种子和调参预算。

### 4. 现有代码不是完整的 BayesianRouter 原实现

当前 kernel 是论文式 selected-reward Gaussian 更新的独立实现；Morgan 诊断的 BT+CLS 头也是本任务适配。它可以作为清楚标注的 BayesianRouter-style baseline，但不能把原仓库默认脚本或这段 kernel 直接称作原方法的完整复现。原仓库已审出的固定选臂、零 prior 和分类损失偏离必须继续保留在报告里。

### 5. 标准递推不能包装成创新

rank-one Gaussian/ridge 更新、普通 H=1 disjoint LinUCB、cosine/UCB 和 scalar projected Thompson sampling 都是 baseline 组件。它们的价值是提供可复算的近邻和公平下界；动态 JEV 的新机制必须另有可证伪差异，并接受 Frozen、Online-SGD、Replay-SGD、PILOT 和 BayesianRouter 的共同预算对照。

## 建议执行顺序

1. **先跑 BayesianRouter 诊断（优先级最高）**：使用已有 frozen Morgan input、五个 scaffold fold、两 seed、delay 0/1/4；恢复完整事件、checkpoint、每步选择和最终 posterior，主比较只写 BayesianRouter-online 对 Frozen BT prior。若三个 delay 都没有收益，就停止把普通后验更新当成主创新。
2. **接着实现 PILOT 离线 fit**：单独写 train-only projection/preference 函数；平票不造胜负，hard-negative 成本没有可靠来源时不伪造。先用合成小例子验证 cosine 与 prior/ridge 的数值，再接共同 runner。
3. **补统一 runner 合同**：候选菜单、fit/validation/evaluation 划分、selected-only release、失败和 unresolved 处理、事件 schema、输入与源码 hash 都由一处负责。这样 Greedy、PILOT、BayesianRouter 和 matched controls 才真正可横向比较。
4. **实现 matched Frozen/Online-SGD/Replay-SGD**，使用与最终 JEV 完全相同的 backbone 和初始化；没有这三个对照，不判断“实时更新机制”是否有独立贡献。
5. **最后做 GraphRouter**：它是静态强对照，至少先完成 BBB 单端点的图构造和训练边权限核验。它不应阻塞先验证最近的在线方法，也不能用 confirmation 指标选 epoch。

## 这一审查支持的研究判断

目前可以声称“在线适配接口和反馈合同已经被真实缓存验证”，不能声称“PILOT/BayesianRouter 已完成正式复现”，更不能声称“动态 JEV 已经有效”。下一条 A800 任务应回答一个明确的 baseline 问题，而不是继续扩充标准算法名称。
