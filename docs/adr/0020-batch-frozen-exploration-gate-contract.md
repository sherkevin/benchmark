# ADR-0020：按 batch 冻结探索 gate，并把置信目标限定为 calibration 流

状态：Proposed  
日期：2026-09-25

## 背景

ADR-0019 选择只用随机探索样本更新 bounded mean。统计复核发现其第一版实现有两个问题：gain 的取值为 $[-1,1]$，Hoeffding 半径少了范围因子 2；同一个 batch 内逐 episode 重查 LCB，但 alpha spending 只按 batch look 分配。

此外，探索样本只能对预先固定的 calibration 流有限总体均值作条件无偏估计，不能自动给 scaffold-heldout 流的均值提供覆盖。

## 决定

1. 使用 $r=\min(2,2\sqrt{\log(2/\delta_\ell)/(2n)})$，LCB 下截到 $-1$，不再把半径截到 1。
2. 每 64 个 episode 建立一个确定性 look；batch 起点计算每个 context 的 gate/winner，整个 batch 复用，探索 coin 和 provider 抽样仍逐 episode 进行。
3. 把统计目标写成固定 calibration 流的 $\mu^{\mathrm{cal}}_{c,a}$。BBB scaffold-heldout 只作为 structural retrospective utility evaluation，不把 calibration CI 写成 heldout coverage。
4. 只在分析集、菜单、探索概率和 provider 抽样概率预先固定时使用未加权 exploration-only mean；这些量变化时转为已知包含概率的 HT/IPW 状态或重新定义目标。

## 理由

修正后的半径才覆盖 $[-1,1]$ gain 的真实范围；batch 冻结让 alpha spending 的一次 look 对应一次 gate 评估。目标限定避免把校准流的抽样结论误写成跨 scaffold 的泛化结论。

## 后果

- 统计门更保守，短校准流上可能长期 STOP；这是证据不足的真实结果，不通过缩小半径解决。
- 反馈在 batch 内只影响日志和下一个 batch 的状态，实时性是 batch 级而不是 episode 级。
- 若要支持漂移、动态菜单或即时 gate，需要另行实现 confidence sequence、HT/IPW 或显式的分布漂移模型。

## 证据

- `research/AAMAS2027_ExplorationOnly_JEV_Statistical_Contract_v1.md`
- `research/AAMAS2027_ExplorationOnly_JEV_Statistical_Audit_20260925.md`
- `experiments/logs/exploration_only_finite_population_prefix_audit_v2_20260925/`

