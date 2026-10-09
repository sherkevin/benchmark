# Information screen v1

## 目的

matched-SGD 已经说明普通 selected-only 参数更新没有超过 train-only Best Fixed。下一步不能只换优化器，必须先确认反馈里是否存在可迁移的信息。本筛选检查两种来源：provider 之间的错误相关性，以及相似分子之间的残差相似性。

## 做法

数据是已经冻结的 `fingerprint_diagnostic_v1/inputs.json`：722 个 BBB development 分子、33 个 provider、5 个 scaffold-disjoint outer folds。没有新工具调用、没有训练、没有打开 confirmation。

每个 fold 的 fit 区只用于估计 provider 的基线正确率。provider transfer 用 fit 残差相关性筛出正相关有序 pair，再在 evaluation 上测试：观察 provider $a$ 的残差能否预测 provider $b$ 是否高于其 fit 基线。local memory 用 fit 区 Morgan bit-set 的 Jaccard top-1/top-5 邻居预测 evaluation 残差，并用固定种子的随机邻居作对照。

可执行入口是 `src/probe_headless/screen_pharma_information_advantage.py`。原始结果在 `experiments/logs/task49_information_screen_20260927/analysis_v1.json`，汇总在 `summary_v1.json`。

## 结果

| fold | fit-positive provider pair AUC | fit/eval pair-correlation | Morgan top-1 AUC | random top-1 AUC | Morgan top-5 AUC | random top-5 AUC |
|---:|---:|---:|---:|---:|---:|---:|
| 0 | 0.7312 | 0.7801 | 0.5708 | 0.5045 | 0.6284 | 0.5131 |
| 1 | 0.7220 | 0.7649 | 0.5543 | 0.4700 | 0.5699 | 0.4863 |
| 2 | 0.7802 | 0.8499 | 0.6082 | 0.4862 | 0.6518 | 0.4736 |
| 3 | 0.7121 | 0.7669 | 0.6343 | 0.5259 | 0.6595 | 0.4331 |
| 4 | 0.7375 | 0.8680 | 0.6292 | 0.4764 | 0.6146 | 0.4396 |
| **mean** | **0.7366** | **0.8060** | **0.5994** | **0.4926** | **0.6248** | **0.4691** |

两个信号在五个 outer fold 都同方向。fit 相关 provider pair 在 held-out evaluation 上的平均 transfer AUC 为 0.7366；Morgan top-5 残差 AUC 为 0.6248，而随机邻居为 0.4691。这个结果比之前的 stream drift 审计更有用：可迁移信息来自分子/provider 的关系，而不是样本到达顺序。

## 解释和限制

这不是在线策略效果，也不是证明最终方法有效。筛选使用了完整 evaluation panel 来测“是否存在信息”，在线策略仍只能看到所选 provider 的已释放反馈。AUC 只说明反馈有预测价值，还没有回答跨 provider 更新是否会在真实选择轨迹上提升准确率、是否会把错误传播出去、以及延迟下是否稳定。

local memory 目前只筛了 Morgan 表示，Laya 表示的同类筛选另行处理；因此不能据此断言 Morgan 是最终 backbone。provider transfer 的 pair 选择严格来自 fit 区，避免把 evaluation 相关性写入候选方法。

## 下一步裁定

provider transfer 通过了“有额外信息”的第一道筛选，优先进入方法合同：用 fit-only provider 残差关系构造受约束的非对角/低秩在线更新，并保留 Frozen fallback 或置信门，防止一次错误反馈扩散。local residual memory 作为轻量对照和消融保留，不先把它写成主创新。

在申请新的 A800 任务前，先完成三件事：

1. 写清 provider-transfer 的最小可实现更新、状态大小、延迟释放和误差传播边界；
2. 用只读回放做一个不涉及新训练的 transfer replay，比较独立更新、相关更新和 Frozen 的逐题状态变化；
3. 若 replay 信号在预先登记的硬否定条件下仍成立，再接入统一 runner，和 Best Fixed、Frozen、Online-SGD、Replay-SGD、BayesianRouter-style 同表跑 development。

confirmation 集继续关闭。
