# 33 个具体 provider 的表示诊断 v1

2026-09-26，开发实验卡，结果尚未读取。执行 ADR-0021/0022；不使用最终确认集。

## 唯一主问题

在现有 BBB development 上，共享非线性分子表示能否比线性正确性头和训练区选出的固定 provider 更好地选择**具体工具**？这是选择 backbone 和更新方法之前的可学习性诊断。

旧 0.8947 来自七家族投票和选择前可见缺失掩码，不能回答这个问题。本次使用 33 个具体 provider，同一完整菜单，选择前不看当题输出/失败。工具失败 system success=0，不删分母。

## 数学和比较对象

$x$ 是 Morgan radius=2、1024-bit 指纹（原始 0/1）。$R_a$ 是该分子调用 provider $a$ 后的 system success。离线训练区向所有对照开放同一 full panel，评价区所有输出留在 evaluator。

$$
L_{\mathrm{BCE}}=\frac{1}{NK}\sum_{i,a}\operatorname{BCEWithLogits}(s_a(x_i),R_{i,a}).
$$

实例：一个训练分子有 33 个具体工具输出；正确返回记 1，错误返回和实际失败记 0。对这 33 格平均损失是训练目标，但统计样本仍是一个分子。线性头 $s=Wx+b$；MLP 头 $s=W_2\operatorname{ReLU}(W_1x+b_1)+b_2$，隐藏维度 64。

BayesianRouter 离线核心的适配使用相同 64 维共享表示 $h=\operatorname{ReLU}(W_1x+b_1)$，独立 BT / CLS provider 向量、无 head bias：

$$
L=L_{\mathrm{BT}}+0.2L_{\mathrm{CLS}},\qquad
L_{\mathrm{BT}}=\frac1{|D|}\sum_{(i,a,b)\in D}\operatorname{softplus}(s_b(x_i)-s_a(x_i)),
\quad D=\{(i,a,b):R_{i,a}=1,R_{i,b}=0\}.
$$

实例：CNN 正确、GraphB3 错误，形成 CNN 胜 GraphB3 的 pair；两者都正确或都错误不构造胜负。没有 disagreement 的整个训练区拒绝 BT 运行，不能静默报告完整 BT 方法。CLS 用所有训练格，BT 对整个训练区全部 disagreement pairs 平均。

最近邻是 BayesianRouter 离线双目标（式 2–3）和普通线性/MLP correctness head。这里只复用其学习目标；SMILES 指纹替换原 preference-pair 编码，没有 SmolLM2 全参数训练，不称原论文 full reproduction。

对照：train-only Best Fixed、线性 BCE、64维 MLP BCE、64维 BT+0.2CLS、同训练真值的直接性质 MLP。Oracle 仅 evaluator 上界。直接性质模型不调用工具，因此单独作为标签访问诊断，不列为合法路由器。所有训练方法读取完全相同的训练分子和标签权限。

## 冻结数据与预算

- 数据：722 个已登记参考标签的 development 分子，33 provider、round 1。8 个 unresolved 排除；6 个 B3clf 失败分子保留。
- 特征仅来自 canonical SMILES；保存输入 JSON 和 SHA-256。Bemis–Murcko 不含手性 scaffold；空 scaffold 统一为 ACYCLIC，避免把同类无环分子伪装为独立骨架。
- 外层 5-fold GroupKFold；每折的训练候选再以 GroupShuffleSplit(seed=20260926, validation=20%) 得到 fit/validation。所有三组 scaffold 严格分离；固定 split ID 保存，不能按效果重新分。
- 固定初始化 seeds 0、1，epochs=200，AdamW lr=0.001、weight_decay=0.01，float32、full-batch；不扫参。按 validation BCE 或对应 BT+0.2CLS 最小值选 checkpoint，同分取早 epoch。不用 outer evaluation 选 epoch。
- Best Fixed 仅看 fit 区的 system success，平分按菜单顺序选；所有其它方法可用 validation 选 epoch，但不回流 validation 梯度。
- 一条 Nebula A800、1 worker/1 GPU，训练核心最长 10 分钟、入口含依赖最长 15 分钟。无需 OSS 或外部权重；输入通过已推送 git，结构化逐 epoch/逐样本日志输出到平台 stdout 并取回本地。
- 每个保留模型的真实 state_dict 以压缩分块形式写 stdout，带字节数和 SHA；保存本地后校验。不能只保留汇总数。无外部科学工具/LLM/API调用；训练使用真实缓存标签。

## 检验和结果处置

主指标：每个 seed 的五折外层 OOF system-success accuracy；train-only fixed 的配对差和逐折差。记录 33 个得分、选择、实际结果、错误/失败、oracle、直接预测，以及训练/验证 loss、最佳 epoch、峰值显存、训练耗时。seed 0/1 描述初始化波动，不当作两个独立数据集。

开发继续信号：MLP 或 BT+CLS 相比 fixed 至少 +0.02，且两个 seed 各至少 4/5 折方向为正；这是投入下一阶段的启发式条件，不是统计显著性或新机制成立标准。不满足时如实报告：当前非线性指纹方案没有通过此诊断；不能据此排除 Laya 或改阈值搜正结果。

如果线性头也达到同样表现，不能归因为非线性表示；如果 direct predictor 已解释优势，需要重新检验选工具相对于直接预测的价值。只有静态上限信号仍不证明在线可学；下一轮才以 selected-only 的完整近邻/SGD 对照检验更新。

失败停止：输入哈希/资格不符、scaffold 跨区、非 A800、NaN/Inf、训练意外读到外层数据、超时或日志/权重无法恢复。保留失败；不在该任务内扫参、换分母或重启训练。

本轮不做确认推断，置信区间留在单独分析卡：在线算法尚未运行，不把 OOF 行或 2 个 seed 当正式独立检验。主文效果表仍不回填本开发数字。
