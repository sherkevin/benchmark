# ADR-0034：AG-JEV 在当前 benchmark 上因审计支持不足而停止

- 状态：Accepted
- 日期：2026-09-27
- Supersedes：候选文档 `next_method_candidate_v2.md` 的“只待 CPU 回放”状态

## 背景

Audit-Gated Calibrated JEV 用固定概率的随机审计得到 context-provider residual，再用 alpha-spending 置信门决定是否修正冻结选择器。它的安全语义清楚，但门必须先获得足够的随机支持。

## 判定

当前 BBB development 只有 722 条题、33 个 provider。即使审计概率为 1、把 context 合并为一个 cell，每 provider 的最大期望支持数为 $722/33=21.88$。取预先约定的 $\delta=0.05$、最后 look $\ell=12$，Hoeffding 半径为

$$\sqrt{2\log(2\cdot33\cdot12^2/0.05)/21.88}\approx1.05.$$

该半径已经大于 residual 的最大绝对值 1；保留四个 cell 时支持数约为 5.47，半径达到截断值 2。于是 gate 在当前 benchmark 上不可能得到正的 signed correction。这个结论不依赖某个模型实现，也不占 GPU。

## 决策

停止 AG-JEV，不提交 CPU 效果回放或 Nebula 任务；不通过缩小置信半径、降低 $\tau$ 或合并事后标签来挽救。保留方法卡作为统计设计审查材料。下一候选必须在当前样本支持下有可识别的更新，或明确提出新增合格数据并先完成数据资格审查。

## 后果

当前动态 JEV 主方法仍未确定。现有负结果缩小了可行空间：标准参数更新、provider-transfer、局部 memory 和审计门都没有形成可写的正向主张。后续优先做不依赖稀疏 context-provider 计数的、可校准且 selected-only 的 state compression 方案；在 CPU 预审查通过前不占 A800。
