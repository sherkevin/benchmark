# Task Template — 一个 task 的标准结构

> 本目录是**新 task 的起点**。复制整目录开始一个新 task:
>
> ```bash
> cp -r materials/templates/task_template taskN
> # 然后按顺序填:goal.md → experiment/ → result.md
> ```

## 一个 task 的三个组成部分

| 文件 | 作用 | 何时填 | 谁看 |
|---|---|---|---|
| `goal.md` | 任务目标(要做什么、为什么、什么算完成)| **task 开始前**先写 | 自己 + 上游需求方 |
| `experiment/` | 实验过程(逐次测试、原始数据、卡点)| **任务过程中**逐次写,一次实验一篇 | 自己 + 复现者(内部) |
| `result.md` | **对外成品**:任务执行情况 + 结论 + 后续(对照 goal 勾验)| **task 收尾时**写 | **外部人员 / 上级 / 跨团队** |

## 三者关系

```
goal.md  ─>  experiment/*.md  ─>  result.md
(任务契约)   (过程逐次记录,内部)   (对外成品,自包含)
```

- **goal** 是**任务契约**:开始前写,写完不轻易改。
- **experiment** 是**过程日志**(内部):一次实验一篇 .md,套 `experiment/TEMPLATE.md`。
- **result** 是**对外成品**:对照 goal 勾验、给结论、给后续动作。**外部读者只看 result**。

## 为什么没有单独的 block 文档(重要)

早期版本有第 4 个文件 `block.md`(对外汇报)。**已废除并入 result**,因为:

> **"block(受阻/失败)本身就是一种 result。**" 一个 task 要么做完(done)、要么受阻(blocked)、要么放弃(abandoned)——这都是它的**执行结果**,没必要拆成两个文件。受阻解除后,把 `goal.md` 重做即可。

所以 **result 用顶部的 `Status` 字段表达三种结局**:

| Status | 含义 | result 里必须有 |
|---|---|---|
| `done` | 任务完成(无论结论是正面还是负面)| 对照 goal 勾验 + 结论 + 下一步 |
| `blocked` | 受阻待解 | 上述 + **§4 受阻说明**(现状 + 影响 + 已试过什么 + **后续可推进的动作** + 复活条件)|
| `abandoned` | 决策放弃不做 | 上述 + §4(为何弃 + 是否留口子) |

**关键**:`blocked` / `abandoned` 的 result **不是写完失败现状就完事**——必须写清"**后续能开展哪些动作把它推下去**"(模板 §4.4)。失败的 result 的价值,正在于告诉接手人下一步怎么走。

## result 必须自包含(对外铁律)

**外部读者看不到我们仓库里的任何其他文件**(`experiment/`、`docs/`、别的 task)。所以:

- 每条结论和关键证据**写全在 `result.md` 里**(把关键数字/事实 inline 进去)。
- 指向其他文件的路径只能作"更多细节"补充,**不能是某条结论的唯一来源**。
- 读者只读 `result.md` 一个文件,就应完整理解:做了什么、结论是什么、接下来怎么办。
- 补一节"最小背景"(模板 §6),让不了解项目的人也能读懂。

## 共同写作原则(三个文件都遵守)

1. **一句话结论必须最前**——读者 30 秒拿到判断。
2. **清晰易懂为第一要务,不冗长不繁琐**。
3. **result 自包含;experiment 可只放细节**(result 引用它作补充)。
4. **每个 claim 带证据**(result 里 inline,不只甩路径)。
5. **诚实**——失败要写失败,且写清后续怎么推进。

## 命名与放置

```
taskN/
├── goal.md                    # 任务契约
├── result.md                  # 对外成品(Status: done/blocked/abandoned)
├── experiment/                # 实验过程(内部)
│   ├── README.md
│   ├── 0001-<slug>.md         # 一次实验一篇,顺序编号
│   └── runs/                  # (可选)原始数据 / 工具输出
└── <other artifacts>/         # 代码、数据、子产物
```

> *`NNNN-<slug>.md` 让实验按顺序自动排好;`<slug>` 用 kebab-case 描述这次测什么。*

## 与上游 / 下游

- 上游来源(填到 `goal.md` 与 `result.md` 头部):指向 PRD / 项目级文档(`src/prd/...` / 根 `PROJECT_RESULT.md`) 。
- 下游消费方:本 task 的 `result.md` 会被项目级汇总文档引用作证据。

## Lite 版(小 task 的简化约定)

如果 task **≤ 1 小时、单次实验**,允许简化为:

- ✅ `goal.md`(5–10 行:一句话目标 + 完成标准 + 上游来源)
- ✅ `result.md`(Status + 对照 goal 勾验 + 1–2 条产出 + 后续)
- ❌ 不创建 `experiment/`(关键数据 inline 进 `result.md`)

`result.md` 顶部加一行 **`> Lite task: 实验数据已 inline,无独立 experiment/ 子目录。`**

**判断标准**:**外人 30 秒看完 `result.md` 就能完整知道做了啥、得了啥结论 → 是 lite**。否则(过程复杂、卡点多、需多次复跑)走完整三件套。

## 字段一致性约定

两个带 meta 表的模板(goal / result)字段对齐:

| 字段 | goal | result |
|---|---|---|
| Task ID / 路径 | ✅ | ✅ |
| 类型 | ✅ | ✅ |
| 状态 | 🟢 Drafting goal / 🟡 Running / ✅ Done | **顶部 `Status` 行**:`done` / `blocked` / `abandoned` |
| 负责人 | ✅ | ✅ |
| 时间 | "**预计起止**" | "**起止**" |
| 上游来源 | ✅ | ✅ |
| 下游消费方 | ✅(可选) | ✅ |
