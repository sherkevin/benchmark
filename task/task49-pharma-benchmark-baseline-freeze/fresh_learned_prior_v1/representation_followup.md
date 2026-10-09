# 下一问题：表示保真，而非继续换损失

2026-10-07。准备材料，不是冻结实验卡，未运行新特征统计/训练。依据是本轮BCE/BT均低于Corrected Frozen且四组LOO全负，不能把该结果外推为所有非线性表示失败。

## 需要区分的解释

固定Morgan64下，线性修正强于两种MLP训练；原因可能包括表示压缩、样本量、优化/模型复杂度和工具错误本身难预测。现有证据不能只选其中一个当已证根因。下一步优先检验能复用且无需新provider调用的输入保真变化，不继续扫同一模型的loss/seed/lr。

## 已有资产及陷阱

只读核查者`/root/fresh_headroom_locator`确认：

- `fingerprint_diagnostic_v1/inputs.json`已有722行`on_bits`，`audit_pharma_menu_sensitivity.py::_features(data)`可解码Morgan1024。但旧构造未显式启用chirality、未归一化；不能直接接上后说只改位数，且旧33-provider标签禁止带入。
- `run_provider_fresh_menu_cpu.py::fingerprint(smiles)`可从当前722个canonical SMILES构造radius2、chiral Morgan1024，配合原L2归一化保持其余特征语义。没有已封存的722行矩阵；CPU只算特征，不调用任何provider。
- 现有Laya表示是旧33菜单的`[N,33,1024]`candidate-marker，连均值也仍是菜单条件化表示，不能冒用为四provider query特征。若选Laya，需定义新的query提取和单独encoder运行。

## 下一卡必须先解决的两个数学问题

1. 将当前64位构造与1024位构造放在同一RDKit版本、半径、chirality和规范化下，明确折叠关系与哪些信息可能丢失。不能把旧1024缓存当干净的同源控制。
2. 1024→64第一层比64→64有更多参数，提升最多先解释为“更完整输入与对应scorer方案有用”，不能直接宣布bit collision是原因。卡中须选最小且能排除主要容量混杂的控制，明确成本；不得先看OOF分数再加对照。

分子、四checkpoint、truth、划分、验证规则和选择成功指标保持；旧157 confirmation继续只读。只有完成这两点的理论审查后才固定最小新矩阵。该准备不取消论文能力提升目标，不自动升级任何新方法，也不把更高维指纹作为创新。

## 独立方法审查返回（执行前）

审查者`/root/headroom_diagnostic_math`认为以下两组方案值得进入下一张冻结卡，不增加BT：原Morgan64零填充到1024 vs 同源Morgan1024，统一1024→64双head BCE+0.2CLS、5fold×2seed、200步。两者有66112个登记参数，但零填充只使用前64列，参与预测的仍为4672；所以这只匹配网络实现和配方，不能声称有效容量相同，不能分离碰撞/自由度/几何的各自因果贡献。

初始化从本轮对应fold/seed的sealed initial checkpoint继承前64列、hidden bias与两个head，新增960列用独立固定随机流按原第一层同分布初始化。两组复制完整state；避免直接采用1024 fan-in默认尺度，使零填充也改变初始化函数。零填充在数学上保持原初始化输出，浮点实现允许小误差。

特征须使用当前`GetMorganGenerator`同一接口和RDKit版本，只改变fpSize，不能一组用旧AllChem接口。准备时重建64位须与seal一致，并逐行确认原始布尔bit64[j]等于bit1024[j+64k]的OR（k=0..15）；不假设该关系自动成立。随后各自L2归一化，不做无关碰撞统计。

下一卡的资源线建议：1024组两个seed同时超过零填充控制和Corrected Frozen各至少1pp，两个差值各自5个LOO全正。只超过一者不够支持完整改善。未达到就停止该固定增维方案，不扫宽度/投影/初始化。上述仍是准备审查，待输入合同与实现固定后执行；当前尚无新特征计算和新A800任务。
