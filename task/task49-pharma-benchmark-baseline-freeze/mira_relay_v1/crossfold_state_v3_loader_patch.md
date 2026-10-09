# Cross-fold state v3 loader patch

日期：2026-10-04。此文件只记录 provenance 修正，不是新的 fit 或 replay 结果。

第一次 fold0 回放启动在第一条事件前耗时约 7 分钟，原因是 runner 对已经封存的 fit-state 重复执行了完整 objective/Hessian verifier。为使 replay 成本可控，新增 `load_sealed(...)`，把昂贵的独立数值核验固定在 seal/verify 阶段；replay 阶段只检查 manifest、候选/数组 hash、status policy、源码 hash 和 canonical state hash。

`load_sealed` 只读已有 state archive，不改变 theta、direct prior、family residual、precision、runtime counts 或任何候选状态。五个 fold 的 `fit_state.npz` SHA-256 和 canonical state hash 未变。由于 loader 属于同一模块，五个 state config/manifest 的 `crossfold_module` source hash 已更新，并保留旧 hash、seal 时的 code identity 和 patch commit `42d81f1b`。这是可追溯的元数据刷新，不是重新拟合。

没有使用中止的 fold0 进程输出作科学证据；它只有 inputs/precompute 配置，没有 decision/terminal/feedback。
