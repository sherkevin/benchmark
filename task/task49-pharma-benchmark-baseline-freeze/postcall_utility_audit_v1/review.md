# 调用后效用审计 v1：零机时设计审查

日期：2026-10-01  
状态：通过设计审查；允许下一步做 CPU development replay，不申请 A800，不打开 confirmation。

## 审查结论

这项审计补的是当前 Gate C/E5 的一个具体缺口：已有拒答和追加结果只有 accuracy/coverage/call count，尚未回答“少答或多调用付出多大代价后仍然值得”。它不替代 direct selection 主比较，也不把 post-call verifier 写成 MIRA 在线收益。

审查通过，理由是：

1. 动作和信息边界已经固定。阈值只用 fit correctness/confidence，evaluation 只看被调用 provider 的返回；拒答不读取真值、不释放 chemical correctness，也不更新在线状态。
2. 效用把错误答案、最终无返回、主动拒答和调用数分开记账。它不使用 `system_success` 直接代替错误，因为该字段把 runtime failure 也编码为 0。
3. 已有汇总不是逐样本证据。本轮明确从冻结 input/cache 独立重建每一行，并用 `ReplayEpisode` + `TruthStore.evaluate` 重算终止和逐调用 correctness；旧汇总只作 hash/计数交叉核对。
4. 真实输入的零机时分母核对已通过：五折 evaluation 共 722 行；第一 provider 全部 `status=ok` 且 confidence 数值存在；threshold policy 接受 573、主动拒答 149；escalation 候选的第二 provider 失败 6 次，需在最终失败/追加调用账中单列。

机器收据：`experiments/logs/task49_postcall_utility_contract_audit_20261001/v1/denominator_audit.json`，状态 `verified`。配置与输入 hash：`experiments/logs/task49_postcall_utility_contract_audit_20261001/v1/config.json`、输入 SHA `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。

## 必须遵守的实现约束

- `system_success=0` 只能作为历史选择反馈字段；utility 运行时从 `output_statuses`、`output_labels` 和 `TruthStore` 分开判断 `wrong` 与 `runtime failure`。
- 第一 provider 的 failure/缺失 confidence 若在新重放中出现，立即标记 `BLOCKED`，不静默缩小分母。
- 第二 provider 的 6 个 cached failure 不能转成主动拒答，也不能携带 `chemical_correct`；若第二调用失败且没有最终答案，计 final failure；若已有可用终止结果，failure 只进事件账。
- `ReplayEpisode` 每条记录最多两次调用；主动拒答使用显式 `episode.abstain()`，其 `answer_correct` 必须为 null，且不计 wrong/failure。
- 逐 entity 保存 action、provider/status/raw/label/error code、confidence、threshold、scaffold、终止 grade、failure/refusal/wrong 计数。不能只输出 pooled accuracy。
- independent recovery 不导入 runner 的结果对象；从相同 input/cache 重新计算 provider、threshold、动作、grade、计数和 utility，再逐字段比对。

## 结果解释边界

本轮若通过，只能补充：在冻结的 BBB development post-call cohort 上，静态 confidence refusal/escalation 的 accuracy、coverage、failure 和调用成本如何转换成预登记效用边界。它不能支持：

- pre-call direct selector 的提升；
- selected-only online learning 或 MIRA 动态状态的提升；
- hERG/AMES、多端点、model-fresh 或 confirmation 泛化；
- “拒答一定有临床收益”这类未定义成本主张。

如果 utility 只在很低的拒答/调用代价下为正，下一步应优先改机制或获取新的可观测信号，不靠换成本点包装结果。该审计本身不自动提高 Gate C 分数；它只为 E5 提供一条可恢复的效用和错误归因证据。

## 下一步

先实现独立的 development runner/recovery，执行一次 CPU full replay。若 provenance、failure/unknown/refusal 语义或独立恢复任一不通过，保留失败现场并停止；若通过，再把结果写入 Gate C 矩阵，之后才讨论是否值得做动态 verifier 的 selected-only 更新。GPU 与 confirmation 继续关闭。
