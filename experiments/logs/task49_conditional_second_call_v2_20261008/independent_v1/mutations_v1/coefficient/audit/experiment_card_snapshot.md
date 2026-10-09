# 条件调用 v2：状态表达的2×2开发诊断

2026-10-08；执行前冻结。状态：候选修复实验，不替换生效方法，不读confirmation。

## 问题与原始证据

v1在722个开发分子上未胜出；条件value的162次追加中136次未改变终局动作。目标是检验一个具体解释：只使用首次概率的绝对置信度，并以线性函数拟合gain，是否丢失了必要的方向信息或无法表达拒答边界。不是通过扩大数据、重新选backbone或更改utility追胜。

最近邻是CR的输出条件质量/成本估计及其utility适配。v1最近邻报告已证明同一终局损失下决策公式与G等价。本轮只检验表示修复，不把新增特征、ridge或G称为新算法。

## 数学检查

固定首次验证器输出q，有符号概率s、绝对置信度c和拒答标志r均由当前合法证据计算：

$$s=2q-1,\qquad c=|s|,\qquad r=\mathbf 1[.25<q<.75].$$

Case：q=.1与q=.9的c均为.8，但s分别为−.8和+.8，准备回答的类别相反；q=.4的c=.2、r=1，当前会拒答。s恢复被绝对值删除的信息；r可由c<.5得到，**没有增加信息**，只是把已知的非线性决策边界提供给线性head。端点q=.25/.75按既有规则作答。

有一个不依赖BBB结果的反例：相同a、b，在两个等概率区域中q分别为.1/.9，且Y条件概率等于q。低q区的B总返回Y，高q区的B总返回1；固定追加终止规则使用B的二值结果。低q区原答0，预计错误.1，追加后错误0；高q区原答1，追加后动作不变。κ=.01时，条件净收益分别+.09与−.01。

Case：旧pair+c特征在两个区域完全相同，必须给相同value，无法表达相反的调用决定；加s后这两个值可由同一直线表达。该构造证明特征压缩可能造成不可辨识，不证明BBB中必然如此，也不是共享logistic验证器的效果实验。

## 冻结项与数据权限

- 复用v1的A/B/C scaffold划分、四provider、722开发分子、首次CorrectedFrozen、已拟合9参数终局验证器、原B paired targets与权重1/3。
- v1 full summary SHA `8edd5955077387be843bfb57d05b29c892a343d2f2a84fb9855aae6a2fd44542`；独立复算状态必须为verified，所有复用模型/原始输入hash先检查。
- v1模型不再拟合。仅在原B区拟合新增value系数。B实际每fold86–133分子；C仍是已查看的development，不能写成未读确认。
- 保留λ=1、全部系数等权正则、四工具固定排序、g>0才请求追加、未支持pair禁止追加、相同token控制器及订单。每个前缀额外调用≤floor(t/4)，最多两次调用。
- 读C全表仅限evaluator，policy只能读取已调用返回、首次selector分数和冻结状态。无provider forward、API、GPU或157确认数据读取。

## 2×2实验矩阵

全部有12个ordered-pair截距和4个candidate-specific c斜率。只改变两个因素：

| 条件 | 增加s | 增加r | 参数数 |
|---|---|---|---:|
| base_confidence | 否 | 否 | 16 |
| signed_only | 是 | 否 | 20 |
| refusal_only | 否 | 是 | 20 |
| signed_refusal | 是 | 是 | 24 |

每个新增量乘candidate one-hot形成4列。Case：候选b=1时只在其s/r对应列填写当前s/r，另外三个候选的列为0。不得使用B本题的实际输出来产生决策特征。

另加一个预先固定的24参数容量对照 **magnitude_polynomial**：旧16列，加candidate-specific c²/c³各4列。它只能看到c，不能恢复符号；用于检查更丰富的置信度函数是否已解释收益。它也不是正则几何完全等价的控制。

为避免重复列等价于放松ridge惩罚，所有新增列均先在B上投影掉旧16列的线性部分，变换不使用gain标签：

$$T=X_0^+Z,\qquad Z_\perp=Z-X_0T.$$

Case：X₀是B的旧16列，Z是s/r或c²/c³列，伪逆固定rcond=1e−12；B每行权重都为1/3，因此与普通投影相同。推断时用本题合法x₀、z计算z−x₀T，T随模型保存。若某candidate在B中全为q>.5，s=c，新增s残差为0，不能通过复制c降低其惩罚取得训练优势。全24条件的两组新增列分别对同一X₀投影，不再互相投影。

该变换是诊断归因的标准线性代数处理，不是创新主张。记录每组B新增列投影前/后rank、范数和各pair×终止动作支持，不根据这些统计选择模型或改λ。每个新模型的原16系数应与v1保持数值一致（正交性误差容限1e−8）；如不成立先查实现。

固定对照还包括v1的pair_mean_gain、confidence_cascade、stop_only。base_confidence和三个固定对照在原订单上的全部动作、loss与汇总必须重现v1；新增模型使用同一A验证器和B标签。没有超参搜索或结果后新增条件。

## 指标与判据

- 主指标沿用U=−(wrong+.25refusal+.01calls)/N，并报告错误、拒答、调用、动作不变调用、净收益及实际pair选择。
- 主要修复对比：signed_refusal相对v1最强已实现控制（stop/cascade/pair_mean/base_confidence中最大utility）。只有差≥.01且paired95%CI下界>0，才能称本开发修复过线；它仍须强CR和Online/Replay。
- 归因对比：signed_refusal−refusal_only检验s的增量；signed_refusal−signed_only检验r的增量；signed_only−base与refusal_only−base展示单因素。各对比完整报告，区间跨0不称相应因素有效。
- signed_refusal还须与magnitude_polynomial比较，差的95%CI下界>0才支持超出该置信度容量控制的收益；未过不能把改善归因为新增状态信息。
- 另报告utility交互项 U(s+r)−U(s)−U(r)+U(base)。正点值不等于组合机制已证实；需CI和最近邻。
- 这些是嵌套basis对比，同时改变维数和正则几何；即便有效，只能支持此表示修复，不能严格排除所有“容量”解释，更不能把r解释为额外信息。
- 复用v1的10,000组fold内scaffold block抽样；每条重采样序列、每个policy重新执行token控制器。拟合模型不重训，区间不是训练不确定性。保存每次bootstrap值。
- 先fold0 smoke检查合法特征、原策略重现、模型方程和调用账；通过后完整5fold，无论smoke正负都运行并报告。出现合同错误修复后新建运行目录；保留失败记录。
- 全部新增条件若均不能越过最强控制，保留负结果，不搜索更多basis/阈值来凑胜；返回反馈与终止规则的机制审查。任何单项改善不得替代online/agent证据。
- C已用于提出本次修复；配对区间仅是开发诊断，不因重用draws就消除自适应选择偏差。通过开发线也不能当独立确认。

## 日志与预算

开始前写config（时间、argv、父commit、源文件/card/输入与复用结果hash、环境、所有设置），保存源码和card快照。保存B特征/目标、系数、逐决策合法receipt、evaluator结果、原策略逐行对账和bootstrap draws；独立脚本不得导入训练runner核对核心结果。

CPU每运行≤5min；没有新科学工具推理或API成本。旧v1代码、模型、日志与主版PDF全部保留。本轮对应方法M6/M13和实验B12/B13的明确疑问，不自动改变评分或打开gate。
