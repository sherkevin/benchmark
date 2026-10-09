# task25 · 小流量全链路 pipeline 演示（本周汇报用）

> 上游：owner 2026-09-17「我需要快速跑出来结果，以供我们本周的汇报，需要你快速跑通一个完整的 pipeline 出来……小流量先把整个 pipeline 走一遍看看效果比较好，更能出展示的效果」+「并行开 subagent 来做不同的工作（可以在 jingwu 下运行 ./init 初始化），时间很紧张」。
> 性质：**演示批，不是验收集**。目标是证明六步链路端到端可跑、每一站都有落盘产物、指标可复现；不追求统计功效，不替代 ADR-0007 的验收数（≥5000 题 / 难分子 ≥500）。
> 与七道闸的关系：本批**不跳闸**——闸4（判别关离散零超参）、闸5（三条零成本基线）、闸6（预登记）照走；「小流量」体现在分子数与 run 数收缩，不体现在省略判据。预登记落盘后才许开 API 批。

## 1. 范围（fixed 类，BBB 单族，open 类不碰）

| 维度 | 定值 | 依据 |
|---|---|---|
| 族 | BBB（唯一 730/730 全实验真值） | task24 §4 实跑家底 |
| 题目单位 | （分子, 工具子集）| ADR-0007 钦定单位 |
| 分子数 | **6**（按难度轴 tw 分层：易 2 / 中 2 / 难 2） | 演示批最小可分层样本；tw 口径 = 分歧族数（D2-① 同源） |
| 工具档 | `some_tools` 单档（六配置矩阵收缩为 1 配置） | 演示走通为先；三档对比留给验收批 |
| 带工具批 | **2 模型 × 6 题 = 12 run** | BENCH_SPEC §9.1 每批 ≤9 run 精神，演示批一次性 12 run 须在本 task.md 预登记中写明 |
| 裸答基线 | 1 模型 × 6 题 = 6 run（同分子同判分口径） | 复现 task23 方法学（逐样本，非聚合引用） |
| 冒烟 | ≤3 run（任何模型，跑通即停） | 先证链路再开批 |
| API 总预算 | **≤21 run**，全部走 jingwu 自己的 idealab token（init 内），**不碰 chenxihe 配额** | owner 授权 init；scope.md 规则 3 |

## 2. pipeline 六站（每站必须有落盘产物，产物路径即证据）

```
站1 选题采样      screening_ledger.jsonl(730) + divergence CSV(tw) ──确定性采样(seed 固定,无时间戳)──▶ sample.json
站2 题面构造      sample.json ──题面生成器(单一权威模式,仿 qed01_task.py)──▶ 每题: task.md题面 + answer_schema + ground_truth
站3 后端视图      per_case_view(ViewedBackendReader 白名单) + 工具面 ──▶ 每题 view.json + 自检 assert_per_case_view
                 【owner 2026-09-17 裁定】工具面走**进程内 Gateway 直连**（降级通路，非 mcp_mock_server）：
                 `mcp` 包全机缺失（系统 python + 5 conda env 均无，requirements.txt 钉 mcp==1.29.0 但装它=停机条件3），
                 站6 冒烟实测进程内 Gateway 数据面可用（search_candidates 命中、18 次预算内 get_property、判分 0.8253）。
                 隔离实质判据不变：per_case_view 白名单强制 gt∉可见集（与 MCP 传输层无关，MCP 只是协议壳）。
                 降级口径与等价性论证写进 preregistration.json；将来验收批若走 MCP 面，须另立预登记对照。
站4 零API基线     恒答基线(bbb_constant_answer_baseline.py) + 方差分解 + preflight 闸门1~6 ──▶ baselines.json + preflight 通过记录
站5 预登记        批次构成/判分口径/停止条件 落盘 ──▶ preregistration.json（闸6；开 API 前完成）
站6 agent 实跑    chain_harness_stream(idealab, IDEALAB_AK 路由) × mock server ──▶ runs/<model>__<tag>/answer.json + tool_calls.jsonl
站7 判分与汇总    judge_contract(L = I(答错) + c·I(弃答), c 网格报告) ──▶ scores.json + demo_report.md
```

## 3. 交付物

- [ ] `experiment/sample.json` —— 6 分子 + tw 分层 + 工具子集，确定性可复现
- [ ] `experiment/cases/` —— 每题题面 / answer_schema / ground_truth / view.json
- [ ] `experiment/baselines.json` —— 恒答基线 + 裸答投影（逐样本，禁止只引聚合数）
- [ ] `experiment/preregistration.json` —— 闸6 预登记
- [ ] `experiment/runs/` —— ≤21 run 现场（answer.json + tool_calls.jsonl）
- [ ] `experiment/scores.json` + `report.md` —— 判分汇总 + 演示报告（含 pipeline 图、每站产物路径、易/中/难 × 裸答/带工具 指标表）
- [ ] `TODO.md` T-281 登记

## 4. 停止条件（命中即停，交 owner）

1. 冒烟 3 run 内链路跑不通（auth / mock server / 判分任一断）。
2. 裸答 6 题全对或全错（演示无区分度，换分子重采）。
3. 任何需要装新工具 / 新依赖。
4. 任何判据需要改动既有口径。

## 5. 边界

- 不碰 open 类；不动冻结输入；不改 `.gitignore`；不 commit（由主会话按变更组统一提交）。
- 不在任何输出 / 日志 / transcript 打印 token 值（凭据只在内存与 env 中流转）。
- task24（ADR-0009 + 真值候选表）并行推进，互不阻塞；demo 实践中形成的口径决定**倒推登记**进 ADR-0009。
- 汇报口径如实标注：演示批 n=6，无统计功效主张（DECISIONS 坑⑦：小样本不断言「必然/结构性」）。
