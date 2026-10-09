# The Playground 判分机制拆解：从题面到分数的完整链路

> 取证日期 2026-07-31，浏览器实地拉取真实数据，全程只读。
> 本文回答七个具体问题：题目分几类、agent 交什么、工具怎么调、复现脚本是什么、"跑正向模型"什么意思、藏起来的答案怎么核验、检查点怎么设计。
> 每条都配真实数据或原文。另有一道题的题面全文留档（12,179 字）：[真题原文_SSNP-ODT.md](./Playground平台造题与判分机制/真题原文_SSNP-ODT.md)

---

## 取证结论（owner 定性，2026-07-31）

**这个平台不能为我方的中间过程量评测提供参考。三条理由：**

**第一，它没有测中间过程量。** 它的轨迹分（trace score）测的是**轨迹本身的合法性与真实性** —— 步骤类型合法吗、调用与返回配对吗、时间戳在窗口内吗、文件修改时间对得上吗、花钱超过一分钱吗。**没有任何一条在判"这一步的功能实现效果对不对"。**

**第二，它的所谓"检查点"是终态输出的字段拆细。** 以 APOE 题为例：11 个检查点全部是最终答案 JSON 里的字段（3 个频率 + 3 个标准差 + 5 个百分比），**属于过程的为 0 个**。即使那些标 `Role: scored` 的步骤，也是一对一绑定一个输出文件、只看文件内容；唯一采集过程数据的地方（光学题的 `loss_history.csv`，记录每轮迭代失配），其判分策略是 `structural_audit` —— **只查列名齐不齐，不看下降曲线合不合理**。

**第三，任务形态与我方根本不同。** 全站 6,405 道题都是"把论文里的某个计算重做一遍"，交付物本质上是**代码及其跑出的数**；题面把方法、参数、公式全给死（实测 20 道抽样题，16 道明确指定方法、17 道给具体参数），**agent 不需要做科学决策**。而我方是材料发现与药物发现：只给约束与目标，不给方法，agent 必须自己决定查什么、怎么收敛。

**一句话：它在验"这份记录是不是真的"，我们要判"这些决策是不是对的"。两件事。**

> 根因在信任模型：它的 agent 跑在选手自己的机器上（官方工具 `trace_record` 的 README 明说打包、写代码、跑实验、上传全是 agent 自己的责任），平台收到的是**选手自报的记录**，所以必须先验真伪。我方 agent 经 MCP 调我们的工具，trace 由我们自己落盘，**不存在伪造问题，所以那五条校验对我方零参考价值**。中间过程量评测的参考对象另见：[中间检查点构建方法调研.md](./中间检查点构建方法调研.md)。

---

## 零、总分公式（后面所有细节都挂在这条上）

$$\text{总分} = \text{harbor\_reward} \times 100 \times \text{trace\_factor}$$

**其中：**
- **harbor_reward** —— 取值 $[0,1]$，**结果对不对**。由隐藏验证器读 agent 交的输出文件、逐项检查后按权重合成。
- **trace_factor** —— 取值 $[0,1]$，**过程可信不可信**的折扣系数。由过程记录质量分换算，记录不可信时归零。
- **相乘** —— 所以**结果算对了但过程记录不可信，总分是 0**。

在 5 份真实作答上逐条复算，5/5 成立：

| 作答 | harbor_reward | trace_factor | 算出 | 平台给分 |
|---|---|---|---|---|
| 19415 | 1 | 1 | 100 | **100** |
| 20040 | 0.8 | 1 | 80 | **80** |
| 19583 | 0.8 | 0.49 | 39.2 | **39.2** |
| 19586 | 0.8 | **0** | 0 | **0** |
| 20039 | 0.8 | **0** | 0 | **0** |

判分由两个独立程序做，字段里写明：`scored_by: "playground-worker/harbor-lbg+trace-score-cli"` —— 前者管结果，后者管过程。

---

## 一、题目分几类

**这个平台上所有题都是同一类：复现一篇已发表论文的计算结果。** 差别只在学科。

实测学科分布（`GET /api/challenges/facets`，全站 6,405 题、13,774 次作答）：

材料 1413 · 物理 1309 · 生物 955 · 流体力学 594 · 数值分析 70 · 热工 62 · 传热 52 · 非线性动力学 47 · 数学 45 · 宇宙学 36 · 基因组学 35 · 燃烧 33 · 信号处理 32 · 分子动力学 30 · 晶体学 29 · 量子场论 29 ……

一共 **695 个学科标签**，长尾很长。

**关键点：没有"产出一个真实材料/分子"的题。** 因为题的本质是"把论文里那个计算重新做一遍，看你能不能算出同样的数"。所以：

- 材料类题目 → 交计算出来的**能量、径向分布函数、配位数**
- 光学类题目 → 交重建出来的**三维数组**
- 流体类题目 → 交算出来的**压降值和最优几何参数**

**产出永远是数据，不是实物，也不是代码本身。**

### 材料与生物题是不是都得写代码

**结论：几乎都得写代码，但交的不是代码。** 实测两个领域各抽几道有作答的题：

| 领域 | 题目 | 要交什么 | 判分策略 |
|---|---|---|---|
材料 | 液态氯仿的蒙特卡罗模拟（207 次作答） | 每分子平均势能 + 碳碳径向分布函数曲线 + 0.64nm 配位数，共 3 个文件 | `reference_match` |
材料 | 铜中排斥型位错交互的分子动力学模拟（207 次） | 模拟算出的物理量 | — |
材料 | 有限时域竞争毒化平衡估算（211 次） | 估算出的平衡量 | — |
生物 | 图信息对比学习做空间域识别（12 切片 DLPFC 复现，164 次） | 聚类指标结果 | `threshold_or_better` |
生物 | APOE 基因频率与表型百分比 | 一个 JSON（6 个频率/标准差 + 5 个百分比） | `exact_match` |

**两个领域的共同点：题目本身就是“把论文里那个计算重新做一遍”。** 所以：

- **必然要写代码** —— 蒙特卡罗模拟、分子动力学、对比学习、基因频率统计，都得写程序才算得出来；
- **但交的是程序算出的数** —— 能量、频率、配位数、聚类指标；
- **代码作为“证据”一并交上去**（第四节），用来证明这些数是算出来的、不是编的。

**没有“交一块材料”或“交一个分子实物”的题** —— 因为全站题目都是论文复现，而论文里能被复现的只有计算结果。

---

## 二、agent 最终交什么，JSON 模板是谁定的

**模板是题面预先给定的，精确到字段名、类型、单位。agent 只负责把数填进去。**

### 例一：流体题，交四个数

题目《排气歧管压降优化》，题面里的契约原文：

> Output file: `/app/outputs/optimum_geometry.json`
> Contract: `{"bending_angle_deg": integer, "bending_radius_mm": integer, "pipe_diameter_mm": integer, "minimum_pressure_loss_kPa": float}`

agent 最终交的就是（真实作答 19415）：

```
{"bending_angle_deg": 30, "bending_radius_mm": 40,
 "pipe_diameter_mm": 34, "minimum_pressure_loss_kPa": 10.806}
```

四个数，没有代码。

### 例二：材料题，交三个文件

题目《液态氯仿的蒙特卡洛模拟》，题面给的契约（原文摘录）：

> ### step_02_avg_energy.txt
> - required: `average_potential_energy_per_molecule`, `standard_error`
> - units: 两者均为 kJ/mol
>
> ### step_03_rdf.json
> - required: `r_values`, `g_CC`, `first_peak_position`, `coordination_number_0_64`
> - `r_values`: list of float (nm), strictly increasing, covering the whole first shell
> - `g_CC`: list of float, same length as `r_values`, normalised so g_CC → 1 at large r
> - `first_peak_position`: float (nm), **must agree with the maximum of the submitted `g_CC` to within 0.015 nm**
> - `coordination_number_0_64`: float, average number of other molecules whose centres lie within 0.64 nm of a given molecule's centre

**这就是"材料类题目怎么交"的答案**：交的是模拟算出来的物理量 —— 每个分子的平均势能、碳碳径向分布函数曲线、第一峰位置、0.64 纳米内的配位数。**不是交一块材料。**

注意 `first_peak_position` 那条要求，后面第七节会专门讲，它是个很妙的设计。

### 模板是预先给定的，而且给两遍

题面里同一份契约会出现两次：一次在「Workflow steps」里（告诉你哪一步交），一次在「Output contract」节（给完整 schema 和单位）。第二遍还附一份**机器可读的 JSON 副本**，供 agent 写脚本自查。

所以答案是：**是预先给定的，agent 没有自由发挥的空间**。文件路径、文件名、字段名、类型、单位全部写死。

---

## 三、agent 怎么调工具（5 万工具在哪里）

**官方赛事文案写的是“支持参赛人使用 170 多种编程语言、50000 多类科学工具”。** 这个数字在官网宣传文案里，不在站内页面上 —— 站内页面只能搜到 `6405 CHALLENGES` 与 `13774`（作答数）。

### 那 5 万工具指的是什么

**是 agent 可用的计算环境，不是平台给的 API 接口。**

这两者的区别用一个对比就能说清：

| | 他们（计算环境） | 我们（API 接口） |
|---|---|---|
| agent 拿到什么 | 一台装好了大量科学软件的机器 | 一份工具说明书（可调的函数清单） |
| agent 怎么用 | 自己写代码、自己调用本地软件 | 发一个调用请求，我们返回结果 |
| “工具”是什么 | Python、OpenFOAM、LAMMPS、R 包……这些**安装在机器上的软件** | `search_candidates`、`get_entity` 这些**我们写的函数** |
| 平台看得见调用过程吗 | **看不见**，只能事后看提交的记录 | **看得见**，每次调用当场落盘 |
| “5 万类”怎么数出来的 | 环境里可用的科学软件包总数量级 | — |

**打个比方：**

- 他们的做法像“给你一间配齐了仪器的实验室，你自己动手做，做完把实验记录和数据交上来”。他们不在现场盯着。
- 我们的做法像“你隔着窗口提需求，我们代你操作仪器并把读数递给你”。你每提一次，我们都记一笔。

依据是官方文案自己的措辞：“支持参赛人使用……**按题目提供基础算力支持**，支撑选手灵活调度 Agent，自由组合出属于自己的解题工作流”—— “算力支持”和“自由组合工作流”这两个词说明给的是环境、不是接口。题面里也明说“可以联网装包”，如果是固定的 API 清单就不存在“自己装包”这回事。

**为什么这个区别对我们重要：它直接决定了判分能拿到什么证据。** 他们看不见过程，所以必须搞一套十六条反作弊规则去事后验真（第七节）；我们能看见每次工具调用，所以可以直接拿调用记录反查真值。

**站内“Tools”那个板块是另一回事，它不是这 5 万个工具。** 它的真实数量是 **Skills 33 个 · Agents 38 个 · Datasets 23 个**，定位写在页面上：

> Build, share, and install reusable capabilities for scientific computing.
> （构建、分享、**安装**可复用的科学计算能力。）

> One-line install + local pre-flight for **Claude Code / OpenCode / Cursor / OpenClaw** users.
> （给 Claude Code / OpenCode / Cursor / OpenClaw 用户的一行安装 + 本地预检。）

拉一个 skill 看实体（`/reproduce-paper`，作者 ASURF Team）：

> Orchestrate end-to-end paper reproduction through 5 phases: preparation (parse paper, identify figures), parameter extraction (conditions, models, numerics), independent reference validation, target computation, and comparison with quantitative error metrics.

**这是一份提示词流程包**，教 agent 按五个阶段做论文复现，不是可调用的计算接口。官方文案里“优质解题思路将自动生成 Skill”说的就是这东西 —— 它是**赛后沉淀的经验包**，不是解题时调的工具。

### 关键区别：agent 跑在哪里

**agent 跑在答题人自己的环境里，平台只收提交物、事后验真。**

官方那个轨迹录制工具 `trace_record` 的 README 写得很直白，它把责任划得很清：

> 把 **BohrClaw / arkclaw** 的 session 转成 ARM v1.1 `trace.jsonl`……打包 bundle、写代码、跑实验、上传等所有其他事情都是 **agent 的责任**。

它的“边界声明”还明列了不做什么（原文）：不打包 bundle、不写 `src/reproduce.py`、不跑实验、不写 `characterization.json`、不帮你 POST —— **全部是 agent 自己的活**。

完整链路是：agent 在本地干完活 → 用 `trace-submit` 把 session 转成规范轨迹 → 自己组装 ARM 包（代码/日志/结果/轨迹/对比报告）→ zip 上传 → 平台事后验真。

**这跟我们的做法根本不同。** 我们是 agent 通过 MCP 调我们提供的工具，每次调用我们当场就看得见；他们是 agent 在外面干完活、把结果和过程记录打包交上来。

**这直接解释了他们为什么要把轨迹分做成十六条反作弊规则（第七节）** —— 过程不在他们眼皮子底下发生，只能靠事后查。

### 一个顺带挖到的细节：为什么“开源模型更容易拿高轨迹分”

官方评分细则里有一句：“开源模型思考链路完整，更容易拿高分；闭源模型也可正常参赛，但部分模型推理内容极少，轨迹分可能会偏低。”

原因在录制工具的 README 里找到了：轨迹里会带 **完整的 `reasoning_content`（模型的推理内容）**。开源模型把推理过程完整吐出来，闭源模型往往只给结果不给推理 —— 而第七节那些检查里有好几条是直接看“记录里有没有真实的思考与执行痕迹”的。

另外，这份原始推理记录（`raw_messages.jsonl`）上传后会被平台存档，README 原话：**“被存档供赛后 AI for Science 模型训练研究使用”**。但它也实测声明该文件**对评分零影响**，且不想被收集就可以不放进包里。

---

## 四、"复现脚本"是什么

**是 agent 自己写的那段算出答案的程序，作为证据交上去，不是作为答案。**

协议规定提交包里必须有三样东西（`GET /api/protocol` 的 `modalities.required`）：

- `src/reproduce.py` —— 复现脚本
- `execution/run.log` —— 真实运行日志
- `execution/results/*` —— 产出目录

**脚本的作用是"证明这个数是算出来的，不是编的"。** 答案本身是第二节那些 JSON/txt 文件里的数。

### 材料类题目怎么交脚本

跟其他题一样交。因为材料类题目也是"用蒙特卡洛/分子动力学算出某个物理量"，本身就是计算任务，自然有脚本。

举例：那道氯仿题要交的是模拟算出的平均势能。agent 必须写一段跑 NVT 蒙特卡洛的程序，跑完把能量写进 `step_02_avg_energy.txt`。程序是脚本，能量值是答案。

**这个平台上不存在"没有计算过程的题"** —— 全部是论文复现，所以全部有脚本。

---

## 五、"跑一遍正向模型"是什么意思

这是那道光学题的说法。**正向模型指"从原因算结果"的那个计算，反过来叫反演。**

具体到那道题：

- **反演（agent 要做的）**：给你测到的光场 → 反推样品内部的三维折射率分布。这是难的方向。
- **正向（判分器要做的）**：给定一个折射率分布 → 算出它应该产生什么样的光场。这是容易的方向，因为物理规律是确定的。

判分器的做法（题面原文）：

> The verifier reloads your volume and runs the forward model itself, so the misfit it scores is recomputed from your reconstruction rather than read from your report.

翻译：判分器把 agent 交的那个折射率体积重新载入，**自己跑一遍正向模型**，算出"这个体积应该产生什么光场"，然后跟真实测量数据比 —— 差得越少，说明反演得越对。

**为什么这样判分很硬**：agent 报的失配值可以随便写，但判分器自己正着算一遍，就绕不过去了。这叫"用容易的方向验证难的方向"。

生活类比：让你从一堆零件的照片反推组装图（难），验证的时候不看你说的，而是照你的组装图真的装一遍，看装出来跟照片像不像（容易且不可作假）。

---

## 六、藏起来的答案怎么核验，举例

用那道氯仿蒙特卡洛题，它的判分说明是我见过写得最完整的。**原文全引：**

> A hidden verifier scores the three output files you write. Most checks read a reported value and compare it against a reference derived from a correct stochastic Monte Carlo run, using a tolerance that accounts for run-to-run variation and degrades smoothly rather than in steps — being slightly outside a tolerance costs you part of that check, not all of it. The heaviest single check is the average potential energy per molecule. The structural check is different in kind: the peak position you report is cross-checked by the verifier against the `r_values`/`g_CC` arrays you submit, so those arrays must be a genuine g(r) consistent with the scalar you quote. The error bar is graded on its scale rather than against an exact figure. The check scores are combined by weight into a final reward in [0,1]. Submitting fabricated numbers without actually performing the simulation is detectable and will not earn full credit.

拆成六件事：

**1. 参考答案是怎么来的。** 不是人手填的标准答案，是"**一次正确的蒙特卡洛运行**"跑出来的结果（原文 `a reference derived from a correct stochastic Monte Carlo run`）。也就是出题方自己先做了一遍。

**2. 容差是平滑的，不是一刀切。** 原文 `degrades smoothly rather than in steps — being slightly outside a tolerance costs you part of that check, not all of it`。稍微超出容差只扣这一项的一部分分，不是整项归零。而且容差专门考虑了蒙特卡洛是随机算法、每次跑结果本来就有波动。

**3. 各项权重不等。** 原文 `The heaviest single check is the average potential energy per molecule` —— 最重的单项是每分子平均势能。

**4. 误差棒按量级判。** 原文 `The error bar is graded on its scale rather than against an exact figure` —— 只看你报的误差是不是在合理量级，不要求精确对上。这是合理的，因为误差本身有随机性。

**5. 各项按权重合成为 $[0,1]$ 的 reward。** 原文 `The check scores are combined by weight into a final reward in [0,1]`。**这就是第零节那个 `harbor_reward` 的来源。**

**6. 编数字查得出来。** 原文 `Submitting fabricated numbers without actually performing the simulation is detectable and will not earn full credit`。

### 最妙的一条：不用标准答案也能查出你在编

第 6 点靠什么实现？靠**自洽性检查**。看契约里这条要求：

> `first_peak_position`: float (nm), **must agree with the maximum of the submitted `g_CC` to within 0.015 nm**

意思是：你要交一条径向分布函数曲线（`r_values` 和 `g_CC` 两个数组），同时还要报一个"第一峰的位置"。**判分器会去你交的那条曲线上找最大值，看跟你报的峰位是否吻合到 0.015 纳米以内。**

判分说明里也点了这件事：

> the peak position you report is cross-checked by the verifier against the `r_values`/`g_CC` arrays you submit, so those arrays must be a genuine g(r) consistent with the scalar you quote

**为什么这招厉害**：它**不需要标准答案**。如果你抄了论文里的峰位数字、但曲线是随便编的，两者必然对不上；要让它们对上，你就得真的算出一条正确的曲线 —— 而算得出曲线就说明你真做了模拟。

这是"**用你自己交的材料互相验证**"，比对标准答案更难绕。

---

## 七、检查点是怎么设计的，怎么叫通过

要分开讲：**结果检查点**是出题时按题设计的，**过程检查点**是全平台统一的。

### 7.1 结果检查点：出题时定，每题不同

设计方式是"**一个待验目标 → 一个指标 → 一个容差**"。

以氯仿题为例，三个输出文件对应的检查点：

- 平均势能 → 跟参考运行的值比 → 容差考虑随机波动 → **权重最高**
- 径向分布函数 → 峰位与自交曲线交叉校验（容差 0.015 nm）+ 配位数比对
- 静电贡献 → 跟参考值比

**指标从 12 个里选**，每个指标怎么算分是定死的。误差类都走同一个式子：

$$\text{score} = \max\!\left(0,\ 1 - \frac{\text{偏差}}{\text{容差}}\right)$$

**其中：**
- **偏差** —— agent 报的值与参考值的差距，算法由所选指标决定（相对误差、均方根误差、L2 相对范数等）；
- **容差** —— 低于它直接满分 1.0；相对误差默认 0.1，绝对误差默认 1.0，出题人可覆盖；
- **$\max(0,\cdot)$** —— 偏差达到容差即 0 分，不给负分。

相关性类走 $(r+1)/2$ 裁剪到 $[0,1]$；分布距离类走 $e^{-\text{kl}}$ 或 $1-\text{ks}$；物理规则类（能量守恒、质量平衡）是二值且**必须附一个能重算分数的脚本**。

**"通过"在这里不是二值的** —— 除了物理规则和精确匹配这两个指标，其余都是 0 到 1 的连续分。

### 7.2 轨迹分的第一层：五条硬校验，每条 0.2 分

官方完整规范 `ARM_PROTOCOL_REFERENCE.md`（40,099 字）里直接给了函数名和五条检查，**原文逐字引**：

> `validate_trace(zf, names, prefix, manifest)` runs five cross-checks against the JSONL file:
>
> 1. **Typed step_type enum** — every row must declare one of `{thought, tool_call, tool_result, artifact, decision, error, observation}`.
> 2. **tool_call/tool_result pairing** — every `tool_call` must have a matching `tool_result` with the same `tool_call_id`.
> 3. **Timestamp window** — `timestamp` must fall within `execution.ran_at ± wall_time_s` (parsed as ISO-8601).
> 4. **Artifact file existence** — `step_type="artifact"` rows must reference a file that exists in the bundle.
> 5. **Cost lower bound + stdout anchor** — total `cost_usd` must clear 0.01; at least one step `body` must be greppable (substring match) in `execution/run.log`.
>
> Tier scoring (max 1.0): typed steps (0.2) + paired tool calls (0.2) + artifact evidence (0.2) + stdout anchor (0.2) + cost lower bound (0.2).

逐条说清“怎么叫通过”，并对上官方宣传里说的四项：

**第 1 条，步骤类型合法（对应“完整思考”）**。每一行必须声明自己是七种类型之一：思考 / 工具调用 / 工具返回 / 产物 / 决策 / 错误 / 观察。通过条件是全部行都在这七个值里。**这条强迫你把思考单独标出来** —— 这就是为何推理内容少的模型轨迹分吃亏。

**第 2 条，调用与返回配对（对应“工具调用”）**。每个工具调用必须有一个带**相同调用编号**（`tool_call_id`）的返回。有调用没返回就不通过 —— 因为“只说我调了、拿不出返回”无法审计。

**第 3 条，时间戳落在窗口内**。每步的时间戳必须落在“实际运行时刻 ± 墙钟时长”这个区间里。事后补写的时间戳会落在窗口外。

**第 4 条，产物真实存在（对应“代码执行”）**。凡是声明“我生成了某个文件”的步骤，那个文件必须真的在提交包里能找到。

**第 5 条，花费下限 + 日志锚点（对应“代码执行”与防造假）**。两个子条件：总花费必须超过 **0.01 美元**；且**至少有一步的内容能在真实运行日志 `execution/run.log` 里被字符串搜到**。没真花钱说明没真调模型；编的过程在日志里搜不到。

$$\text{trace\_quality} = \min\!\left(1.0,\ 0.2 \times N_{\text{通过}}\right)$$

**其中：** $N_{通过}$ 是上面五条里通过的条数（官方分项列的是 typed steps / paired tool calls / artifact evidence / stdout anchor / cost lower bound 五项，每项 0.2），五条全过即满分 1.0。

**真实例子（满分作答 19415）**：41 步里工具调用 8 步、工具返回 8 步 —— **数量严格相等**，第 2 条通过。类型分布为思考 8 / 调用 8 / 返回 8 / 观察 7 / 产物 8 / 决策 2，全部在合法类型内，第 1 条通过。

### 7.3 轨迹分的第二层：十六条反作弊规则，按编号扣分

五条硬校验之外，还有一套按编号的扣分规则。**我扫了 620 份真实作答，采到 12 个编号（最大编号 N16，说明至少 16 条）**。

拉到完整说明与分值的五条（均为平台返回的原文）：

| 编号 | 标题 | 分值 | 官方判定说明 |
|---|---|---|---|
`N09` | 没有真实执行证据 | **−30** | *"Scientific agent traces need actual tool results connected to computation or outputs."* |
`N13` | 轨迹太短，撑不起这个结果 | **−35** | *"Briefness is not punished by itself; the trigger requires an evidence floor to be missed."* |
`N08` | 工具调用与返回未配对 | **−18** | *"Unpaired calls make execution claims difficult to audit."* |
`N11` | 提交的产出没有因果支撑 | **−6 或 −15** | *"The scorer checks worker receipts first, then file-name/content linkage to the execution trace."* |
`N16` | 重复或突发式提交 | **−15** | *"Submission-level abuse is a sequence property and cannot be inferred from one trace in isolation."* |

**注意 `N11` 的分值出现过 −6 和 −15 两种** —— 说明**扣分不是固定值，按严重程度动态给**。

只采到编号、未拉到详细说明的另七条，从编号名已能看出它们分别堵什么：

| 编号 | 堵的是什么 |
|---|---|
`N01_CROSS_TASK_TRACE_REUSE` | **跨题目复用同一份轨迹** |
`N04_TRACE_SCHEMA_INVALID` | 轨迹格式不合规 |
`N05_PROMPT_INJECTION_OR_SCORER_GAMING` | **用提示词注入去操纵判分器** |
`N06_FABRICATED_OR_UNSUPPORTED_EXECUTION` | 伪造执行或执行无支撑 |
`N12_TRACE_REPETITION_OR_INFLATION` | **轨迹重复注水凑长度** |
`N14_METHOD_SUBSTITUTION_OR_FALLBACK` | **中途换成别的方法或降级应付** |
`N15_PROVENANCE_METADATA_ANOMALY` | 溯源元数据异常 |

**这七条正好对应官方那句“刻意造假、绕开规则刷分会大幅扣减轨迹分”。** 其中 `N12`（注水）和 `N13`（太短）是一对 —— 两头都堵上了，光写长没用、光写短也不行，看的是证据密度。`N14`（方法替换）对应官方说的“迭代全链路” —— 题面要你用某个方法，你中途换成个简单的、或降级应付，会被抬出来。

### 7.4 轨迹分的第三层：大模型用自然语言指出缺了什么

同一份作答里还会附一张“缺什么证据”清单（作答 20039，原文）：

> - Tool calls and corresponding results (e.g., Python script execution logs, CFD solver output).
> - Derivation of Reynolds number, Moody friction factor, and pressure loss formulas.
> - Raw search results or intermediate values for the 5430 configurations.
> - Any artifact linking the final JSON to the claimed computation trace.
> - No evidence of code execution (Python, OpenFOAM, or any tool).

**这层必然是大模型做的** —— 它点到了“雷诺数与穆迪摩擦因子的推导过程”、“5430 个配置的中间值”这种题目专属内容，得读懂题才说得出来。判分引擎的版本号也印证这一点：`trace-score-cli/0.3.0-beta.1+evidence-checklist-v6-contextual-signals` —— “证据清单第 6 版 + 上下文信号”。

它还会给三个判定字段：`trace_decision`（accept / block）、`trace_quality_class`（acceptable / blocked）、以及一句总评。满分那份的总评是 *"Received and graded in full. Strong submission."*；得 0 分那份是 *"The grader did not find a complete, genuinely-derived solution…"*。

### 7.5 三档折扣，低于线直接归零

| 过程分 | 判定 | 折扣系数 |
|---|---|---|
| 98.75 / 94.75 | accept | **1**（不打折） |
| 49 | — | **0.49**（按比例） |
| 29 / 14.375 | **block** | **0**（总分归零） |

### 7.6 步数多不等于质量高

真实数据对比：

- 作答 20040：**10 步**，过程分 **98.75**
- 作答 19583：**105 步**，过程分 **49**
- 作答 19586：**182 步**，过程分 **29**（归零）

10 步那份的判语原文：*"Full computation script content is not shown in the trace, but the output is causally linked and matches the tool output."*（完整脚本没展示在记录里，但产出有因果关联、且与工具输出一致。）

**判的是证据链完不完整，不是写了多少。** 182 步全是自言自语、没有真实工具返回，照样归零。

### 7.7 官方自己提供了本地预检工具

那个 `trace_record` 工具跑完会打印 `[submit] ✓ valid.`，README 原话是“表示 trace 过了 **6 条反作弊规则**的本地预检”。

> 注：官方口径在两处不一致 —— `trace_record` README 与 `/api/protocol` 说的是“6 条”（把花费下限与日志锚点算作两条），而 `ARM_PROTOCOL_REFERENCE.md` 的 `validate_trace` 写的是“5 条交叉校验”（两者合为第 5 条），且分项计分确实是五项×0.2。内容一致，只是切分口径不同。

---

## 八、一个生物题的完整例子：从题面到分数，逐步看轨迹怎么被检验

前面讲的都是规则。这一节拿一道**生物题**从头走到尾 —— 题面、agent 的 13 步行动、每步对应哪条检查、最后得了多少分。

### 8.1 题目（生物 / 临床遗传）

【计算 APOE 基因频率与表型百分比】题面 7,049 字。APOE 是一个与血脂代谢、心血管风险相关的基因，有三个主要等位基因（ε2 / ε3 / ε4），其中 ε4 与高 LDL 胆固醇及阿尔茨海默病风险相关。

**题面直接把输入数据和公式都给全了**（原文节选）：

> From the provided observed apo E phenotype counts (E2/3: 12, E2/4: 1, E3/3: 143, E3/4: 30, E4/4: 2; no E2/2; total n=188), calculate the phenotype percentages, the gene frequencies (ε2, ε3, ε4) using the standard allele counting method (each subject contributes two alleles, total alleles = 376), and compute the standard deviations of the gene frequencies using the maximum-likelihood variance formulas: V_p = p/(8n)[4 - 3p + pr/(pq + r)], V_q = q/(8n)[4 - 3q + qr/(pq + r)], V_r = V_p + V_q - pq/(4n)[4 - pq/(pq + r)], where n=188…

说人话：188 个人的基因型计数已给（比如 E3/3 有 143 人），要你算三个等位基因的频率、它们的标准差（最大似然方差公式也直接给了）、以及各表型的百分比。

**要交的东西**：一个 JSON，题面给了完整契约（原文）：

> Output file: `/app/outputs/allele_frequencies.json`
> Contract: `{"epsilon2_freq": float, "epsilon2_sd": float, "epsilon3_freq": float, "epsilon3_sd": float, "epsilon4_freq": float, "epsilon4_sd": float, "phenotype_percentages": {"E3/3": float, "E3/4": float, "E2/3": float, "E4/4": float, "E2/4": float}}`
> target_policy: **exact_match**

判分说明（原文）：

> A hidden verifier will read your submitted `allele_frequencies.json`. It compares each computed value — the gene frequencies, their standard deviations, and the phenotype percentages — to independently stored reference values using appropriate tolerances, and assigns a score. **Accuracy of the computed numbers determines the score; merely reporting a particular set of numbers is not sufficient.**

最后那句是重点：**分数由数字的准确度决定；光把一组数字报上来是不够的** —— “不够”指的就是轨迹那一关。

### 8.2 agent 的 13 步，逐步对应哪条检查

作答 19953，模型 `claude-haiku-4-5`。每步的类型是它自己声明的，内容是真实拉下来的：

| 步 | 类型 | 它做了什么 | 对应检查 |
|---|---|---|---|
| 0 | thought | 读题：列出五个基因型计数，确认“数据自足” | 第①条（思考需单独标出） |
| 1 | decision | 定口径：ε2 计数 = E2/3 + E2/4 + 2·E2/2，总等位基因 376，方差公式逐字用题面的 | 第①条 |
| **2** | **tool_call** | 跑 `python3 scripts/solve_apoe.py` | **第②条要求此处必有配对返回** |
| **3** | **tool_result** | 真实输出：`e2=13, e3=328, e4=35, total=376`；`p=0.034574 q=0.872340 r=0.093085` | **与上一步配对成功** |
| 4 | thought | 读结果，并做常识校验：“ε3 是最常见等位基因（q=0.87），与日本人群 APOE 分布相符” | 第①条 |
| 5 | decision | **定独立验算策略**：用不同求和顺序重算，期望偏差为 0 | 第①条 |
| **6** | **tool_call** | 跑 `python3 scripts/verify_independent.py` | 第②条 |
| **7** | **tool_result** | `epsilon2_freq submit=0.034574468085 indep=0.034574468085 |dev|=0.00e+00`（逐项列出） | 第②条配对 |
| 8 | thought | 确认 6 个频率/SD + 5 个百分比全部偏差为 0 | 第①条 |
| **9** | **tool_call** | **“洁净室”重跑**：从头跑一遍再读回 JSON 校对 | 第②条 |
| **10** | **tool_result** | `freqs: 0.0345744… pct sum: 100.0` | 第②条配对 |
| **11** | **artifact** | 声明交付 `outputs/allele_frequencies.json` | **第④条（声明的文件必须真在包里）** |
| 12 | decision | 提交决定，并声明“公式完全由题面给定，无隐藏约定” | 第①条 |

**三次工具调用（步 2/6/9）对应三次返回（步 3/7/10）—— 严格配对，第②条通过。** 步 11 是 `artifact` 类型，它引用的文件真存在，第④条通过。步 3/7/10 的内容都是真实程序输出，能在运行日志里对上，第⑤条的日志锚点通过。

### 8.3 它得了多少分，为什么扣了那一点

平台返回的真实分数：

```
结果分 harbor_reward = 1（数字全对）
轨迹分 trace_score = 95.45      折扣系数 trace_factor = 1
总分 = 1 × 100 × 1 = 100
总评："Received and graded in full. Strong submission."
```

但轨迹分不是满分 95.45，被记了一条扣分（原文）：

> `N11_OUTPUT_NOT_CAUSALLY_SUPPORTED`，`score_effect: -6`
> 缺失证据：*"Solver script code not shown in trace, but output values are causally supported by tool results and independent verification."*

**说人话：它只写了“我跑了 solve_apoe.py”，没把这个脚本的代码内容贴进轨迹里。** 扣 6 分。但判定语里跟了一句“不过输出值有工具返回和独立验算作因果支撑”，所以没重罚、也没进 block 档。

对比同一道题的另一份作答（编号 18886，`deepseek-v4-pro`，只 6 步）：轨迹分 90，被记的是 `N16_DUPLICATE_OR_BURST_SUBMISSION`（重复或突发式提交）。**同一道题、都是 100 分，但扣分理由完全不同** —— 一个因为没贴代码，一个因为提交行为像刷题。

### 8.4 从这个例子能看出的三件事

**第一，满分作答的共同特征是“自己验自己”。** 这份 13 步里，步 5–8 专门用另一种求和顺序重算一遍、逐项对比偏差；步 9–10 又从头“洁净室”跑了一次。前面那道流体题的满分作答也是同一个路子（自查 8 项 PASS）。**这不是巧合 —— 因为轨迹分就是在查“你的结论有没有证据支撑”。**

**第二，“思考”真的得写出来。** 这 13 步里 thought 和 decision 共 7 步，占一半以上。而且内容不是凑字数 —— 步 1 把等位基因计数公式写清了，步 4 做了专业常识校验（“ε3 最常见，符合日本人群分布”）。

**第三，扣分很细，而且真的会扣。** 这是一份 100 分的作答，依然因为“没把脚本代码贴进轨迹”被扣了 6 分轨迹分。只是它轨迹分还在 95 分以上，落在不打折那一档，所以最终总分没受影响。

---

## 九、轨迹格式不可预知，评测脚本怎么写

这是整套机制的地基。**答案：判分器不去猜格式 —— 格式被规范强制统一了，转换的责任在 agent 一侧。**

### 9.1 每一步长什么样是官方定死的

平台发布了一份 JSON Schema：`GET /api/schemas/trace-step/v1`（1,654 字）。它给每一行轨迹规定了字段，**开头的 description 直接写明了它就是为反作弊服务的**（原文）：

> One row in a typed-step trace JSONL file. **Used for anti-fraud verification**: each `tool_call` must be matched by a `tool_result`; `artifact` steps must produce files that exist in the bundle with **mtimes inside the run window**.

关键字段及其约束（均为 schema 原文）：

| 字段 | 约束 | 作用 |
|---|---|---|
**`step_type`** | **唯一必填项**，枚举限定七值：`thought / tool_call / tool_result / artifact / decision / error / observation` | 定此步是什么 |
**`tool_call_id`** | *"Required for step_type=tool_call and tool_result; **must pair**"* | 配对的依据 |
`tool_name` | *"Required for step_type=tool_call"* | 调了哪个工具 |
`tool_args` | 调用参数，object | 传了什么参数 |
`tool_output` | 返回内容，同一 call_id | 工具返了什么 |
**`artifact_path`** | *"path inside bundle. **File must exist with mtime in run window**"* | 产物验真 |
`artifact_id` | 可对应 `execution.artifacts[].id` | 产物溯源 |
**`timestamp`** | ISO-8601，*"Must fall within `execution.ran_at ± wall_time_s`"* | 时间窗校验 |
`cost_usd` / `tokens` / `duration_s` | 数值，最小 0 | 花费与耗时 |
`step_order` | 整数，最小 1 | 步序 |
`title` / `body` / `code` | 字符串 | 内容本体 |

**所以评测脚本不需要“读懂” agent 写了什么，只需要按字段查：**

- 第①条查 `step_type` 是否在七个枚举值内 → 纯枚举比对
- 第②条把所有 `tool_call` 的 `tool_call_id` 收集成一集合，再看 `tool_result` 能不能一一对上 → 纯集合运算
- 第③条把 `timestamp` 解成时间、比一下是否落在区间内 → 纯数值比较
- 第④条拿 `artifact_path` 去包里 `os.path.exists` 一下，**再查文件修改时间是否在运行窗口内** → 纯文件系统操作
- 第⑤条把 `cost_usd` 求和比 0.01，再拿某步 `body` 去 `run.log` 里做子串匹配 → 纯求和 + 字符串搜索

五条全部是确定性操作，**一行 LLM 都不需要**。

> 附带发现：schema 里那句 **“mtimes inside the run window”（文件修改时间必须在运行窗口内）** 是我之前没注意到的一道防作弊。意思是：你声明“我跑程序生成了这个文件”，那么这个文件的修改时间必须落在你声称的运行时段里。**提前准备好的文件、或从别处拷来的文件，时间戳对不上。**

### 9.2 格式不一致的问题，用一个转换工具解决

你担心的“格式不可预知”是真问题 —— Claude Code、Cursor、OpenClaw 各家的原生会话日志格式完全不同。

**官方的解法：把转换这件事从判分器那边挪到答题人这边，并且官方提供转换工具。** 就是前面提过的 `trace_record`，README 原话它只做两件事，第一件就是：

> **trace 转换**：`trajectory.jsonl` → ARM v1.1 `trace.jsonl`

它还会本地预检，跑完打印 `[submit] ✓ valid.`，意思是“你转出来的这份已经能过反作弊检查了”。

**整个链路是这样的：**

```
agent 自己的会话日志（各家格式各异）
        ↓  trace-submit 转换
统一的 ARM trace.jsonl（每行带 step_type / tool_call_id / artifact_path …）
        ↓  上传
判分脚本 validate_trace（只认这一种格式）
```

**这就是为什么官方非得自己写一个转换工具** —— 不是为了方便选手，是因为如果不把格式统一，判分脚本根本无法写。

### 9.3 但有一部分确实写不成脚本

你的直觉对了一半。**把三层拆开看，就能看出哪些能写脚本、哪些不能：**

| 层 | 内容 | 能否写成确定性脚本 | 为什么 |
|---|---|---|---|
第一层 | 五条硬校验 | **能，完全能** | 全是字段比对、集合配对、文件存在性、数值比较 |
第二层部分 | `N04` 格式非法、`N08` 未配对、`N16` 重复提交、`N15` 元数据异常 | **能** | 都是结构或序列属性，schema 校验 + 提交时间序列即可判 |
第二层其余 | `N09` 无执行证据、`N11` 无因果支撑、`N12` 注水、`N13` 过短、`N14` 方法替换、`N01` 跨题复用、`N05` 操纵判分器 | **不能纯靠脚本** | 这些是语义判断：“这步算不算真的执行证据”、“这是否偏离了题面要求的方法” |
第三层 | 指出缺了什么证据 | **不能** | 需要读懂题目才能说出“缺雷诺数推导” |

**所以他们的做法是分层：能确定性判的就确定性判，剩下的交给大模型。**

判分引擎的名字也印证了这一点：`trace-score-cli/0.3.0-beta.1+**evidence-checklist-v6-contextual-signals**` —— “证据清单”是可枚举的条目，“上下文信号”就是靠理解的部分。而 `0.3.0-beta.1` 这个版本号说明它自己也还在调 —— 前面那个 `harbor_rescore_campaign=ssnp-verifier-hotfix` 就是判分器出问题后重刷历史分数的痕迹。

### 9.4 对我们的直接启示

我们的处境比他们好，但有一条值得抓过来。

**我们本来就不面临“格式不可预知”这个问题** —— agent 通过 MCP 调我们的工具，调用名、参数、返回值全部由我们当场落盘，格式天然统一。他们需要一个转换工具才能押平格式，我们不需要。

**但他们那条“产物修改时间必须在运行窗口内”值得看** —— 它的内核思路是“拿一个 agent 无法伪造的客观信号（文件时间戳）去交叉验证它的主观声明”。这与我们已有的“真值从 tool call 反查、不信 LLM 自报”是同一条原则。

---

## 十、每道题的检查点、分值、以及有没有顺序依赖

### 10.1 判分脚本是不是从 output json 出发写的

**是，但准确地说是从“每个待验目标”出发，输出文件只是目标的容器。**

一道题的结构是三层嵌套：

```
一道题
 └─ 多个输出文件（每个带 target_policy）
      └─ 文件里多个字段，每个字段就是一个检查点
```

拿氯仿那道材料题举例：三个输出文件，但检查点比三个多 —— 光 `step_03_rdf.json` 一个文件就包含四个字段（`r_values`、`g_CC`、`first_peak_position`、`coordination_number_0_64`），每个都被单独查。

而且输出文件与待验目标之间有一层显式映射，叫 `produced_by`（规范原文例子）：

> `"execution": {"artifacts": [{"id": "art_metric_a", "path": "execution/results/metric_a.json", …}]}`
> `"expected_outputs": [{"name": "metric_a", "produced_by": ["art_metric_a"]}]`

意思是：“`metric_a` 这个待验目标，由 `art_metric_a` 这个产物文件支撑”。规范还说明了为何要搞这层（原文）：

> v1.0 used **brittle path-matching**… v1.1 introduces an **id-reference layer**。Validator (`validate_bundle`) **refuses bundles where any `produced_by` id does not resolve to an `execution.artifacts[].id`**。

旧版靠路径字符串匹配，太脆（改个文件名就断）；新版改成 id 引用，**而且引用解不开就直接拒收这个包**。

### 10.2 检查点怎么定义

**一个检查点 = 四件东西：查哪个量 + 用哪个指标 + 容差多少 + 参考值从哪来。**

以氯仿题的三个检查点为例：

| 查什么 | 指标 | 容差 | 参考值来源 |
|---|---|---|---|
每分子平均势能 | 误差类 | 考虑随机波动的容差 | 出题方跑一次正确模拟 |
第一峰位置 | 与自交曲线交叉校验 | **0.015 nm** | 不靠参考值，靠自洽性 |
误差棒 | 按量级判 | — | 不比精确值 |

**指标只能从 12 个里选，每个的算分公式官方定死了**。规范里甚至直接给出了实现函数名（原文）：

> Per **`_metric_score(metric, deviation)`** in `arm_service.py`

误差类全部走同一个式子：

$$\text{score} = \max\!\left(0,\ 1 - \frac{\text{deviation}}{\text{tolerance}}\right)$$

**其中：**
- **deviation** —— agent 报的值与参考值的差距，算法由所选指标决定；
- **tolerance** —— 容差，**规范明确了默认值：缺省为 1.0，相对类指标为 0.1**，出题人可覆盖；
- **$\max(0,\cdot)$** —— 差距达到容差即 0 分，不给负分。

其余：相关性类 $(r+1)/2$ 裁到 $[0,1]$；分布距离类 $1-\text{ks}$ 或 $e^{-\text{kl}}$；物理规则与精确匹配为二值；SSIM 贡献封顶 0.3；自定义必须附重算脚本。

### 10.3 分值怎么定义

**两层：单项分由公式算出（上面），总分按权重合成。**

权重体现在两处：

**第一处，题面里的 `load-bearing` 标注。** 氯仿题的四个步骤里，两个标了 `Role: scored (load-bearing)`（承重），一个只是 `Role: scored`，第一个是 `Role: process`（不单独计分）。**这就是轻重之分的声明。**

**第二处，判分说明里直接点名。** 氯仿题原文：

> **The heaviest single check is the average potential energy per molecule.**
> The check scores are **combined by weight** into a final reward in [0,1].

即：各检查点按权重合成一个 $[0,1]$ 的 reward，**这就是 `harbor_reward` 的来源**，乘 100 就是结果分。

两个文件级的聚合公式规范也给了：

$$\text{output\_coverage} = \frac{|\ \text{deviations.target} \cap \text{expected\_outputs.name}\ |}{|\ \text{expected\_outputs}\ |}$$

$$\text{result\_fidelity} = \operatorname{mean}(\text{deviations.score})$$

**其中：** 前者是“该交的目标交齐了几成”（名字对不上不算）；后者是各目标得分的算术平均（SSIM 项先截到 0.3）。

### 10.4 有没有“先做 A 再做 B”的依赖评分

**结论：没有真正的顺序依赖判分。他们是“平铺加权”，不是 DAG。**

我查了四处，逐条说：

**第一，`produced_by` 不是步骤依赖。** 它是“待验目标 → 产物文件”的溯源映射（哪个文件支撑哪个结论），**不是“步骤 B 必须在步骤 A 之后”**。

**第二，题面的 Step 1/2/3 编号是叙述顺序，不是判分约束。** 五条硬校验里没有任何一条查步骤先后；`step_order` 字段只是记录序号，schema 里没说它参与判分。

**第三，`Role: process` 确实表达了一种弱依赖，但不单独计分。** 两道题的第 1 步都是 `process`（光学题是“搭正向模型”，氯仿题是“跑蒙特卡罗模拟”）—— **后面所有计分步骤都建立在它上面，但它自己不得分**。它的依赖关系是隐含的：你不跑模拟，后面四个文件自然交不出来。

**第四，真正的依赖是用“互相校验”表达的，不是用顺序。** 这是他们最巧的一招。氯仿题原文：

> **The curve itself is read, not merely stored:** `first_peak_position` is cross-checked against the `r_values`/`g_CC` arrays you submit… **A peak position that disagrees with the submitted curve scores zero.**

你报的第一峰位置，必须等于你自己交的那条曲线的最大值（差不过 0.015 nm）—— **对不上直接零分**。这强制了一种先后关系：你得先真的算出曲线，才能报得出峰位。**但它不是“检查执行顺序”，而是“检查两个产物自洽”。**

同一道题里还有一句依赖表述：*"reproducing the reported energy **depends on getting it right**"*（复现报告的能量取决于把这个做对）—— 但这是写给人看的提醒，不是机器约束。

### 10.5 这一节对我们的意义

**他们没有做的，正是我们在做的。** 他们的检查点是平铺的一组加权项，没有偏序约束；我们的 checkpoint 有 DAG 偏序语义（哪些必须在哪些之前完成才算有效）。

**但他们那招“自洽校验”值得学** —— 用两个必须一致的产物互相验证，既不靠标准答案，也不靠检查执行顺序，却能达到“不真算就过不了”的效果。这比硬查执行顺序更可靠 —— 因为顺序可以伪造，两个产物的数值一致性很难伪造。

---

## 附：说明与出处

**一处更正。** 本文最初版本（提交 `f699e33`）写"判分靠大模型当裁判"，**错了**。实际是：结果判分走确定性数值检查（`harbor-lbg`），过程判分是六条机器检查 + 大模型判证据链（`trace-score-cli`）。"大模型照人写的评分标准判"那套只用于「benchmark 套件」功能，而那个功能全站只有 1 个套件、0 道题、未发布，实际没人用。错因是我先读了讲 benchmark 套件的官方文档，恰好是没人用的通道。

**一处上轮未决、本轮已解。** 上一版说"总分合成权重未公开"。本轮换到未被比赛遮蔽的社区题（40 道，字段全部可见），在 `scoringDetails` 里直接拿到 `trace_factor`，公式确认并在 5 份作答上逐条验过。此前拉的是比赛期题目，字段被服务器遮蔽。

**一处待你确认已解。** 上一版我写“找不到 5 万工具的出处”。你提供的官方赛事文案确实写了“170 多种编程语言、50000 多类科学工具”—— **我只搜了站内页面、没看宣传文案，这条是我的缺失**。已按官方文案改写第三节，并区分清了“5 万计算环境”与“站内 Tools 板块 33/38/23”是两回事。

**出处清单。** 题面与学科分布：`GET /api/challenges/{id}`、`GET /api/challenges/facets`；判分公式与 12 个指标、五条防伪造校验：`GET /api/protocol`（4,806 字）与 **`GET /api/docs/dev/ARM_PROTOCOL_REFERENCE.md`（40,099 字，含 `validate_trace` 五条原文与评分推导路径）**；判分字段定义：`GET /api/schemas/characterization/v1`（4,328 字）；分数明细与扣分编号：`GET /api/attempts/{id}`（**扫 620 份作答采得 12 个编号**）；41 步行动轨迹：`GET /api/attempts/{id}/trace`；工具区实体：`GET /api/skills`；录制工具与责任边界：`github.com/zhizhengzhao/trace_record` 的 README。全程只读，未提交任何作答。
