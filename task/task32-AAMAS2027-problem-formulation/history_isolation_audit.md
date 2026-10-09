# Task32 历史信息隔离复核（修复前登记）

状态：进行中；本页是 Task32 诊断的更正附件，不修改 Task31 正式协议。

## 发现的问题

当前源码的 `rel_h=Rh[:,hist]-Bh[:,hist]` 仅从左项移除真实 challenger j。右项 `Bh[:,k]` 的 incumbent 仅排除 pseudo-challenger k，仍可能使用 j 的训练标签和预测，间接泄漏 j 的历史。receptivity 使用相同中间量。

旧两个 JSON 原样保留，暂不得据此把 H1/H2 写成已证伪；不删除、不覆盖失败记录。

## 修复与验收（先登记，后开跑）

对真实 held-out j，历史 pseudo-task k 的 incumbent 必须同时排除 j 和 k；实际部署 incumbent 仍只排除 j。保持原嵌套划分、特征、rank=8、Ridge alpha=1、B=4/8/16/32、五个 probe seeds，不搜索超参。

新增独立 r2 脚本和结果，不更改旧脚本及原始结果。对所有 j 扰动其 history reward 与预测列，历史 residual、receptivity、投影特征、incumbent prediction 必须不变；阳性对照必须检出旧路径的依赖。补充 chosen-feedback 与 evaluation-label 隔离测试。

输出需包含精确样本 ID/划分摘要、source hashes、算法参数、逐 episode 数据、分母及未定义比率；生成器重放哈希一致。

## 解释边界

本实验仅是嵌套训练区内 few-shot / ranking diagnostic，不是 Task31 正式在线结果。offline top-B ranking 不是 passive-stream 策略；few-shot evaluation 不含 probe 损失，不能当端到端 IOU。单个外层训练区的结果不能推广为所有划分的定理。

通过本轮审计只说明隔离缺陷关闭，不证明 H1/H2 为真或为假；负结果仅约束本次具体构造。

## 修复完成与验收

本项状态：完成。Task32 整体的问题/结构假设收敛仍进行中，不因此进入 Ours。

已实现真实 held-out j / 历史 pseudo-k 的双重排除。新增代码 `t32_strict_history_pack.py`、`t32_strict_history_audit.py`、`t32_verify_history_isolation.py`；原两个脚本和两个 JSON hash 全部不变。

三个 benchmark 共 58 个 challenger 的 poisoning 不变性通过，few-shot 结果共 4640 行；不是 4640 个独立样本。Task32 的 8 条定向测试通过，Task32/34 合计 15 条通过，完整结果表与限制见 `history_isolation_results.md`。

首次并行 ExtraTrees 版产生了不可逐字节重复的预测数组哈希，已保留失败 receipt 与源代码快照。仅独立诊断修订为 n_jobs=1，两个独立进程重放的 SHA256 均为 `76dec8b55cd2de59309797f03f6886f95d49005cfe154a52536dd002d3e792b4`；Task31 正式代码/配置未改。

验签权威：`data/tool_results/divergence/t32_history_isolation_verification.json`。旧版对照 decision 指标仅浮点级差异；MSE 差异另行报告，未宣称与旧版逐字节相同。

进一步发现 Task34 global auxiliary calibration 同样存在间接依赖，本轮按其 task.md 追加的修复登记独立关闭，详见 `task/task34-T3-modern-safe-calibration-closure/isolated_results.md`。
