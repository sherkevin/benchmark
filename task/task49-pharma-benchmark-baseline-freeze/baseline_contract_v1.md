# Task49 Baseline 合同 v1：医药直接工具选择

日期：2026-09-29
状态：候选合同；不替代 `docs/研究主线/生效/03-Benchmark与Baseline.md`，不改变任何已生效决议。
目的：把已阅读的近邻方法转换成可以在同一医药任务上公平比较的实验角色，防止把静态路由、在线选择、特权上界和不同反馈合同混在一起。

## 1. 统一任务合同

每个 episode 给出分子 $x$、性质端点 $e$、当前候选 provider 菜单 $A_t$、已经释放的历史反馈 $H_t$ 和调用预算。方法在读取当前 provider 输出之前选择一个 $a_t\in A_t$。工具终止后，只释放本次选中的运行状态、预测和正确性反馈；反馈按预注册的延迟队列进入之后的决策。未选 provider 的输出、当前真值和未来反馈不能进入选择或更新。

每个 baseline 都必须记录：输入表示、预训练/离线标签来源、在线可见反馈、provider 事件处理、每事件计算量、状态大小、随机种子和失败语义。若一个论文方法原本需要 reward/cost、组合动作、战略报价或全 panel 标签，不能悄悄删掉这些假设后仍称为原方法；只能写成“适配版”，并把差异列出来。

## 2. Baseline 角色分层

### 2.1 必须进入主实验的控制

| ID | 名称 | 作用 | 允许信息 | 不能回答的问题 |
|---|---|---|---|---|
| C0 | Random | 无学习下界，检查任务是否有可学结构 | 菜单、随机数 | 不能说明方法优于现代路由 |
| C1 | Best Fixed | 训练区完整 provider 标签选一个固定 provider，测试区不更新 | 训练区 full panel；测试区仅不使用反馈 | 不能证明动态更新有效 |
| C2 | Frozen Static | 训练区训练静态选择器，测试区冻结 | 训练区 full panel；测试区菜单与分子特征 | 不能证明在线反馈有价值 |
| C3 | Oracle | 评测器用当前完整 panel 和真值逐题选正确 provider | 评测时全 panel + 真值 | 不是可部署算法，只给机会空间上界 |
| C4 | Matched Online SGD | 与 Ours 同 backbone、初始化和标签预算；每次反馈只做一次新样本梯度更新 | 已释放 selected-only 反馈 | 不能单独代表现代 bandit 路由 |
| C5 | Matched Replay SGD | 同 C4，但从已合法观测的 replay buffer 更新 | 已释放 selected-only 历史 | 不能读未选动作标签 |
| C6 | Greedy LinUCB | P06 的直接近邻：每个 provider 一个线性上下文模型，按 selected correctness 在线更新 | 当前分子特征、已释放 selected-only 反馈 | 原论文的 unstructured context 演化与我们单步分子请求不等价 |
| C7 | PILOT-adapted | PILOT（Findings of EMNLP 2025）公开目标的离线 query/provider 先验 + 选中 provider 的轻量在线更新 | 训练区 full panel 可做共同 warm start；测试区 selected-only | 不能称官方医药复现；cosine、先验和反馈时序必须明写 |
| C8 | BayesianRouter-adapted | P06 邻近的离线 provider 先验 + selected reward 后验更新 | 训练区 full panel；测试区 selected-only | 不能用仓库默认坏路径或零先验冒充论文方法 |

C0–C8 是主结果的最小集合。若 Ours 不能超过 C6/C7/C8 中最强合法近邻，不能把“动态 JEV”写成已证明的算法贡献；若只能超过 C1/C2，最多说明当前表示或训练实现有收益。

### 2.2 现代静态/冷启动对照

| ID | 名称 | 原论文作用 | 在本任务中的合法适配 | 结论边界 |
|---|---|---|---|---|
| S1 | GraphRouter-adapted | P08 的图结构和新 provider few-shot/cold-start 对照 | 用训练区分子-provider 成功边构图；测试区冻结 router；新 provider 只按预注册 few-shot 事件接入 | 静态/少量适应控制，不是持续 selected-only JEV |
| S2 | RouterBench-style supervised router | P07 的静态 full-panel routing 参考 | 只用训练区全 panel 训练；测试区不读未选输出 | 全 panel 预生成结果可作静态 sanity check，不能证明实时学习 |
| S3 | RouterDC-adapted（资源允许） | NeurIPS 2024 dual-contrastive static router 参考 | 独立重实现或仅作论文对照；不得使用无许可证代码 | 可选，不得因未实现而声称已完成 baseline |

S1–S3 的目的，是回答“提升是否只来自静态表示/训练标签”。它们不能替代 C6–C8，也不能进入在线 baseline 的同一平均排名而不标注信息合同。

### 2.3 只在扩展实验中出现的方法

| ID | 方法 | 为什么不直接列入主表 | 合法用途 |
|---|---|---|---|
| E1 | P04 Reverse contextual MAB / ROSA | 原论文核心是 strategic provider cost truthfulness；医药 provider 没有报价和策略性上下文。去掉 reverse auction 后只剩普通 contextual MAB，和 C6 重合 | 在合成 strategic-cost 扩展中检验“truthfulness”边界；不得把删掉 auction 的版本称 P04 |
| E2 | P01 DOL-RM | 原目标是 reward/cost ratio，且有未知 task-arrival 分布；与二元 correctness 单臂选择不相同 | 增加调用成本/延迟目标后，作为 cost-aware delayed-feedback 对照；需单独写 reward/cost 合同 |
| E3 | P05 C2MAB-V | 原问题是组合式多 LLM 选择和长期 cost budget，主任务一次选一个 provider | 在允许多工具组合或硬成本预算的扩展中使用；主任务不伪装成同任务 baseline |
| E4 | P06 Greedy LinUCB 的原始脚本 | 原论文代码和上下文演化假设不直接适用于分子 provider | 主表使用 C6 的透明适配版，并同时报告适配差异 |

## 3. 动态 provider 事件合同

“动态”必须由事件驱动，而不是把静态训练集重新跑一遍。事件至少预注册一种，且事件发生时不泄漏该 provider 在当前测试流的全 panel 标签：

1. **arrival**：新 provider/version 加入 $A_t$，没有当前测试标签；
2. **retirement**：provider 从菜单移除，历史状态不能继续被选择；
3. **drift**：同一 provider 的 correctness 分布或 checkpoint 发生变化，变化点只在事件日志中记录，不提前给方法真值。

动态实验的最低对照：

- `Frozen-pool`：不处理事件或继续使用旧 provider 状态；
- `Reset-on-event`：事件后清空该 provider 的在线状态，不读取额外标签；
- `Warm-start`：只使用训练区允许的 provider 关系迁移；
- `Ours`：按方法卡规定的状态更新；
- `Full-panel event oracle`：事件后允许评测器读取完整当前 panel，仅作上界。

事件后至少报告 recovery accuracy、达到预注册阈值所需的 selected feedback 数、额外调用数、更新 p50/p95 延迟和状态内存。没有 arrival/retirement/drift 事件的实验，只能叫 fixed-pool online selection，不能支持 dynamic claim。

## 4. 公平性与泄漏检查

每个 run 启动前写入配置和输入哈希；每条选择日志必须能回答：

- 当时菜单里有哪些 provider；
- 方法选择前读了哪些字段；
- 选择时哪些反馈已释放；
- 选中 provider 的结果何时终止、何时释放；
- 未选 provider 的结果是否被读取；
- 更新前后状态 hash、更新时间和耗时；
- 失败、缺失、unknown truth 是否被错误填成 0。

所有方法使用相同分子顺序、菜单事件、延迟、调用预算、特征版本和测试区。训练区标签可以按合同共同开放，但必须逐方法记录使用方式。confirmation/test 只能在代码、provider checkpoint 绑定、冲突处理和事件方案冻结后打开。

## 5. 指标和主张映射

主指标先固定为 selected correctness 的测试前缀/后缀曲线及其 scaffold-disjoint 聚合；同时记录：

- 相对 C1 的累计错误差；
- 反馈达到固定准确率所需的数量；
- 事件后的 recovery steps；
- 每次选择和更新的 p50/p95 延迟、峰值内存；
- provider failure、unknown 和未完成 episode 的分母。

主张必须和对照一一对应：

| 观察结果 | 可以支持 | 不能支持 |
|---|---|---|
| 超过 C1/C2，但不超过 C6–C8 | 表示或训练有帮助 | 新在线机制成立 |
| 超过 C6–C8，仅在固定池 | 固定池在线选择有改进 | dynamic provider claim |
| 只在 arrival/drift 后超过所有合法近邻 | 动态事件适配可能是贡献 | 普遍工具可靠性或所有端点泛化 |
| 只降低更新延迟/内存，准确率相当 | efficiency claim | accuracy improvement |
| 只超过 Oracle 以外的某个弱 baseline | 该 baseline 不足 | state of the art |

## 6. 当前完成度与停止条件

已完成：论文和源码的首轮审计、C0–C8 的接口草案、Greedy LinUCB/PILOT/BayesianRouter kernel 的数值与反馈时序测试；C4/C5 matched SGD、C6 Greedy LinUCB、C7 PILOT-adapted、C8 BayesianRouter-adapted 已有 development 效果证据；Laya 静态表示诊断已完成并保留负结果。
未完成：S1 GraphRouter 的 checkpoint/训练边绑定、Laya/JEV 的 selected-only 在线适配、动态事件输入、provider 训练成员资格和独立 confirmation。已有 smoke 或静态诊断不能替代这些缺口。

在以下条件全部满足前，不提交新的效果型 A800 任务：

1. BBB 冲突组处理、truth split 和 confirmation 口径固定；
2. provider checkpoint 与训练来源绑定到可复核 manifest；
3. C0–C8 的输入/反馈/预算 schema 通过静态检查；
4. 至少一轮本地适配正确性回放能从原始 JSONL 独立恢复；
5. 动态事件的 arrival/retirement/drift 至少选定一种并写入实验卡。

## 7. 与论文近邻的可审计差异

- 相比 P04：不借用其 reverse auction/truthfulness 结论；我们的待验证差异是科学工具的二元 correctness、selected-only delayed feedback 和 provider/version events。
- 相比 P06：承接其 provider-specific contextual online selection，但把 fixed pool、myopic reward 和即时反馈扩展为可记录的 delayed feedback 与动态事件；若没有事件和延迟压力，实验不构成扩展。
- 相比 P07/P08：它们是静态或 offline/few-shot routing，对照价值在 representation/cold-start，不证明实时反馈更新。
- 相比 P01/P05：它们是 reward/cost 或组合动作问题；只有在扩展合同匹配时才纳入，不用作主任务“先进 baseline”的装饰。

## 8. 机器可读合同与静态检查

机器可读 roster 在 `baseline_contract_v1.json`，检查器在 `src/probe_headless/validate_baseline_contract_v1.py`。检查器只验证协议字段和主实验 roster，不读取 provider 输出、不训练、不产生效果结论。当前检查要求 C0–C8 唯一、selected-only binary correctness、选择前隐藏真值和未选输出、事件可记录，以及状态 hash/反馈释放/更新时间等日志字段齐全。

## 8. 版本与后续

这份合同是 Task49 的候选 v1。若后续要把 C0–C8 或动态事件口径写入生效 benchmark 主文档，必须先补齐 provider 资格和独立验证，并与用户共同确认；旧版本不静默覆盖。
