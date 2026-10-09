# Affine Structured JEV 静态诊断 v1

日期：2026-09-29  
状态：候选诊断，未运行。

## 问题

Structured CPU v1 的 Frozen structured 明显低于 fit-only Best Fixed。当前 h 头没有 provider-specific intercept，可能无法表示 provider 的全局可靠性差异。这个诊断只检验该实现瓶颈，不宣称动态方法有效。

## 固定设置

- 输入、722 BBB development molecules、33 providers、5 个 scaffold-disjoint folds、2 seeds 与 `structured_cpu_effect_v1` 完全相同；输入 SHA 写入配置。
- 读取同一份已恢复的 `bt_cls` 1024→64 ReLU encoder，只用 `encoder.0.weight/bias`；不重新训练 encoder。
- q 改为 $q(v)=\sigma(b_q+\theta^\top v)$；h 改为 $h_a(v,y)=\sigma(b_a+\phi_a^\top[v;y])$。这是唯一改变：每个头增加一个 fit-only intercept。
- fit-only AdamW、200 epochs、lr `0.001`、weight decay `0.01`、无超参搜索；validation-loss 最小 epoch；随机初始化标准差、betas、epsilon 与 v1 一致。
- h 的 fit/validation 仍排除 provider failure；direct/system-success 不作为该诊断的目标。
- evaluation 时只使用 frozen affine score；不做在线更新，不读当前 evaluation truth 进入 score。

## 对照和门槛

- 主要对照：v1 Frozen structured 的同 fold/seed evaluation outcome；外部参考：fit-only Best Fixed。
- 预注册 go 条件：affine Frozen 相对 v1 Frozen pooled accuracy 至少提高 `0.01`，且每个 seed 至少 4/5 folds 的 paired delta 为正；同时不能出现 evaluation leakage 或 checkpoint/hash 错误。
- 若不满足，停止把“缺少 provider intercept”作为下一方法依据；不运行 affine dynamic 效果流，不提交 A800。
- 若满足，也只能进入下一份独立预注册：在同一信息合同下比较 affine Frozen、affine matched SGD、affine structured；本诊断本身不支持动态 JEV 主张。

## 证据

输出必须保存配置、源/输入/checkpoint hash、逐样本 selected provider/outcome、fit manifest、汇总和独立检查结果。该轮仍是 BBB development 诊断，confirmation 保持关闭。
