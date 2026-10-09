# case_dr_aedit_01 · fixed · enough_tools

**母题描述、真值、判分口径见
[fixed·some 侧 README](../../fixed_some_tools/case_dr_aedit_01/README.md)——本页只记差异。**
（本题无 basic 档正式 case：basic = 裸答探针留痕，锚定档是 some，见该 README 的三档表。）

## 与 some 档的唯一差别：工具面多一个 Oracle

| 项 | fixed·some | fixed·enough（本格） |
|---|---|---|
| 题面 / 真值 / 判据 / 预算 | — | **全部相同**（判分器 `judge_enough.py` 一行逻辑不新写、整套转发 some，带 golden + truth 双重同源断言） |
| 工具面 | 库查询三件套 | 三件套 + **`predict_admet`** |

`predict_admet(smiles, prop="logS")` **恒给对值**（owner 2026-08-26 裁定）：库内分子给实测值，
20 个子分子给 ground_truth 同源拟合真值，域外分子 NOT_IN_DATABASE；调用与三件套
**共享同一 50 次全局预算**（`_call_local` 收口，白嫖不出第二个预算）。

## 本格的角色：对照，不是判别

enough 档验证"题面在信息充足时确实可解"、给工具收益一个上界。**能力判别只看档差
S(some)−S(basic)**（DECISIONS §2.6⑥），本格分数不作能力判别依据——预期各模型
在本格趋近满分（20 次 Oracle 调用在 50 次预算内），那是设计行为。
