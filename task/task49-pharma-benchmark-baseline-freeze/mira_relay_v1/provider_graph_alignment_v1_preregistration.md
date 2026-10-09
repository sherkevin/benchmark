# Provider-Graph Relay v1：held-out residual alignment 诊断预登记

- 日期：2026-10-04
- 状态：执行前预登记；只读诊断，不是效果实验
- 目的：判断 fit-only correctness graph 的正边是否在未参与建图的分子上预测同向 residual，而不是把静态相关误当作可迁移信号
- 输入：Task49 `fingerprint_diagnostic_v1/inputs.json`、同一五折 fit state、同一冻结 encoder
- 不做：不更新策略、不读取未选结果供决策、不修改图参数、不运行 replay、不打开 confirmation、不申请 Nebula

## 1. 唯一问题

对 fit split 中生成的 directed edge $q\to p$，在同一 fold 的 validation rows 上，provider $q$ 的 selected feedback 是否与 recipient $p$ 的 correctness residual 同向？如果没有，Graph Relay 的邻居传播没有进入 replay 的科学依据，应停止该候选。

## 2. 固定对象和信息边界

- 图严格按候选卡：`m_min=128`、`lambda=32`、`k=2`、`tie_tol=1e-12`；只使用 `fold.fit` 的 `status=ok` 与 `system_success` 构图。
- fallback 从 writable target 中排除；source/target 方向固定为 selected $q$ 到 writable recipient $p$。
- residual 只作诊断：用 sealed fit state 的 direct prior、family residual 和 provider bias，在 validation molecule 上计算 frozen pre-update probability $s_p$；不把 validation correctness 写回任何 state。
- 对 validation 行，只有两个 provider 都 `status=ok` 时才计算这条 edge 的 residual product；失败、unknown 和缺失从该 pair 的分母剔除，并按 edge 单独报告有效数。
- 同一输入生成一个 in/out-degree-matched random target control：保留每个 source 的 writable out-degree 和全图 target in-degree 多重集，只随机置换 source-target 配对；随机 seed 固定为 `20261004 + fold`，不使用 validation 数字选边。

## 3. 诊断量

对 provider $p$ 的 correctness indicator $c_p$，令 $\delta_p=c_p-s_p$，令 $h=\lVert x\rVert_2^2$。每条 directed edge 的主量为

$$g_{p,q}=h\,\delta_p\delta_q.$$

它对应候选更新对 recipient BCE 的一阶变化：$-\eta a_{p,q}g_{p,q}$。因此正的 $g$ 才表示该 edge 在该 query 上有局部降损方向。

每个 fold 和 edge set 报告：

1. pairwise valid count；
2. $\mathrm{mean}(g)$ 及其正值比例；
3. graph edges 与 random-control edges 的 molecule-weighted aggregate mean；
4. graph minus random 的绝对差和 sign agreement。

不把 edge 行当独立样本；主汇总按 fold 报告，validation molecule 只用于诊断，不作跨 fold 显著性承诺。

## 4. 继续/停止规则

这是资源闸门，不是论文效果门：

- **继续 fold0 replay**：至少 4/5 folds 的 graph aggregate mean 为正，且至少 4/5 folds 比 random control 高；每个通过 fold 的 primary graph edge 有效数不少于 64，不能靠极少数 edge 支撑。
- **停止候选**：任一条件不满足，保留完整 edge-level 结果，Provider-Graph Relay 不进入 replay，降为 baseline/control；不通过扩大 seed、改变 `k`、`lambda`、`rho` 或换 validation slice 修复。
- 无论结果正负，都不修改主 benchmark、baseline 和论文 claim；它只决定是否值得支付后续 replay 成本。

## 5. 复现和产物

执行前保存 `config.json` 和 source/input hashes；执行后保存 raw edge-level JSONL、`summary.json`、人读报告和独立复算 hash。若脚本或 sealed state 不一致，状态为 `BLOCKED`，不输出 alignment summary。

## 6. 预期解释

- graph > random：只说明 fit-only 图有 held-out transfer signal，仍需 fold0 replay 验证 action-level utility；
- graph ≈ random：说明“有邻居”可能比“相似邻居”重要，候选应降为 control；
- graph < random：说明无条件 correctness graph 误配，停止该候选并保留负结果。
