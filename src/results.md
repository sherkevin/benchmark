# src —— 测量机器验证报告

- 任务集:`/Users/jingwu/work/benchmark/src/tests/fixtures`,**2** 个任务(阶段数 [4, 3])。
- 参考 agent:oracle, strong, medium, weak, never_asks, random, reversed;每格 12 个种子重复;最大介入预算 6。
- ⚠️ **诚实边界**:agent 为带技能参数的**模拟器**,本报告证明测量机器成立且有区分度,**非真实 LLM 结果**。

## 1. autonomy@0 + 介入-成功率曲线(L0,主条件)

| agent | autonomy@0 | curve b=0..6 | 达标介入数(≥80%) |
|---|---|---|---|
| oracle | 1.00 | 1.00 1.00 1.00 1.00 1.00 1.00 1.00 | 0 |
| strong | 0.67 | 0.67 1.00 1.00 1.00 1.00 1.00 1.00 | 1 |
| medium | 0.12 | 0.12 0.54 0.79 1.00 1.00 1.00 1.00 | 3 |
| weak | 0.00 | 0.00 0.04 0.12 0.25 0.54 0.67 0.75 | >6 |
| never_asks | 0.00 | 0.00 0.04 0.12 0.25 0.54 0.67 0.75 | >6 |
| random | 0.00 | 0.00 0.00 0.00 0.00 0.00 0.00 0.00 | >6 |
| reversed | 0.00 | 0.00 0.00 0.00 0.00 0.00 0.00 0.00 | >6 |

**读法**:oracle→strong→medium→weak 的 autonomy@0 与曲线**单调递降**,机器有区分度;`never_asks` 与 `weak` 曲线**一致**(autonomy@0=0)→ 指标按**成功**而非**沉默**计分,**不奖励莽撞**;`random`/`reversed` 全程 ~0 → 下界正确。

## 2. intervention-conditioned pass^k(τ-bench pass^k 的推广)

| agent | pass^1@b0 | pass^2@b0 | pass^4@b0 | pass^2@b6 |
|---|---|---|---|---|
| oracle | 1.00 | 1.00 | 1.00 | 1.00 |
| strong | 0.67 | 0.49 | 0.34 | 1.00 |
| medium | 0.12 | 0.01 | 0.00 | 1.00 |
| weak | 0.00 | 0.00 | 0.00 | 0.58 |

**读法**:pass^k 随 k **下降**(可靠性更严)、随介入预算 **上升**;把 pass^k 让给 τ-bench 并推广为**介入条件化**。

## 3. 介入守恒:总介入 = 前置(L 档)+ 运行时(固定 cohort、逐实例、无偏)


**weak**:
| L | 前置 pre | 运行时(均) | 总介入 | binding占比 | cohort |
|---|---|---|---|---|---|
| L0 | 0.0 | 3.833 | 3.833 | 1.0 | 18 |
| L1 | 1.0 | 2.833 | 3.833 | 0.94 | 18 |
| L2 | 2.0 | 1.889 | 3.889 | 0.83 | 18 |
| L3 | 3.0 | 1.056 | 4.056 | 0.67 | 18 |

**medium**:
| L | 前置 pre | 运行时(均) | 总介入 | binding占比 | cohort |
|---|---|---|---|---|---|
| L0 | 0.0 | 1.542 | 1.542 | 0.88 | 24 |
| L1 | 1.0 | 0.667 | 1.667 | 0.46 | 24 |
| L2 | 2.0 | 0.208 | 2.208 | 0.21 | 24 |
| L3 | 3.0 | 0.0 | 3.0 | 0.0 | 24 |

**读法**:在固定 cohort(同 task+seed 跨所有 L 都完成)上逐实例计,避免「完成集随 L 漂移」的偏置。**绑定区间**(`binding占比`高,如 weak)总介入近似常数 → 前置 prompt 详细度与运行时求助**同币种、可换算**(守恒);一旦 prompt 详细度**超过**该 agent 所需(`binding占比`塌缩,如 medium 的 L2/L3),总介入随 pre 抬升 → **过量提示=浪费/收益饱和**(印证 PRD「真自主 agent 很快饱和」)。两点合起来正是 wedge:介入是有上限的守恒预算。

## 4. prompt 详细度敏感性曲线(weak)

| L 档 | autonomy@0 | 达标介入数(≥80%) |
|---|---|---|
| L0 | 0.00 | >6 |
| L1 | 0.04 | 6 |
| L2 | 0.12 | 5 |
| L3 | 0.25 | 4 |

**读法**:prompt 越详细(L0→L3),autonomy@0 **升**、所需运行时介入 **降**——前置介入替代了运行时介入(与 §3 守恒一致)。真自主 agent 在 L0 即高、随 L 很快饱和;伪自主曲线陡。

## 5. 许可校准诊断(与自主性正交)

| agent | balanced_acc | precision | recall | 混淆 |
|---|---|---|---|---|
| oracle | 1.0 | 1.0 | 1.0 | tp36/fn0/fp0/tn48 |
| medium | 1.0 | 1.0 | 1.0 | tp36/fn0/fp0/tn48 |
| never_asks | 0.5 | 0.0 | 0.0 | tp0/fn36/fp0/tn48 |

**读法**:well-calibrated agent 恰在高风险节点请示(balanced_acc≈1);`never_asks` 在高风险也莽撞(recall=0,balanced_acc≈0.5)→ 与自主能力**正交**地暴露其鲁莽。许可性中断按 PRD 统一 auto-approve、**不计入**介入预算。

## 6. 单实例 trace(weak,逐预算逐阶段)

```
# task=fixture_drugscreen agent=weak(sigma=3.0) seed=3
[autonomy@0] decompose:✗ screen:✗ admet:✗ decide:✗ → FAIL
        ↪ 触发最低必要介入 D1 指向(对失败阶段给分级介入卡)
[介入#1] decompose:✗ screen:✓ admet:✗ decide:✗ → FAIL
        ↪ 触发最低必要介入 D2 方向(对失败阶段给分级介入卡)
[介入#2] decompose:✗ screen:✓ admet:✓ decide:✗ → FAIL
        ↪ 触发最低必要介入 D3 显式(对失败阶段给分级介入卡)
[介入#3] decompose:✗ screen:✓ admet:✓ decide:✗ → FAIL
        ↪ 触发最低必要介入 D3 显式(对失败阶段给分级介入卡)
[介入#4] decompose:✓ screen:✓ admet:✓ decide:✗ → FAIL
        ↪ 触发最低必要介入 D3 显式(对失败阶段给分级介入卡)
[介入#5] decompose:✓ screen:✓ admet:✓ decide:✗ → FAIL
        ↪ 触发最低必要介入 D3 显式(对失败阶段给分级介入卡)
[介入#6] decompose:✓ screen:✓ admet:✓ decide:✗ → FAIL
        ↪ 触发最低必要介入 D3 显式(对失败阶段给分级介入卡)
```

> 复现:`python run_validation.py --tasks /Users/jingwu/work/benchmark/src/tests/fixtures --seeds 12 --max-budget 6`
