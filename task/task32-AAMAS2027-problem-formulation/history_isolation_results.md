# Task32 r2 严格隔离诊断结果

源数据：`data/tool_results/divergence/t32_history_isolation_r2_serial.json`。本表为嵌套训练区 pooled diagnostic，不是正式 test 主表，也不是 LLMRouterBench 的 dataset-macro 主指标。

## 修复后的 H1 具体构造

| Benchmark | B | absolute-shared IOU | relative-shared IOU | absolute AP | relative AP |
|---|---:|---:|---:|---:|---:|
| LLMRouterBench | 4 | -0.032009 | -0.030750 | 0.081214 | 0.070451 |
| LLMRouterBench | 8 | -0.023195 | -0.022743 | 0.081968 | 0.073316 |
| LLMRouterBench | 16 | -0.015924 | -0.017602 | 0.081268 | 0.072810 |
| LLMRouterBench | 32 | -0.009430 | -0.010683 | 0.082557 | 0.074329 |
| ASlib SAT12-ALL | 4 | -0.015495 | -0.013270 | 0.213847 | 0.214055 |
| ASlib SAT12-ALL | 8 | -0.003681 | -0.004247 | 0.249616 | 0.241224 |
| ASlib SAT12-ALL | 16 | -0.003651 | -0.004134 | 0.263722 | 0.261789 |
| ASlib SAT12-ALL | 32 | -0.001844 | -0.001933 | 0.289083 | 0.284384 |
| BBB | 4 | -0.023172 | -0.023172 | 0.029667 | 0.030967 |
| BBB | 8 | -0.012079 | -0.011614 | 0.030713 | 0.031742 |
| BBB | 16 | -0.001242 | -0.005714 | 0.042874 | 0.037205 |
| BBB | 32 | -0.000289 | -0.002020 | 0.047389 | 0.043629 |

结论：relative-shared 在 B=32 的三套环境均弱于 absolute-shared；小预算并非每一格都更差。仅能说该 rank-8 线性构造没有稳定优势，不能据此否定整个 residual-transfer 假设。IOU 不包含前 B 个 probe 的损失。

## H2 排序诊断（不是 passive online 策略）

| Benchmark | top-B | beneficial precision | precision lift | mean probe IOU | harmful rate |
|---|---:|---:|---:|---:|---:|
| LLMRouterBench | 4 | 0.025000 | 0.346345 | -0.275000 | 0.300000 |
| LLMRouterBench | 8 | 0.043750 | 0.771432 | -0.350000 | 0.393750 |
| LLMRouterBench | 16 | 0.059375 | 1.065101 | -0.325000 | 0.384375 |
| LLMRouterBench | 32 | 0.060937 | 1.001300 | -0.339062 | 0.400000 |
| ASlib SAT12-ALL | 4 | 0.370968 | 3.747128 | -0.230930 | 0.604839 |
| ASlib SAT12-ALL | 8 | 0.250000 | 2.408366 | -0.327763 | 0.717742 |
| ASlib SAT12-ALL | 16 | 0.209677 | 2.004091 | -0.372504 | 0.739919 |
| ASlib SAT12-ALL | 32 | 0.174395 | 1.638126 | -0.394690 | 0.742944 |
| BBB | 4 | 0.071429 | 1.039683 | 0.071429 | 0.000000 |
| BBB | 8 | 0.053571 | 0.779762 | 0.053571 | 0.000000 |
| BBB | 16 | 0.026786 | 0.389881 | -0.080357 | 0.107143 |
| BBB | 32 | 0.044643 | 1.445809 | -0.084821 | 0.129464 |

ASlib 的 positive precision lift 并不等于正价值；BBB top-4/top-8 的正值必须保留，不得选择性省略。BBB 是单个 131-molecule inner stream、7 个相关 challenger episodes，不作显著性或普适性结论。

## 隔离与复现

| Benchmark | 改变的 legacy residual cells | challenger 隔离审计数 | few-shot rows |
|---|---:|---:|---:|
| LLMRouterBench | 15791 | 20 | 1600 |
| ASlib SAT12-ALL | 37372 | 31 | 2480 |
| BBB | 228 | 7 | 560 |

两个独立进程 SHA256 相同：`76dec8b55cd2de59309797f03f6886f95d49005cfe154a52536dd002d3e792b4`。8 条定向测试通过；58 个 held-out / evaluation-label poisoning 审计通过。

未修改路径的 decision 指标最大差 `3.469446951953614e-18`；旧版与新版 MSE 最大差 `5.927195040333899e-09`，不冒充逐字节相同。四个 r1 原文件 hash 全部不变。

初次并行版复现检查失败已保留：`t32_history_isolation_r2_first_verification.json`。串行版只将诊断中的 ExtraTrees n_jobs 固定为 1；Task31 正式配置、代码和结果未修改。
