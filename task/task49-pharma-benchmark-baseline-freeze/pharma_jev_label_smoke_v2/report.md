# JEV label-interface CPU smoke v2

日期：2026-10-05
状态：**通过；仅证明标签接口可执行，不提供选择效果或在线收益结论。**

## 结果

- fit records：14,652（444 molecules × 33 providers）；validation：4,389；evaluation：4,785。
- fit `r_chem=1/0/null`：12,550 / 1,958 / 144。
- fit `r_run=1/0`：14,508 / 144。
- correctness null 数与 runtime error 数一致；每条 receipt key 唯一，raw status/output/truth 与 receipt 逐格核对通过。
- 5 个 epoch 的两头 train/validation loss 均为有限值，并下降：train total `1.5096 → 1.3899`；validation correctness `0.7234 → 0.6776`；validation runtime `0.7495 → 0.6705`。
- evaluation read-set 仅包含 molecule bits、provider id、`r_chem`、`r_run`；`truth_y`、`output_label_raw`、未选 provider 当前输出列为 forbidden。

## 结论与边界

label contract 已能直接驱动 JEV correctness/runtime 双头，且 runtime error 不会被误训成 chemical wrong。该 smoke 没有比较 provider 选择策略，没有检验 VOI、bandit regret 或实时适应，也没有打开 A800 confirmation。

机器收据：`result.json`、`evaluation_predictions.jsonl`；协议：`preregistration.md`；实现：`src/probe_headless/jev_label_smoke.py`。
