# selected output + correctness 真值恢复审计 v1

日期：2026-09-29
性质：只读一致性审计，不训练、不调用外部 API、不读取 confirmation。

## 目的

验证现有 selected-only 事件日志里的二元 provider 输出 `z`、已记录的 correctness `r` 和 `recovered_label` 是否满足

$$
\tilde y=z\quad\text{if }r=1,\qquad \tilde y=1-z\quad\text{if }r=0.
$$

这只验证已有日志的字段关系，不证明新方法有效，也不证明所有失败/unknown 事件都能恢复真值。

## 输入

扫描 `experiments/logs/task49_*/*/events.jsonl` 中已有的 terminal 事件。只纳入 `selected_output.label`、`chemical_correct`、`recovered_label` 都是二元值的事件；缺字段、失败或 unknown 单独统计，不补标签。

## 预登记判据

- 有效事件的恢复公式不允许出现 mismatch；
- 缺字段事件不得进入有效事件分母；
- 输出每个输入文件的 SHA-256、有效数、缺失数、mismatch 数和命令环境；
- 任一 mismatch 只说明日志合同或实现有问题，停止使用该通道，不能通过删除样本修复。
