# 过程分拆解表骨架（各 checkpoint 通过率 + gate 失败归因）

> ⚠️ **占位数据，非真实结果**。所有带 † 的数值为随意填入的占位数，仅验证表结构；**正式跑测未开**，占位数据不得出现在任何对外文档。真实数据落盘时本文件同步改名（去占位标识），以便下游 glob 聚合可识别。

**字段口径**：与 `src/benchmark/evaluate.py` 对齐——cp 得分 sᵢ 由 `compute_scores_*` 逐 cp 计算，`process = Σ(wᵢ·sᵢ)/Σ(wᵢ)`（`CP_WEIGHTS`）；gate 失败时 `aggregate()` 输出 `gate_failed_cps`（未达阈值的 cp 列表）+ `no_path_violation`（路径拓扑项）。

## A. 各 checkpoint 得分/通过率（fixed 侧，按模型 × harness × 题类）

cp 语义（evaluate.py 实测口径）：

| cp | 判据（一句话） | 适用题类（现行实现口径） |
|---|---|---|
| cp1 | 文献检索门控：≥1 次检索且命中 ≥1 篇论文（二值） | type1 / type2 |
| cp2 | 候选池构建：池非空 × 池内分子可查证比例 × gt 在池内（三项乘积） | type1 / type2 |
| cp3 | 性质评估覆盖：已评 (分子 × 必需性质) 覆盖率 | type1 / type2 |
| cp4 | 筛选合格集：与金标准合格集的 F1（2tp/(提交+金标)） | type1 / type2 |
| cp5 | 目标量排序：与金标准排序的匹配度 | **现行实现对一切 fixed case 按 6 项 cp 加权/设阈值**（见下方 ⚠️ gap） |
| cp6 | 终答提交：best == gt 记 1.0；best ∈ 合格集记 0.5；否则 0 | type1 / type2 |

> ⚠️ **cp5 口径 gap（已登记上报，待仲裁）**：`query_spec.json → per_type[type1].scoring` 声称 type1 为 5 checkpoints、无 cp5；但 `evaluate.py` 现行实现的 `CP_WEIGHTS = {cp1:1, cp2:2, cp3:2, cp4:2, cp5:1, cp6:2}`、`CP_THRESHOLDS = {cp1:0.6, cp2:1.0, cp3:0.8, cp4:0.5, cp5:1.0, cp6:1.0}` 对**一切 fixed case 恒含 cp5**（process 按 6 项加权、gate 按 6 项阈值）。本表如实描述现行实现；type1 case 不产出 cp5 排序产物，cp5 恒判 0，会拉低 process 并令 gate 因 cp5<1.0 失败。evaluate.py 属 E3 写作区不可改，**此 gap 已报监工请 E3/owner 仲裁**（仲裁前 type1 的 process/gate 数字按现行实现口径解读）。

| 模型 | harness | 题类 | cp1† | cp2† | cp3† | cp4† | cp5† | cp6† | process 均值† |
|---|---|---|---|---|---|---|---|---|---|
| qwen3.7-max | thinagent | type1 | 0.90† | 0.80† | 0.65† | 0.60† | 0.00† | 0.55† | 0.75† |
| qwen3.7-max | thinagent | type2 | 0.88† | 0.78† | 0.62† | 0.58† | 0.40† | 0.50† | 0.70† |
| …（M×H 全组合同构展开） | | | | | | | | | |

> † 占位数据，非真实结果。cp 列报均值（连续分）；type1 的 cp5 列在现行实现下恒为 0（不参与产出不等于不参与加权，见上方 ⚠️ gap），待仲裁后按裁定口径调整本列解读。

## B. gate 失败归因（gate_failed_cps + 路径违例拆解）

gate = 全 cp ≥ 阈值 且 `no_path_violation` 通过。失败 run 按下表归因到首个未达项（一条 run 可多因，归因列允许多选计数）：

| 模型 | harness | 题类 | gate 失败 run 数† | 归因: cp1† | 归因: cp2† | 归因: cp3† | 归因: cp4† | 归因: cp5† | 归因: cp6† | 归因: no_path_violation=False† | 归因: path_validation_mode 降级† |
|---|---|---|---|---|---|---|---|---|---|---|---|
| qwen3.7-max | thinagent | type1 | 23† | 2† | 5† | 8† | 6† | 12† | 9† | 1† | 1† |
| …（M×H 全组合同构展开） | | | | | | | | | | | |

> † 占位数据，非真实结果。
> 归因口径说明：
> - `gate_failed_cps` 直接来自 `aggregate()` 输出（未达阈值 cp 列表），**归因列按 cp 分列计数，不另立口径**；
> - `no_path_violation=False`（路径拓扑违例：跳步/逆序/缺节点）单独成列——它是过程卖点核心判据，不能淹没在 cp 归因里；
> - `path_validation_mode` 为 `compat` / `no_recording` / `unavailable` 的 run 属**降级判定**（拓扑项未严格判），单独计数披露，不与严格判定（strict）的通过率混合。

## C. 渲染校验清单

- [ ] 所有 † 替换为真实 aggregate() 输出；
- [ ] type1 行 cp5 列按仲裁后口径解读（现行实现：恒 0 且参与加权/阈值，见 ⚠️ gap）；
- [ ] 归因计数之和 ≥ gate 失败 run 数（多因 run 重复计数，脚注披露）；
- [ ] 文件头「占位数据」警示已随真实数据更新。
