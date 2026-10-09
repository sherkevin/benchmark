# case_sc_constraint_02_open · 约束优化 · **open 配置**

> **状态**：尺子② **✓ 极强**（fixed 0.647 > open 0.287，差 0.360，置换 p<0.0001，3 模型池）；
> 尺子① **✗ 无解**——该题在 open 下难度分布极端（turbo 0.067 ↔ deepseek 0.992）：
> 3 模型池均值 0.287（低于带），扩到 6 模型池均值 0.592（落带）但最强 0.992（破 <0.85 天花板）。
> **没有任何模型池能同时满足两项**，且靠换池调均值属操纵指标。
> 裸答 0.000 ✓、分档 3 档 ✓、真值口径一致 ✓。
> ⚠️ 另注：open 的 turn 上限 100 vs fixed 40 属未受控变量，但方向使 open 更易，故尺子②结论只会被低估。

**配置**：材料 · atom · **open** · 基本工具（ROADMAP 阶段 2）。题型：约束优化。

---

## 一、与 fixed 版的唯一差别：候选可见性

| | fixed 版 | **open 版（本 case）** |
|---|---|---|
| 候选清单 | 题面列出全部 24 个 | **不给**，只给 1 个 seed + 库规模 |
| 发现方式 | 无需发现 | `search_db(query, page)`，每页最多 **4** 个 |
| 交互轮次 | 40 | **100**（补偿发现所需的额外交互）|
| 真值 | `ground_truth.json` | **同一份**（复制，内容未改）|
| 判分 | `judge_constraint02.py` | **代码级复用**（`import` fixed 版 validate/score）|

## 二、发现有后果（goal②）

| 发现覆盖率 | n | 总分均值 |
|---|---|---|
| < 90% | 18 | **0.151** |
| ≥ 90% | 27 | **0.500** |

置换检验 **p = 0.0008** ★。发现覆盖率分布很宽（0/1/5/9/13/14/15/18/24 都出现过），逐 run 记录在 `discovery.json`。

## 三、尺子②：本 case 的 open 效应是三道里最强的

| 模型 | fixed | open | 差 |
|---|---|---|---|
| gpt-4.1 | 0.732 (n=19) | 0.660 (n=20) | 0.072 |
| qwen3-coder-flash | 0.580 (n=20) | **0.135** (n=20) | **0.446** |
| qwen-turbo | 0.629 (n=20) | **0.067** (n=5) | **0.563** |

- **case 层面（3 模型池）**：fixed **0.647** > open **0.287**，差 **0.360** ✓
- **扩池 6 模型后**：open 均值 0.592（deepseek 0.992 / max 0.987 / plus 0.713 / gpt 0.660 / flash 0.135 / turbo 0.067）
  —— 均值落带但最强破天花板，见顶部状态说明
- **run 级置换检验**：**p < 0.0001**，Bonferroni×2 < 0.0001 ★★

## 四、尺子①未达标的结构性根因（重要方法论发现）

**根因：`cp_objective × cp_measured` 的 grounding 乘法惩罚，与 open 配置叠加成双重惩罚。**

fixed 版为防"猜汞系"引入了 grounding（目标分必须被实测锚点支撑）。在 open 下，模型要先花大量交互发现候选，实测锚点普遍做不好——一旦 `cp_measured` 掉到 0，**目标分整段归零**，总分暴跌：

| 模型 | open 的 cp_legal | open 的 cp_measured | open 总分 |
|---|---|---|---|
| gpt-4.1 | 0.982 | 0.921 | 0.660 |
| coder-flash | 0.755 | **0.111** | **0.135** |
| qwen-turbo | 0.335 | **0.000** | **0.067** |

### 两次校准均证明无法同时满足尺子①与尺子②

| 测量预算 | 尺子① 均值 | 尺子② 置换 p | 副作用 |
|---|---|---|---|
| **20（采用）** | 0.287 ✗ | **p<0.0001 ★** | 弱/中模型崩塌 |
| 30（回退） | 0.415 ✗ 仍不达 | 0.2048 ✗ | **gpt 的 open(0.742) 反超 fixed(0.732)**，open 效应消失 |

放宽预算让强模型在 open 下做得更彻底 → 反超 fixed → 尺子②失效。这与 satisfy_02_open 放宽分页的失败同类：**放宽难度参数必然削弱 open 效应，两者是同一个旋钮**。
故保留 budget=20（优先保住本阶段核心目标尺子②），废弃批归档 `runs_budget30_worse/`。

### 补充证据：软化乘法门也救不了（已用离线扫描排除判分结构假设）

把硬乘法门 `obj × measured` 改为 partial-credit 软门 `obj × (FLOOR + (1−FLOOR)·measured)`
（理由：实测锚点是证据强度，证据不足应打折而非归零），对现有 runs 做离线重判：

| FLOOR | fixed 均值 | open 均值 | 尺子② 差 |
|---|---|---|---|
| 0.0（现行硬门）| 0.647 | 0.287 | 0.360 |
| 0.3 | 0.647 | 0.344 | 0.303 |
| 0.6 | 0.647 | 0.401 | 0.246 |

**即使 FLOOR=0.6，open 均值也只到 0.401，远不到 0.50**；且 fixed 均值完全不变（fixed 下 `cp_measured` 已近 1.0）。
⇒ **排除了“判分结构导致 open 均值低”这一假设**，故未采用软门（保持与 fixed 定稿完全一致）。

**真正根因（剔除分析）**：剔除 turbo 后 open 均值仍只 **0.398**——**中档 flash 也崩到 0.135**
（`cp_legal` 0.755 但 `cp_measured` 仅 0.111）。即本题在 open 下对**中弱模型都过难**：
要先发现全部候选、再逐个测量、再找出最大值，链条比 satisfy_02 更长。
这与 chain_01_open 的根因**不同**（那道剔除 turbo 后就落带 0.558，属弱档模型选错）。

### 替代设计（跨 case 的规律，已验证）

对照 satisfy_02：它的 open 版**达标**（均值 0.554 ∈ 带内）。差别在判分结构：

| case | fixed 版判分结构 | open 版尺子① |
|---|---|---|
| satisfy_02 | **四关加法平均**（实测锚点独立成关）| **0.554 ✓** |
| constraint_02（本 case）| **乘法 grounding**（目标分 × 实测锚点）| 0.287 ✗ |

⇒ **规律：乘法 grounding 与 open 配置不兼容。** 若要 open 版落在预期带内，fixed 版就应把"实测锚点"设计成**独立加法关**而非乘法门（既能防可背，又不会在 open 下造成整段归零）。这条规律已写入尺子文档，供后续造题遵循。

## 五、其余验收项

| 项 | 证据 | 判定 |
|---|---|---|
| 最强模型 <0.85 | 0.660 | ✓ |
| 答案不可背（goal⑤）| 裸答 k=5 **全部 0.000**（连候选 id 都猜不出）| ✓ |
| 分档显著（goal⑥）| `cp_legal` 3 档：弱↔中 Bonf **0.0007**★、中↔强 Bonf **0.0000**★ | ✓ |
| 真值与判分口径一致（goal⑦）| GT 复制自 fixed 版；judge 代码级 `import` | ✓ |
| 预定轮次 | 开跑前定死 r1–r20 | ✓ |

## 六、复现

```bash
python src/probe_headless/sweep_constraint02_open.py
python cases/materials/atom/open_basic_tools/case_sc_constraint_02/judge_constraint02_open.py
```

## 七、文件清单

- `artifacts/ground_truth.json` — 与 fixed 版同一份真值（仅加 `candidate_visibility: open`）
- `judge_constraint02_open.py` — 判分薄壳（`import` fixed 版 validate/score）
- `runs/` — 60 个 workspace（budget=20，采用版），含 `discovery.json`
- `runs_budget30_worse/` — 60 个（budget=30，失败校准证据：尺子②失效）
- `src/probe_headless/openai_constraint02_open_runner.py` — open harness
- `src/probe_headless/sweep_constraint02_open.py` — 预定轮次批跑
