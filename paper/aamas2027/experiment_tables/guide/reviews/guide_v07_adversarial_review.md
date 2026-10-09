# Guide v07 反方审查：不要让条件目标冒充 MIRA 结果

**审查对象**：`versions/v07_20261008/guide.pdf`（v07，2026-10-08）及其修订说明、现有科学审查。  
**审查范围**：benchmark/baseline 合理性、效果解释、方法优越性边界、可迁移性和误差表达。只读审查，未运行新实验、未修改主 Guide。

## 总评

**总评分：58/100；建议：Major Revision / 暂不把它当作 MIRA 论文的效果证据。**

v07 作为“固定 157 分子、固定四工具、固定 fit/seed/delay、给定自然或受控 arrival/stress law 下的参考分布与条件账本”是有价值的：它正确区分了 fit SD、order SD、Monte Carlo mean interval、empirical quantile 和 win-probability interval；保留了 paired rescue/harm、provider joint bits 和负收益顺序；也明确说条件预算不是候选预测。问题在于，图表仍很容易被读成“已有 MIRA 优于 baseline 的证据”：表 1/2 给出 MIRA 的受控 +14.18 pp，表 14/15 给出受控 MIRA 相对 SGD +4.74 pp 和 125/128 胜，而自然 MIRA 与 Frozen 相同或略低。这些数字是**既有策略在固定已看队列上的行为描述**，不是新候选的泛化、因果归因或方法优越性证明。

最危险的误读是：受控 flip 过程被当成真实 provider drift；Retro/fit-only fixed 被当成可部署上界；旧 MIRA 的误差账本被当成未来 MIRA 的可达选择性；以及 128 个 order 被当成分子或部署不确定性。v07 有文字免责声明，但主表的版式和 baseline 行仍给这种读法留下空间。

## 评分分解

| 维度 | 分数 | 反方判断 |
|---|---:|---|
| Benchmark truth / split / lineage | 13/20 | TDC truth、157 条件队列和 joint bits 说明清楚；但 Guide 本身没有逐项绑定 provider checkpoint、训练成员风险和 source/commit，不能独立确认“fresh”。 |
| Baseline 合同与可比性 | 10/20 | Frozen、Online SGD、LinUCB、BayesianRouter、Retrospective Best Fixed 角色有区分；但方法版本、适配差异、warm/cold exposure、tuning budget、同一 feature/初始化仍需从 baseline contract 外部拼接。 |
| 结果解释与统计 | 16/20 | 配对单位和三类区间区分得好；但 order-law 仍可能被读作泛化，且零 practical margin、重复 fit/seed/cohort 的可迁移性没有进入主结论门槛。 |
| 方法优越性与机制归因 | 8/20 | MIRA 受控优势与自然 null 并存，不能支持 generic online gain；controlled credit/permutation 只证明某个 evaluator-side assignment intervention，不能隔离表示、先验、更新规则或调参。 |
| 可复现性与范围声明 | 11/20 | raw hashes、replay verification、负订单较完整；但没有为未来候选规定自己的冻结、自己的 matched controls、自己的 candidate-specific CI 作为硬 gate。 |

## 必须修改项（阻塞主张）

### 1. 把“已测策略”与“未来 MIRA”在视觉和语义上彻底分栏

**问题（CRITICAL）**：表 1/2、14/15 同时列 MIRA 和 baseline，容易把 MIRA-Relay 读成已确认方法；而第 5 页说的是固定队列上的参考分布。`MIRA−SGD=+4.74 pp` 与 `125/128` 只能回答“该记录策略在该 order/stress law 下相对该 SGD 回放如何变化”。它不能回答新 prior、新表示或新 update 的性能。

**最小解决办法**：把表题改成“Existing-policy reference only”，每个 MIRA 行改为 `Recorded Relay (reference policy)`；在表 1/14/15 首行加入醒目标记：`No candidate/generalization claim; 157 molecules were already inspected`。正文/摘要若引用 +14.18 或 +4.74，必须同时写 cohort、order law、stress law、matched unit。

### 2. 受控 correctness drift 不得称为 provider drift 或部署鲁棒性

**问题（CRITICAL）**：受控场景从 tick 78 翻转 fit-best provider；顺序还改变哪些分子承受翻转。因此它混合了到达顺序、受损分配和策略反馈，不能证明自然 lifecycle drift、真实模型退化或一般环境非平稳性。

**最小实验/检查**：同一 candidate 冻结后，做一个预登记的 held-fixed intervention：跨 order 固定受损 molecule×provider 集合，只改变 arrival order；另做固定 order、随机受损集合的 stress replication。若不做，所有叙述统一为 `evaluator-side controlled correctness-drift law`，禁止写“部署漂移”“真实 provider degradation”。

### 3. 为每个 baseline 补完整 fidelity/fairness matrix，避免不可比

**问题（MAJOR）**：Guide 只给方法名及输出数字。没有在同一页绑定论文/代码版本、commit、适配改动、输入特征、初始化、tuning budget、selected-only feedback、delay、compute、失败语义。尤其 Online SGD/LinUCB/BayesianRouter 与 MIRA 的 feature、warm-start 和更新 exposure 可能不同；Retrospective Best Fixed 使用 confirmation truth，不能排进可部署方法排名。

**最小 source check**：逐 baseline 生成一行机器表，字段与 `03-Benchmark与Baseline评价标准` H6/H7/B8/B10 一致；独立 checker 从 raw event 验证 pre-choice visible fields、相同菜单、delay、budget、seed。无法补齐的行降级为 audit-only，并从“方法优越性”比较移除。

### 4. 先给 Best Fixed/Frozen 的 headroom，再给动态结果；禁止旧 comparator 归因

**问题（MAJOR）**：表 2 的受控 MIRA +14.18 pp 很醒目，但它相对 Frozen；表 15 又相对 SGD +4.74 pp。自然 MIRA 与 Frozen 持平且低于 Frozen 的 order mean。若没有统一 headroom 和 own-permutation，读者可把“超过 Frozen”误解成超过最强静态可部署选择。

**最小实验**：冻结一个候选后同一 order 同时跑 candidate、candidate-Frozen、candidate-Online-SGD/Replay-SGD、candidate-own-permutation 和 fit-only/Retrospective controls；报告 pooled、equal-scaffold、family-macro 三个预登记 estimand，及 rescued/harmed/both-wrong。旧 Relay 的 +14.18 只能保留为 reference，不得替代该实验。

### 5. 条件 α/β 目标必须改成“需求规格”，并禁止反算成预测

**问题（MAJOR）**：`α=59.67%, β=1.13%`、E3/E4 的 81.63%/80.95% 是在观测 error strata 上的均匀响应假设。它们没有 candidate policy、没有新 trajectory、没有 CI；也不保证可由局部 context 识别。表格中的精确小数仍会产生“可达目标”的错觉。

**最小 source check**：对每个条件预算增加一列 `assumption / falsifier / observed candidate evidence`；没有 candidate evidence 时统一显示 `UNTESTED`，只保留区间或整数计数（如“至少修复 24/28 harmful switches”），不报告伪精确预测 CI。候选一旦冻结，必须在自己的 event replay 上重新估计 α/β，不沿用旧 strata。

### 6. 处理重复使用数据和伪样本量

**问题（MAJOR）**：每 cell 4,710 records = 157 molecules × 5 fitted priors × 2 fixed seeds × 3 delays；128-order页再平均同一 30 configs。它们不是 4,710 个独立分子，也不是 61,440 个独立部署。fit folds、seed labels、delay 和 orders 复用同一 cohort；不能用 record-level bootstrap 或把 fit SD 当标准误。

**最小统计检查**：所有主差异以 order（n=128）或 molecule/scaffold（n=157，按预登记权重）为推断单位；给出 cluster/family-aware paired CI 和 exact denominator；删掉“4,710 observations”式可能误导的标题，改为“157 molecules, 30 fixed configurations per order”。若要声称新分子泛化，增加完全未读 scaffold confirmation，而不是增加 order 数。

### 7. 把 practical benefit margin 与零假设胜出分开

**问题（MAJOR）**：125/128 的 win interval 检验的是严格大于 0，未检验预先有意义的最小提升；受控 MIRA−SGD 的 3 个负 order 仍在 −2.68 pp。`q025>0` 也只是固定过程 order quantile，不是未来候选保证。

**最小实验/检查**：在 confirmation 前冻结非零 margin（例如由调用成本/风险 utility 推导），报告 paired CI 是否超过 margin、margin 下的胜率、最坏/下分位 order 和 harmed rate。若没有合理 margin，结论只能是“方向性参考分布”，不能用“robust improvement”。

### 8. 补 prior-art overlap 与“组合即创新”的反证

**问题（MAJOR）**：Guide 以 Online SGD、LinUCB、BayesianRouter 和固定/回放对照覆盖了部分近邻，但没有在 Guide 中绑定它们的原始论文、代码版本和任务差异，也没有证明 MIRA 的 residual、fit-rate-centering、selected-only delayed update 相对这些近邻形成新的可检验接口。把现有组件串起来不自动产生方法优越性。

**最小 source check**：在 baseline contract 增加 nearest-work 表：每个近邻列输入、反馈、delay、状态、目标、探索、provider scope、本文改动和可能泄漏；至少为 SGD/linUCB/BayesianRouter/Replay 类方法给 primary citation + commit + adaptation diff。若无法证明 integration gain，Guide 只保留为设计审查材料，不写“方法优越”。

## 在什么地方 MIRA 合理地应该优于 baseline

以下是**机制预测**，不是 v07 已证明的结果；每条都必须由 candidate-specific matched replay 验证：

1. **受控、可识别的 correctness drift 且反馈已释放**：MIRA 若正确使用 selected-only residual，应比 Frozen 少受损 provider 的重复选择，并可能优于普通 Online SGD；应看 post-release trajectory、switch-away、rescue/harm，而不只看最终 accuracy。
2. **provider 工作范围有 context-dependent 差异**：若同一 fingerprint/局部上下文对 provider 可靠性有稳定信号，context-aware residual 应优于只用全局计数的 marginal reliability；需要 held-out scaffold 或 provider-family 分层。
3. **延迟仍在声明范围 D∈{0,1,4} 且 pending queue 合法**：MIRA 的优势应随已释放反馈出现；delay=0 的优势不能外推到未释放反馈，delay 增大若仍无变化需要检查泄漏。
4. **错误不是全工具共同错误**：E3 的 all-wrong 区域理论上不可由 routing 修复；MIRA 只能在 mixed/avoidable strata 改善。若声称全体准确率提升，必须把可改善 headroom 分母写清。

## 在什么地方 MIRA 合理地应当逊色

1. **自然无漂移或 drift 很弱的队列**：Frozen 已是 89.81%，Relay 自然 order mean −0.29 pp；额外状态和更新可能只增加方差/成本，不应宣称稳定提升。
2. **冷启动、短轨迹、反馈稀疏或大 delay**：没有足够 selected-only receipts 时，残差估计不稳定；MIRA 可以低于 Frozen/Best Fixed，且应报告 cold-start 分层。
3. **all-wrong、provider family 高相关或模型成员重叠**：共同错误不能靠 selector 解决；把相关 provider 数量当独立证据会夸大 headroom。
4. **fit prior 更差或 representation 不匹配**：v07 的 BCE/BT 学习型 prior 已有负结果；MIRA 必须允许低于 corrected Frozen，不能把 prior gains 归给 updater。
5. **成本受限场景**：MIRA selection 27–32 μs、update 2.8–3.5 μs 是 macOS arm64 kernel-only 的参考，不是 A800/end-to-end；若加入编码、队列、I/O，可能逊于 Frozen/SGD 的实际延迟。

## 可保留目标

- 保留四工具 joint correctness bits、all-correct/mixed/all-wrong 分层和 paired rescue/harm/both-wrong。
- 保留自然 null 与 controlled evaluator-side stress 分表；保留 128 order 的 adverse orders 和跨方法 covariance。
- 保留三种不确定性对象的明确区分：order-law mean 的 Monte Carlo interval、描述性 order quantiles、win-probability interval。
- 保留 E3/E4/E6 作为条件需求与停止规则，但统一加 `UNTESTED / no candidate evidence` 标签。
- 保留“157 已查看分子不是新 confirmation”及“future candidate 不继承旧 SD/covariance/wins”的边界。

## 建议的分布与误差表达

- 主分析单位：128 matched orders；次级单位：molecule/scaffold，按预登记权重；不要把 4,710 records、61,440 streams 或 9,646,080 decisions 当样本量。
- 所有候选差异用 paired mean、paired SD、95% CI、q025/q975、full range、strict-win fraction、harmed/rescued 共同报告；明确哪些是 descriptive quantile、哪些是 mean uncertainty。
- family/provider 重复使用时给 family-macro 与 scaffold-cluster CI；seed/fold 作为固定或随机因素写清，不能隐含独立。
- 对 controlled stress 报告 stress-law 条件、affected molecule allocation 和 pre/post-release trajectory；不能只给 pooled accuracy。
- cost 报 kernel-only 与 end-to-end 分开，给 p50/p95、峰值内存和硬件；当前数字只能叫 macOS arm64 kernel reference。
- 对 practical claim 预先给 nonzero margin；若未过 margin，使用“方向性/条件参考”而非“robust improvement”。

## 结论

v07 最适合作为**反 cherry-picking 的条件设计指南和固定队列参考**。它还不能单独支撑“MIRA 方法优于 baseline”或“在线学习泛化”的论文主张。提交前至少完成上面 1–4、6–8 项：重新标注 reference-only scope，补 baseline fidelity/nearest-work 证据，冻结 candidate-specific matched replay，并以 order/scaffold 分层报告 practical margin 与 paired uncertainty。若这些工作显示自然场景仍不超过 Frozen，应该保留为边界结果；这会让论文更可信，也会直接检验 MIRA 的真实适用范围。
