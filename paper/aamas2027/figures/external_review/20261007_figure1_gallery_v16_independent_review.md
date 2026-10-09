# Figure 1 图库风格独立审查：v16 prompt / v08 AI

- 日期：2026-10-07。
- 对象：完整 `prompts/figure1/v16.txt` 与原始 `generated/figure1/v08_ai.png`。
- 结论：**prompt 通过；生成图不通过，不进入论文。**
- 查看证据：已通过 `view_image` original 实际查看 v08，2060×763；三张 gallery 原图此前逐张查看，来源和 hash 见 [v13 独立审查](20261007_figure1_gallery_v13_independent_review.md)。
- 纸面：未测。本版已有实质错误，尚未进行 7-inch 论文页面检查。
- 范围：只读图稿与方法符号核对；没有修改正文、实验、方法、prompt 或 AI 像素。

## Prompt 与方法依据

当前机制是冻结 scorer 加各 provider 的已释放计数残差，逐项生成 score 并选一个工具。只有有效返回经过冻结 Judge 判分后的 selected event，才能进入队列；在 `τₜ=t+1+D` 释放后更新对应 provider 的 `n,s`。Runtime error 仅进入独立账本。

v16 的 11 组完备连边忠实该机制，并用菜单紧贴 Select 的空间安排消除了此前的菜单混接风险；prompt 通过。生成图不完整满足其第 6 组 Return→Judge→Chemical feedback。

## 必须修复（2 项）

1. **有效返回绕过 Judge。** Return 的“valid”箭头直接进入 Chemical feedback。Judge 位于其上方，只有指向反馈卡的下箭头，没有来自 Return 的入边；图把判分器画成孤立来源。必须实际连成 Return→Judge→Chemical feedback，再由反馈卡进入 queue。
2. **Features 的额外类型注释符号错误。** 图写 `x∈R^d`。只读核对当前 `paper/aamas2027/main.tex:132–134`：冻结分子表示为 `φ(x)∈R^d`，scorer 读取 `φ(x)`。应写 `φ(x)∈R^d`，或去掉这条未经 prompt 要求的注释；不能把原生输入和其表示混同。

第一项是反馈数据路径的实质错误，独立阻断接入。符号校正保持当前方法，不提出新模型。

## 已关闭的前轮问题与建议

Molecule+Endpoint 共同进入 Features；Eligible tools 紧贴 Select 上方且独立接入；Update 仅回到计数寄存器，没有直接进入 Select 的旁路。B 在 score、选择、计数与事件中保持一致；valid/error 分支仍可区分。这些修复应在后续新图保持。

建议仅两项：

1. 删除 Frozen scorer 的重复标题和方法区的长副标题，保留小型区域标题与内部图形层次。
2. Eligible tools 的 A/B/C 保持中性；在 q、选择卡、B 计数行与事件链中表达选中身份即可。

## 八项评分

| 项目 | 0/1/2 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 主方法明显最大，计算结构为视觉主体 |
| 模块表达 | 2 | Features、权重、计数、向量、合成算子和事件卡分别呈现 |
| 信息密度 | 1 | 方法副标题、重复 Frozen scorer 与额外类型注释增加冗余 |
| 布局 | 2 | 菜单与 Select 就近连接，底部仅保留紧凑反馈对象 |
| 色彩线条 | 2 | 蓝/灰/琥珀/紫语义清楚；主路径和反馈路径可以逐段追踪 |
| 数据路径 | 0 | valid Return 直接进入反馈，Judge 缺少入边 |
| 方法一致性 | 0 | 反馈未经过判分器；输入表示符号与正文不同 |
| 纸面可读性 | 0（未测） | 没有实际查看本版 7-inch 论文页面 |
| 合计 | **9/16** | 方法与数据路径错误不能由其余分数抵消 |

## 留存与复用

保留 v16 完整 prompt 和 v08 原始输出，不覆盖 v13/v06 或 v15/v07 的失败记录。可复用就近的菜单约束入口、合并查询输入、B 身份一致性、无旁路的计数回写、局部等待区与内部向量/算子表达。下一轮只需在该构图基础上恢复判分器的真实入边并校正表示符号，随后重新审查原图及最终 7-inch 论文页面。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v16.txt` | `77b931722d28c293be1640a9187f569b207188e2e0ba74f2ec19bf1dc0130a7f` |
| `generated/figure1/v08_ai.png` | `8bd585cce26bcd471c4eca860f476a009bbcd0ac7551ffe26242b062b56b8ad5` |

主 session 的 TODO_LOG 由协调者汇总；本审查仅新增本文件。
