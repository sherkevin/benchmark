# Task34 严格历史隔离后的校准诊断

本表仅报告固定 B / alpha 的嵌套训练区诊断；不是已发表 conformal 方法的忠实复现，不是正式 test 主表。
IOU 包含前 B 个 challenger probes 的损失；post-gate IOU 单独列出，不能替代总 IOU。

数据：`data/tool_results/divergence/t34_meta_calibrated_gate_trainonly_isolated.json`。

| Benchmark | B | 方法 | 总 IOU | probe IOU | post-gate IOU | gate rate |
|---|---:|---|---:|---:|---:|---:|
| llmrouterbench | 4 | uncalibrated | -0.0342679901 | -0.000306038048 | -0.033961952 | 0.256298594 |
| llmrouterbench | 4 | meta_alpha_0.2 | -0.000306038048 | -0.000306038048 | 0 | 0 |
| llmrouterbench | 4 | meta_alpha_0.1 | -0.000306038048 | -0.000306038048 | 0 | 0 |
| llmrouterbench | 4 | meta_alpha_0.05 | -0.000306038048 | -0.000306038048 | 0 | 0 |
| llmrouterbench | 8 | uncalibrated | -0.0310049628 | -0.000620347395 | -0.0303846154 | 0.234193548 |
| llmrouterbench | 8 | meta_alpha_0.2 | -0.000620347395 | -0.000620347395 | 0 | 0 |
| llmrouterbench | 8 | meta_alpha_0.1 | -0.000620347395 | -0.000620347395 | 0 | 0 |
| llmrouterbench | 8 | meta_alpha_0.05 | -0.000620347395 | -0.000620347395 | 0 | 0 |
| llmrouterbench | 16 | uncalibrated | -0.0159925558 | -0.00148883375 | -0.0145037221 | 0.197961125 |
| llmrouterbench | 16 | meta_alpha_0.2 | -0.00148883375 | -0.00148883375 | 0 | 0 |
| llmrouterbench | 16 | meta_alpha_0.1 | -0.00148883375 | -0.00148883375 | 0 | 0 |
| llmrouterbench | 16 | meta_alpha_0.05 | -0.00148883375 | -0.00148883375 | 0 | 0 |
| llmrouterbench | 32 | uncalibrated | -0.0178577337 | -0.00262200165 | -0.015235732 | 0.186770058 |
| llmrouterbench | 32 | meta_alpha_0.2 | -0.00262200165 | -0.00262200165 | 0 | 0 |
| llmrouterbench | 32 | meta_alpha_0.1 | -0.00262200165 | -0.00262200165 | 0 | 0 |
| llmrouterbench | 32 | meta_alpha_0.05 | -0.00262200165 | -0.00262200165 | 0 | 0 |
| aslib_sat12all | 4 | uncalibrated | -0.0233417179 | -0.0102442173 | -0.0130975005 | 0.0356829948 |
| aslib_sat12all | 4 | meta_alpha_0.2 | -0.0102442173 | -0.0102442173 | 0 | 0 |
| aslib_sat12all | 4 | meta_alpha_0.1 | -0.0102442173 | -0.0102442173 | 0 | 0 |
| aslib_sat12all | 4 | meta_alpha_0.05 | -0.0102442173 | -0.0102442173 | 0 | 0 |
| aslib_sat12all | 8 | uncalibrated | -0.023154333 | -0.0216075725 | -0.00154676049 | 0.00744723218 |
| aslib_sat12all | 8 | meta_alpha_0.2 | -0.0216075725 | -0.0216075725 | 0 | 0 |
| aslib_sat12all | 8 | meta_alpha_0.1 | -0.0216075725 | -0.0216075725 | 0 | 0 |
| aslib_sat12all | 8 | meta_alpha_0.05 | -0.0216075725 | -0.0216075725 | 0 | 0 |
| aslib_sat12all | 16 | uncalibrated | -0.0443721446 | -0.0435971738 | -0.000774970729 | 0.00836320191 |
| aslib_sat12all | 16 | meta_alpha_0.2 | -0.043597204 | -0.0435971738 | -3.02004513e-08 | 0.000119474313 |
| aslib_sat12all | 16 | meta_alpha_0.1 | -0.0435971358 | -0.0435971738 | 3.80326563e-08 | 3.9824771e-05 |
| aslib_sat12all | 16 | meta_alpha_0.05 | -0.0435971358 | -0.0435971738 | 3.80326563e-08 | 3.9824771e-05 |
| aslib_sat12all | 32 | uncalibrated | -0.0880958426 | -0.0862422671 | -0.00185357543 | 0.0126244524 |
| aslib_sat12all | 32 | meta_alpha_0.2 | -0.0863225771 | -0.0862422671 | -8.03099695e-05 | 0.000477897252 |
| aslib_sat12all | 32 | meta_alpha_0.1 | -0.0862819307 | -0.0862422671 | -3.96636134e-05 | 0.000238948626 |
| aslib_sat12all | 32 | meta_alpha_0.05 | -0.0862422307 | -0.0862422671 | 3.64396655e-08 | 3.9824771e-05 |
| bbb | 4 | uncalibrated | -0.0209378408 | -0.00130861505 | -0.0196292257 | 0.553980371 |
| bbb | 4 | meta_alpha_0.2 | -0.00130861505 | -0.00130861505 | 0 | 0 |
| bbb | 4 | meta_alpha_0.1 | -0.00130861505 | -0.00130861505 | 0 | 0 |
| bbb | 4 | meta_alpha_0.05 | -0.00130861505 | -0.00130861505 | 0 | 0 |
| bbb | 8 | uncalibrated | -0.0174482007 | -0.00152671756 | -0.0159214831 | 0.502071974 |
| bbb | 8 | meta_alpha_0.2 | -0.00152671756 | -0.00152671756 | 0 | 0.00174482007 |
| bbb | 8 | meta_alpha_0.1 | -0.00152671756 | -0.00152671756 | 0 | 0.000872410033 |
| bbb | 8 | meta_alpha_0.05 | -0.00152671756 | -0.00152671756 | 0 | 0 |
| bbb | 16 | uncalibrated | -0.0152671756 | -0.00392584515 | -0.0113413304 | 0.322355507 |
| bbb | 16 | meta_alpha_0.2 | -0.0052344602 | -0.00392584515 | -0.00130861505 | 0.0320610687 |
| bbb | 16 | meta_alpha_0.1 | -0.00414394766 | -0.00392584515 | -0.000218102508 | 0.0213740458 |
| bbb | 16 | meta_alpha_0.05 | -0.00392584515 | -0.00392584515 | 0 | 0 |
| bbb | 32 | uncalibrated | -0.0178844057 | -0.0080697928 | -0.00981461287 | 0.280916031 |
| bbb | 32 | meta_alpha_0.2 | -0.0104689204 | -0.0080697928 | -0.00239912759 | 0.0730643402 |
| bbb | 32 | meta_alpha_0.1 | -0.00894220284 | -0.0080697928 | -0.000872410033 | 0.0200654308 |
| bbb | 32 | meta_alpha_0.05 | -0.0080697928 | -0.0080697928 | 0 | 0 |

## 结论与限制

固定的 4 个 B × 3 个 alpha 在三套环境均未给出正的平均总 IOU。LLM 的所有 meta-calibrated 设置在 probe 后 gate rate=0；其较小损失来自回退，而不是发现可利用区域。
ASlib 在 B=16、alpha=0.10/0.05 的 post-gate IOU 为 +3.803265631e-08；B=32、alpha=0.05 为 +3.643966547e-08。这些正数必须保留，不能四舍五入为零后声称全部非正；但对应总 IOU 仍负。BBB 的 meta-calibrated 平均 post-gate IOU 非正。不能把近零覆盖的保守包装成识别成功。
这只约束当前 pooled auxiliary error quantile + Ridge1 构造；不排除所有 meta-calibration / conformal / safe-learning 方法。
历史 incumbent 的 in-sample 拟合误差和目标 stream 的 out-of-sample 误差不是同一分布；没有证明 task exchangeability、选择后风险或逐 context guarantee。
所有 challenger episodes 共享 context/splits，不独立；本轮没有声称统计显著性。

## 验证

独立进程两次执行逐字节一致，SHA256：`a59bfdc4ad1841601a96ca29164af6602dc1f17029cc2ef17c9d9e4c87c63fab`。
58 个真实 challenger 的历史及 evaluation-label poisoning 检验通过；4640 条方法/预算/seed 结果均满足 total=probe+post-gate。
原脚本与原结果 hash 保持不变，见 `legacy_isolation_snapshot.json` 与 `data/tool_results/divergence/t34_isolated_calibration_verification.json`。
Task32/34 合计 15 条定向回归通过，记录在 `isolation_regression_tests.log`；不是全仓测试通过声明。

## 复跑命令

```bash
cd /Users/jingwu/work/benchmark
OPENBLAS_NUM_THREADS=1 OMP_NUM_THREADS=1 VECLIB_MAXIMUM_THREADS=1 python3 src/probe_headless/t34_meta_calibrated_gate_isolated.py
python3 src/probe_headless/t34_verify_isolated_calibration.py
python3 -m pytest -q src/probe_headless/tests/test_t32_history_isolation.py src/probe_headless/tests/test_t34_isolated_calibration.py
python3 task/task34-T3-modern-safe-calibration-closure/build_isolated_report.py
```

重复进程使用同一 module，仅将 `OUT` 指向 `_isolated_repeat.json`；所有参数、源代码与环境保持不变。正式冻结脚本没有改动。
