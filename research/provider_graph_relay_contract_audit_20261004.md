# Provider-Graph Relay v1 contract audit

日期：2026-10-04  
范围：只读审查 `provider_graph_relay_v1_candidate.md` 与 `src/probe_headless/check_provider_graph_relay_v1.py`；未运行效果、未修改代码、未启动实验。

## 结论

草稿目前只能证明“同一实现两次构图得到同一结果，并且一个合成稀疏更新的系数和为 1”。它还不能证明候选 relay 的实现正确、信息边界完整、fallback 不变，或与 MIRA 公平可比。以下 checks 中有几项是空证明或实现镜像；在进入 fold0 replay 前应补齐。

## 逐项审查

### 1. `initial_logits` copy equality：空证明

`run()` 先计算 `base_logits = hidden @ direct_prior.T`，随后执行 `graph_initial_logits = base_logits.copy()`，再检查两者 `np.array_equal`。右侧是左侧的内存副本，结果必然相等；它没有构造 graph policy，也没有证明 graph residual 为零时 policy 的 logits 与 frozen direct logits 相等。应改成独立 graph score 函数（显式加入零 residual、provider bias/fallback 规则）并与 direct logits 做 shape、逐元素 equality；同时检查 fallback logit。

### 2. “独立重算图”：目前是同一函数的双调用

`graph2 = build_graph(...)` 仍调用被审查的 `build_graph`；`manifest != manifest2` 只能发现随机性或可变全局状态，不能发现 Pearson、NA mask、收缩、排序或方向写错。需要一个不复用 `build_graph` 内部函数的 NumPy reference（至少独立计算 overlap、均值、协方差、收缩和 top-k），再比较邻居、overlap、similarity 和 manifest hash。

### 3. fit-only 输入：路径基本正确，但边界未封存

`build_graph` 只接收 `split["fit"]`，并只读取这些行的 `truth/output_statuses/output_labels`；这条数据路径没有主动读 evaluation 行。但脚本没有把该 fit index 列表与 sealed fit-state/config 的 `fit_indices_sha256`、fit 行数、provider roster 和 registry hash 交叉核对，也没有将 fit-row hash 写入 graph manifest。因此它信任 `inputs.folds` 本身，无法发现 split 被换过或图和 fit-state 不匹配。应拒绝缺失/不一致的 fit-index hash，并显式记录每 provider 的有效交集及 NA 计数。

### 4. 相似度方向与归一化：公式方向合理，check 不足

候选定义的是选中 provider `q` 向邻居 `p` 传播，`W_{p,q}=s_{pq}/sum_r s_{rq}`；代码读取 `similarities[p,q]`，与这一列归一化方向一致。`sims / sum(sims)` 也只使用正邻居，因此系数和应为 1。可是 synthetic check 只验证总系数和，没有验证每条边使用的是 source `q` 的列、每个 `W` 与 manifest 数值一致、无重复/自环、非负和有限。应逐边验证 `coeff[p]/rho == manifest similarity[p,q]/column_sum(q)`，并覆盖一个有多个邻居的 provider。

### 5. 随机图 degree：只保持出度，不是完整 degree sequence

`random_neighbours` 对每个 source 保留 `len(ns)`，所以保持 out-degree；它没有保持 in-degree，也没有保存随机图边集或 hash。若“degree-matched”意指无向度或入/出度序列，当前实现不满足。即使只定义为 out-degree，也应把随机 seed、边集、每个 source 的 degree 写入 manifest，并检查无自环、边数、系数质量与候选图一致。当前 check 返回一个布尔值，不能复核具体随机对照图。

### 6. 公平更新：合成 mass check 不能证明公平

`sparse_update_check` 只验证候选一次更新的 `sum(coeff)=1`。这不等价于与 MIRA 的更新预算、logit 变化、状态写入数或 `O((k+1)d)` 相同；MIRA 同时有 family residual 和 provider bias，候选写的是 provider local residual，二者不能仅凭 mass=1 对齐。需要预先定义公平单位（例如 selected feedback 的总传播系数、写入 provider 数、同一 $x\delta$ 的 logit effect），并在 reference policy 上独立计算和报告。

### 7. immutable fallback：完全没有检查

脚本没有实例化 selector、没有 fallback provider 参数，也没有 delayed release/availability/hazard ledger。因而无法证明 fallback 的 score 不含 online graph/local residual，或 fallback 在 provider graph update 后仍可选。应加入 fallback 的 pre/post state snapshot 和同一 query score equality；并在最小 selected-only replay 中验证 failure、unknown、delay release 都不改变 fallback logit。

## 影响结论

在上述缺口修复前，不能把“zero-cost checks passed”解释为 graph relay 可运行或信息边界已证明；也不能据此申请 fold0 effect replay。候选文档中的 opportunity gate、paired action gate 和 delay 结论必须等待真正的 policy/replay receipt。当前唯一可支持的结论是：候选图的静态构图代码在给定输入上可重复调用，且 synthetic example 的传播系数被归一到 1。
