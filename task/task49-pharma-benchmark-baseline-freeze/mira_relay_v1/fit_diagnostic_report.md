# MIRA-Relay fit-only 初始化与可行动性诊断

日期：2026-10-03
状态：**保留失败证据；fit 优化未收敛，因此本诊断不能进入 full replay。**

## 数据和配置来源

本次只运行 fold 0 / seed 0：

- frozen 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256
  `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；fit 444 行、
  validation 133 行、evaluation 145 行；特征来自已恢复的 64 维 `bt_cls` encoder。
- encoder：`fold0_seed0_bt_cls.pt`，SHA-256
  `971084523d8bee30019b16a895426df589a5179614971a7a93bc63f364bfc82b`。
- provider family registry：`task/task38-p01-provider-pool/external_qualification.json`，
  SHA-256 `9738363d3ad64d0e52ceffb71acfe7b4f3db5bea9ba007a475876f7e0f2ff365`，33 个 provider。
- direct head：复用 `fit_fold_priors` 的 seed 0、200-epoch AdamW；该过程使用 validation
  loss 选 direct head。MIRA residual 只使用 444 个 fit cell 行。

执行环境是 macOS CPU，Python 3.9.6、NumPy 1.26.4，未使用 GPU，未读取 confirmation。

## 诊断内容

1. 用 fit-only `system_success` 矩阵拟合 family residual 和 provider bias；direct head 作为
   冻结 Bernoulli logit offset。
2. 保存 summed NLL、L2、优化状态、梯度范数、precision/variance 以及输入和源码 hash。
3. 对 145 个 evaluation feature 行只计算候选分数和可行动作；`load_inputs` 仅做冻结输入
   schema/hash 校验，policy/action 计算不接收 evaluation truth、output label 或 status，
   这些字段也不写入 action receipt。
4. 用一条 fit-derived feature，分别注入合法的 `y=0` 和 `y=1` selected release，检查
   score/ranking 是否改变。这是预登记的数学合同 fixture，不是效果实验。

## 结果

| 检查 | 结果 |
|---|---:|
| 初始 summed NLL + L2 | 5363.2964408635 |
| 最终目标值 | 3444.2792905806 |
| L-BFGS 迭代 | 100（达到上限） |
| 最终梯度无穷范数 | 12.5075153750 |
| SciPy 状态 | `STOP: TOTAL NO. of ITERATIONS REACHED LIMIT` |
| evaluation 非 fallback exploitation | 116 / 145（80.0%，仅可行动性诊断） |
| y=0/y=1 state→action sensitivity | passed |

核心结论是 **fit 没有收敛**。因此 116/145 不能作为 MIRA-Relay 的可用策略结果，
也不能据此开启 full replay 或宣称方法有效。它只说明当前未收敛参数在这组 feature 上
确实能产生非 fallback 分数，以及 online 合同 fixture 能改变分数/排序。

## 失败原因与下一步接口

当前固定合同要求 `max_iter=100`、`ftol=1e-12`、`gtol=1e-9`，诊断没有调常数追分。失败
更像是 33 provider、64 维 family residual + bias 的 L-BFGS 在这个 summed objective 上
达到迭代上限；没有证据表明数据或 family registry 本身损坏。这个判断需要独立复核，不能
把“目标值下降”当作收敛。

后续同一目标的数值上限诊断见 `optimizer_diagnostic_report.md`：解析 Hessian 与有限差分
检查支持目标/梯度实现一致，说明当前失败主要与条件数和 100 次预算有关。该诊断不改变本
报告的 `fit_not_converged` 状态，也没有把替代优化器静默带入 replay。

进入 full replay 前必须先解决并重新封存：

1. 明确是否允许在不改变预登记数学目标的前提下增加优化器数值稳定实现（例如解析等价的
   block/coordinate 求解或等价的收敛检查）；若要改 `max_iter`/归一化，需先更新决议和
   preregistration，不能静默修改。
2. 为 fit receipt 增加独立 verifier：从 fit labels、direct offset、family map 重算目标、
   gradient、参数 hash 和 diagonal Fisher precision。
3. 收敛后再复跑 state→action sensitivity、145 行 label-free switchability，并仅在这些
   CPU gate 通过后实现 NoProbe/ProbeNoUpdate/IndependentResidual/MIRA-Relay replay。

## 原始证据

- 输出目录：`experiments/logs/task49_mira_relay_fit_diagnostic_20261003/v1/`（初次收据）和
  `v2/`（加入 fit/evaluation feature 与 fit-label matrix hash 的当前收据）
- `config.json`：输入、配置、源码、checkpoint 以及矩阵 hash
- `fit_receipt.json`：优化结果、参数、precision/variance
- `evaluation_action_records.jsonl`：145 行不含 truth 的 action 诊断
- `summary.json`：`status=fit_not_converged`
- `test.log`、`test_config.json`：14 条单测、py_compile 和失败状态记录

本批没有运行 GPU，也没有把 evaluation truth 送入 policy/action 计算、没有运行 full matrix，
也没有生成 superiority 结论。
