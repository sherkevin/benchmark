# cases 目录组织

> 依据总纲（[`../research/我方与现有benchmark差异对照.md`](../research/我方与现有benchmark差异对照.md)）：
> **case 优先，不预设 template**；前几个合格 case 作为 fewshot 指导后续批量构造。

## 三级分类

```
cases/
  <领域>/            materials（材料）/ gene（基因）/ drug（药物）
    <测试类型>/       atom / chain
      <难度档>/        {fixed,open}_{basic,some,enough}_tools
        <具体 case>/
          runs/       每次 agent 的 workspace（<model>__<round>/）
```

### 维度一 · 领域
- `materials` 超导材料（Tc 等）
- `gene` 基因
- `drug` 药物

### 维度二 · 测试类型
- **`atom` 原子操作**：单一 state，考一种核心推理。当前重点。
  典型形态：所有候选可测但 credit 不够遍历，agent 须**测锚点→摸清规律→序贯决定下一个测谁→credit 用尽给最优解**。
- **`chain` 链式**：多 state、明确步骤，一步输出是下一步输入，可测中间 state 转换。
  是 atom 的组合，属后续阶段（原子操作立住后再做）。

### 维度三 · 难度档（问题范式 × 工具供给，正交叠加）
问题范式：
- **`fixed`** 判别式——候选与真值都在库里、都可测，从中选/推最优。
- **`open`** 生成式——不给候选清单，agent 自行探索（A/B 库：A 可见、gt 藏在不可见的 B 里）。

工具供给（工具越强越多，越替代推理、难度越低）：
- **`basic`** 只给最基本工具（测性质 + 读写文件），推理全靠 agent。
- **`some`** 加若干便捷工具（把某步推理变成一次调用）。
- **`enough`** 给足工具，更多考"会不会用工具"而非纯推理。

> 同一道题的 `basic/some/enough` 用**相同 query 与判分**，只改工具供给——这是总纲"分阶段验证"的落地：先用 basic 测出失败点，再针对性给工具降档。

## 硬约束（所有 case 通用）
1. **fixed 类工具无死角**：所有候选特征都能从 `measure_*` 拿到真值，工具不允许误差、不允许"测不到"。难度只来自 **credit 不够遍历全部**。
2. **credit 初值与每次消耗写进 prompt**：让 agent 自主决定测谁、何时停——这本身是要考的能力。
3. **只给目标，不给步骤**：任务分解与"下一个测谁"的规划是核心推理，不能在题面写成配方。
4. **judge 机器硬判、无 LLM judge**；真值来自离线真库快照。
5. **造题必过可解性闸**：候选间关系"半规律"（可推、非离散非明显），且金标准与次优的差距 > 数据噪声。

## 当前已落地
| 路径 | 说明 |
|---|---|
| `materials/atom/fixed_basic_tools/case_sc_dopant_01` | Pr 掺杂抑制律的序贯决策题（credit 逼推理），调优中 |
| `materials/atom/fixed_basic_tools/case_sc_constraint_01` | 约束陷阱（早期，单次 API 范式）|
| `materials/atom/fixed_basic_tools/case_sc_extrap_01` | 趋势外推（早期探索）|
| `materials/chain/fixed_basic_tools/case_sc_chain_01` | 四步依赖链（多 state，明确步骤）——属 chain 类 |

## 方法论文档
见 [`_methodology/`](_methodology/)：**`实验规范.md` 是跑实验的唯一标杆**（claude code workspace 范式、四道隔离锁、预登记纪律、判分口径 B）；另有好 case 四条标准与时延边界实测。
