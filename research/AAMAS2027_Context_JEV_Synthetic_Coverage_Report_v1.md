# Context-conditioned JEV synthetic coverage v1

日期：2026-09-25

## 结果

4-cell `ContextHTState` 在固定 potential gain 下的覆盖检查通过：500 个 seed、每个 seed 1000 个 episode、4000 个有足够 observed support 的 cell-provider 检查，coverage 为 `1.0`。

但在 `call_cost=0.1`、探索概率 `0.8`、propensity floor `0.4` 下，平均 gate open count 为 `0`。平均 STOP 为 `200.218`，平均 Morgan/MPNN action 分别为 `399.816/399.966`。这说明当前 deterministic overlap Freedman 半径保持了保守覆盖，但在这个样本量和置信预算下过宽，无法产生有效的利用门。

## 过程中的错误配置

第一版实验把每个 episode 的序号直接当成 look index，alpha spending 很快变得过小，门同样永远关闭。该日志保留在 `experiments/logs/context_jev_synthetic_coverage_v1_20260925/`，作为 look 生命周期错误的审计记录。

第二版按现有 M2 语义改为每 64 个 episode 一个 look，日志在 `experiments/logs/context_jev_synthetic_coverage_v2_20260925/`。代码随后固定为 v2 配置并重跑，正式日志为 `experiments/logs/context_jev_synthetic_coverage_v3_20260925/`。

## 判断

当前 context 分层状态可以通过统计合同测试，但还不能作为有效的实时门。继续接入 BBB 回放之前，需要设计有严格覆盖依据的更紧 confidence sequence，或预先接受更长 calibration horizon；不能通过调大门概率或修改 held-out 成本来掩盖这个问题。

## 产物

- 规格：[AAMAS2027_ContextConditioned_JEV_MVP_Spec_v1.md](AAMAS2027_ContextConditioned_JEV_MVP_Spec_v1.md)
- 代码：[context_dynamic_jev.py](../src/probe_headless/context_dynamic_jev.py)
- 合同测试：[test_context_dynamic_jev.py](../src/probe_headless/tests/test_context_dynamic_jev.py)
- 正式 synthetic 日志：[summary.json](../experiments/logs/context_jev_synthetic_coverage_v3_20260925/summary.json)

