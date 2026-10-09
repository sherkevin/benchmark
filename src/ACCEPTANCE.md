# 验收报告 —— 夜班自主任务(会话 6986b67e, 2026-06-06)

> ⚠️ **2026-06-08 后注**:本验收报告写于 `goal.md` v1.0 锁定之前,框架沿用 PRD 的"介入度为主轴"。**结论与代码全部仍成立**——`src/` 的测量机器本身就是过程量度量的实现,介入指标只是它输出的指标之一。新框架(过程量主轴 / 介入度操作化之一)详见 `../goal.md`。

## 目标(我自定并锁定的长程任务)

把 **Paper A**(AI4S 长程科学 agent 的**过程量 / 中间 checkpoint** 评测基准;此前称"以介入程度为核心轴",已对齐 `../goal.md` v1.0)从"提案 + 单 checkpoint 原型"
推进成**可运行、可验证的 benchmark MVP**,并起草论文可写部分。**全程诚实:不跑真实 LLM、不编造科学数值、不做不可逆/对外操作。**

## 30 秒验收(跑这两条)

```bash
cd /Users/jingwu/work/benchmark/src
python3 tests/test_core.py                 # 期望:21 passed, 0 failed
python3 run_validation.py --tasks tasks --seeds 20 --max-budget 8 --write /tmp/check.md
#   期望:oracle autonomy@0=1.00 → strong .74 → medium .15 → weak 0;never_asks==weak;random/reversed=0
```

## 交付物(看什么)

| 看 | 在哪 | 是什么 |
|---|---|---|
| 测量机器 | `benchmark/src/core/` | 9 个模块,纯标准库:judge/interruption/metrics/conservation/sim_agents/harness… |
| 验证报告 | `benchmark/src/results.md`(合成 fixture)`results_real.md`(4 真实任务) | 5 个签名输出的表格 + 单实例 trace |
| 真实任务集 | `benchmark/src/tasks/*.json` | 超导Tc / 半导体带隙 / ADMET / MolBench-MS3 CK2α;数值 verbatim 自真实数据 |
| 单元测试 | `benchmark/src/tests/test_core.py` | 21 条断言固化"机器应有性质" |
| 论文草稿 | `topic/autonomy-benchmark/draft/draft.md` | 方法/指标/相关工作/协议;结果诚实标 PENDING |
| schema/说明 | `benchmark/src/{schema.md,README.md}` | 任务规格 + 用法 + 诚实边界 |
| 工作日志 | `work/todo_6986b67e-….md` | 全程 to-do 即 log |

## 做出了什么(相对 task3 的增量)

task3 已有"单 checkpoint(虚拟筛选)测量机器"。本夜**加性扩展**为:
1. **多阶段金标准轨迹**(分解/预测/选型,3 类 checkpoint:ranking/value_set/selection)。
2. **完整指标套件**:autonomy@0、介入-成功率曲线、**intervention-conditioned pass^k**(τ-bench pass^k 的推广)、
   **介入守恒**(前置 L 档 + 运行时 = 同币种;固定 cohort 逐实例无偏度量)、prompt 详细度敏感性、许可校准。
3. **三分类中断**(能力性求助计入 / 许可性不计 / 校准正交)实装。
4. **对照裁判**泛化到多阶段(holds GT 的低难偏离比对)。
5. **7 个合成参考 agent** + 验证机器正确/单调/不奖励莽撞(never_asks 负对照)。
6. **4 个真实锚定任务** + 论文草稿。

## 诚实边界(务必知道)

- **agent 全是模拟器**:验证的是"测量机器成立且有区分度",**不是任何真实 LLM 的成绩**。真实排行榜 = 待平台接入(draft §7)。
- **介入守恒是建模选择**:让前置/运行时介入走同一 competence 通道 → 守恒由构造成立;是"操作化 + 内部自洽"验证,**非**对真实 LLM 的经验断言。
- **科学数值未编造**:gold 数值 verbatim 自 `benchmark/reference/NobelClaw/证据_*_gt.md` 与 MolBench data_full;阈值/介入卡/prompt 为作者撰写(各任务 `provenance` 标注)。
- **相关工作让步**:pass^k 与终态金标准判分让给 τ-bench(我们推广为介入条件化 + 金标准**路径**偏离);医疗 benchmark 先例待补核(arXiv 当时限流)。

## 我没做 / 没碰(自律)

- 没跑真实 LLM;没 git commit/push;没装包;没改别的会话 todo、`agent_os/决议`、他人文件;只新建 `src/` 与 `draft/` 与本会话 todo。

## 建议的下一步(等你拍板)

1. 接 1 个真实 agent(哪怕 Claude Code 自己)跑通 draft §7 协议,把 PENDING 换成首批真实曲线。
2. 补核医疗 agent benchmark 先例(用 idealab scholar 搜),夯实"医疗方向"新颖性。
3. 按需把任务集从 4 扩到 PRD 的 30–50 题(模板与 loader 已就绪)。
4. 评审 draft 的相关工作让步是否到位,再决定投 NeurIPS D&B / ICLR / ARR。
