# JEV label-interface CPU smoke v2

日期：2026-10-05

## 目的

验证 MIRA/JEV 的两个监督头能从正式 label receipt 读取不同标签：correctness head 对 `r_chem=null` 做 mask，runtime head 使用全部 `r_run`。这是接口与数据语义闸门，不是工具选择效果实验。

## 固定协议

- 输入：`fingerprint_diagnostic_v1/inputs.json`；label receipt：`pharma_label_contract_v1/labels.jsonl`。
- fold 0：fit 444 molecules、validation 133 molecules、evaluation 145 molecules；evaluation 不参与优化。
- 特征只包含 Morgan 1024 bits 与 provider one-hot；不读当前 provider output、truth 或未选 provider 当前结果。
- 共享 32 维 ReLU encoder，两个线性头；AdamW，5 epochs，lr `1e-3`，weight decay `1e-2`，seed `20261005`。
- correctness BCE 只在 `r_chem != null` 的 fit cells 上计算；runtime BCE 在所有 fit cells 上计算。

## 通过/失败条件

必须满足：receipt 对齐、`r_chem` null 数等于 runtime error 数、两项 loss 有限、evaluation read-set 不含 truth/output label。任何一项失败则不进入 JEV policy replay。
