# Conditional Fusion Intervention v1

状态：**候选实验卡，未冻结执行，未运行。**

日期：2026-10-08。来源是 `conditional_second_call_v2/fusion_diagnostic_report_v2.md` 的分支 2。该卡只处理终局融合问题；不改变 selector、第二工具 acquisition、gain learner、反馈时钟或调用预算。

## 1. 唯一问题和主假设

在同一已取得的第二工具输出流上，当前 masked-logistic verifier 是否因为没有一个共同的 provider-output 分布而把 pair evidence 融合错了？

主假设 H1：在相同首动作、相同已取得 pair、相同损失和相同动作阈值下，**Coherent Gaussian Evidence Fusion (CGEF)** 的终局真实 utility 高于原 masked-logistic verifier 和 `ignore-second`。

H1 只针对固定开发流的终局融合。它不等价于“第二工具普遍有价值”、不等价于在线选择收益，也不证明高斯分布正确。

## 2. 不改变的东西

- 四个 provider、native probability、provider 版本、真值和分子顺序；
- B fold、fit/validation/evaluation 划分、首工具和已付费 pair 流；
- 首工具的原始动作、gain learner、第二工具候选、token budget、propensity 和 release clock；
- 三个终局动作：答 0、答 1、拒答；拒答损失 $\rho=0.25$，追加成本 $\kappa=0.01$；
- 所有输入权限。CGEF 训练只读 A fit 分区的四个 native 输出和 truth；B 只用于冻结后的评价，不回流拟合或调参。

## 3. CGEF 定义

把 provider 输出变成固定 logit：$u_a=\log(p_a/(1-p_a))$，其中 $p_a$ 是该 provider 在当前分子上的 native probability。对训练分区内的每个 provider，保留完整四维向量 $u\in\mathbb R^4$。

对 $Y\in\{0,1\}$，用共享协方差的类条件高斯模型：

$$u\mid Y=y\sim\mathcal N(\mu_y,\Sigma),\qquad \log\frac{P(Y=1)}{P(Y=0)}=\alpha.$$

$\Sigma$ 是对称正定的固定收缩协方差：先按 fit 分区计算 pooled covariance，再加预登记的 $\lambda_\Sigma I$，其中 $\lambda_\Sigma=0.10$，不看 B 选择它。对任意实际观察到的 provider 子集 $M$，CGEF 只取对应的子向量和子矩阵：

$$q_M=\sigma\left(\alpha+\log\frac{\mathcal N(u_M;\mu_{1,M},\Sigma_{MM})}{\mathcal N(u_M;\mu_{0,M},\Sigma_{MM})}\right).$$

这给每个 mask 一个同源的 posterior。共享协方差保留 provider 误差相关性；不同 mask 使用同一个 joint distribution 的 marginal，而不是为每个 mask 单独拟合系数。四 provider 的 concrete case 是：$M=\{\texttt{ecfp\_logreg},\texttt{graph\_message\_passing}\}$ 时，只读取这两个已返回的 logits 和 $2\times2$ 的 $Σ_{MM}$。

训练细节在运行前固定：类别 prior 用 fit 分区的正例率，$\alpha$ 为其 logit；均值按类计算；协方差为 pooled covariance 加 $0.10I$；不做 validation 搜索，不读 B/C，不做 provider independence assumption。

## 4. 匹配评价

### A. 固定 acquisition 的主要评价

在每个 B fold 中，重放真实已取得的三候选 pair。首动作沿用原 verifier 的 `single_action`，保证只改 pair 终局融合。对每个 pair，分别计算：

1. `ignore-second`：沿用首动作；
2. 原 masked-logistic：使用已保存的 pair action；
3. CGEF：用同一 pair 的 $q_M$ 和相同 $0.25/0.75$ 阈值决定动作。

主指标是每题终局 utility，包含真实错误、拒答和已付追加成本；同时报告 correctness、refusal、calls（固定）、rescued/harmed pairs、Brier 和 calibration。统计单位是 molecule within fold，pair 行不作为独立样本。

主要对照是 CGEF−masked-logistic 和 CGEF−ignore-second 的 paired difference。最低开发线沿用现有卡的 $+0.01$ utility/题；不能因结果不利而降低。若两项均未过，CGEF 停止，不进入闭环。

### B. 固定流控制

- 单 provider singleton：CGEF 与原 verifier 应按同一可见 evidence 合同比较；
- $D=0$ 与 $D\in\{1,4\}$ 只影响后续更新，不应改变这一固定 pair 融合比较；
- 将 B fold 中 `K=(0,3,拒答)` 单独列出，但不得只报告该分组；全 B 的 paired utility 是主结果。

## 5. 最强对照和公平性

| 方法 | 新读信息 | 终局规则 | 作用 |
|---|---|---|---|
| Ignore-second | 无 | 首动作 | 付费后不融合控制 |
| Masked-logistic | 已返回 native outputs | 当前冻结规则 | 当前方法对照 |
| CGEF | 已返回 native outputs | 固定 joint Gaussian marginal | 单因素候选 |
| Oracle evaluator | 真值/全面板 | 事后最优 | 仅上界，不排入部署排名 |

CGEF 与原 verifier 使用同一 A fit 分区、同一 provider logits、同一 B 评价流、同一动作阈值；CGEF 不能读取 selector 特征、未选输出或真值。

## 6. 失败条件和停止规则

- 任一 mask 的 $Σ_{MM}$ 非正定、输入概率非有限、或恢复输出与冻结 native cache 不一致：该 fold fail-closed；不能自动改 $λ$；
- independent verifier 不能从 A fit、CGEF 参数快照和 B raw event 重建所有 q/action/utility：实验无效；
- CGEF 与 masked-logistic 的主要 paired difference 未超过 $+0.01$ utility/题，或只改善 Brier、不改善 utility：停止 CGEF；
- 只在 `K=(0,3,拒答)` 改善而全 B 不改善：记为条件诊断，不升级整体方法；
- 若 CGEF 超过原规则，下一张卡才做相同 gain learner/预算下的闭环 acquisition；本卡结果本身不证明实时学习增益。

## 7. 运行预算和产物

CPU first，≤5 分钟、≤1 GiB RSS、≤100 MB 新结果。执行前保存 card/source/input hashes、fold/seed、$λ_\Sigma$、运行环境和命令。原始 event、CGEF 参数、逐样本 q/action/loss、paired summary、failure、独立恢复和资源收据都要保留。未通过前不得读 confirmation。

## 8. 论文位置

本卡结果只用于决定下一步方法。只有在独立确认、完整 matched baseline、实时更新和 agent 端到端闭环均通过后，才考虑把 CGEF 或其修订版本写入主方法。当前不修改三份生效主线文档或 8 页主版。
