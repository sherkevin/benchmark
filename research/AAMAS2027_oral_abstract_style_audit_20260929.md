# AAMAS 2027 摘要与优秀 Oral 论文风格审查

日期：2026-09-29
状态：研究依据与工作稿审查；不是 AAMAS 官方评分公式，也不是最终投稿摘要。

## 1. 调研范围

本轮优先读取有明确 Oral 标记的官方摘要，并补充一篇与延迟反馈直接相关的 NeurIPS 论文。材料包括：

- [ICML 2024 Oral: Pausing Policy Learning in Non-stationary Reinforcement Learning](https://icml.cc/virtual/2024/oral/35461)
- [ICML 2024 Oral: APT](https://icml.cc/virtual/2024/poster/35453)
- [ICML 2023 Oral: Hierarchies of Reward Machines](https://icml.cc/virtual/2023/oral/25486)
- [ICML 2024 Awards: Probabilistic Inference in Language Models via Twisted Sequential Monte Carlo](https://icml.cc/virtual/2024/awards_detail)
- [ICML 2024 Awards: Debating with More Persuasive LLMs Leads to More Truthful Answers](https://icml.cc/virtual/2024/awards_detail)
- [NeurIPS 2024: Online Learning of Delayed Choices](https://proceedings.neurips.cc/paper_files/paper/2024/file/047bf3f8aa5a050351de38df589cc6af-Paper-Conference.pdf)

这些论文覆盖 agent、在线学习、延迟反馈、工具使用和方法型研究。它们不是“录用论文的固定模板”，而是用来检查摘要的论证行为。

## 2. Oral 摘要的共同结构

### 2.1 第一层：先把现实冲突说成一个可判断的问题

优秀摘要的开头通常不是“某领域很重要”，而是指出一个旧设置在现实条件下为什么不够：

- Pausing Policy Learning 把实时更新和非平稳环境之间的冲突放在第一句，并直接挑战“持续更新一定更好”的默认看法。
- Debating with More Persuasive LLMs 先指出人类标注可能不再可靠，再把它压缩成一个问题：弱模型能否判断强模型的正确性？
- Twisted SMC 先把多个 LLM 技术归到同一个序列采样问题，再说明现有推断需要更有效地使用未来价值。

**对本项目的要求**：第一句要让读者看到“工具都能返回结果，但 agent 在调用前不知道哪个结果值得信任”这个冲突，而不是从 `33 providers` 开始报配置。

### 2.2 第二层：只说一个缺口，马上给出研究对象

这些摘要通常用一句话把已有方法的缺口和本文对象连起来：

- APT 先分开参数高效微调与结构化剪枝的收益，再提出同时处理训练和推理效率的机制。
- Hierarchies of Reward Machines 先给出平面 reward machine 的分解能力，再指出子任务组合仍缺少抽象层，随后定义 HRM。
- Delayed Choices 先说明未知参数和延迟反馈共同造成偏差，随后直接说要设计处理延迟反馈的算法。

**对本项目的要求**：不能只说“我们研究 outcome-driven adaptation”；必须紧接着说我们改变的对象是什么：每次更新只绑定已选 provider 和反馈释放时刻，调用后验证是另一种动作。

### 2.3 第三层：机制只保留一个动作

Oral 摘要不会罗列完整系统组件，而是用一个动词讲清新东西：introduce、propose、define、leverage、model。之后只解释这个动作改变了什么计算或信息。

当前摘要的 `pre-call selection / post-call verification / state updates / accuracy / failure coverage / extra calls / latency / update cost / online / replay / contextual-bandit / confidence-gated` 一次出现太多名词，读者会把它当成实验协议清单。

**本项目的一个核心动作**：把 selected-only delayed feedback 作为更新时序约束，并把 verification 从 selection 中拆出来。其余字段应进入正文和实验表。

### 2.4 第四层：结果必须是 headline result，不是过程报告

优秀摘要通常包含一个可以复述的结果：

- Debating 摘要给出 debate 相对朴素基线的准确率，并说明对模型和人类都成立。
- APT 同时给出保留性能、加速倍数和内存下降，直接对应论文的效率主张。
- Twisted SMC 把理论对象、评估用途和三个应用场景连起来，但没有把所有消融实验塞进摘要。

**对本项目的要求**：最终投稿版必须给出一组确认集 headline result，包含 comparator、分母、成本和适用范围。当前只有开发 BBB 结果，因此工作稿可以写“selected-only update 尚未超过 fixed selector，confidence gate 提升到 0.9238、平均 1.206 次调用”，但必须保留 development scope，不能写成最终方法效果。

### 2.5 第五层：结尾给机制性结论，不写工作日志

Oral 摘要结尾通常说明结果揭示了什么新的关系：

- Delayed Choices 把复杂度解释为反馈缺失量，而不只是延迟时间。
- Hierarchies of Reward Machines 说明层级表示能让平面表示无法学习的任务变得可学。
- Debating 把实验结果连接到“没有 ground truth 时如何对齐”的更广问题。

当前摘要最后的“keeps adaptive selection, verification, and confirmation as separate claims”是内部验收语言，应该改成研究结论：selected-only feedback 缺少 counterfactual information，因此选择学习和调用后验证必须作为两个不同问题评估。

## 3. 语气和信息密度

| 维度 | Oral 摘要的做法 | 当前摘要的问题 | 修改方向 |
|---|---|---|---|
| 语气 | 主动、直接、克制地自信 | `We study`、`We build` 连续出现，像项目报告 | 用 `formulate / define / show / reveal`，每句承担一个动作 |
| 背景 | 一句现实冲突 | 开头直接进入 provider 配置 | 先写 agent 为什么必须在错误信息出现前做选择 |
| 方法 | 一个机制动作 | 协议和组件清单过长 | 只保留 selected-only delayed update + verification 分离 |
| 实验 | 一个主结果 | baseline 和指标全部平铺 | 最终只放一张 headline 表能回答的数字 |
| 负结果 | 转成机制发现 | `do not exceed` 和 `does not establish` 像自我辩护 | 写成“揭示 counterfactual outcome 缺失的信息瓶颈” |
| 结尾 | 可迁移的科学结论 | `reusable basis` 偏泛 | 明确 delayed feedback 是设计约束，而不是实现细节 |

## 4. 当前摘要的具体问题

当前摘要原稿有 185 词，长度本身合适，但有四个结构问题：

1. **缺少 stakes**：`Tool-augmented agents often choose...` 说明场景，却没有立即说选择错误会造成什么决策风险。
2. **机制不够具体**：`outcome-driven adaptation` 是名字，不是计算动作；更新绑定 selected provider 和 release time 应更早出现。
3. **实验细节过载**：33 providers、五种 split/失败/延迟条件、七类指标和五类 baseline 同时出现，削弱了主张。
4. **结尾像验收记录**：`does not establish a dynamic selector` 和 `keeps ... as separate claims` 对内部审查有用，对投稿摘要不够自然。

## 5. 工作稿修改规则

在方法和 confirmation 尚未冻结前，摘要采用以下边界：

- 可以写：问题、selected-only delayed feedback、协议、真实开发结果和信息瓶颈。
- 不可以写：dynamic environment、real-time learning、continual tool lifecycle、universal tool use、已验证的新 JEV 算法。
- 开发结果必须带 `current BBB development stream` 或等价边界；最终投稿版再替换为 model-fresh confirmation 的 headline result。
- 最终版只保留一个主要数字组；准确率、额外调用和置信区间必须绑定同一个 comparator 与数据分母。

## 6. 当前采用的 Oral 风格版本

当前 `paper/aamas2027/abstract.tex` 已改成 137 词、8 句的版本：

`现实冲突 → 信息缺口 → 明确问题 → 一个更新时序机制 → benchmark 实例 → 开发证据 → 信息瓶颈 → 可证伪目标`。这与调研样本中约 129–199 词、5–7 句的摘要密度接近。confidence-gated verification 已从摘要 headline 移出，保留在正文的辅助实验中，避免与 direct selection 形成两条主线。

它仍然是 Internal Research Draft。方法冻结后，摘要只需替换结果段和最后一句，不应重新引入 provider 清单或实验流水账。

## 7. 投稿候选版与当前诊断版的区别

在摘要截止先交的版本不能继续像“我们当前还没有做成什么”的诊断报告。当前 `abstract.tex` 已改为投稿候选口径：

- 先写 frozen router 与 unconstrained online head 的两种失效；
- 再写目标模块的三个约束：calibrated cold-start prior、selected-only release-time update、uncertainty-gated switch；
- 用同一信息和计算预算定义 baseline 与评估指标；
- 暂不编造最终 accuracy、recovery 或 confirmation 数字，等主方法和实验冻结后只替换结果段。

这种写法把方法论的优越性写在“它同时保留冷启动能力和允许受控适应”上，而不是把当前开发负结果当成论文主结论。它仍然保持证据边界：provider-version stress events 只能写成受控压力场景，不能写成自然环境漂移已经被证明。

## 8. 当前版本的风险

这版摘要已经比诊断版更像方法论文，但它对最终方法有一个明确承诺：状态更新、先验保持和 uncertainty gate 必须在方法章节中有可执行定义，并在主实验中分别被消融。若最终实验发现该模块没有在相同信息/成本预算下优于强 baseline，不能只保留这版摘要的正向措辞；必须回到结果事实并收窄 claim。
