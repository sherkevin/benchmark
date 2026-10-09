# 表示对照执行前审查

2026-10-07；独立审查 `/root/headroom_diagnostic_math`，root依据其完整意见落盘并统一LaTeX格式。只读审查时未生成新特征、训练或读取confirmation。

## 判断

允许进入实现和准备。科学矩阵只需两组BCE+CLS；无需增加BT、第三组或搜索。实际训练还需新执行commit、manifest和initial绑定。

## 控制范围

$W^+[u^{64};0]=Wu^{64}$。

例：新增列即使很大，乘恒零输入也不影响输出；没有数据梯度，AdamW衰减也不改变预测。因此同66,112登记参数不等于有效容量匹配：补零组实际4,672个。正结果不能单独证明hash碰撞是根因。

## 特征语义

$b^{64}_{ij}=\bigvee_k b^{1024}_{i,j+64k}$需要实际逐行核验。

例：高维位1和65映射到低维位1，低维失去区别，但未证明这个区别能预测工具正确性。两组同RDKit2025.9.2、GetMorganGenerator、radius/chirality/SMILES处理，仅fpSize变化。各自L2归一化：高维两个1折叠成低维一个1后，高维非零为$1/\sqrt2$，低维为1；同单位范数不代表相同几何。

## 初始化与训练

$W^+[u^{64};0]+b=Wu^{64}+b$。

例：旧第一层输出0.4，补零扩展后数学上仍为0.4；复制旧bias和双head，不另抽head。新列独立CPU流、固定seed与旧fan-in分布，避免默认1024初始化尺度混杂；不要求CUDA训练逐位复现。

两组BCE(score,r)+0.2BCE(CLS,r)，完整fit面板。例：四工具均正确的分子仍入两个BCE头。validation top1最大、平局最早；无mixed用epoch200。例：较晚loss更低但top1不增，不替换早checkpoint。不能因上一轮若干best epoch为1就改规则。

## 评价与终止

按分子配对差和LOO；两seed共享722分子，仅描述初始化敏感性。1024须在两seed分别胜补零和Corrected Frozen至少1pp，且两个差值的每seed五LOO均正。例：只胜补零仍低于Frozen，不能升级。资源线不等于显著性，也不证明在线/漂移/未见provider泛化。

复用已核四工具标签、split、initial、训练选择、回收和独立forward核验。没有发现需要扩大矩阵的阻断。负结果停止此固定构造，只有真实合同错误才重开相关检查；确认仍需新未读数据。
