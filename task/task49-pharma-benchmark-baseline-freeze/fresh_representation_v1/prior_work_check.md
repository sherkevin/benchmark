# 后续方向的已有实现核查

2026-10-07。当前A800批次运行期间只读核查，不是另开实验或修改方法。

独立审查者 `/root/headroom_diagnostic_math` 指出：二值工具输出与correctness恢复truth、再更新共享性质头的构造已经在本仓存在，不能重新命名为新发现。

## 可复用资产

- `src/probe_headless/truth_recovered_structured_kernel.py`：共享性质头、条件正确性头、选中输出绑定及释放更新；接口报告 `../composition_20260928/truth_recovered_kernel_v0.md`。
- `src/probe_headless/candidate_par_jev.py`：PAR候选；原数学和可用性审查见 `research/task49_composition_20260928/`。
- 原CPU gate与release队列、failure/unknown测试可复用；旧33工具原型与合同不能替代当前四工具效果证据。

## 数学边界

合法释放后，$\widetilde y=z$（$r=1$）或$\widetilde y=1-z$（$r=0$）。

例：选中工具返回$z=0$，真实化学正确性$r=0$，可恢复$\widetilde y=1$。失败或未知没有该恢复；受控翻转必须使用与$r$绑定的实际terminal输出，不能混用翻转前native输出。

令$q(x)=P(Y=1\mid x)$，$h_{ay}(x)=P(Z_a=y\mid Y=y,x)$，则$s_a(x)=q(x)h_{a1}(x)+(1-q(x))h_{a0}(x)$。

例：$q=0.7,h_{a1}=0.8,h_{a0}=0.6$，该工具分数0.74。恢复的$y$能监督共享$q$，但未选工具的$Z_a$仍不可见，不能凭空补其correctness。全概率分解不要求工具间独立。

若$q$误差≤$\epsilon$、每个$h$误差≤$\delta$且概率合法，则每个分数误差≤$\epsilon+\delta$，贪心选择的期望收益损失≤$2(\epsilon+\delta)$。

例：已成立的界若为0.02和0.03，收益损失界0.10；实际拟合并不自动给出这两个误差保证。用无条件输出边缘代替条件$h$不成立：$q=P(Z=1)=0.5$同时容许$Z=Y$全对及$Z=1-Y$全错。

## 后续取舍

不重新实现上述原型或宣称新颖性。若当前表示结果支持继续，可考虑复用原型，固定条件头、仅比较共享性质头是否在线更新；但须先核对历史效果与当前差异，并与direct-property控制区分。当前仅保留可复用入口，未冻结/启动这个候选，不改变原论文目标或已定方法。
