# 医药评价与反馈协议审查（Task42，2026-09-23）

本轮不是重新选择benchmark，而是使Task40的C1小批真正测到“条件选择是否改善任务成功”，不混淆失败分母、未来标签、学习时钟或上游模型曝光。具体推荐及未批准条件见 `../task/task42-p02-evaluation-contract/protocol_proposal.md`。

## 一手核读及用途

| 来源 | 本轮实际核读 | 对当前协议的约束，不是额外创新 |
|---|---|---|
| DeepChem官方Splitters说明 [S1] | 训练/验证/测试职责，分组保存与分组大小不等的边界 | 变换在训练区拟合、确认集最后使用；分组只是结构规则，不证明33个外部预测器没见过样本 |
| MoleculeNet与DeepChem BBBP说明 [S2] | 分子性质benchmark、BBBP任务与推荐骨架划分 | 继续采用已确定的分子任务视角；不因推荐scaffold就宣称特定权重外部资格合格 |
| Li等，WSDM2011 [S3] | 作者摘要、发表信息，部分反馈历史上的replay评价目标 | 本仓完整缓存矩阵逐动作回放不是随机日志拒绝采样replay；不能借其无偏定理担保本仓外部泛化 |
| PAK/RFF-UCB，ICML2025 [S4] | 官方发表页、同context多模型选择与核/RFF更新 | 上下文模型选择已有现代正式baseline，不是本项目自己命名的方法 |
| KMTL，NeurIPS2017 [S5] | 官方发表页，与Task40/41原文方程审查交叉核对 | 共享估计用具名参考，固定核并不等于候选新机制；当前参数/反馈适配需明确 |

[S1] https://deepchem.readthedocs.io/en/latest/api_reference/splitters.html
[S2] https://arxiv.org/abs/1703.00564 ; https://deepchem.readthedocs.io/en/latest/api_reference/moleculenet.html
[S3] https://arxiv.org/abs/1003.5956 （作者页说明修订自WSDM2011，不把2010预印本日期当会议年）
[S4] https://proceedings.mlr.press/v267/hu25m.html
[S5] https://proceedings.neurips.cc/paper_files/paper/2017/hash/b06f50d1f89bd8b2a0fb771c1a69c2b0-Abstract.html

本轮广泛关键字检索曾返回无关社交结果，这些未作为证据。随后按已核精确URL核读上述官方入口；没有宣称完成系统综述，也没有重新核验所有原论文实验。库网页可能更新，正式实现仍须锁实际安装版本。

补核：LinUCB出处为Li、Chu、Langford、Schapire，*A Contextual-Bandit Approach to Personalized News Article Recommendation*，WWW2010，作者页 https://arxiv.org/abs/1003.0146 。与[S3]不同论文，不混淆两组作者或把replay论文当LinUCB原创出处。

[S1] ScaffoldSplitter说明明确遇无效SMILES会跳过该数据点，本项目主分母合同不能直接继承这个静默行为。拟运行器需要先验证全体输入身份及集合覆盖；本轮仅实现调用方身份清单检查，不宣称已完成实际化学规范化和正式划分。

本轮方法学结论：只报告有效返回准确率可能逆转全任务选择排名；后续训练所优化的目标与系统计分必须一致。冻结策略的分组重采样与在线自适应轨迹的总体推断不同，纯逐行bootstrap不能替代后者。上述为本项目的概率/协议推导，人工数值检查不作为新理论贡献或医药效果。
