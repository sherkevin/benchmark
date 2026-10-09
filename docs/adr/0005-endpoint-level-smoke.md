# ADR-0005: 冒烟判据改端点级，正式阶段一次性不重试

- 状态：Accepted
- 日期：2026-09-11（2026-09-13 独立成文）
- 决策人：jingwu（计划文档口径 4、口径 5，2026-09-11 确认）
- 规格落点：`docs/远程跑批任务_全部工具.md`「第二轮批次总派发」§一、§二

## 背景

冒烟是用三个探针分子试跑，确认工具能出数，过了才放行双轮全量。旧代码要求三个分子在每个端点上全部成功，见 `src/probe_headless/drug_prediction_campaign.py` 的 `:718` 与 `:549`。而正式跑 730 个分子时只要求记录失败，并不要求全部成功。

闸门比它守的那道门更严，结果是可用工具被挡在外面。CypReact 是实例：它的解析器不认蔗糖，九个端点全判失败，于是整个工具被永久挡住，而它对另外 728 个分子都能正常出数。

三个探针分子写死在代码 `:175-179`，蔗糖当初是作为血脑屏障的阴性对照选的，对测代谢酶反应性的工具没有对照意义。用一个对当前工具无意义的分子去决定它能不能进数据集，判据本身就错了。

## 决策

**一、冒烟判据改端点级。** 四条规则：

1. 每个端点至少有一个探针分子出数，且所有出了的值都合法。合法指数值有限、分类值在合法集合内。
2. 某个探针分子在某端点出不来数，记录该分子该端点失败，不阻塞进入全量。
3. 某个端点在全部探针分子上都没出数，这个端点拦下，说明它根本不工作。
4. 全部端点都没出数，整个工具拦下。

**二、正式阶段一次性，不重试。** `smoke`、`full-r1`、`full-r2` 每个阶段只允许一次尝试。失败即闭账，要改输入、改 warning 处理或改适配逻辑，必须新建 campaign，不能在原 campaign 里补跑。

**三、判据改版后整批重跑。** 旧批次封存留档，已有的失败证据一条都不删。开新批次，所有工具在新判据下统一重跑冒烟，这样整个数据集只有一套判据版本。

## 理由

冒烟的职责是判断「这个工具在这个批次上能不能工作」，不是判断「它在三个特定分子上表现完美」。端点级把判据落在工具的能力单元上：一个端点只要在某个分子上出过合法值，就证明该端点的解析链路是通的；某个分子出不来数是那个分子的性质，记进失败清单即可，主表本来就有失败清单这个位置（ADR-0002、计划文档口径 6）。

一次性约束是审计强度，不是工程方便。允许原地重试，等于允许用多次尝试挑选成功结果，失败现场会被覆盖，「这个工具在这批输入上失败了」这条证据就消失了。CypReact 与 PKSmart 的失败之所以还能被 owner 裁定，正因为现场没有被重跑覆盖。

## 后果

1. 需补两项测试：单分子失败仍能进入 `full-r1`；某端点在全部探针分子上无值时仍被拦下。现有 20 项批次测试与 32 项运行时核验测试不得回归。
2. 验收必须在真实批次上复现一次「单分子失败仍可进全量」。实现与验证归远程工程师，因为真实批次和已装工具只在远程机器上。
3. 失败闭账的工具如需新实验，新报告必须同时引用旧失败现场，不把旧失败从总账里删除。CypReact 待 owner 决定是否允许改输入规范化或输出语义；PKSmart 待决定 warning 由 worker 重定向到 stderr，还是继续按严格 stdout 合同判失败。
4. 首批十工具的现行终态由此而来：8 工具 38 端点双轮交付，CypReact 9 端点与 PKSmart 3 端点共 12 端点失败闭账。见 `governance/交接_20260912_首批正式跑批完成状态与后续TODO.md` §2.2、§2.3。

## 当前代码状态：实现已于 2026-09-13 落地

本节初版（2026-09-13 早）记的是「决策已立，实现未做」。同日稍晚实现已落地，本节按落地后的事实重写，并保留旧判据的位置作对照。

**落地范围：两个跑批控制层共四处旧分子级判据全部改为端点级。** ADR 表格原先点名三处，落地时核出**第四处**——`execute()` 在写回执时的盖章闸。这一处是真正决定 smoke 回执终态的地方，只改另三处会让它继续把单分子失败盖成 `failed_protocol`，而 `_load_validated_run` 只认 `succeeded` 回执，于是另三处的放宽变成空转。四处必须一起改。

共享判据函数 `smoke_endpoints_without_values(rows)` 落在 `drug_tool_adapters.py`，两个控制层都已 import 它作 `adapters`，避免两套判据漂移。值的合法性（数值有限、分类值在合法集合内）由 `validate_long_rows` 在更早环节强制，因此端点级闸只需「每个端点至少一行 `ok`」这一条。

| 文件与行（改前） | 旧判据 | 改后 |
| --- | --- | --- |
| `drug_prediction_campaign.py:629`（`execute()` 盖章闸，**ADR 表格未点名的第四处**） | `if phase == "smoke" and status_counts != {"ok": len(validated_rows)}: raise ValueError(...)` | 调 `smoke_endpoints_without_values`，仅当存在「全探针无值」端点时盖章 `failed_protocol` |
| `drug_prediction_campaign.py:724`（`_load_validated_run` 读取闸） | `if phase == "smoke" and counts != {"ok": len(rows)}: raise RuntimeError(...)` | 同上，端点级 |
| `drug_prediction_campaign.py:555`（进 `full-r1` 前复查） | `if any(row["status"] != "ok" for row in smoke_rows): raise RuntimeError(...)` | 删除冗余的分子级复查，改为委托 `_load_validated_run`（它已拒非 `succeeded` 回执并端点级复查） |
| `numeric_qualification_campaign.py:768`（`_phase_status`） | `if phase == "smoke" and counts != {"ok": terminal_rows}: return "failed_protocol"` | `_phase_status` 末参由聚合 `terminal_rows` 改为 validated `rows`，调 `smoke_endpoints_without_values`；两个调用点（`:999` run、`:1257` verify）都已持有 rows。紧邻的全量宽松口径不变 |

第三批两个单工具驱动脚本 `jplogp_cdk_campaign.py:442` 与 `xlogp3_pug_campaign.py:568` **不在本 ADR 点名范围**（它们不是跑批控制层，是已裁定/已交付的单工具脚本），保持原样不动；改后复核其四个 campaign 的 verify 退出码仍为 0/0/1/0，与改前逐一致。

三个探针分子仍写死在 `drug_prediction_campaign.py:175-179` 的 `build_smoke_inputs()`，`numeric_qualification_campaign.py:392` 调的就是它——本 ADR 只改判据，不改探针集合。

**测试。** ADR §后果 1 要求的两项测试已补，并各做了变异验证证明断言真能挡住回退：
- `test_drug_prediction_campaign.py::test_single_probe_molecule_failure_still_enters_full_runs`（规则 2：单探针分子失败仍进 full-r1）。把盖章闸改回分子级，此测试失败。
- `test_drug_prediction_campaign.py::test_endpoint_without_any_probe_value_blocks_smoke_and_full`（规则 3：某端点全探针无值仍拦下）。把共享函数改成恒返回 `[]`，此测试失败。
- 数值层对等两条：`test_smoke_single_molecule_failure_does_not_block_full_phases`、`test_smoke_with_no_ok_rows_anywhere_still_fails_protocol`（规则 4）。

基线对账（§后果 1「20 项批次测试与 32 项运行时核验测试不回归」）：改前 `test_drug_prediction_campaign.py` + `test_verify_runtime.py` 合跑 52 passed，改后 54 passed（新增 2 条药物层测试，无一回归）；三个相关测试文件改前 63 passed，改后 67 passed（新增 4 条）；全仓改前 169 passed 1 skipped，改后 173 passed 1 skipped。

**真实批次复现（§后果 2 的验收判据）。** 本机同时持有代码、`.bench_runs` 封存现场与已装运行时，因此复现可在本机做，不必等远程。唯一的真实「工具+探针分子」失败案例是 CypReact 对蔗糖：封存现场 `.bench_runs/drug_prediction/drug_tools_20260908/runs/cypreact/smoke/20260910T145931Z-8adfad45/` 的回执在旧判据下被盖成 `failed_protocol`（`protocol_error: Smoke produced non-ok terminal rows: {ok:18, tool_error:9}`）。对该真实冻结证据**只读**施加新端点级判据：9 个端点各 `ok 2/3`，无死端点，新闸放行进全量；旧分子级闸则因 `counts != {ok:27}` 拦下。复现全程只读，封存回执的 `failed_protocol` 历史终态不被改写。

两个封存的正式 campaign 在新代码下 `verify` 仍 `exit 0`（`drug_tools_20260908`、`numeric_qualification_formal_20260911`），四个第三批 campaign 退出码亦不变。判据改版不改变任何已交付 campaign 的既有裁定。

**仍归 owner、本步不自行启动的一件事：CypReact 是否在新判据下重开一个 campaign。** 真实批次复现已证明新闸会放行 CypReact 的 smoke，但 ADR-0005「正式阶段一次性」与交接文档 P6 都规定：CypReact 旧 smoke 现场封存不改，要出全量必须**新建 campaign**，而是否允许改输入规范化或输出语义本身是 owner 裁定项。本步只落地判据，不新建任何 CypReact/PKSmart campaign。第二轮批次（§二）按「整批在新判据下重跑冒烟」推进时，CypReact 的重开随该批次走，须 owner 放行。

## 相关

- [ADR-0004](0004-verification-label-level.md)：核验准入判标签级，与本 ADR 同属 Step 10 补立
- [ADR-0003](0003-step1-verification-protocol.md) §一：L3/W/V 三档。本 ADR 改的是 L3 进入判分前的冒烟闸门，不改档位定义
