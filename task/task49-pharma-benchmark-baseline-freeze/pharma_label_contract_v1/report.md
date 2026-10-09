# Pharma label contract v1

日期：2026-10-05
状态：label receipt；不是方法效果实验。

## 标签来源

- 输入：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- 独立真值：`truth_y`
- provider 输出：`output_labels`
- provider status：`output_statuses`
- runtime status：`output_statuses`
- legacy end-to-end success field：`system_success`（该字段是 $r_{\mathrm{run}}r_{\mathrm{chem}}$；不是 runtime success；在 error cell 上不能解释为 chemical correctness）
- provider 菜单：`providers`

## 记录级定义

对每个 molecule/provider 记录：

$$r^{\mathrm{run}}=\mathbf{1}[\mathrm{output\_status}=\mathrm{ok}]$$

$$r^{\mathrm{chem}}=\begin{cases}\mathbf{1}[o=y],&\mathrm{output\_status}=\mathrm{ok}\\\mathrm{null},&\mathrm{output\_status}\ne\mathrm{ok}\end{cases}$$

失败观测统一写成 `output_observation=⊥`。原始 output/status 仍保留，便于审计；失败不被改写成化学错误 0。

## 产物

- 机器 receipt：`labels.jsonl`
- 汇总与输入/输出 hash：`summary.json`
- 生成脚本：`src/probe_headless/build_pharma_label_receipt.py`

该 receipt 是后续 JEV correctness head、runtime hazard head、bandit delayed reward 和 offline joint evidence 的共同输入。evaluation policy 不能读取当前未选 provider 的这些记录；它们只在 fit-only 训练或独立审计中可见。

## 语义纠错记录

第一次生成 receipt 时把 `system_success` 错当成 runtime success，得到的 `wrong=0` 与原始输出分布矛盾。该尝试没有被用于训练或报告；现已按 `output_statuses` 重建，并在生成时逐格验证 `system_success == 1[status=ok 且 output_label=truth]`。失败尝试保留在 `invalid_attempt.md`。
