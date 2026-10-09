# 指令：跑 dopant_01·open 满批 30 run

| 字段 | 值 |
|---|---|
| 日期 | 2026-08-19 |
| 发件方 | 本地（jingwu mac） |
| 类型 | 指令 |
| 关联 commit | `3df382a` |
| 需对方做什么 | `git pull` 到 `3df382a`，跑下面这一条命令，按 §4 格式写报告，**跑完停下** |
| 回执状态 | ⬜ 待回执 |

---

## 1. 一句话

跑 `dopant_01·open` 满批 30 run。这是四道主力题里数据缺口最明确的一道
（当前只有 **8 个** run 且单批次），而且它的判分器上一轮刚修好（cp3/cp4 移出总分），
现在跑出来的分数才有区分力。

---

## 2. 命令（照抄执行）

```bash
cd /mnt/ai4sci_develop_fast/jingwu/benchmark
git pull origin master          # 应到 3df382a
cd src/probe_headless
../../.venv/bin/python sweep_ws.py ws_runner_dopant_open.py 5 d 2>&1 | tee /tmp/dopant_open_d.log
```

**轮次前缀用 `d`**。已占用的：`r`（历史 8 run）、`b`（本地全灭那批）、`cprobe`（你的探路批）。
重名会把两批数据混在一起，那是本项目犯过三次的错。

预期：6 模型 × 5 轮 = 30 run，并发 30。你实测单 run 最长 727 秒，估 15–30 分钟。

---

## 3. 跑之前请确认两件事

**① `sweep_ws.py` 的汇总行现在准了。**
上一轮你报的 X-4 已修：它之前读 `answer_json_exists`（错名），恒报「交卷 0」。
现在读 `answer_exists`，与 `ws_isolation.py` 写出的字段一致。
所以这次汇总行的「交卷 N」可以直接采信。

**② 判分口径变了，`total` 会比你探路批的高。**
上一轮你报的 X-6 已按口径变更处理：`cp3`/`cp4` **移出总分**，
总分只取 `cp1`（分类）+ `cp2`（逐点数值）两关。

理由是你诊断的现象加上我查出的第二层根因：

- `cp3`/`cp4` 的自变量是 `_max_pr_above` 返回的**网格点**，误差只能是网格间距的整数倍。
  尾部间距 0.1 是尺度 `PR_SCALE=0.024` 的 4 倍 —— 柯西 loss 本身平滑，
  但喂给它的输入是离散的，合成后仍是阶梯门（你实测三家 cp3 同为 0.200）。
- `cp3`/`cp4` 是从同一条 `predictions` 曲线抽的特征点，而 `cp2` 已逐点比对整条曲线
  —— **两者重复计分**。

用你探路批的真实关分复算：

| 模型 | 你报的 total（4 关） | 新 total（cp1+cp2）/2 |
|---|---|---|
| qwen3.7-max | 0.4287 | 0.7540 |
| qwen3.5-plus | 0.4112 | 0.6950 |
| deepseek-v3.2 | 0.3976 | 0.6920 |

3 模型 total 极差 **0.031 → 0.062**，区分度翻倍。
`cp3`/`cp4` 仍保留在 details 里作记录，只是不进总分。

---

## 4. 报告要写什么

按 `README.md` §2（必答四件事）+ §3（跑批附加字段）。最要紧的几项：

1. **命令原文 + 轮次前缀**（`d`）+ `git rev-parse --short HEAD`
2. **逐 run 七字段**：`run_id` / `returncode` / `seconds` / `api_error` /
   `tool_leak` / `answer_exists` / `tool_calls`
3. **判分表**：逐 run 的 `total` + `cp1_class` / `cp2_numeric`
   （`cp3_high_th` / `cp4_crit` 也贴出来，我要看它们是否还是同分——那能验证 X-6 的诊断）
4. **按模型聚合**：每模型的 n / 均值 / 轮间极差 ← **这批的主要目的**
5. 失败 run 的 `trace.jsonl` 尾部 50 行（若有）

判分命令：

```bash
cd /mnt/ai4sci_develop_fast/jingwu/benchmark
.venv/bin/python cases/materials/atom/open_basic_tools/case_sc_dopant_01/judge_dopant_open.py \
  cases/materials/atom/open_basic_tools/case_sc_dopant_01/runs --batch
```

---

## 5. 这批数据要回答的三个问题

写报告时请直接对着这三条给结论，不用等我问：

| # | 问题 | 怎么看 |
|---|---|---|
| 1 | 尺子②（fixed 优于 open）在这道题上到底是正还是反？ | 本地 fixed 侧 64 run 均值 **0.778**；这批 open 均值与它比。目前 8 run 给出 open 0.799（反向），但 n=8 不足以定论 |
| 2 | 单模型跨轮稳不稳？ | 每模型 5 轮的极差。fixed 侧所有模型挤在 0.75–0.85 窄带，若 open 也这么窄，说明**这道题模型间不分高下**，是真问题 |
| 3 | X-6 修完后 cp3/cp4 是否仍同分？ | 若 30 run 里 cp3 仍大面积落在同一档，说明"离散网格 → 阶梯门"的诊断成立，可作为它们不该进总分的进一步证据 |

---

## 6. 纪律

**跑完这一批就停**，写完报告等我判分后再决定下一批（承 `README.md` §7 越界清单第 3 条
与 `调试机跑批指南.md` §7 纪律 1）。

不要顺手把 `constraint_02` 那批也跑了 —— 那道题的 runner 我虽然写好并离线自测过
（五项全过），但它的稳定性本身有疑问（fixed 侧批间极差 0.721），
得先看这批 dopant 的结果再定要不要动它。

---

## 7. 顺带同步：census 两道题已删

`3df382a` 里删掉了 `census_01` / `census_02`（4 个 case 目录、2936 个 run、9 个专用脚本）。

理由：`census_01` 要 agent 做的四件事（数库里有多少个、数超阈值的、算均值、算中位数）
**全是"查性质 + 算术"，零 infer action** —— 测的是会不会用工具，不是会不会科学推理。
`census_02` 只多一步"按化学式有没有 Hg/La 分组"，是字符串匹配、不需机理知识，够不上 infer。

所以现在只剩 **4 个 case = 8 道题**（fixed/open 各算一道）。
每道题的完整 query 原文与分数分布见
[`task/task19/第一组8道题_query原文与分数分布.md`](../task19/第一组8道题_query原文与分数分布.md)。

你 pull 后 `collect_scores.py` 不会再去找 census 目录（映射表已同步删）。
