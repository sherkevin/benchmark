# NobelClaw — 项目自造长程任务题集（材料发现 + 药物设计）

> 本文件夹是**项目自造的 benchmark 题集**（非公开数据集的采样）：39 道面向材料发现与药物设计的长程任务，以"多阶段强依赖 + 可机器测量的桩点（checkpoint）"评测 agent 过程量而非仅终答。新一代构造法（GT-first 反查 + DAG + 双验证）的可运行 POC 见根目录 `dag_poc/`。

## 文件夹结构

| 条目 | 内容 |
|---|---|
| `定义_长程任务与桩点.md` | 题集的总定义：什么是长程任务、桩点怎么设 |
| `case1_材料发现_超导体.md` 等 6 个 case 文档 | 六类任务族的设计文档（超导体/GLP1R/药物扰动/BaTiO 闭环/逆合成/靶点验证） |
| `instantiate/` | 实例化脚本与 45 个实例文件（case1/2/3/D/E/F 各自的 instantiate 器与产物） |
| `verifiers/hard_verifiers.py` | 4 类硬判分器（structure/conservation/tool_grounding/graph），带自检 |
| `ckpt_master_list.md` · `ckpt_gt_binding.md` | checkpoint 总表与"checkpoint→数据源"绑定关系 |
| `证据_admet_gt.md` 等 5 份 | **GT 事实底座**：各任务真值的出处核查（药物 ADMET/半导体带隙/超导 Tc/MolBench 数据集与亲和力池），保证"科学数值未编造" |

## 与仓库其余部分的关系

- `src/tasks/*.json`（带隙/超导/ADMET 任务）的 provenance 链指向本夹 `证据_*_gt.md`；
- `dag_poc/`（根目录）是方法升级后的新构造 POC：模板+DAG+双验证引擎+16 实例，真值快照源自本夹半导体证据链；
- 调研与索引见 `research/INDEX.md`。
