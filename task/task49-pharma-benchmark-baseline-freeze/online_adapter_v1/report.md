# 在线 baseline 接口验证结果

2026-09-26。

## 这一步推进了什么

三个 baseline 的在线核心已能接入同一条真实 BBB 回放流。反馈到期后做一次 selected-arm 更新，队列按实际到达顺序工作。它们现在有可复算的实现；还没有完整离线先验，也没有动态 JEV 的效果结论。

代码与数学对应关系见 [contract.md](contract.md)，开跑前的范围和停止条件见 [preregistration.md](preregistration.md)。

## 实际执行

- 20 项单元检查通过：非零先验/非单位噪声的递推等价、固定官方 Greedy LinUCB 源码对齐、PILOT cosine、TS 投影采样、延迟与重复反馈、失败会计、Frozen 控制。
- 真实缓存：64 个 development 分子、33 个具体 provider，3 个 kernel × 3 种延迟，共 576 次决策和 576 次终局反馈。
- 独立 fixed-provider 检查直接使用 `b3clf_dtree_common` 的 6 条真实失败；全部保留在分母，system success=0，chemical correctness 与可恢复真值保持 null。
- 自适应流也实际遇到 23 次失败，均按同一规则保留。不同流重复使用分子，这个 23 是事件数，不是独立分子数。
- 所有日志中的所选输出与原缓存一致；无重复消费、无提前更新、无遗留 pending。全部九条流的在线终态与独立批量求解一致，均值最大绝对差约 $2.17\times10^{-15}$，协方差约 $1.00\times10^{-15}$，小于预登记容差。

没有新科学工具调用、LLM/API、backbone forward、GPU 训练或 Nebula 提交。CPU 用于接口数值检查；后续科学训练仍按授权使用 A800。

## 证据

日志根目录：`experiments/logs/task49_online_adapter_20260926/`。

| 产物 | 内容 |
|---|---|
| `tests_v1/config.json`、`pytest.log`、`junit.xml`、`events.jsonl` | 命令、源码 hash、环境、逐检查结果 |
| `real_cache_v1/config.json`、`input_manifest.json` | 固定参数、执行 HEAD、修改代码 hash、缓存/参考源 hash |
| `real_cache_v1/inputs.json` | 64 个 ID、完整菜单、冻结 feature 和随机 fixture prior |
| `real_cache_v1/events.jsonl` | 每条决策、真实输出、终局、到期 feedback 和耗时 |
| `real_cache_v1/*_state.npz` | 每条流初态、终态和更新计数 |
| `real_cache_v1/summary.json` | 逐流守恒及数值误差；正确总数仅供审计 |

随机 prior 和 64-bit feature 是验证 fixture，不是调好的模型。因此没有用本轮准确率排 baseline 名次，也没有效应置信区间或 p 值。本地耗时不能代表 A800 全链路实时性。

## 回头看有没有跑偏

没有改变“给分子选一个科学工具，再按题后反馈更新”的任务。代码复用了已有环境，排除了会毁掉对比的数值和信息泄漏问题。它使下一次训练可以被严格检查，但还不能支持论文主张。

下一步两个必要工作：补齐 PILOT / BayesianRouter 的离线 fit 与预算；解决模型资产可达性后，在单条 A800 任务提取 Laya 表示，再按新实验卡比较固定表示与可学习表示。若固定表示没有有效信号，需要回到 backbone；若标准在线头已解释全部收益，则必须重新审查我们的创新点。
