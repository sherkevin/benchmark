# H=0 双标签 JEV 选择器 CPU 回放 v1

日期：2026-10-05
状态：开发期预登记；不打开 confirmation，不申请 Nebula。

## 目的

验证新 label contract 是否足以构造一个可用的冷启动选择器。模型分别学习
`r_chem`（仅在 provider 正常返回时监督）和 `r_run`（每个 provider cell 都监督），
选择前用两头概率乘积估计端到端成功概率。这个实验只测一次调用的 H=0 选择，不含
bandit 探索、延迟更新、验证调用、拒答或 VOI。

## 主张、近邻与否证条件

- **候选主张**：将 runtime hazard 与 chemical correctness 分开建模，可能比一个直接
  `r_sys` 头更适合 cold-start tool selection。
- **最近邻**：同一 Morgan-1024、同一 64 维隐藏层、同一 fit/validation scaffold split
  的 direct MLP BCE（历史 fingerprint diagnostic）；训练区 Best Fixed 是强固定参照。
- **否证条件**：双头选择器没有超过同折 Best Fixed，或没有超过同容量 direct `r_sys`
  头；任一条件成立都不把双头接入 online 主方法，只保留接口和负结果。

## 数据和信息权限

- 输入沿用 `fingerprint_diagnostic_v1/inputs.json`，722 个 BBB development 分子、33
  个 provider、5 个 scaffold-disjoint outer folds；输入 SHA 由运行时记录。
- 标签只读 `pharma_label_contract_v1/labels.jsonl`：`r_chem=null` 仅表示 runtime error，
  不能当成 chemical wrong；`r_run` 取 raw status。
- fit 区标签用于梯度，validation 只选 checkpoint，evaluation 只由 evaluator 读取真值和
  完整缓存计算成绩。决策时禁读 truth、raw output、未选 provider 当前结果和 evaluation labels。

## 固定训练协议

- Morgan radius 2、1024 bit；共享 ReLU hidden-64；两个 33-way linear heads。
- AdamW，learning rate `1e-3`，weight decay `1e-2`，full batch，200 epochs；seed slots
  `0,1`，不扫参。validation objective 是 masked correctness BCE + runtime BCE，平分取早 epoch。
- 选择分数：`sigmoid(correctness_logit) * sigmoid(runtime_logit)`；菜单固定为 33 个 provider，
  argmax 平分按 provider 菜单顺序。
- 训练、评估和恢复均在 CPU；固定一条线程；无外部 API、无新预测、无 Nebula 资源。

## 记录与恢复

每个 fold/seed 记录 epoch loss、validation-best checkpoint、每个 evaluation 分子的 33
个分数、选择、`system_success`、fixed 和 oracle 对照。独立 NumPy verifier 重新执行
`Linear → ReLU → heads → sigmoid product`，核对 checkpoint hash、选择、成绩、分母和十个
fold/seed 槽位。原始事件、checkpoint、summary、verification 都保存在对应实验日志目录。

## 结果处置

这是 cold-start diagnostic，不是 online gain、regret、VOI、confirmation 或 AAMAS 最终效果。
无论正负，都保留完整收据；只有通过预登记否证条件后的下一阶段才可实现 selected-only
online adapter，且必须重新登记延迟 `{0,1,4}`、Frozen、Matched Online-SGD 和主方法的同表比较。
