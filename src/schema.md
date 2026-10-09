# 任务规格 schema(src)

一个任务 = 一个 JSON 文件(见 `tasks/` 与 `tests/fixtures/`)。字段:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | str | 唯一标识 |
| `domain` | str | `drug` / `materials` / … |
| `goal` | str | 目标(自然语言;**目标明确性在 L0-L3 恒定**) |
| `success_criteria` | str | 何为成功(全阶段 checkpoint 通过) |
| `provenance` | str | **真实锚 vs 作者撰写** 的标注(诚实纪律) |
| `prompts` | obj | `{L0,L1,L2,L3}`:目标明确性恒定、**路径指导性递增**(PRD §2.3) |
| `stages` | list | **金标准轨迹**:每个 stage = 一个 checkpoint(对照裁判判定点) |
| `intervention_cards` | obj | `stage_name -> {D1,D2,D3}`:深度递增的分级介入卡(PRD §2.5) |
| `counterfactuals` | list | 反事实变体(换靶点/改目标/改起点;抗污染 PRD §2.2) |

## stage(checkpoint)字段

| 字段 | 类型 | 说明 |
|---|---|---|
| `name` | str | 阶段名(如 decompose/screen/predict/select) |
| `kind` | str | `ranking` / `value_set` / `selection` |
| `pass_threshold` | float | 通过阈值(作者撰写,可调) |
| `high_risk` | bool | 是否高风险动作节点(用于**许可校准**诊断) |
| `gold` | obj | kind 专属金标准(见下) |

### gold 按 kind

- **ranking**(虚拟筛选/排序):
  `{"candidates":[id...], "actives":[id...], "scores":[pChEMBL...]}`
  判定:hits@3≥1 且 auroc≥`pass_threshold` 且 ef@10%≥2(复用 task3 指标)。
- **value_set**(数值预测:Tc/带隙/ADMET…):
  `{"targets":{name:gold_value}, "tol":{name:abs_tol}}`
  判定:落在容差内的目标占比 ≥ `pass_threshold`。
- **selection**(分解/选型/决策):
  `{"options":[item...], "correct":[item...]}`
  判定:选择对 correct 的 F1 ≥ `pass_threshold`。

## 真实性纪律(强约束)

- `gold` 的**科学数值**只允许来自**真实数据/文献**(在 `provenance` 注明来源:MolBench data_full / ChEMBL / Materials Project / task5 GT findings 等);**不得编造数字**。
- 阈值、介入卡、prompt 文本、反事实设计为**作者撰写**,需在 `provenance` 标注。
- 合成 fixture(仅验证机器、非科学任务)必须在 `provenance` 显式声明「合成,非真实科学数据」。
