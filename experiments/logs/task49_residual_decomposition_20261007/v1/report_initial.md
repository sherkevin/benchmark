# MIRA-Relay 残差来源诊断结果

日期：2026-10-07。状态：完成；development-only 追溯分析，不是新的确认实验。

固定实验卡：[experiment_card.md](experiment_card.md)。只复用冻结 development 日志的合法 Frozen 分数和 MIRA 已选反馈；没有加载 checkpoint、读取 confirmation、产生新 provider 输出或运行新闭环策略。

## 实测结论

主分析为 seed0 的 15 条 fold/delay 流，共 2,166 次决策、722 个实体标识。同一分子在三个 delay 下重复。实际 MIRA 与 Frozen 动作不同 769 次（35.50%）；保留同一实际历史、移除组成项后，shadow 与实际动作不同 401 次（18.51%）。

分数重构最大绝对误差 2.2204460492503131e-16，恒等式最大绝对误差 2.7755575615628914e-17，均低于固定阈值 1e-10。实际 released-selected 的平均冻结分数 0.580164、正确率 0.791782、Brier 0.213847。这是选中样本上的描述性校准诊断，不能外推到所有 provider 输出。

分数组成项在本面板确实可能改变动作，因此不能把当前 provider 残差全部解释成冻结预测误差。sigmoid 来自 class-balanced logistic 模型，分数未被证明等于自然正确性概率；两项不能分别命名为真实环境漂移和纯粹选样偏差。相对冻结分数的误差项为正、组成项为负，存在明显抵消。

## 残差大小

每次决策的每个 provider 为一个描述性记录：4 × 2,166 = 8,664 项等权汇总；不是 8,664 个独立统计样本。

| 项 | signed mean | mean absolute | max absolute |
|---|---:|---:|---:|
| 相对冻结分数的误差项 | 0.076017 | 0.078485 | 0.295039 |
| 分数组成项 | -0.062692 | 0.062692 | 0.178538 |
| 总残差 | 0.013325 | 0.035389 | 0.165160 |

## 各折与延迟

动作差异分母为该行决策数。完整分组的残差 signed mean/absolute、各 provider 校准、数值误差见 summary.json。

| 折/延迟 | 决策数 | 实际 vs Frozen | shadow vs 实际 | 误差项 signed mean | 组成项 signed mean |
|---|---:|---:|---:|---:|---:|
| fold0 | 435 | 92 | 97 | 0.065971 | -0.069305 |
| fold1 | 435 | 73 | 73 | 0.074071 | -0.071249 |
| fold2 | 432 | 170 | 75 | 0.076233 | -0.060976 |
| fold3 | 432 | 243 | 94 | 0.107380 | -0.056167 |
| fold4 | 432 | 191 | 62 | 0.056513 | -0.055657 |
| D=0 | 722 | 253 | 140 | 0.075685 | -0.064355 |
| D=1 | 722 | 265 | 126 | 0.074830 | -0.061993 |
| D=4 | 722 | 251 | 135 | 0.077535 | -0.061728 |
| fold0:delay0 | 145 | 28 | 34 | 0.066155 | -0.071644 |
| fold0:delay1 | 145 | 31 | 31 | 0.066389 | -0.069852 |
| fold0:delay4 | 145 | 33 | 32 | 0.065367 | -0.066418 |
| fold1:delay0 | 145 | 28 | 22 | 0.073455 | -0.071885 |
| fold1:delay1 | 145 | 23 | 25 | 0.076443 | -0.072268 |
| fold1:delay4 | 145 | 22 | 26 | 0.072315 | -0.069595 |
| fold2:delay0 | 144 | 53 | 22 | 0.077385 | -0.062761 |
| fold2:delay1 | 144 | 54 | 30 | 0.078581 | -0.062608 |
| fold2:delay4 | 144 | 63 | 23 | 0.072733 | -0.057559 |
| fold3:delay0 | 144 | 86 | 31 | 0.100394 | -0.053147 |
| fold3:delay1 | 144 | 89 | 26 | 0.099206 | -0.052465 |
| fold3:delay4 | 144 | 68 | 37 | 0.122541 | -0.062890 |
| fold4:delay0 | 144 | 58 | 31 | 0.061119 | -0.062232 |
| fold4:delay1 | 144 | 68 | 14 | 0.053581 | -0.052646 |
| fold4:delay4 | 144 | 65 | 17 | 0.054839 | -0.052094 |

## 已释放样本的分数校准

尾部排空反馈全部计入校准；尾部标签不会倒灌入此前决策。跨 delay 的同一分子可能选中不同 provider；下表是实际选择轨迹的记录汇总，不是独立泛化估计。

| provider | 释放数 | 平均冻结分数 | 已选正确率 | Brier |
|---|---:|---:|---:|---:|
| ecfp_logreg | 371 | 0.594738 | 0.811321 | 0.223717 |
| ecfp_mlp | 421 | 0.535680 | 0.812352 | 0.234750 |
| ecfp_random_forest | 455 | 0.570545 | 0.848352 | 0.209239 |
| graph_message_passing | 919 | 0.599422 | 0.746464 | 0.202568 |

## 校验与证据边界

- 三个源文件 SHA-256 与卡内固定值一致；60 条 Frozen/MIRA 双 seed 流、25,992 条事件通过 canonical stream digest、exact event allowlist、摘要计数、decision→terminal→feedback 绑定、释放顺序、版本和 opaque state hash 连续性检查。没有声称重建 learned-state 字节或 state hash。
- MIRA 与对应 Frozen decision 的 fold/seed/delay/entity/tick/menu/feature hash 绑定一致。MIRA 原分数和 argmax 均由已释放 selected feedback 独立恢复；没有导入生产 runner/policy。
- 非目标方法不进入分析；Frozen terminal 数值只作为 canonical digest 和 seed 语义一致性检查的 opaque 内容，不进入分解、校准或 reward 计算。只有 MIRA 自身选择并已经释放的 reward 更新状态。
- seed1 的 Frozen/MIRA 共 30 对流在排除运行时间、规范化 seed 身份后完全相同；seed1 仅核对确定性，不计入主指标。
- shared-scalar 排序检查全部通过；这是固定菜单、同一平局规则下的代数检查，不是额外科学证据。
- 15 项定向工程测试通过：包含延迟可见性、尾部排空、重复反馈、摘要/菜单/时钟/绑定/digest 变异、错误奖励、分数重构和 feature 绑定失败。合成工程夹具不作 provider 实验结果。
- 未计算 shadow accuracy、counterfactual gain 或 closed-loop reward；改变动作后原本会得到哪些反馈不可观察。没有将 15 条流视为独立样本计算置信区间；722 分子跨 delay 重复，fit folds 也重叠。

## 复用与下一步

可复用的是独立时钟/绑定/分数重构检查器、2,166 条逐决策分解和 2,166 条真实 selected-release 校准记录。下一步先解决冻结分数校准，再另立开发集闭环中心化对照实验卡。本轮不改已报告 confirmation 方法、超参或表格，也不执行后续候选；任何候选确认需要新的未使用数据。

## 可复现记录

- 命令：`/Users/jingwu/work/benchmark/.srcprobe/beir_venv/bin/python src/probe_headless/analyze_mira_residual_decomposition_v1.py`。
- Git：`222320030fe32bfeb4786ca301364217860bcbd8`；脚本 SHA-256：`b3242af80bc5589325cd8295d8702bbba2b09ca2d7d0762477c84772e09c5a92`。
- 实验卡 SHA-256：`7632a5463082c0c46cde5d476ba519b53c563ee09eb4c8334ac4cf0078f93bc8`。
- 开始：`2026-10-07T03:19:37.808804+00:00`；结束：`2026-10-07T03:19:38.512277+00:00`；耗时 0.703 秒。Python 3.9.6、NumPy 1.26.4，本地 CPU、0 API 调用。
- 源目录：`experiments/logs/task49_provider_fresh_dynamic_diagnostic_20261006/v1/`；原始文件未修改。
- 输出目录：`experiments/logs/task49_residual_decomposition_20261007/v1/`。
- `config.json`：分析前命令、版本、时间、源码/卡/输入 hash；`execution.jsonl`：流式执行记录。
- `decisions.jsonl`：2,166 条逐决策记录；`release_calibration.jsonl`：2,166 条 selected-release 记录。
- `stream_audit.jsonl`：60 条流校验回执；`summary.json`：全部分组指标和产物 hash。
- `tests_attempt01.jsonl` / `tests_attempt01_stdout.txt`：测试配置、源码 hash、状态和原始输出。
- `report_generation_attempt01.json`：首次报告拼装脚本的语法错误，发生于解释器解析阶段，未影响分析和原始数据；修正后生成本报告。
- summary.json SHA-256：`371ea84eb304c3b962a7a0b24f6ddf46875277849789ef39170c3c775ca963a6`。
