# case_sc_constraint_02 · 约束优化 + 不可测最优（第一组「约束优化 · fixed · basic」合格 case）

> 一句话：在"**恰好 4 种元素**"的硬约束下找 Tc 最高的候选。三层难点叠加——全场最高 127K 是 **5 元素陷阱**；真最优（4 元素 Hg-1201，98K）的 Tc **被工具拒答**（"暂未探测到"）；可测的合法候选最高只有 **39.25K**。必须从可测的 5 元素 Hg-1223 系（121–127K）归纳出"Hg-Ba-Cu-O 体系 Tc 高"，才敢把不可测的 4 元素成员当最优提交。

**配置**：材料 · atom · fixed · 基本工具（ROADMAP 阶段 1）。题型：约束优化。
**设计血缘**：复用 chain_01 已验证的杀手机制（工具拒答 → 逼纯 infer）+ 多产物答卷（防可背）。

---

## 一、v2 → v3：一次必须记住的失败（可背陷阱）

| 版本 | 答卷要求 | 裸答基线（无工具，纯记忆）| 判定 |
|---|---|---|---|
| v2 | 只交 1 个 `final` | gpt-4.1 **0.969 / 0.971** | ❌ **可背**——凭"汞系铜氧化物 Tc 高"的教科书常识直接猜中 4 元素 Hg-1201，无需任何测量或外推 |
| **v3** | 交 5 个产物（见下）| 最高 **0.563**、均值 **0.113**，`cp_measured` **全 0** | ✅ 不可背 |

**根因（普适规律，已写进方法论）**：对照 chain_01——它的答案**同样是** Hg-1201 这种著名材料，裸答却只有 0.279。差别在于 chain 要交 5 个实测派生产物。
⇒ **「不可背」不来自答案难猜，而来自"必须报出大量实测派生的中间产物"。单产物题必然可背。**

v3 答卷（5 个产物，逐个判分）：`final` / `legal_set`（全部合法候选）/ `measured_max_id` + `measured_max_tc`（可测合法最高者及其精确 Tc，**纯记忆答不出**）/ `predicted_tc`。

---

## 二、判分（机器硬判、真值取自离线真库、不采信 agent 自报）

| 关 | 判什么 | 口径 |
|---|---|---|
| `cp_constraint` | 硬约束闸门（二值）| `final` 元素数 == 4；违约束 → **总分 0**（题面明写取消资格）|
| `cp_legal` | 确定性热身关（连续 F1）| `legal_set` vs 真实 13 个 4 元素候选（提供基础分）|
| `cp_measured` | **实测锚点**（连续）| `measured_max_id` 命中 0.5 分 + `measured_max_tc` 容差 2K 内 0.5 分 |
| `cp_objective` | 目标质量（连续）| `final` 的 tc / 98.0 |
| **`cp_obj_grounded`** | **grounded 目标分** | `cp_objective × cp_measured`——目标达成只有被实测支撑时才算数 |
| **总分** | | `cp_constraint × (0.35·cp_legal + 0.65·cp_obj_grounded)` |
| `cp_extrap` | **判别性硬关**（二值，不进总分）| `final` 是否取自不可测集——只有归纳外推成功者才敢交它 |

### 为何目标分必须乘上实测锚点（grounding）——两次失败换来的

落实 `goal.md` §2.1「产物落地验证」：产物数值必须被真实工具调用支撑。设计弯路如下：

| 版本 | 均值（尺子①要 0.60-0.70）| 排序 | 裸答（要 <0.6）|
|---|---|---|---|
| 等权（各 1/3）| 0.785 ✗ 超带 | **turbo 0.810 > gpt 0.786 ✗ 反序** | 0.563 ✓ |
| 提高目标权重（w=0.6）| 0.669 ✓ | gpt 第一 ✓ | **0.759 ✗ 可背** |
| **grounding（本版）** | **0.647 ✓** | **gpt 第一 ✓** | **0.325 ✓** |

- 等权版的病：两个过程关饱和（legal 全 1.000、measured 0.92-1.00）占 2/3 权重 → 抬高并抹平分数；
- 单纯提高目标权重的病：“哪个 4 元素材料 Tc 最高”是汞系常识，**`cp_objective` 本身可背**，权重一高裸答跟着飙；
- **grounding 版的关键**：不是调权重掉分，而是把可背的关变成不可背——裸答 `cp_measured=0` → 目标分归零；真做了实验的 agent 不受影响。

---

## 三、实测结果（3 模型 × **预定 n=20** 一次性检验，杜绝 optional-stopping）

| 模型 | n | 总分均值 | 约束遵守 | cp_legal | cp_measured | cp_objective | **cp_extrap 通过率** |
|---|---|---|---|---|---|---|---|
| gpt-4.1（强）| 19 | **0.732** | 0.84 | 0.993 | 0.917 | **0.737** | **11/19 = 0.58** |
| qwen-turbo（弱）| 20 | 0.629 | 1.00 | 1.000 | 1.000 | 0.429 | 1/20 = 0.05 |
| qwen3-coder-flash（中）| 20 | 0.580 | 0.95 | 1.000 | 0.950 | 0.431 | **0/20 = 0.00** |

（gpt 1 轮协议失败已剔除；gpt 约束遵守 0.84 < 弱模型的 1.00 —— 它更激进、偶尔被 127K 陷阱勾走，这是它换取高 objective 的代价。）

**尺子①对照**：总分均值 **0.647** ∈ [0.60, 0.70] ✓；最强 **0.732** < 0.90 ✓；排序 **gpt 第一、无反序** ✓。

### 统计检验（两个独立指标，均抗 Bonferroni×2）

- **硬关 cp_extrap**：gpt **11/19** vs 弱中池 **1/40** → Fisher **p = 2.75e-06**，Bonferroni×2 = 5.5e-06 ★
- **cp_objective（连续）**：gpt 0.737 vs 弱中池 0.430 → 置换检验 **p < 0.0001**，Bonferroni×2 < 0.0001 ★

### ⚠️ 早期等权版的总分 washout（已修，作反例保留）

等权版的总分排序曾是 turbo 0.810 > gpt 0.786 > flash 0.760，**弱模型反而最高**。因为 `cp_legal`（全 1.000）和 `cp_measured`（0.92–1.00）对所有模型饱和，把唯一有区分的 `cp_objective` 稀释了。
grounding 版修正后排序转正。但教训仍然成立：**对多检查点题，验收必须看逐关（尤其硬关）通过率，不能只看聚合分**（尺子 §0.5 镜头②）。

---

## 四、验收结论

| 验收项 | 证据 | 判定 |
|---|---|---|
| 答案不可背 | 裸答 k=5 最高 **0.325**（grounding 版；等权版 0.563、提权重版 0.759）| ✓ |
| 尺子①均值带 | 总分均值 **0.647** ∈ [0.60,0.70] | ✓ |
| 尺子①最强<0.90 | **0.732** | ✓ |
| 无反序 | gpt-4.1 0.732 > turbo 0.629 > flash 0.580 | ✓ |
| 统计显著能力分档 | cp_extrap Fisher p=2.75e-06（Bonf 5.5e-06）；cp_objective 置换 p<0.0001 | ✓ |
| 判分机器硬判 + 真值一致 | harness 与 judge 同读 `ground_truth.json`；golden 已从 truth 重算校验一致 | ✓ |
| 预定轮次、非 optional-stopping | 开跑前定死 r1–r20；权重修正后用**已有 runs 离线重判**（零额外 API）| ✓ |

**诚实边界**：只有 **2 档**（能外推 vs 不能）。turbo 1/20 与 flash 0/20 不可分，中/弱之间无统计差异。
若要 3 档，需按 chain_01 的做法再引入一个"早关"来分开弱与中（chain 用的是 cpB 筛选关）。

**→ case_sc_constraint_02 定为第一组「约束优化 · fixed · basic」合格 case。**

---

## 五、复现

```bash
python src/probe_headless/sweep_constraint02.py                                   # 3模型×20轮
python cases/materials/atom/fixed_basic_tools/case_sc_constraint_02/judge_constraint02.py
```

## 六、文件清单

- `artifacts/ground_truth.json` — 24 候选 + 真值 + 可测/不可测划分 + golden
- `judge_constraint02.py` — 四关判分器（多产物、连续分）
- `runs/` — 60 个 workspace（3 模型 × 20 轮）
- `runs_v2_singleproduct_archive/` — **v2 单产物设计的 38 个 run，作"可背"反例保留**
- `src/probe_headless/openai_constraint02_runner.py` — harness（OpenAI function-calling）
- `src/probe_headless/sweep_constraint02.py` — 预定轮次批跑

> 前身 `case_sc_constraint_01`（单陷阱、只交 final）20 轮全 1.000——**不合格反例**，不在第一组内。
