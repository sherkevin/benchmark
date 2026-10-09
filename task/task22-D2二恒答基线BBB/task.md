# task22 · D2-② 裸答检索天花板 + 恒答基线（BBB 族）

> 上游：owner 2026-09-14 批准（「可以」）。ADR-0028 D2 五条零机时检验的第二条。
> 规格出处：ADR-0028 D2-②、入库版提案 §3.4.6 第 2 条、讨论稿 §4 第 1 条、
> 方向7 调研（c 区间机械锚）。

## 1. 目标

一句话：**量出「不靠工具、只靠记忆或固定动作」的退化策略能拿多少分，从而判断
这道题会不会被记忆检索或恒答白嫖。**

D2-② 分两半，本任务先做能零 API 完成的那半：

| 半 | 验什么 | 怎么验 | 本任务状态 |
|---|---|---|---|
| **恒答基线** | 「一律说不可信 / 一律答同一标签」能拿多少分 | 纯计算：在冻结的 7 族投票 × 真值上算各退化策略的损失 | ✅ 本任务完成 |
| **裸答检索天花板** | 模型能不能不看工具直接背出实验真值 y_i | 调 LLM：不给工具，直接问每个分子的 BBB 实验标签 | ⛔ 阻塞——凭据合规，见 §4 |

判据（入库版 §3.4.6 第 2 条）：**恒答得分高 ⇒ 必须同时报弃答率，否则分数无意义。**
本任务把每条损失都与弃答率成对落盘。

## 2. 输入（全部只读冻结，与 D2-① 同三份）

| 输入 | 路径 |
|---|---|
| 分歧投票 | `data/tool_results/divergence/血脑屏障 BBB.csv`（730×7 族） |
| 实验真值 | `data/bbb_census_v3/.../screening_ledger.jsonl` |
| 矛盾排除表 | `data/bbb_census_v3/.../truth_unresolved_bbb.json`（排除在池 8 个） |

样本口径：722 可用 / 652 平衡面板。真值 515 阳 207 阴，少数类占比 28.67%。

## 3. 交付物

- [x] `src/probe_headless/bbb_constant_answer_baseline.py` — 恒答基线脚本（零 API）
- [x] `data/tool_results/divergence/constant_answer_baseline_bbb.json` — 结果 JSON
- [x] `src/probe_headless/tests/test_bbb_constant_answer_baseline.py` — 14 条测试，三次变异验证
- [x] `task/task22-D2二恒答基线BBB/report.md` — 结论与证据
- [ ] 裸答检索天花板半（阻塞，凭据决策交 owner）

## 4. 边界与阻塞

- **不动判分口径。** 损失式 `L = I(答错) + c·I(弃答)` 与三条规则式基线
  （从不弃答 / 多数投票 / Oracle-yield）在 `目标.md` 已定死，本任务只**应用**，
  不重新定义——不撞停机条件第 4 条。
- **c 区间是本数据现算的，不复用方向7 的 (0.157, 1.59]。** 方向7 那套是
  ESCALATE 行动经济体、跑在旧 4 工具普查上（Ref-A err 0.15717、oracle 升级 9.86%）；
  本任务是 ABSTAIN 经济体、跑在当前 7 族数据上，两个区间不可互换，JSON 里显式标注。
- **裸答检索天花板半阻塞在凭据合规。** 唯一可用凭据 `ANTHROPIC_AUTH_TOKEN` 经
  `~/.claude` 软链落到 chenxihe 目录，`ak.py` 顶部自认「在用别人的凭据，违反 scope 规则」；
  仓内无 `.env`（`IDEALAB_AK` 未设）。调 API 会花到别人账上，按 `scope.md` 规则 3
  不擅自决定，交 owner 裁。
- 二值响应、平票排除、deeppurpose 泄漏敏感等 caveat 沿用 D2-①，全部落盘。
