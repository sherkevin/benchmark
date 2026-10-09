# src —— 测量机器验证报告

- 任务集:`tasks`,**4** 个任务(阶段数 [4, 3, 3, 3])。
- 参考 agent:oracle, strong, medium, weak, never_asks, random, reversed;每格 20 个种子重复;最大介入预算 8。
- ⚠️ **诚实边界**:agent 为带技能参数的**模拟器**,本报告证明测量机器成立且有区分度,**非真实 LLM 结果**。

## 1. autonomy@0 + 介入-成功率曲线(L0,主条件)

| agent | autonomy@0 | curve b=0..8 | 达标介入数(≥80%) |
|---|---|---|---|
| oracle | 1.00 | 1.00 1.00 1.00 1.00 1.00 1.00 1.00 1.00 1.00 | 0 |
| strong | 0.74 | 0.74 0.99 1.00 1.00 1.00 1.00 1.00 1.00 1.00 | 1 |
| medium | 0.15 | 0.15 0.62 0.89 0.97 0.99 1.00 1.00 1.00 1.00 | 2 |
| weak | 0.00 | 0.00 0.07 0.20 0.41 0.62 0.74 0.84 0.90 0.95 | 6 |
| never_asks | 0.00 | 0.00 0.07 0.20 0.41 0.62 0.74 0.84 0.90 0.95 | 6 |
| random | 0.00 | 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 | >8 |
| reversed | 0.00 | 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 | >8 |

**读法**:oracle→strong→medium→weak 的 autonomy@0 与曲线**单调递降**,机器有区分度;`never_asks` 与 `weak` 曲线**一致**(autonomy@0=0)→ 指标按**成功**而非**沉默**计分,**不奖励莽撞**;`random`/`reversed` 全程 ~0 → 下界正确。

## 2. intervention-conditioned pass^k(τ-bench pass^k 的推广)

| agent | pass^1@b0 | pass^2@b0 | pass^4@b0 | pass^2@b8 |
|---|---|---|---|---|
| oracle | 1.00 | 1.00 | 1.00 | 1.00 |
| strong | 0.74 | 0.55 | 0.32 | 1.00 |
| medium | 0.15 | 0.02 | 0.00 | 1.00 |
| weak | 0.00 | 0.00 | 0.00 | 0.90 |

**读法**:pass^k 随 k **下降**(可靠性更严)、随介入预算 **上升**;把 pass^k 让给 τ-bench 并推广为**介入条件化**。

## 3. 介入守恒:总介入 = 前置(L 档)+ 运行时(固定 cohort、逐实例、无偏)


**weak**:
| L | 前置 pre | 运行时(均) | 总介入 | binding占比 | cohort |
|---|---|---|---|---|---|
| L0 | 0.0 | 4.013 | 4.013 | 1.0 | 76 |
| L1 | 1.0 | 3.013 | 4.013 | 0.92 | 76 |
| L2 | 2.0 | 2.092 | 4.092 | 0.79 | 76 |
| L3 | 3.0 | 1.303 | 4.303 | 0.57 | 76 |

**medium**:
| L | 前置 pre | 运行时(均) | 总介入 | binding占比 | cohort |
|---|---|---|---|---|---|
| L0 | 0.0 | 1.375 | 1.375 | 0.85 | 80 |
| L1 | 1.0 | 0.525 | 1.525 | 0.38 | 80 |
| L2 | 2.0 | 0.15 | 2.15 | 0.11 | 80 |
| L3 | 3.0 | 0.037 | 3.038 | 0.03 | 80 |

**读法**:在固定 cohort(同 task+seed 跨所有 L 都完成)上逐实例计,避免「完成集随 L 漂移」的偏置。**绑定区间**(`binding占比`高,如 weak)总介入近似常数 → 前置 prompt 详细度与运行时求助**同币种、可换算**(守恒);一旦 prompt 详细度**超过**该 agent 所需(`binding占比`塌缩,如 medium 的 L2/L3),总介入随 pre 抬升 → **过量提示=浪费/收益饱和**(印证 PRD「真自主 agent 很快饱和」)。两点合起来正是 wedge:介入是有上限的守恒预算。

## 4. prompt 详细度敏感性曲线(weak)

| L 档 | autonomy@0 | 达标介入数(≥80%) |
|---|---|---|
| L0 | 0.00 | 6 |
| L1 | 0.07 | 5 |
| L2 | 0.20 | 4 |
| L3 | 0.41 | 3 |

**读法**:prompt 越详细(L0→L3),autonomy@0 **升**、所需运行时介入 **降**——前置介入替代了运行时介入(与 §3 守恒一致)。真自主 agent 在 L0 即高、随 L 很快饱和;伪自主曲线陡。

## 5. 许可校准诊断(与自主性正交)

| agent | balanced_acc | precision | recall | 混淆 |
|---|---|---|---|---|
| oracle | 1.0 | 1.0 | 1.0 | tp80/fn0/fp0/tn180 |
| medium | 1.0 | 1.0 | 1.0 | tp80/fn0/fp0/tn180 |
| never_asks | 0.5 | 0.0 | 0.0 | tp0/fn80/fp0/tn180 |

**读法**:well-calibrated agent 恰在高风险节点请示(balanced_acc≈1);`never_asks` 在高风险也莽撞(recall=0,balanced_acc≈0.5)→ 与自主能力**正交**地暴露其鲁莽。许可性中断按 PRD 统一 auto-approve、**不计入**介入预算。

## 6. 单实例 trace(weak,逐预算逐阶段)

```
# task=drug_admet agent=weak(sigma=3.0) seed=3
[autonomy@0] decompose:✗ predict_logS:✗ predict_logD:✗ decide_herg_safe:✓ → FAIL
        ↪ 触发最低必要介入 D1 指向(对失败阶段给分级介入卡)
[介入#1] decompose:✗ predict_logS:✓ predict_logD:✗ decide_herg_safe:✓ → FAIL
        ↪ 触发最低必要介入 D2 方向(对失败阶段给分级介入卡)
[介入#2] decompose:✗ predict_logS:✓ predict_logD:✓ decide_herg_safe:✓ → FAIL
        ↪ 触发最低必要介入 D3 显式(对失败阶段给分级介入卡)
[介入#3] decompose:✗ predict_logS:✓ predict_logD:✓ decide_herg_safe:✓ → FAIL
        ↪ 触发最低必要介入 D3 显式(对失败阶段给分级介入卡)
[介入#4] decompose:✓ predict_logS:✓ predict_logD:✓ decide_herg_safe:✓ → 任务 PASS ✅
→ weak 在第 4 次介入后完成;autonomy@0=未通过;达标介入数=4。
```

> 复现:`python run_validation.py --tasks tasks --seeds 20 --max-budget 8`
