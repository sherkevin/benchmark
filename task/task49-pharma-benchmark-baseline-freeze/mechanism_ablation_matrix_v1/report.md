# E3 机制消融矩阵 v1

日期：2026-10-06  
状态：**verified aggregation；development only；不读取 confirmation；不申请 A800**

## 目的

把已经完成的 MIRA veto ablation 收据放入 E3 的统一机器表，回答一个窄问题：在同一 selected-only、同一 feedback clock 和同一 event schedule 下，动态收益来自 runtime-failure ledger、correctness Beta gate，还是二者的组合？

这不是新实验。脚本只读取并聚合已通过独立核验的 300 条 stream 收据：
`experiments/logs/task49_mira_veto_ablation_20260930/run_v1/`。

机器结果：[`result.json`](result.json)；生成器：[`build_mechanism_ablation_matrix_v1.py`](../../../src/probe_headless/build_mechanism_ablation_matrix_v1.py)。四份源文件的 SHA、event counts、full equivalence 和 analysis SHA 都保存在机器结果中。

## 合同

- 5 scaffold folds × 2 protocol slots × 2 scenarios × 3 delays × 5 variants = 300 streams；
- 43,320 decisions、43,320 terminals、43,320 feedback，均通过 verifier；
- variants：Frozen、streak-only、beta-only、full MIRA、matched Online-SGD；
- delay `{0,1,4}`；runtime failure 只在 selected provider 的 release 后更新；未选 provider 不产生反馈；
- no-event 与 controlled-event 分开；不把 controlled event 写成自然 drift。

## 结果

controlled-event 的 post accuracy / target-after-release exposure：

| variant | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen | 0.8485 / 11.92% | 0.8485 / 12.11% | 0.8485 / 11.87% |
| streak-only | 0.8910 / 1.67% | 0.8883 / 2.27% | 0.8883 / 1.54% |
| beta-only | 0.7804 / 18.06% | 0.8054 / 12.39% | 0.7686 / 16.80% |
| full MIRA | 0.8855 / 1.64% | 0.8814 / 1.51% | 0.8759 / 1.18% |
| matched Online-SGD | 0.9036 / 0.00% | 0.8967 / 0.00% | 0.9035 / 0.00% |

no-event 相对 Frozen 的 full MIRA delta 为 `-0.28/-0.42/-0.42 pp`（delay 0/1/4）；streak-only 为 0。controlled-event 中，streak-only 恢复了 full MIRA 的主要收益，beta-only 既没有可靠的 runtime-failure response，还带来明显的 stationary 损失。matched Online-SGD 在三个 delay 的 post accuracy 都高于 full MIRA。

## E3 机制判断

这条收据支持一个明确的机制结论：**在当前受控 failure channel 中，连续 runtime-failure ledger 是主要恢复通道；correctness Beta gate 没有增加事件恢复，反而引入静态误切换。** 因此方法实现应保持 `availability-hard / correctness-soft` 的方向，不能继续把未经校准的 Beta upper bound 当作硬 veto。

它还没有证明 MIRA-Relay 超过 matched Online-SGD。当前结果是机制定位，不是主效果 claim。

## 缺口与闸门边界

| E3 轴 | 状态 | 说明 |
|---|---|---|
| full / remove correctness gate | covered in this scope | full、streak-only、beta-only、Frozen、matched 同 schedule |
| remove runtime channel | covered in this scope | beta-only 是机制对照 |
| remove-state baseline | covered in this scope | Frozen |
| interface removal | **MISSING** | 尚无同 schedule 的 no-update/interface-null receipt |
| independent residual | **MISSING** | 尚无 provider-independent residual 对照 |
| family permutation | **MISSING** | 尚无 family label permutation 收据 |
| paired scaffold uncertainty | **MISSING in this artifact** | 原始 analysis 有 paired stream 点估计，但本聚合器不生成 CI |

所以本矩阵关闭了 E3 的一个真实子轴，并把剩余缺口机器化；它不能单独把 Gate C 改成通过。下一步只需补 interface-null、independent-residual 和 family-permutation 的最小同 schedule CPU 表，不重跑已有 no-event 或 A800。
