# 01 · 建立安全网：跑通既有三个测试并装 pytest

Type: task
Status: resolved
Blocked by: —

## 为什么先做这个

要改造的 `tool_conflict_census.py` 有 1933 行，且是唯一产出过 4 轮漂移 0 结果的代码。改它之前必须有回归安全网，否则改坏了不会立刻知道，等跑完 730×22 才发现就晚了。

## 做什么

1. 在项目 venv（`.venv`，Python 3.12.13）装 pytest（已获授权）
2. 跑 `src/probe_headless/tests/` 三个文件：`test_tool_conflict_census.py`、`test_tool_conflict_candidate_pool.py`、`test_probe_tool_conflicts.py`
3. 记录初始通过/失败状态到本 ticket 的 `## Answer`

## 验收

- 三个测试文件的逐项通过/失败清单已落盘
- 若有失败项：判断是「代码坏了」还是「测试依赖已移动的路径」（`cases/` 已进 archive，测试很可能引用了失效路径）。后者属环境问题，修路径即可；前者要在改造前先修
- 若全绿：作为基线 commit 记下 sha，改造后逐项对比

## 备注

若测试大面积红且根因是路径失效，不要为了变绿而改测试断言——先确认断言本身仍然表达正确的契约。

## Answer

执行时间：2026-09-04。环境：`.venv`，Python 3.12.13。

**pytest 无需安装**——venv 里已有 pytest 9.1.1。计划里"装 pytest"这一步是我的误判，实际不成立。

**先撞上一个未声明依赖**。三个测试文件里有两个连收集阶段都过不去：

```
src/probe_headless/probe_tool_conflicts.py:19: from sklearn import __version__ as sklearn_version
E   ModuleNotFoundError: No module named 'sklearn'
```

`probe_tool_conflicts.py` 用 sklearn 跑 LogisticRegression 基线和 GroupShuffleSplit 划分（第 20–21、236、267 行），是真实运行依赖，但 `requirements.txt` 里没有它。为不破坏可复现性，版本没有随手取最新，而是按历史产物里记录的 orchestrator 环境锁定：`archive/cases/_methodology/tool_conflict_discovery/tool_manifest.json` 记的是 python 3.10.14 / rdkit 2025.03.6 / numpy 1.26.4 / **scikit-learn 1.7.2**，故装 `scikit-learn==1.7.2`。

**基线读数（逐文件）**：

| 测试文件 | 结果 |
| --- | --- |
| `test_tool_conflict_census.py` | 20 passed, 1 skipped, 4 subtests passed |
| `test_probe_tool_conflicts.py` | 11 passed |
| `test_tool_conflict_candidate_pool.py` | **13 errors**（全部 setUpClass 阶段） |

合计 31 passed / 1 skipped / 13 errors。

**13 个 error 是路径失效，不是代码坏了**。报错文本写的是 `Frozen source hash mismatch: conflicts.json`，但触发它的是同一个 `if` 里的前半个条件 `not path.is_file()`——`tool_conflict_candidate_pool.py:27` 的 `SOURCE_DIR` 指向 `cases/_methodology/tool_conflict_census/bbb_authoritative_v2`，重组后这个路径已不存在。产物本身完好，我把 archive 里的三份文件重算了 sha256，与代码第 29–35 行冻结的期望值逐位一致：

| 文件 | 实测 sha256 | 与冻结值 |
| --- | --- | --- |
| conflicts.json | `5e55fa1a…e3f43734` | 一致 |
| summary.json | `f8053e92…14fe2a7b` | 一致 |
| verification.json | `a3d7bb8f…945142d2` | 一致 |

所以断言表达的契约仍然正确，**不改断言**。修法归 ticket 02：产物落到 `data/bbb_census_v3/` 后，把 `SOURCE_DIR` 指过去，再重跑这 13 项验收。

**那 1 个 skip 是唯一真正加载模型权重的测试**（`test_tool_conflict_census.py:463`，闸门 `RUN_BBB_MODEL_INTEGRATION=1`）。本机没有任何模型 runtime（`~/.venvs/` 不存在），所以现在跑不了；它要等 ticket 05 建好 runtime 才能解封，届时必须跑绿——否则安全网覆盖不到"权重真的能载入"这一层。

**没做基线 commit**。验收里写的"若全绿则记基线 sha"前提不成立（有 13 red），且工作区有 4209 个未提交删除（重组遗留），此刻提交会把 owner 在途的重组一并固化，越权。基线以本 ticket 的读数为准。

## Comments
