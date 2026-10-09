# Tartarus — 采样说明【分子发现·材料+化学+医药，物理模拟 oracle】

## 论文与来源
- 论文：*Tartarus: A Benchmarking Platform for Realistic And Practical Inverse Molecular Design*（Aspuru-Guzik 组，arXiv:2209.12487，NeurIPS 2023 Datasets & Benchmarks）
- 代码：https://github.com/aspuru-guzik-group/Tartarus
- 本地采样通道：GitHub raw + raw.githubusercontent（大 csv 截前 30 行），2026-07

## 这是什么
一个"**逆向分子设计**"benchmark：被测对象（生成模型/优化算法/agent）产出 SMILES 分子，Tartarus 用**物理模拟当 oracle** 打分。它的独特之处——**同时覆盖材料 + 化学 + 医药**，且刻意用物理模拟而非易被刷分的 ML 预测器。

## 四大领域任务 = 四组 oracle（采样的 `oracles/` 里是真实打分脚本）
| 领域 | oracle 脚本 | 打什么分（真实数据表头印证） |
|---|---|---|
| **医药** | `docking.py` | 蛋白-配体对接分（1SYH/4LDE/6Y2F 三个靶点）→ 结合亲和力=成药性。`docking.csv` 表头：`smiles,1syh score,4lde score,6y2f score` |
| **材料·OLED** | `tadf.py` | 有机发光分子：单-三线态能隙、振子强度（量子化学 xTB/DFT） |
| **材料·太阳能** | `pce.py` | 有机光伏功率转换效率。`hce.csv` 表头：`smiles,pce_pcbm_sas,pce_pcdtbt_sas,pce_1,pce_2,sas` |
| **化学·稳定性** | `reactivity.py` | 反应活化能/反应能 + 可合成性。`reactivity.csv` 表头：`smiles,...,activation_energy,reaction_energy,SAScore` |
| **可用性过滤** | `filter_.py` | PAINS/结构合理性过滤（`data/pains.txt`、`mcf.csv`），淘汰"作弊分子" |

## 怎么测 / 怎么打分（docking.py 真实逻辑）
`run_docking_1syh()` 直接 `subprocess` 调用 **QuickVina/Smina 对接程序**（真实二进制），传入靶点结构 `docking_structures/1syh/prot.pdbqt` 和坐标盒子，返回 docking score；分子坏了（对接失败）返回哨兵值 10000。**这就是你说的"调接口判断分子性质、得到评价指标"**——接口是真实的物理模拟/对接软件，指标是能量/亲和力/效率。

## checkpoint / 判分性质
- **判分器**：每个领域一个 oracle 脚本（`benchmark.py` 统一调度），写一次、全体分子复用；
- **金标准/target**：无需人工标注答案——**oracle 现场算**（对接能量、量子化学能隙、活化能都是物理量）；数据集里的 csv 是"起始分子池 + 参考分数"；
- **人力 vs 自动**：人力=搭 5 个物理模拟 oracle + 备靶点结构（一次性）；每个分子的评分全自动；
- **hard 吗**：是——物理模拟/对接是确定性数值计算，非 LLM。但它是**终态打分**（对最终分子打分），非"过程中间卡点"。

## 采样内容
- `oracles/`：5 个真实 oracle 脚本（docking/tadf/pce/reactivity/filter_）。
- `sample_data/`：docking/hce/reactivity 各 30 行真实分子（含 SMILES + 各 oracle 分数列）。
- `benchmark.py`/`example.py`：统一调度与调用示例。

## 对我们的价值
这是"**调性质接口打分**"范式最全的代表（材料+化学+医药一网打尽），且 oracle 是 hard 物理模拟。对本项目：若把这些 oracle 从"只评最终分子"改造成"评中间产物/阶段"，就能得到 hard 中间 checkpoint。它的 SMILES→物理量 oracle 可直接作为我们材料/药物任务卡点的 target 来源。
