# AAMAS 2027 当前评价核对表

> 日期：2026-09-28
>
> 性质：开发阶段暂定审查，不是投稿结论，也不是 AAMAS 官方评分。
>
> 依据：`docs/研究主线/生效/评价标准/` 下三份 v004 标准，以及当前已提交的代码、实验日志和审计报告。

## 1. 先给结论

三份评价标准文件已经写成可执行版本：有硬门槛、逐项 0–4 分锚点、证据路径、通过线和停止动作。它们解决了“以后按什么标准检查”的问题。

项目本身还没有达到这些标准。当前阶段是开发审计和方法筛选，不是投稿准备，也没有打开 confirmation。

| 文档 | 硬门槛 | 暂定分数 | 通过线 | 当前结论 |
|---|---|---:|---:|---|
| 故事线与创新（含论文稿） | H3/H4 FAIL；H7/H9/H10 FAIL或UNKNOWN | **47.5/100** | ≥85 且 H 全 PASS | FAIL |
| 方法论 | H4 FAIL；H3 UNKNOWN；H5 未闭合 | **37/60** | ≥49，规定 M 项均 ≥3 且 H 全 PASS | HOLD/FAIL |
| Benchmark+Baseline | H2/H3/H7/H8 未过 | 37/60 | ≥49 且 H 全 PASS | FAIL |

故事线另有一个只看主线文档、不把论文稿完整性计入的宽松分：55/100。正式投稿闸门采用 47.5/100，因为当前 `paper/aamas2027/main.tex` 在 `\maketitle` 后只有摘要，没有 Method、Experiment、Discussion 和 Conclusion。方法论若暂不计公式控制字符损坏为硬证据缺口，可报 39/60；严格分按 37/60。以上分数都是一次独立保守复核的暂定分，不是按“双人取低分”规则形成的最终生效 scorecard。当前没有把它们写回三份生效主文档。

## 2. 评价标准文件是否写好了

### 已完成

- 故事线标准 v004：S1–S12、硬门槛、故事线通过线和论文段落检查要求已写入。
- 方法论标准 v004：M1–M15、信息边界、数学、实时性、消融、恢复性和主张校准要求已写入。
- Benchmark+Baseline 标准 v004：H1–H10、B1–B15、Stage A/Stage B、confirmation 和停止动作已写入。
- 三份标准都要求 evidence path、hash 或日志行号，允许复核者从原始证据重算。

### 仍有一个治理问题

三份 v004 文件标注的生效日期是 `2026-09-29`，当前日期是 `2026-09-28`。因此本记录把它们当作当前采用的内部审查标准，但不把日期矛盾静默改掉。后续需要一次治理确认：要么将生效日期改为实际确认日期，要么保留日期并标明“待生效”。

## 3. 目前已经完成的工作

### 故事和问题定义

- 已把问题收窄为：给定分子、目标性质和 provider 菜单，在调用前选择一个 provider；调用后只得到被选 provider 的返回状态和延迟 correctness feedback。
- 已明确工具正常返回但结果不一致是可观察失败场景，选择、验证、拒答/升级的边界已写入故事文档。
- 已明确开发结果、负结果、confirmation 不能混写。

### 方法和工程证据

- selected-only、延迟 $d\in\{0,1,4\}$、failure/unknown、pending queue、反馈释放和 future-label 隔离已有代码与测试。
- 独立 replay verifier、hash、tamper rejection 和结构化 raw JSONL 已经在多组实验中使用。
- 做过 PAR 因子化候选的可用性审计；因子化假设被数据否证，已停止把它作为主候选。
- 做过 joint-success residual 候选的 CPU 证伪卡；它比直接 SGD 更好的一些点估计不能抵消区间跨 0，且没有稳定超过 Frozen。
- Best Fixed、Oracle、Frozen、matched Online SGD 和 PILOT 已有可解释的开发证据；其余现代 baseline 仍未齐。

### Benchmark 和数据审计

- TDC BBB/AMES 的来源、版本和 hash 已审计。
- Task36 的 33-provider 逐事件回放、selected-only 反馈、延迟队列和失败语义已有合同与回放证据。
- 已发现 BBB canonical 冲突、旧开发 panel 与官方 test 的 overlap，以及 provider training membership 未证明的问题。

## 4. 具体没有达到的地方

### A. 故事线与创新

1. **没有形成可成立的 integration gain。** 当前 structured 和 joint-success 候选没有超过 Frozen；故事可以描述问题，但不能把“动态 JEV 整体机制”写成已被证明的贡献。
2. **nearest-work matrix 没闭合。** 现有材料列了 PILOT、BayesianRouter、LinUCB、GraphRouter 等，但还没有对每个近邻逐项对齐输入、反馈、目标、延迟和失效边界。
3. **claim–evidence–falsification ledger 尚未形成。** 现在是分散在预注册和报告里，不能用一张表逐条追溯主张。
4. **完整论文段落还没有审计。** 故事文档写了段落顺序和语气要求，但尚无完整 draft、段落级 evidence map 和第二位读者复核。
5. **论文稿还没有正文。** 当前 LaTeX 文件只包含标题和摘要，不能按论文段落标准计入 STORY READY。
5. **故事范围仍略宽。** pre-call selector、post-call verifier、拒答/升级和动态更新都在主线中，但最终论文的主贡献尚未收成一个可以被一组实验直接证伪的切口。

### B. 方法论

1. **Proposed 方法没有冻结。** PAR-JEV 已因独立性问题停掉，joint-success residual 也没有过 CPU gate；当前生效方法文档仍是候选集合，不是最终算法。
2. **创新机制的必要性没有证据。** 目前没有反例或机制消融能证明动态更新的具体结构比 Frozen/Online SGD 必要。
3. **数学还没有到最终方法级别。** PAR 的公式已指出条件独立限制；现有更新可实现，但不能称精确后验、MLE 或 Fisher posterior，也还不是冻结的主方法。
4. **方法文档当前存在控制字符。** `docs/研究主线/生效/02-方法论.md:42` 的 `\begin{cases}` 被写成了 backspace 字符，另有制表符影响公式；这会破坏公式渲染和“公式—case”复核，严格评分已扣在 M1/M6。
5. **实时性证据不完整。** 有部分 p50/p95 和事件级计时，但没有最终方法的压力曲线、内存曲线和 A800 端到端结果。
6. **baseline 和方法边界还没有统一。** BayesianRouter、GraphRouter、Laya/JEV、Replay、Greedy LinUCB 尚未在同一 runner、menu、feature、delay 和 budget 下完成。

### C. Benchmark+Baseline

1. **truth contract 未冻结。** BBB canonical 冲突、duplicate、alias 和 unknown 处理规则仍未最终确定。
2. **独立性未闭合。** 旧 722 开发 panel 与官方 test 存在 strict/scaffold overlap；provider training membership 尚未证明为 0 或按 lineage 分层。
3. **现代 baseline roster 未完成。** 当前能运行的只是部分名单，不能把 adapter smoke 或源码审计写成完整效果比较。
4. **公平矩阵尚未被全名单验证。** 特别是 warm start、feature、provider exposure、compute budget 和训练数据权限需要所有方法逐项一致。
5. **confirmation 尚未打开。** 方法、菜单、epoch、阈值和 baseline 还没有一次性冻结，因此不能写最终主结果。

## 5. 下一步必须按这个顺序做

1. 冻结 truth/duplicate/冲突/unknown 规则，完成旧 panel 与官方 split 的 overlap 处置。
2. 建立 provider training membership 和 checkpoint lineage 表；无法证明 fresh 的 provider 必须单独分层，不能混进同一主结论。
3. 建立 nearest-work 与 claim–evidence–falsification 两张表，逐条绑定现有证据和反驳条件。
4. 在不改生效主线的前提下，只选一个候选机制，补齐统一 runner 下的 Greedy LinUCB、PILOT、BayesianRouter、GraphRouter/明确不可比边界、Replay 和 matched controls。
5. 对候选机制先跑同一 CPU 证伪卡；只有通过预登记 gate，才考虑 A800 资源和 confirmation。
6. 形成完整论文 draft 后，再做段落级审计和第二位读者独立打分；在此之前不能宣称 STORY READY 或 SUBMISSION REVIEW READY。

## 6. 本记录的使用边界

这是一份当前状态审计，不替代三份生效评价标准，也不修改故事线、方法论或 Benchmark+Baseline 主文档。新证据出现后，应新建带日期的 scorecard 或更新本记录的审计版本，保留旧分数和证据路径。
