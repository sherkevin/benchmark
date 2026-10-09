# 项目文档约定 · Templates Index

> 本目录定义本项目所有标准化文档的模板。**新增任何成品文档前先读本文。**

## 1. 标准 task 范式

**一个 task = goal + experiment + result 三件套**(详见 [`task_template/README.md`](task_template/README.md)):

```
taskN/
├── goal.md          # 任务契约(开始前写)
├── experiment/      # 过程逐次记录(内部;一次实验一篇)
│   ├── README.md
│   ├── 0001-<slug>.md
│   └── runs/
└── result.md        # 对外成品(收尾时写;Status: done/blocked/abandoned)
```

**起一个新 task**:
```bash
cp -r materials/templates/task_template taskN
# 然后按顺序填:goal.md → experiment/ → result.md
```

## 2. 三个模板的分工与边界

| 模板 | 路径 | 谁看 | 何时填 | 形态 |
|---|---|---|---|---|
| **goal** | `task_template/goal.md` | 自己 + 上游需求方 | task 开始前 | 任务契约,可被勾验 |
| **experiment** | `task_template/experiment/TEMPLATE.md` | 自己 + 复现者(内部)| 任务过程中 | 一次实验一篇,六节式 |
| **result** | `task_template/result.md` | **外部人员 / 上级 / 跨团队** | task 收尾时 | **对外成品、自包含、带 Status** |

**关键约定**:
- **不再有单独的 `block` 文档**:block(受阻/失败)本身是一种 result,用 result 顶部的 **`Status` 字段**(`done` / `blocked` / `abandoned`)表达;受阻解除后把 `goal.md` 重做。
- **result 必须自包含**:外部读者看不到 `experiment/`、`docs/` 等其他文件,所以关键结论与证据要 inline 写在 result 里,路径指针只作补充。
- **失败的 result 也要写后续**:`blocked` / `abandoned` 的 result 必须写"后续可推进的动作 + 复活条件"(result 模板 §4),不能只写失败现状。
- **experiment 是内部过程件**,result 引用它作"更多细节"补充。

## 3. 共同写作原则

1. **一句话结论必须最前**——读者 30 秒拿到判断。
2. **清晰易懂为第一要务,不冗长不繁琐**。
3. **result 自包含**;experiment 放细节,result 引用它作补充。
4. **每个 claim 带证据**(result 里 inline,不只甩路径)。
5. **诚实**——失败要写失败,且写清后续怎么推进。

## 4. 现状(2026-06-04)

| Task | goal | experiment(内部)| result(对外)| Status |
|---|---|---|---|---|
| task1 | ✅ | `session_*/` + `analysis.md` | ✅ `result.md` | done |
| task2 | ✅ | `research/分子发现GT调研/molbench_dataset_findings.md` | ✅ `result.md`(自包含)| done |
| task3 | ✅ | `A_*` `B_*/` `C_*/` | ✅ `result.md` | done |
| task4 | ✅ | `benchmark_landscape_report.md` + `research/长程任务Benchmark调研/task4_papers/` | ✅ `result.md` | done |
| task5 | ✅ | `experiment/`(已用本模板风格)| ⚠️ `CONCLUSION.md`(用户并行 WIP,待改名 `result.md`)| done |
| **项目级** | PRD | 各 task experiment 聚合 | 根 `PROJECT_RESULT.md`(项目级汇总)| 🟡 进行中 |

**说明**:
- 老 task 的 experiment 是早期格式,**不强求重命名**;新 task 严格按模板。
- task2 的旧报告 `molbench_reuse_assessment.md` 已重写为 `task2/result.md`(自包含、带 Status)。
- `PROJECT_RESULT.md`(原名 `BLOCK.md`,2026-06-04 改名;现居仓库根)是**项目级汇总**(聚合各 task result),给项目级判定与路线。
- task5 是用户并行 WIP,用户主动对齐时再改名。

## 5. 写新文档时的快速决策树

```
要写新文档吗?
├── 新开 task?
│   └── cp -r task/_templates/task_template task/taskN(goal + experiment + result)
├── task 内的某次实验?
│   └── 用 task_template/experiment/TEMPLATE.md 在 task/taskN/experiment/ 加一篇
├── task 收尾(含失败/受阻)?
│   └── 写 task/taskN/result.md,顶部 Status 标 done/blocked/abandoned;失败也要写后续动作
└── 项目级整体汇总?
    └── 改根 PROJECT_RESULT.md(项目唯一汇总文档,不要新建第二个)
```

## 6. 目录约定提醒

- **`research/`** = 平铺调研文档:调研报告 + benchmark 同名解释文档 + 索引;一次调研的报告与中间材料并成一个文件夹
- **`reference/`** = 纯数据资产(公开数据集采样 + 判分脚本 + `README_采样说明.md`),不存分析文档
- **`task/_templates/`** = 本目录(模板与约定)
- **`task/taskN/`** = 一个 task 的全部产物(goal + experiment + result + 子产物)
- **根 `PROJECT_RESULT.md`** = 项目级汇总;**根 `TODO.md`** = 全项目唯一待办
