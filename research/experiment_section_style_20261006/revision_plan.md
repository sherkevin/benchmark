# 正文实验章节重排计划（2026-10-06）

## 目标

用顶会正文常见的“短 Setup + 紧凑导航矩阵 + 主结果表/图 + 简短结论”的结构替换当前冗长矩阵。保留真实结果和不确定性，正文仍为 8 页。

## 计划改动

1. Section 4 的 baseline 表压缩为按功能分组的 deployable comparators；不适用工具只用一句范围说明。
2. Section 5 的实验矩阵改为四行：静态可学性、自然 fresh stream、controlled drift recovery、效率与成本；列为 Question / Setting / Methods / Metric。
3. 删除主文空的 VOI 预留表；VOI 和未来扩展只保留在 artifact/限制说明。
4. Section 6 按“自然结果—漂移结果—代价/机制”组织。每节先给结果，再用短段说明差值和区间。
5. 审查并修正审计性过强、fallback 语义和 oracle 命名；不改动真实结果。
6. 编译、渲染并做 8 页和投稿 gate 检查。

## 成功条件

- 正文矩阵符合 `report.md` 的四列/四行标准；
- 无未完成实验伪装成主结果；
- 数值与 `task/` 报告、原始 JSONL 和哈希一致；
- PDF 正文恰好 8 页，references 从第 9 页开始；
- 变更、审查结果、PDF 哈希和未解决问题写入 TODO_LOG。
