# case_sc_extrap_01 · 超导体趋势外推发现题

> 一句话：给一批候选材料，其中藏着一条"掺杂比例—临界温度"单调变化的同族序列；
> agent 要在干扰项中认出这条序列、看出趋势、并**外推出一个候选清单之外、临界温度更高的新组成**。

本文件夹是一道自包含的 benchmark case：题面、真值、判分器、判分标准与流程、
一次真实 agent 作答的完整轨迹、以及"本题测什么能力"，全部齐备、可复现。

---

## 一、这道题测 agent 的什么能力

被测能力对应 `research/agent_action_全量枚举.md` 的 infer 动作编号（材料领域）。
本题**一题锚定三个深推理能力**，且天然规避"暴力枚举"与"靠运气"：

| checkpoint | 对应 action | 测的是 | 为什么这题非它不可 |
|---|---|---|---|
| cp1 序列归类 | **#8 family 归类判断** | 从 20 个候选里认出哪 8 个是同一掺杂序列 | 其余 12 个是他族干扰，蒙不出来 |
| cp2 趋势查证 | **#5 同族 Tc 趋势** | 真测序列成员、掌握 Tc 随成分的走向 | 不测就没有趋势可外推 |
| cp3 外推 | **#2 比例调整** | 顺趋势提出候选清单**之外**的新组成 | 答案不在候选里，只能算出来 |
| cp4 命中 | （#5+#2 的落点） | 提出的新组成确实达标（Tc≥目标） | 库内可验证，canonical 精确比对 |

**规避的两个陷阱**：
- **无"全查"捷径**——答案不在候选清单里，逐个测候选也测不到它，必须靠趋势外推。
- **不靠运气**——只要趋势方向判对，顺着调比例就能命中；方向错则再多尝试也无用。
  （对比：约束优化题"预算不足挑哪个测"落在 #40 决定优先级，那是策略/运气、项目明确不测。）

---

## 二、题目怎么构造的（数据地基）

- 领域：超导材料，铜氧化物（Cuprate）。
- 序列来源：全库扫描找 Tc 随某元素比例**单调**的同族序列，命中 `B-C-Cu-O-Sr`
  碳掺杂系列——spearman ρ=1.000（完美单调）、10 个成员、近似线性 `tc ≈ -190·C + 196`。
- 展示集（给 agent 看）：序列里 Tc<30 K 的 8 个成员（C=0.91→0.985，Tc 26.5→0）+ 12 个他族干扰，共 20 个。
- 答案集 g_b（藏起来、库内可验证）：序列里 Tc≥30 K 的成员 = `B0.150C0.850O5Cu1Sr2`(32 K)、`B0.140C0.860O5Cu1Sr2`(31 K)。**命中任一即算对。**
- 所有数值来自离线真库（`dag_poc/data/backend`），无编造；匿名标签已按
  `src/probe_headless/deanonymize.py` 还原为真实规范化组成式。

真值文件：[`artifacts/ground_truth.json`](artifacts/ground_truth.json)（含 visible_set / series_members_gt / g_b / 每个候选的 tc·element_count·family 真值）。

---

## 三、判分标准与流程

**判分是硬判定**：读真值 + agent 的真实工具调用记录，机器算分，**不使用 LLM judge、不采信 agent 自报值**（真值一律从 tool call 记录反查）。判分器：[`judge.py`](judge.py)。

### 四个 checkpoint 的判据

- **cp1 序列归类**（权重 1，阈值 1.0）：`min(1, 真测过的序列成员数 / evidence_k)`，evidence_k=4。
- **cp2 趋势查证**（权重 2，阈值 0.75）：`真测过的序列成员数 / 序列总数`（覆盖足够跨度才谈得上趋势）。
- **cp3 外推**（权重 1，阈值 1.0）：提交组成**不在展示候选集里** = 1，否则 0（外推的定义就是提出清单之外的点）。
- **cp4 命中**（权重 3，阈值 1.0）：提交组成 ∈ g_b（canonical 相等）= 1，否则 0。

### 总分（门控式）

$$\text{process} = \frac{\sum_k w_k \cdot s_k}{\sum_k w_k}, \qquad
\text{total} = \begin{cases} 0.5\cdot\text{process} + 0.5 & \text{全部 cp 过阈值} \\ \min(0.3,\ 0.5\cdot\text{process}) & \text{否则} \end{cases}$$

其中：$w_k$ 是 cp 权重（1/2/1/3），$s_k$ 是该 cp 得分，$\text{process}$ 是加权平均；
**门控**指"每个 cp 都过各自阈值"才算通关，通关基线 0.5、满分 1.0；**任一 cp 不过则总分封顶 0.3**——防止"部分做对"被算成高分。

### 复现判分

```
cd cases/case_sc_extrap_01
python3 judge.py
```

---

## 四、agent 作答轨迹（一次真实运行）

模型 qwen3.7-max，Claude Code 无头模式，**纯净工具隔离**（只暴露 3 个基本工具
`literature_search`/`get_property`/`submit_answer`，内置工具全禁，避免读文件作弊）。

- 完整流：[`trace/agent_stream.jsonl`](trace/agent_stream.jsonl)
- 可读版（思考/调用/结果逐条）：[`trace/agent_readable.txt`](trace/agent_readable.txt)
- 工具调用账本（判分依据）：[`trace/tool_calls.jsonl`](trace/tool_calls.jsonl)

**这次的判分结果**（`python3 judge.py` 实跑）：

| cp | 得分 | 说明 |
|---|---|---|
| cp1 序列归类 | 1.0 | 测到全部 8 个真序列成员 |
| cp2 趋势查证 | 1.0 | 序列成员全覆盖 |
| cp3 外推 | 1.0 | 提交 `B0.150C0.850O5Cu1Sr2`，不在候选清单内 |
| cp4 命中 | 1.0 | ∈ g_b（Tc=32≥30）|
| **总分** | **1.0** | 通关 |

**过程证据（trace 可查）**：agent 先测遍序列成员、写出趋势 "Tc increases monotonically
with B doping ratio"，再朝"提高 B"方向试了 5 个库外组成（B=0.10/0.11/0.095/0.12/0.20，
均返 NOT_FOUND），最后落到库内真实存在的 `B0.150C0.850`。**单向定向探索** = 理解趋势的硬证据。

---

## 五、文件清单

```
case_sc_extrap_01/
├── README.md                    本文件：能力映射 + 判分标准与流程 + 复现
├── query.txt                    给 agent 的题面（全英文）
├── judge.py                     判分器（硬判定，可执行）
├── artifacts/
│   ├── ground_truth.json        真值：候选/序列/答案集/逐候选性质
│   └── mcp_server.py            本题的 3 工具 MCP server（含数据与 NOT_FOUND 兜底）
└── trace/
    ├── agent_stream.jsonl       agent 完整流式记录（请求/思考/调用/结果）
    ├── agent_readable.txt       上者的人可读整理版
    └── tool_calls.jsonl         server 端工具调用账本（判分唯一依据）
```

## 六、难度调节（备注，非本 case 改动）

本题 agent 满分，因序列线性度极高（ρ=1.0）、外推跨度小。难度**连续可调**：降低序列
线性度（加噪声）、拉大外推跨度、增加同形近似干扰序列（考验 #8 更细的区分），即可
造出不满分的同型题。本 case 定位为该题型的**可复现基线原型**，不是难度上限。
