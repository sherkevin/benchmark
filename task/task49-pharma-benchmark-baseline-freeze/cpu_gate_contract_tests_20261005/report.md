# CPU contract gate check v1

- 日期：2026-10-05
- 状态：`passed`；只读核对，不重跑策略、不估计效果、不授权 GPU。

## 目标

在继续 E5 schema/denominator 和 E2 role-qualified 主表之前，检查当前 E1–E5 coverage、baseline eligibility、endpoint qualification 和 Replay provenance 的机器收据仍然一致。

## 结果

执行 7 个 pytest：**7 passed, 0 failed, 1.48 s**。

通过的检查覆盖：

- E1–E5 coverage 仍是 `partial`，E6 没有打开；
- baseline 角色分层没有跨层合并；
- BBB/AMES/hERG endpoint qualification 的状态和输入 hash 未漂移；
- Replay-SGD 的 separate-control provenance 没有被误升格为 strict matched。

## 边界

这不是效果实验，也不证明 MIRA-Relay、FamilyGuard 或任何 baseline 优于其他方法。它只证明当前审计索引和分层合同没有被论文收口工作破坏。完整记录见 [`result.json`](result.json)。

## 下一步

继续 E5 schema/denominator closure，再生成 role-qualified E2 development table；在 E1–E5 仍为 partial 前不打开 E6 confirmation，也不申请效果型 A800。
