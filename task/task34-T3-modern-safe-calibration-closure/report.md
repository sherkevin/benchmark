# task34 report — Modern Safe/Calibration Closure

> 状态：⚡️进行中

## 起点

Task32 已否定 relative-residual mean transfer 与 simple historical receptivity；Task33 已证明 AISTATS 2026 risk-sensitive abstention 在 frozen high-dimensional learned context geometry 下 assumption mismatch / near-always-commit。

本任务审计 modern conservative/contextual risk-control 与 meta-calibration，并测试“历史 pseudo-arrival 能否迁移 uncertainty certificate”这一更弱假设。

## 2026-09-22 — Auxiliary calibration 隔离修复与实测

状态：本次实现修复/固定构造诊断完成；整个 modern safe/calibration 文献与 fidelity closure 尚未全部验收，不标整条完成。

旧实现先对所有 pseudo-k 全局预计算 `Bh/Ih`，再仅从 auxiliary tasks 删掉实际 challenger j。这样 k 的 baseline 仍可含 j，违反零历史协议。修复已按 task.md 事前登记：先删除 j 的 reward/model columns，再为 pseudo-k 删除 k；原脚本/原结果不覆盖，保留 `legacy_isolation_snapshot.json`。

新脚本：`src/probe_headless/t34_meta_calibrated_gate_isolated.py`。保持 B=4/8/16/32、三档 alpha、五个 target seeds、三个 calibration seeds、Ridge alpha=1；未改 Task31。使用的严格 pack 与 Task32 r2 相同。

全部三套内层数据已跑完，4640 条方法/预算/seed 记录；固定的 meta-calibrated 设置均未得到正平均 total IOU。LLM 的 probe 后 adoption gate 全为零；ASlib 三个设置的平均 post-gate IOU 有约 +3.6e-08 至 +3.8e-08 的微小正值，不能误写成全部非正，但其总 IOU 仍为负。BBB 的 meta-calibrated post-gate 平均贡献非正。减小损失不等于成功找到 challenger 优势区域。完整三种 alpha/四种 B 的结果与 probe-inclusive 分解只在 `isolated_results.md` 维护。

原始结果：`data/tool_results/divergence/t34_meta_calibrated_gate_trainonly_isolated.json`。重复进程输出 `_isolated_repeat.json`，两者逐字节相同。验签：`data/tool_results/divergence/t34_isolated_calibration_verification.json`，状态 PASS。

新校准路径真实数据 58 个 challenger 的 poisoning 不变性通过；与 Task32 合计 15 条回归通过，含旧路径泄漏的阳性对照、auxiliary quantile 不变性、probe 成本记账。日志 `isolation_regression_tests.log`。总 IOU=probe IOU+post-gate IOU 的最大绝对误差约 1.1e-16。

### 结论边界

这是 pooled auxiliary overestimation quantile 的自定义诊断，**不是**某个正式 conformal / risk-control baseline 的 fidelity reproduction，也没有 distribution-free certificate。历史 in-sample incumbent error 与目标 out-of-sample error 的分布差仍未校正，不能仅凭本轮负结果否定整个 meta-calibration 方向。

Task32 r1 的“已否定 H1/H2”前提已被更正，详见其 r2 report。当前仍不进入 Ours；后续若测试 cross-fitting 或改 calibration 定义，需独立登记新任务/版本，保留当前失败证据，不覆盖结果挑赢家。
