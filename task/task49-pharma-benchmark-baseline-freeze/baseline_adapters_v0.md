# Task49 baseline 适配合同 v0

日期：2026-09-26，2026-09-27 更新
状态：候选源码和论文首轮审计已完成；统一合同 smoke 已通过，matched SGD、PILOT-adapted 和 BayesianRouter-adapted 已有 development 效果证据；Greedy LinUCB、GraphRouter 和在线 Laya/JEV 仍未形成完整主表。

## 统一任务接口

每轮输入为一个分子 $x$、性质端点 $e$、候选 provider 集 $A_t$、到当前时刻已经释放的历史反馈 $H_t$ 和预算。方法输出一个 provider $a_t\in A_t$，只能看到该 provider 的缓存输出；episode 结束后释放 $r_t=\mathbb{I}(\widehat y_{a_t}=y_e)$。所有方法共享输入流、反馈时钟和预算；选择不同会产生不同的反馈记录，不能强制它们读取同一个动作的标签。当前题目的未选输出、真值和未来反馈不可见。

训练/校准区只用于选择超参数和初始状态；最终 confirmation 区只在所有方法和代码冻结后打开。固定策略在训练区选出 provider；Oracle 只作 evaluator-side 上界，不能进入任何更新。

## 必做比较

| 方法 | 原论文/实现 | 适配方式 | 必须保留的差异 | 公平性检查 |
|---|---|---|---|---|
| Best Fixed | 本任务控制 | 训练区按准确率选一个 provider，整个流固定 | 不使用题后反馈 | 不能在 test 上选赢家 |
| Oracle | evaluator 控制 | 看完整预测和真值后逐题选正确 provider | 仅上界 | 不作为可部署算法 |
| Greedy LinUCB | AAAI 2026 *Online Multi-LLM Selection*；MIT 代码 commit `800c9e3b` | 用冻结分子特征作为 context，provider 为 arm，binary correctness 为 reward；按原算法更新 | 原论文是无离线数据、序列 prompt 演化；本适配固定单步科学请求并保留 selected-only feedback | 记录 feature、$alpha$、探索概率、反馈延迟和所有调参预算 |
| PILOT | Findings of EMNLP 2025；论文 SHA `d1fef526…9cd548` | 用同一冻结 encoder 得到 query/context，provider embedding 作为 arm；使用 preference-prior 初始化的 selected-arm ridge/UCB | 保留离线 query projection 与 provider preference fit；共同 warm start 可用训练区 full panel，在线只能用已选反馈 | 报告有无离线 prior 两个版本；prior 只能来自训练区 |
| BayesianRouter | ICLR 2026；参考代码 commit `2ee98a87`，无声明许可证 | 独立重实现论文中的离线 provider prior + selected-reward Bayesian posterior；输入换成分子 context，reward 换成 correctness | 不能直接运行仓库默认脚本冒充论文方法 | 明确 prior 均值/方差、观测噪声、posterior 更新和冷启动；实现只写独立代码 |
| GraphRouter | ICLR 2025；MIT 代码 commit `0edfb875` | 把分子/provider 节点和训练区的成功关系构成图，静态训练 router；最终测试时只读取冻结 router | 官方 `test()` 使用 train+validation edges；这本身不等于泄漏，确认集指标不能选 epoch；原生 `argmax` 有 index tie-break | 调参只用 calibration；报告原生 tie-break 与 tie-aware sensitivity；confirmation 不回流边 |
| RouterDC（可选补充） | NeurIPS 2024；官方仓库未声明许可证 | 仅按论文独立复现 dual-contrastive static router；不复制仓库代码 | 最低要求优先由 GraphRouter 承担；GraphRouter 当前仅审计，尚未满足实现/实验验收 | 若资源不足，保留审计和论文对照，不能写成已运行 baseline |
| Frozen JEV | Laya/JEV 表示冻结，matched head | 冻结同一 backbone，只训练训练区的静态 provider head或按协议零标签初始化 | 没有题后更新 | 作为表示和静态决策能力下界 |
| Online SGD | 同一 backbone/head | 每释放一条 selected feedback，只对新释放样本做一次梯度更新 | 不做历史回放；延迟样本到达后才更新 | 与 Proposed 完全相同初始化、特征和更新预算 |
| Replay SGD | 同一 backbone/head | 每次反馈释放，从固定大小 replay buffer 采样并更新 | 只回放已合法观测的 selected samples | 报 buffer 大小、步数和重采样 seed |
| Proposed | 待方法卡冻结 | 只有在通过近邻和 no-go 审查后进入 | 不得先把标准 ridge/SGD 重新命名 | 由 matched-backbone 消融证明新增机制贡献 |

## 源码审计结果

PILOT 先进行两阶段离线 preference 学习，在线冻结 query projection，并用 provider embedding 初始化 selected-arm ridge。其初始化形式是 $A_{a,0}=\lambda I$、$b_{a,0}=\lambda\theta_{a,\mathrm{pref}}$；每条已选反馈用 rank-one 项更新。选择分数的均值使用 cosine，而非普通点积，再加 UCB 项。二元任务可能多个 provider 同时正确，离线 pairwise preference 必须规定平票处理，不能伪造唯一赢家。这已经覆盖“离线判别先验 + 在线轻量更新”的主要直觉，所以“JEV 加普通在线线性头”不能直接作为新方法。

BayesianRouter 的论文说法是离线 Bradley--Terry embedding 作为每个 reward-model 权重的 Gaussian prior，再以 selected reward 做 posterior。审计的固定 commit 有两个不可忽略的实现事实：`train_bo_router4.py` 中实际选择行固定为 `selected_rm_indices=[2]*N`，真正的 `rpc_select_arms` 调用被注释；`utils4.py` 的一条路径把 prior embedding 置零；README 指向的 100M2 训练入口默认 `lambda_cls=0`，与论文描述的分类项 0.2 不同。故主实验必须独立按论文重实现并把默认仓库作为代码审计材料，不得用坏默认制造对 Proposed 的增益。

GraphRouter 的官方实现可以复用（MIT）。源码每个 epoch 调用 test，构图使用训练和验证边；仅使用训练/校准边本身不构成确认集泄漏，但不得用 confirmation 指标选 epoch。原生多候选平票由 `np.argmax` 按索引打破；二元任务的平票需预登记处理。正式协议中 calibration 边可用于最终 fit，confirmation 边只能在 evaluator 评分；原生和 tie-aware 两种读法都要预先写明。BBB 单端点中 task 节点退化为一个，必须披露这项适配限制。

## 监督量和信息预算

按初始化权限与在线权限两个轴区分，不能按算法名字分配不同的训练信息：

1. **共同 warm start**：同一个训练区 full panel 同时向静态 router、PILOT/BayesianRouter prior、matched heads 和 Proposed 开放。每个方法如何使用这些标签，以及调参预算，全部记录。evaluation 中只有所选动作反馈；静态方法不更新。
2. **零目标标签 cold start**：单列实验，在所有方法都没有本域训练标签时比较。零 prior 的消融不能代替拥有完整离线先验的 PILOT/BayesianRouter 主对照。
3. **Evaluator-only**：Oracle 只用于机会空间，不是训练输入。

另外加入 matched-label direct predictor control：在相同已释放的二值标签访问条件下，直接预测 $y_e$，检查选择器的收益是否只是借助题后可恢复真值，而不是学会 provider 的条件差异。

## 适配后的评价

主指标是未来 episode 的 cumulative error。同步记录前缀/后缀 accuracy、达到固定误差水平所需的反馈数、选择和更新 p50/p95 延迟、峰值内存，以及（如实验卡冻结）调用预算。任何方法必须先决策后更新；延迟反馈使用相同队列规则、各自状态；失败、缺失和 unresolved 真值单独记账。种子间波动描述顺序和策略随机性，不冒充独立分子泛化区间。若做 scaffold 重采样区间，必须重新运行学习轨迹，不能对固定在线日志逐行 bootstrap。

主比较在开跑前选择一种目标并冻结最小有意义差异：相同计算预算下减少错误，或在预定准确率非劣界内降低完整时延/内存。不能看到结果后切换目标，也不要求效率方法同时打败所有方法的绝对准确率。必须对照 Frozen、Online SGD、Replay SGD 和最强合法近邻；若只有 Best Fixed 被超越，不能据此认定新机制成立。若新增状态在错误和成本两方面都无可识别贡献，撤回该机制的创新主张。

## 许可和复用边界

GraphRouter、Online Multi-LLM Selection、LLMRouter 的本地快照带 MIT；BayesianRouter 和 RouterDC 仓库快照未声明许可证。后两者只作为论文和行为审计来源，主代码独立重写。每个快照、commit、文件 hash 和原论文链接见 `sources.jsonl` 及 `baseline_audit/*/SOURCE.json`。

下一步是把这张合同转成统一 runner 的配置 schema和静态检查，先用冻结 BBB development 数据做小批 **适配正确性** 回放；那一轮只验证信息隔离、更新时序和输出对齐，不汇报方法效果，也不打开 confirmation。

## 2026-09-26 实现进度

Greedy LinUCB H=1、PILOT-online kernel、BayesianRouter-online kernel 已完成数值式与同一反馈队列实现。20 项检查与 576 次真实缓存选择通过，另 6 条固定失败控制通过，见 [实现合同](online_adapter_v1/contract.md) 与 [日志报告](online_adapter_v1/report.md)。2026-09-27 新增 PILOT-adapted 两阶段离线 fit 与 64 题真实缓存接口 smoke，独立恢复通过，见 [PILOT fit smoke](pilot_offline_fit_v1/report.md)。

### 2026-09-29 当前证据更正

- matched Online-SGD/Replay-SGD 已有 5 folds × 2 seed slots × 3 delays 的 development effect；90/90 streams 和独立 NumPy recovery 通过，结果仍低于 Best Fixed。见 `experiments/logs/task49_matched_sgd_development_20260927/report.md`。
- BayesianRouter-adapted 已有同一 BBB 输入上的完整负结果；PILOT-adapted 也有完整负结果。两者都是适配版，不能写成原论文完全复现。
- Laya forward 和静态 representation diagnostic 已完成但为负；在线 Laya/JEV 没有原生 update API，需另定义外接 head 合同。
- Greedy LinUCB 只有 fit-prior 适配候选和独立 verifier，尚未运行效果；GraphRouter 只有源码审计和静态适配阻塞说明。
