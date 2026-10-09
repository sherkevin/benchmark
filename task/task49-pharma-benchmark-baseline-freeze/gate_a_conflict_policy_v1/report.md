# Gate A：BBB 冲突处理政策候选 v1

日期：2026-09-29
状态：**候选政策；未写入生效 benchmark，也未新建 ADR。** 这份报告只把现有审计结果整理成一个可确认的选择。

## 1. 审计对象与计数

对象是 TDC ADMET Group 的 `BBB_Martins`，以 canonical SMILES 识别同一分子。原始冲突收据为：

- `experiments/logs/task49_tdc_admet_split_audit_20260929/bbb_v4/conflicting_groups.json`
- 收据 SHA-256：`9e528562616ec9be1065830bc3104f1919f6da8418b0d00911303987386bd6a6`
- 原始 `bbb_martins.tsv` SHA-256：`eb32676c7bf6e2bcd98b7625186bbb2ba4e4644c1d1468dfa18b6be665bd5e98`

结果如下：

| 范围 | 原始行 | canonical 分子 | 冲突组 | 冲突行 |
|---|---:|---:|---:|---:|
| train/validation | 1,624 | 1,581 | 8 | 16 |
| official test | 406 | 394 | 2 | 4 |
| 合计 | 2,030 | 1,975 | 10 | 20 |

每个冲突组都同时含有标签 `0` 和 `1`，而不是一个多数标签。部分组是不同名称映射到同一结构（例如 `acetylsalicylate`/`aspirin`、`BRL53080`/`loperamide`），部分组甚至使用相同名称（例如 `loratadine`、`levodopa`）。现有公开文件没有逐行实验来源、测量条件或时间顺序，因此不能判断哪一行是真值，也不能证明它们是可聚合的重复测量。

对 confirmation 候选的实际影响已经由固定脚本复核：先排除 2 个 official-test 冲突组，再排除旧 development panel 的 canonical 和非手性 Bemis--Murcko scaffold 重叠，得到 157 条 scaffold-disjoint 候选（标签 36/121，124 个 scaffold）。清单及 SHA-256 见：

`task/task49-pharma-benchmark-baseline-freeze/gate_a_confirmation_manifest_v1/report.md`

## 2. 为什么不采用三种“修标签”办法

### 不采用 majority vote

每个组只有一条 `0` 和一条 `1`，不存在多数票。任意平票规则都会额外引入一个没有数据来源依据的先验。把标签比例当作多数票也会把跨分子总体偏斜错误地施加到单个分子。

### 不采用 first row

TDC 文件没有声明行顺序代表采样时间、实验质量或权威来源。保留第一行等价于把文件序列化方式当作科学证据；重新下载、重排或不同解析器都可能改变结果。

### 不采用 model-based label selection

用某个 provider、selector 或当前方法决定保留哪个标签，会让评价真值依赖被评价的方法，产生标签泄漏和循环论证。它还会把方法误差直接写回 benchmark，无法再作为独立的 confirmation truth。

因此当前不能把冲突行静默合并、改成单标签，或放入 selector 的训练和评分分母。

## 3. 推荐政策（待共同确认）

**P-C1：canonical-conflict exclusion。** 在能够取得逐行原始实验出处之前：

1. 将 `conflicting_groups.json` 中的 10 个 canonical group 从 BBB 的训练、验证、开发评估和 confirmation 评分中整体排除；不保留其中任意一行，不修改原始 TDC 文件。
2. 在每个派生 manifest 中保存 `conflict_excluded=true`、冲突收据 SHA、排除组数和排除行数；报告原始分母与清洗后分母，禁止只报清洗后的数字而不说明删去了什么。
3. 对当前 confirmation 使用已生成的 157 条 scaffold-disjoint 候选清单，锁定其 manifest SHA；不得从 TDC test 临时重新筛选。
4. 冲突记录仍保留在只读 audit 层，用于数据质量报告，不作为任何 provider 的训练标签或 selector 的反馈标签。
5. 若未来获得逐行 assay 来源，另开版本并预注册“重复观测”模型、聚合规则、权重和不确定性；不能在本政策下回填标签。

这个政策牺牲少量样本，换取真值独立性和可复现性：官方 test 只排除 4/406 行、2/394 个 canonical 分子；当前更严格的 scaffold-disjoint confirmation 仍有 157 个候选。它不会把 TDC 官方文件改写成“无冲突”，而是明确区分原始数据和主实验可评分集合。

## 4. 仍需确认的点

1. **Owner 确认：** 是否将 P-C1 作为主 BBB benchmark 的生效冲突政策。确认前，本报告不改变任何生效文档。
2. **分母口径：** 主 routing 结果是否以排除冲突后的 molecule-level manifest 为唯一评分分母，同时在附录报告 TDC 原始行级统计和排除审计。
3. **多 endpoint 一致性：** hERG/AMES 需要分别做同样的 canonical conflict、重复行和来源审计；不能把 BBB 的排除规则未经审计直接复制过去。
4. **确认集与开发集边界：** 157 条候选只有在 provider 子菜单、方法、delay、seed 和 evaluator 私有标签全部冻结后，才能升级为正式 confirmation。

在以上事项确认前，Gate A 的 conflict/overlap 闸门仍为 HOLD；本报告的结论是“有一个可执行的排除候选政策”，不是“confirmation 已完成”。
