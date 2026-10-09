# 实验矩阵与 Best/Outstanding 论文的差距审查

日期：2026-10-08  
对象：`paper/aamas2027/main.tex` 当前生效稿的 `tab:matrix` 及其结果分母。  
目的：判断矩阵的科学覆盖、版式和实验量级是否达到顶会正文的阅读标准，并给出不改变结果的修订方案。

## 1. 结论

当前矩阵**科学覆盖基本够用，论文表达还不够好**。它已经覆盖自然 fresh、声明的 controlled drift、开发诊断和回执/成本审计，但还没有达到 Best/Outstanding 论文常见的“几秒钟看懂实验逻辑”的状态。

主要问题有三类：

1. **层级混在一起。** `Headroom` 和 `Static diagnostics` 是开发支持证据，`Natural adaptation` 和 `Drift recovery` 是主问题，`Cost / validity` 是审计。五行在视觉上同权，读者会误以为有五个同等重要的主实验。
2. **量级的分母没有分层。** `157` 是独立 confirmation molecules；`5 folds × 2 seeds × 3 delays = 30` 是每个方法的重复 schedule；`4,710 = 157 × 30` 是每个方法的记录调用数；`270` 和 `150` 是把方法也算进去的全体 stream 数。当前表只写其中的一部分，容易把重复记录误读成独立样本。
3. **单元格承担了太多协议文字。** 方法名、干预、指标和审计状态被压在同一张宽表里，使用 `\footnotesize` 后信息密度仍高，但阅读抓手不强。Best/Outstanding 论文的主表通常是方法×任务、方法×指标或一个简短的实验地图；完整公平性、回执和实现细节放到正文短段、附录或 artifact。

因此，当前矩阵不需要继续堆实验行，也不应把 4,710 写成独立样本量。需要把它改为一个**导航表**，明确独立单位、重复 schedule 和全体 stream 三个层次，并把主实验、支持证据、审计标出来。

## 2. 与官方 Best/Outstanding 样例的对照

本次对照使用官方奖项页或官方开放获取论文：

| 样例 | 论文正文的实验组织 | 对本稿的启示 |
|---|---|---|
| ICLR 2024 Outstanding，DoLa | Setup 用短段落固定数据、模型、基线和验证方式；主表直接按任务和模型给出结果，后续小节每次只回答一个问题；额外验证和大表放附录。 | 矩阵只能做导航，不能重复列出完整方法合同。 |
| NeurIPS 2024 Best，VAR | 一个短 setup 后用紧凑的 method×metric 表；缩放、泛化和组件消融作为独立问题；表格单元格不写长协议。 | 需要把主结果、机制证据和成本审计拆开。 |
| ICCV 2023，SAM（顶会高影响样例） | 每个任务采用 Task/Datasets/Results 的短结构，主文保留能改变结论的图表，广泛消融转 supplement。 | 每一行应有明确的科学问题和主读出，不能把审计清单伪装成主实验。 |
| AAMAS 2024 Best，Incentives for Early Arrival | 该 Best 论文以形式化性质、定理和少量例子为主，没有靠堆实验矩阵取胜。 | AAMAS 的审美不等于“表越大越好”；问题切口和证据对应关系比行数重要。 |

来源与页码证据保存在 `topconf_experiment_literature.md` 和 `sources.jsonl`；官方来源包括 [ICLR 2024 Outstanding awards](https://blog.iclr.cc/2024/05/06/iclr-2024-outstanding-paper-awards/)、[DoLa official paper](https://proceedings.iclr.cc/paper_files/paper/2024/file/edc36117f795ca52a0cbf6a7b3882859-Paper-Conference.pdf)、[NeurIPS 2024 Best Paper announcement](https://media.neurips.cc/Conferences/NeurIPS2024/NeurIPS2024_Best_Paper_Awards.pdf)、[VAR official paper](https://proceedings.neurips.cc/paper_files/paper/2024/file/9a24e284b187f662681440ba15c416fb-Paper-Conference.pdf)、[SAM official paper](https://openaccess.thecvf.com/content/ICCV2023/papers/Kirillov_Segment_Anything_ICCV_2023_paper.pdf) 和 [AAMAS 2024 Best Paper page](https://www.aamas2024-conference.auckland.ac.nz/awards/best-paper-awards/)。

## 3. 当前矩阵的量级核对

当前 receipt 给出的数量是自洽的，但正文没有把它们写成同一套计数关系：

| 层级 | 自然 fresh | controlled drift | 含义 |
|---|---:|---:|---|
| 独立 confirmation molecules | 157 | 157 | 统计上最接近独立任务单位；置信区间按 scaffold 聚类，而不是按调用行聚类。 |
| 每个方法的 schedule | 30 | 30 | 5 folds × 2 seeds/replicas × 3 delays。 |
| 每个方法的记录调用 | 4,710 | 4,710 | 157 × 30；这是 paired calls，不是 4,710 个独立分子。 |
| 全体 method streams | 270 | 150 | 自然为 9 个方法 × 30，stress 为 5 个方法 × 30。 |
| 每个 stream 的决策 | 157 | 157 | 一次调用/分子。 |
| 全体记录决策 | 42,390 | 23,550 | 157 × 全体 streams；重复 schedule 仍共享同一 157 个分子。 |

开发侧还包含 722 个分子、五个 scaffold folds，以及独立的 score-centering 和 offline-objective 诊断；这些是支持证据，不能和 157 个 fresh molecules 合并成一个“总 N”。

主文应该显式写出：

> We report 157 independent confirmation molecules. Each method is replayed over 30 fold/seed/delay schedules, giving 4,710 recorded calls per method; the 270/150 stream totals count all methods and are not additional independent molecules.

这句话比继续增加一个总调用数字更能解除审稿人的量级疑问。

## 4. 必须修的版式问题

### P0：给矩阵增加“规模/读出”列

当前四列是 `Question / Data / Compared methods / Primary metric`，但 `Data` 同时承载了独立样本、干预和延迟设置。建议将列改成：

`Study` · `Question and setting` · `Comparators` · `Scale and primary readout`

这样可以在一个单元格内并列写 `157 molecules; 30 schedules/method; 4,710 calls/method`，而不把这些数字误认为同一层级。

### P1：用行标签表达证据层级

建议五行保留，但第一列加上短标签：

- `Support — headroom`
- `Primary — natural`
- `Primary — drift`
- `Support — mechanism`
- `Audit — cost/contract`

这比把 `Headroom`、`Natural adaptation`、`Cost / validity` 平铺更符合论文的科学叙事，也不改变任何实验。

### P1：缩短比较方法

完整方法角色已经在 `tab:baselines` 定义。矩阵只写 `Frozen; MIRA-Relay; SGD; LinUCB; BayesianRouter` 等短名，避免一行被方法名撑高。`matched`、`cyclic` 的精确定义留在 baseline 表和正文。

### P1：把成本审计从“实验问题”改成“审计行”

成本行只写 `420 streams; 157 decisions/stream; 270/270 + 150/150 receipts`、`update p95` 和 `receipt pass rate`。不在矩阵承诺未给分母的 `failures`，也不把 replay pass 当作方法效果。

### P2：表格字号和宽度

当前表用 `\footnotesize`、四列长句，视觉上像一张项目台账。修订后先保留 `table*`，改用短句和更均衡的列宽；目标是让表在 `\small` 下完成。如果行高仍超出半页，再降回 `\footnotesize`，而不是继续压缩列间距。

## 5. 推荐的正文矩阵

下面是本次修订采用的内容。它不新增结果，只改变导航方式：

| Study | Question and setting | Comparators | Scale and primary readout |
|---|---|---|---|
| Support — headroom | Can the development panel leave selection headroom? BBB development, scaffold OOF | Best Fixed, Frozen, BCE/BT | 722 molecules; scaffold OOF; selected correctness and oracle gap |
| Primary — natural | Does released selected feedback help on a stationary fresh menu? Four fresh providers, $D\in\{0,1,4\}$ | Frozen, MIRA-Relay, SGD, LinUCB, BayesianRouter | 157 molecules; 30 schedules/method; 4,710 calls/method; prequential success and scaffold-cluster CI |
| Primary — drift | Can the same update recover from the declared midpoint flip? Same menu, selected fit-best label flipped after $t=78$ | Frozen, MIRA-Relay, SGD, LinUCB, BayesianRouter, cyclic | 157 molecules; 30 schedules/method; 4,710 calls/method; paired $\Delta$, rescued/harmed |
| Support — mechanism | Is the effect source-aligned rather than a destination permutation? Sealed matched replay | Frozen, local relay, cyclic relay | 90 streams/condition; 124 scaffold clusters; Local$-$permutation contrast |
| Audit — cost/contract | Does the one-call contract preserve timing and receipt validity? All confirmation streams | All deployable methods | 270 natural + 150 stress streams; 157 decisions/stream; p95 update and receipt pass rate |

The full per-event receipt, hashes, failure ledger and implementation details remain in the artifact. The table is not a replacement for those records.

## 6. Acceptance checks for the revision

The revised matrix is accepted only if all of the following hold:

1. `157` is visibly labeled as the independent confirmation molecule count.
2. `4,710` is labeled as per-method recorded calls, with `30 schedules/method` shown beside it.
3. `270` and `150` are labeled as all-method stream totals, not independent sample counts.
4. Primary, support and audit rows are visually distinguishable by text labels.
5. The table remains a navigation table: no hashes, status codes, out-of-scope lists or long fairness contracts in cells.
6. Every row maps to an existing result or audit block; no new number or claim is introduced.
7. The AAMAS build remains 8 body pages plus references from page 9, with no overfull or undefined-reference warnings.

## 7. Reusable conclusion

The scale is not intrinsically too small for this question, but the current table makes the scale look ambiguous. A routing paper should make the independent unit and the repeated online schedule explicit. The correct repair is denominator separation and evidence hierarchy, not inflating the molecule count or adding more rows.
