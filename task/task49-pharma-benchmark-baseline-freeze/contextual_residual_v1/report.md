# 按分子条件化残差：没有改善当前效果

日期：2026-10-07。状态：90条真实缓存闭环与独立数值重构完成，verified、0 issues；**未达到预冻结升级规则，不升级此构造**。这是否定当前固定版本，不是否定工具选择或在线学习目标。

## 目的与结果

上一轮可救错误大多已有正确候选反馈。本轮只检查：沿用现有Morgan64表示，把每工具统一修正扩展为按分子条件化的ridge修正，能否用好这些反馈？增加范数匹配的常数对照，防止把更新变强误认为context作用。三格的矩阵正则均为20，不搜索超参。

| cell | 特征 | 自然正确 / 2166 | 自然准确率 | 受控正确 / 2166 | 受控准确率 |
|---|---|---:|---:|---:|---:|
| Q20 | 常数`[1;0]`，精确原Q | 1740 | 80.33% | 1704 | 78.67% |
| Norm constant | 常数`[1;v0]`，有效先验10由范数解析导出 | 1734 | 80.06% | 1683 | 77.70% |
| Context | `[1;u(x)]`，现有单位Morgan64 | 1737 | 80.19% | 1690 | 78.02% |
| 对应Frozen（引用原同分数结果） | 无更新 | 1737 | 80.19% | 1371 | 63.30% |

Context相对Q20自然 **−0.139pp**、受控 **−0.646pp**；相对范数匹配常数分别+0.139/+0.323pp。它好于更强的常数更新，却没有超过原Q，自然与Frozen总正确数相同。因此不能说增加context残差改善了方法，也不能把较弱的Norm constant单独当胜利基线。

## 错误切换与稳定性

| 比较 | 自然rescued / harmed | 自然净正确 | 受控rescued / harmed | 受控净正确 |
|---|---:|---:|---:|---:|
| Context−Q20 | 19 / 22 | −3 | 43 / 57 | −14 |
| Context−Norm constant | 19 / 16 | +3 | 35 / 28 | +7 |
| Norm constant−Q20 | 16 / 22 | −6 | 42 / 63 | −21 |

Context与Q20在自然/受控流分别有509/447次选择不同，所以不是更新没有作用；改变选择之后，新增错误抵消了救回的错误。这是闭环真实结果，不是沿旧历史打分的shadow效果。

自然Context−Q20五折净正确差为0、−2、+1、−2、0；三个delay为−4、+4、−3。五个leave-one-fold-out差全部为负。Context−Norm constant自然有一个LOO为负；不挑D=1的正数或单折来升级。受控Context−Q20三个delay均负（−10、−1、−3）。完整分层见paired_comparisons。

预冻结五项效果规则中，仅“自然超过Norm constant”和“不低于Frozen”通过；≥1pp、两组LOO全正、受控不低于Q20均失败。保持旧方法、旧confirmation和原目标，停止这个固定构造的参数搜索。

## 实现正确，计算成本不是本轮瓶颈

- Q20的30条natural/stress轨迹精确还原上轮动作/标签/时钟，最大分数误差3.33e−16；Norm constant与标量Q10解析式误差2.22e−16。
- 独立checker不导入runner，从原缓存、每次调用时q/特征和selected release重建全部数值状态及哈希，再用流末直接ridge解核验；最大参数误差1.46e−16。90流、12,996次决策、39,168事件、配对/LOO/去留规则全部核对，0 issues。
- 实际闭环7.946秒，独立验证16.151秒；0新增provider推理、0离线训练、0API/GPU，**在线ridge训练真实执行12,996次**。
- Context自然流的决策/更新CPU操作p95为9.97/21.47微秒，受控为8.93/20.08微秒；不含模型编码、科学工具、反馈等待、状态编码或I/O，不是端到端服务延迟。
- Context最大数值状态139,880 bytes（含pending），最大完整承诺142,992 bytes；不是进程RSS。三格都用统一65维矩阵实现以核对嵌套关系，不能据此说原标量方法也需要同样内存。标量基线可用计数实现。

## 下一步

本轮没有支持“在同一Morgan64上增加线性context残差就能改善效果”。再调此更新器的正则或学习率缺少新的依据。下一步转向**调用前打分器和强学习型baseline**：核查当前四工具面板是否已有原生BT+CLS/MLP/Laya的合格训练；旧33-provider结果不能移用。优先复用已有训练实现，固定当前面板检验相对工具排序，同时补齐baseline资格和效果证据。

全部工具同对/同错的样本占575/722，独立正确性损失与工具相对排序的侧重点可能不同，这是下一步待检验的假设，不是本轮已证明的原因。新的学习目标或表示必须单独冻结控制；不同时更换benchmark、数据划分和在线更新器。

## 证据与复用

- [冻结卡](experiment_card.md)，SHA `409b8fc8871bc1906e6ea879bd8c60795d5f9bf14caa988ee0a278a7fb773c8b`；[数学审查](math_review.md)。实现前已落入提交`7e4756d2`。
- `src/probe_headless/run_contextual_residual_v1.py`，SHA `d372a5c2c849feeab97cc82d1ced4a26994705b9bc2bdad7eda6022e93df6627`；8项必要合成测试通过。首次测试与成本统计接线后的第二次测试均保留，没有失败科学run。
- `experiments/logs/task49_contextual_residual_20261007/v1/summary.json`，SHA `73f611c61f667ba5340691309c7aa0fe65ec3d1a6428964fd2f40d2abae51a12`；原始88.16MB事件保留本地，Git存11.86MB无损gzip，见日志README。
- `src/probe_headless/verify_contextual_residual_v1.py`，SHA `9c5738e88bd2ec78dec126e7fceedc5da869aece87c1e8819fead25c3df498c2`；复用的只是前一独立输入checker，不导入分析器或policy。
- 独立回执：`experiments/logs/task49_contextual_residual_verification_20261007/v1/verification.json`，SHA `2089e7482a0e56565269761b2e8737378b9c6eca272e395fe36b8bae728bcb89`。原summary的pending保留为run结束时状态，当前核验状态以此回执为准；verified不代表效果规则通过。

可复用：已有输入和表示无需再次提取；本轮提供可核对的ridge增量更新、完整重建记录及公平的更新强度对照。三份完整科学标准仍HOLD。本文为内部development报告，未将负结果包装成主方法胜出，当前论文仍保留上轮已验证的正文8页版本。
