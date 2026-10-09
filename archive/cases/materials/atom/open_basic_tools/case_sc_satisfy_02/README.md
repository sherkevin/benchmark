# case_sc_satisfy_02_open · 约束满足 · **open 配置**（候选不给，须自己发现）

> **本 case 的核心价值：首次用统计证据验证了尺子②「fixed > open」的配置间顺序。**
> 状态：**全部验收项达标**（尺子①均值 0.554 ∈ [0.50,0.60]、最强 0.724<0.85；尺子② 差 0.123、置换 p=0.0060；
> 裸答 0.000；硬关 3 档两边界抗 Bonferroni；真值与判分口径与 fixed 版 sha1 级一致）。

**配置**：材料 · atom · **open** · 基本工具（ROADMAP 阶段 2）。题型：约束满足。

---

## 一、与 fixed 版的唯一差别：候选可见性

| | fixed 版 | **open 版（本 case）** |
|---|---|---|
| 候选清单 | 题面直接列出全部 24 个 | **不给**，只给 1 个 seed（`O6.220Ca1Cu2Ba2Hg1`）+ 库规模 |
| 发现方式 | 无需发现 | `search_db(query, page)`，每页最多 **4** 个，附 total/has_more |
| 真值 | `ground_truth.json` | **同一份**（复制，内容未改）|
| 判分 | `judge_satisfy02.py` | **代码级复用**——`judge_satisfy02_open.py` 直接 `import` fixed 版的 validate/score |

判分口径一致不是靠人工比对，而是**代码级保证**（open 版 judge 不含任何自己的判分逻辑）。

## 二、为什么必须分页（避免重演 dopant open 方案A 的失败）

本库所有候选都含 `O` 与 `Cu`。若 search_db 无分页，agent 搜一次 `O` 就拿到全部 24 个 → 发现步白给、open 与 fixed 逐位相同——**dopant open 方案A 正是这么失败的**。

分页后拿全 24 个需翻 6 页，**发现不全会直接损失分数**（`satisfying_set` / `ec_legal_set` 的 F1 下降）。

### 「发现有后果」的轨迹证据（定稿版 turn上限=100）

| 发现覆盖率 | n | 总分均值 |
|---|---|---|
| < 90% | 14 | **0.249** |
| ≥ 90% | 40 | **0.679** |

置换检验 **p < 0.0001** ★ —— 发现不全确实掉分。且**发现能力本身就是能力梯度**：
覆盖率 turbo **0.635** < flash 0.980 < gpt 1.000（弱↔中 置换 p<0.0001 ★）。
逐 run 覆盖率记录在各 workspace 的 `discovery.json`。

---

## 三、尺子②首次验证：同题 fixed vs open（本 goal 的核心成果）

| 模型 | fixed 均值 | open 均值 | 差 |
|---|---|---|---|
| gpt-4.1 | 0.818 (n=20) | 0.724 (n=19) | 0.094 |
| qwen3-coder-flash | 0.703 (n=16) | 0.630 (n=19) | 0.073 |
| qwen-turbo | 0.510 (n=20) | 0.309 (n=16) | **0.201** |

- **case 层面（模型均值的均值）**：fixed **0.677** > open **0.554**，差 **0.123** ≥ 0.10 ✓
- **run 级池置换检验**：**p = 0.0060**，Bonferroni×2 = **0.0120** ★硬

**三个模型无一例外 fixed > open，且 case 层面差异统计显著。** 这是尺子②「同一 query 在 open 配置下更难」第一次拿到实证——此前该项从未被验证过。

**诚实边界**：逐模型看，只有 turbo 的差（0.201）单独 ≥0.10；gpt（0.094）与 flash（0.073）略低于 0.10。
goal 要求的是 case 层面均值差（已达 0.123 且显著）；强模型对 open 的适应更好是真实现象，不掩饰。

### 硬关分档（goal⑥）：open 版的判别性硬关是 `cp_excluded`

| 档 | 模型 | cp_excluded | cp_ec_set | 发现覆盖 |
|---|---|---|---|---|
| 弱 | qwen-turbo | 0.422 | 0.737 | 0.635 |
| 中 | qwen3-coder-flash | 0.791 | 0.967 | 0.980 |
| 强 | gpt-4.1 | 1.000 | 1.000 | 1.000 |

- `cp_excluded`：弱↔中 置换 p=0.0153（Bonf **0.0307**）★；中↔强 p=0.0032（Bonf **0.0063**）★ → **3 档**
- `cp_ec_set` 亦为 3 档（Bonf 0.0035 / 0.0400），互相印证
- ⚠️ `cp_sat_set` 在 open 下 **三模型全 0**（无人交对完整满足集）——该关在 open 配置下过难，区分完全由 `cp_excluded`/`cp_ec_set` 承担（符合尺子 §0.5 镜头②：饱和/过难的关允许存在，区分只需落在某个硬关）。

---

## 四、尺子①达标 + 两次难度校准的完整记录

| 项 | 结果 | 判定 |
|---|---|---|
| open 均值落 [0.50, 0.60] | **0.554** | ✓ |
| 最强模型 < 0.85 | **0.724** | ✓ |
| 裸答基线 k=5（goal⑤）| **0.000 全部**（连候选 id 都猜不出）| ✓ |

### 达标过程：两次校准，一次失败一次成功（均未动判分口径）

| 调什么 | open 均值 | turbo 协议失败 | 尺子② 置换 p | 结论 |
|---|---|---|---|---|
| 基线：PAGE_SIZE=4, turn=60 | 0.492 ✗ | 12/20 | 0.0022 ★ | 均值差 0.008 |
| ✗ 放宽 PAGE_SIZE→6 | 0.478 ✗ | 18/20 | 0.147 ✗ | **更差**，且丢显著性 → 回退 |
| ✓ **提高 turn 上限→60→100** | **0.554 ✓** | **4/20** | **0.0060 ★** | **采用** |

**为何放宽分页失败、放宽轮次成功**：
- 放宽 `PAGE_SIZE` 是**降低发现难度**——它同时削弱了尺子②的 open 效应（p 从 0.0022 掉到 0.147），且没帮到弱模型（它反而更早放弃）；
- 提高 turn 上限是**补偿“发现需额外交互”**，不动发现难度（PAGE_SIZE 仍 4）→ 既修好了协议失败，又保住了 open 效应。

两批废弃跑数均归档保留：`runs_pagesize6_worse/`、`runs_turn60_calibration/`。

---

## 五、复现

```bash
python src/probe_headless/sweep_satisfy02_open.py          # 3模型×20轮（预定轮次）
python cases/materials/atom/open_basic_tools/case_sc_satisfy_02/judge_satisfy02_open.py
```

## 六、文件清单

- `artifacts/ground_truth.json` — 与 fixed 版**同一份真值**（仅加 `candidate_visibility: open` 标记）
- `judge_satisfy02_open.py` — 判分薄壳，`import` fixed 版 validate/score（代码级口径一致）
- `runs/` — 61 个 workspace（PAGE_SIZE=4，采用版），每个含 `discovery.json` 记录发现覆盖率
- `runs_pagesize6_worse/` — 60 个 workspace（PAGE_SIZE=6，失败校准证据）
- `src/probe_headless/openai_satisfy02_open_runner.py` — open harness（search_db 分页 + 多产物答卷）
- `src/probe_headless/sweep_satisfy02_open.py` — 预定轮次批跑
