# MIRA-Relay 候选审查

日期：2026-10-03
审查状态：候选实现与 CPU 合同审查；尚未进入 full replay。

当前实现已通过 queue/kernel/fit-diagnostic 合同单测，但 fit-only L-BFGS-100 按预登记预算未收敛。
解析 Hessian 的 trust-exact 仅完成数值候选诊断，尚未替换生效 preregistration。

## 通过点

- family residual 使用已经存在的 provider family registry，语义比任意随机 embedding 清楚；
  `z_a^0` 负责 fit-only 冷启动，`u_g` 负责可共享的在线偏差，`b_a` 保留同族 provider 差异。
- 共享项是 family-specific interaction，而不是对所有 provider 加同一个
  `theta^T phi(x)`；后者在排序时抵消，不能形成 action change。
- fallback reference 不被在线更新，避免一次 probe 把安全参考永久改写；runtime failure、unknown
  和 correctness 分开计账。fallback 事件可写入运行可用性和诊断正确计数，但 `residual_update=false`，
  不改变 residual mean/variance 或 provider bias。
- 对角 precision projection 状态使一次 release 为 `O(d)`、一次选择为 `O(Kd)`，可以直接测
  实时成本；它是明确的近似，不能把坐标方差更新写成完整 Laplace 逆矩阵的对角。
- `ProbeNoUpdate` 与 `NoProbeShield` 把 probe 直接效果和 feedback→exploitation 效果分开，
  能对准 ContextProbe 当前不可识别的问题。

## 必须在实现前防住的风险

1. family registry 必须绑定输入 hash；禁止用 evaluation 结果重新归 family。
2. residual fit 必须只读 fit cells，并保存优化状态、loss 和最终参数 hash；不能用 confirmation
   选择 `lambda`、`kappa` 或 residual rank。
3. probe 只能从动态 safe menu 抽取，并记录 menu、eligible set、propensity、hazard 和 block
   原因；blocked provider 不可被 probe 静默绕过。
4. 每条 decision 必须写出 family residual score、uncertainty、feasible challengers 和
   action；缺字段时 verifier 失败，不能用空数组默认。
5. 同一 fold×seed 的 delay 必须共用 probe schedule；否则 delay 比较混入随机抽样差异。
6. 4,332 个 task 不是 4,332 个独立样本；主区间按 molecule/scaffold 聚类，不能把 probe 行当 iid。
7. 在任何 full replay 之前，先用一条 fit-derived `φ` 和合法的 `y=0/1` release 做 state→action
   sensitivity 单测；独立重算后至少一个 challenger 的 score/ranking 必须改变，否则不进入大回放。
8. `ProbeNoUpdate` 只能证明在线更新是否有作用；必须增加同初始化的 `ProbeIndependentResidual`，
   才能判断 family sharing 是否带来额外作用。family permutation 要保留组大小，不能只重命名。
9. 同一随机数组只是 common-random-number coupling；动态 safe menu 变化时不能宣称 probe action
   相同。完整 propensity 和 branch propensity 都要入事件。
10. runtime verifier 只看 output/status；frozen endpoint judge 在终止后产生 correctness label。
    unknown/refusal 不能伪造 0/1；成功返回的 unknown/refusal 可更新运行诊断，但不更新 correctness residual。
11. fit optimizer 的 objective、gradient、Hessian 和参数 hash 必须由独立 verifier 重算。SciPy
    `status=2` 不能直接改写为成功；只有满足预先写明的数值停点和有限性条件，候选 fit 才能进入
    replay。当前跨折 trust-exact 诊断支持其作为候选，但不构成下游效果证据。

## 不能提前写进论文的结论

目前只能称为“共享残差动态选择候选”。在 CPU receipt 通过机制门、对照门、成本门并完成独立
回放之前，不能称它已经提升准确率、保证安全、建立 regret/safety theorem 或证明实时训练优于
静态 selector。
