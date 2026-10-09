# MIRA-ContextShield 实现前审查

日期：2026-10-02  
状态：候选保持 HOLD；允许完成本地实现性检查，不支持效果结论。

## 审查结论

独立方法审查指出：provider 边际 Beta 不足以代表当前分子风险，这个问题值得修；但
ContextShield 的上下文 covariance 本身也不能被自动解释成 calibrated correctness
interval，更不能在 selected-only、无探索时识别未选 provider 的当前潜在结果。

本轮先修以下五项，再做 32 条 implementation smoke：

1. 定义 runtime Beta 账本和 `delta_h=.05`；failure 不作为 correctness 0。
2. 固定 reference provider 和 reference posterior，避免 mutable incumbent 锁定。
3. 明确 `direct_prior` 是 common-stratum 的 sealed BCE/logit head，不是 BT score.weight；
   但预测仍称 logit surrogate，不宣称校准概率。
4. 明确只继承 FamilyGuard 的 hard availability block，不继承 soft family penalty。
5. 当前协议无 refusal，fallback 始终 action-eligible；这个边界必须报告。

## 仍未关闭

- BCE/logit 初始化与 mean-Fisher 尺度已经固定；在线更新仍是局部 MAP/Laplace 近似，不能解释为完整历史后验。
- same-Fisher/no-gate、same-gate/no-update、no-family 和 mutable-reference 机制对照尚未运行。
- 小流只有 fold0/seed0，不能判 harmed/rescued superiority。
- 没有任何 online challenger feedback 仍可使用 fit prior 选 challenger；若要识别未选 provider
  当前质量，需要额外的探索/审计协议，不能通过命名绕过。
- 独立 verifier 必须绑定 inputs/source/config/summary/timing，不能只重算 action 后写 verified。

## 32 条烟雾运行

`experiments/logs/task49_mira_context_shield_smoke_20261002/v1/` 在 Frozen state hash 接口处
失败，尚未产生完整 stream；保留失败目录。

`v2/` 完成 2 streams、192 events：Frozen `28/32`，ContextShield `27/32`。
ContextShield 的 32 次有效反馈均改变 surrogate，但 gate-open=0、action 全为 fit-only
fallback。当前只是“更新状态但未影响动作”，不算实时学习的效果证据。

下一步先补严格 verifier 和日志绑定，再运行 fold0 的 145 条固定小流。若仍无 gate-open，
停止该 gate 构造，不继续调 lambda/delta。
