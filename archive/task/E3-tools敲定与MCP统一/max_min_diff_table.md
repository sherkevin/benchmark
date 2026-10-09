# evidence_time 判据 max/min 对照重放表（修复一，任务 #14）

**结论先行**：68 条有效 trace 逐条以新旧两种 agg 口径重放 no_path_violation，共 **27 条翻转**——其中 **24 条误杀救回**（max 判违例、min 判降级/合法，gate False→True）、**3 条新增真逆序**（max 时被相等/缺证据降级掩盖、min 暴露 cp1→cp2 真逆序，gate True→False）。与监工预估（约 27 条：24 救回 + 3 新增）完全吻合。15 实验点 min 口径重算：cp1–cp6 reward / process / gate / gate_failed_cps / task_score **逐字段零分差**，仅 no_path_violation / path_violations 两诊断字段按修复目标变化。

## 1. 重放口径

- **代码链路**：复用 `evaluate.evaluate_no_path_violation`（判分唯一链路，内部调 `gateway.compute_evidence_time`），仅替换 evidence_map 的 agg 字段：max 口径=修复前（每项 `agg='max'`），min 口径=修复后（每项 `agg='min'`，缺省值）。
- **trace 清单**：`dag_poc/traces/*.jsonl` 共 113 个文件，其中 45 个 `*_stream.jsonl` 为 hook 事件流（零工具事件行，不可重放），**有效 trace 68 条**。（监工口径 69 条与本表 68 条差 1：统计口径差——本表按"每行均为含 tool 键的事件行"取严口径，hook 流一律排除。）
- **submitted/is_open**：按配套 `_response.json` 是否存在且非空取 submitted；按 case 文件是否含 gt 取 is_open（open 类豁免，3 条）。
- **模式分布**：compat 65 条 + open_exempt 3 条 + **strict 0 条**（全仓 68 条均为 legacy harness trace，无网关录像格式 trace；strict 口径的判别力由单测 path_ 组 6 用例承载，见 §5）。

## 2. 翻转明细（27 条）

### 2.1 误杀救回（24 条）：max 判违例 → min 判降级/合法，gate False→True

| # | trace | 事件数 | max 违例边 | min 结果 |
|---|---|---|---|---|
| 1 | case_antibiotic_sulfamethoxazole.jsonl | 73 | cp1→cp2、cp2→cp3 | npv=None（compat 降级） |
| 2 | case_antibiotic_sulfamethoxazole__bailian_glm-5.2.jsonl | 39 | cp1→cp2 | npv=None（compat 降级） |
| 3 | case_antibiotic_sulfamethoxazole__moonshot_kimi-k3.jsonl | 22 | cp1→cp2 | npv=None（compat 降级） |
| 4 | case_antibiotic_sulfamethoxazole__qwen3.7-max.jsonl | 15 | cp1→cp2 | npv=None（compat 降级） |
| 5 | case_anxiolytic_diazepam.jsonl | 68 | cp1→cp2 | npv=None（compat 降级） |
| 6 | case_anxiolytic_diazepam__qwen3.7-max.jsonl | 24 | cp1→cp2、cp2→cp3 | npv=None（compat 降级） |
| 7 | case_bronchodilator_theophylline.jsonl | 56 | cp1→cp2 | npv=None（compat 降级） |
| 8 | case_bronchodilator_theophylline__moonshot_kimi-k3.jsonl | 13 | cp1→cp2 | npv=None（compat 降级） |
| 9 | case_bronchodilator_theophylline__qwen3.7-max.jsonl | 18 | cp2→cp3 | npv=None（compat 降级） |
| 10 | case_cardiac_sotalol.jsonl | 74 | cp1→cp2、cp2→cp3 | npv=None（compat 降级） |
| 11 | case_cardiac_sotalol__bailian_glm-5.2.jsonl | 31 | cp1→cp2 | npv=None（compat 降级） |
| 12 | case_cns_metoprolol.jsonl | 57 | cp1→cp2 | npv=None（compat 降级） |
| 13 | case_cns_metoprolol__bailian_glm-5.2.jsonl | 40 | cp1→cp2、cp2→cp3 | npv=None（compat 降级） |
| 14 | case_cns_metoprolol__qwen3.7-max.jsonl | 12 | cp1→cp2、cp2→cp3 | npv=None（compat 降级） |
| 15 | qodercli_antibiotic_trace.jsonl | 9 | cp1→cp2 | npv=None（compat 降级） |
| 16 | qodercli_anxiolytic_diazepam_trace.jsonl | 13 | cp1→cp2 | npv=None（compat 降级） |
| 17 | qodercli_bronchodilator_theophylline_trace.jsonl | 14 | cp1→cp2 | npv=None（compat 降级） |
| 18 | qodercli_cardiac_sotalol_trace.jsonl | 11 | cp1→cp2、cp2→cp3 | npv=None（compat 降级） |
| 19 | qodercli_cns_metoprolol_trace.jsonl | 15 | cp2→cp3 | npv=None（compat 降级） |
| 20 | qodercli_cns_trace.jsonl | 15 | cp2→cp3 | npv=None（compat 降级） |
| 21 | qodercli_glm_5.2_antibiotic_sulfamethoxazole_trace.jsonl | 9 | cp1→cp2 | npv=None（compat 降级） |
| 22 | qodercli_glm_5.2_anticoagulant_warfarin_trace.jsonl | 11 | cp1→cp2 | npv=None（compat 降级） |
| 23 | qodercli_longacting_antidepressant2_mirtazapine_trace.jsonl | 8 | cp1→cp2 | npv=None（compat 降级） |
| 24 | qodercli_longacting_antipsychotic_haloperidol_trace.jsonl | 6 | cp1→cp2 | npv=None（compat 降级） |

误杀违例边分布：`cp1→cp2` × 21、`cp2→cp3` × 9——全部是"结论后回访再查文献/再检索"被 max 取到回访时刻造成的假逆序，与监工致命 1 实测结论一致。

### 2.2 新增真逆序（3 条）：max 被降级掩盖 → min 暴露 cp1→cp2 真逆序，gate True→False

| # | trace | 事件数 | max 结果 | min 违例边 |
|---|---|---|---|---|
| 1 | qodercli_highsolubility_antidiabetic_metformin_trace.jsonl | 32 | npv=None（eom=cp4→cp5、cp5→cp6） | cp1→cp2 |
| 2 | qodercli_moderate_ace_inhibitor_ramipril_trace.jsonl | 15 | npv=None（eom=cp4→cp5、cp5→cp6） | cp1→cp2 |
| 3 | qodercli_peripheral_cardiosafe_antihypertensive_atenolol_trace.jsonl | 8 | npv=None（eom=cp5→cp6） | cp1→cp2 |

解读：这 3 条 trace 的文献检索首次时刻晚于首次建池（先建池后补文献），max 口径下 cp1/cp2 被取到各自最晚时刻而落入"相等/缺证据"降级分支（gate 不卡），min 口径取首次建立时刻后真逆序显形——**这是判据改 min 后拓扑验证判别力增强的实证**。

## 3. 未翻转 trace（41 条）npv 口径一致性

| mode | npv（max=min） | 条数 |
|---|---|---|
| compat | False | 26 |
| compat | None | 12 |
| open_exempt | None | 3 |

（npv=False 的未翻转条 = min 改不掉的真逆序；npv=None = 相等/缺证据降级；open_exempt = open 类豁免。）

## 4. 15 实验点 min 口径重算（evaluate.py 全量实跑）

- 命令：`cd dag_poc && python3 ../src/benchmark/evaluate.py`（修复后代码，min 缺省口径）。
- 逐字段比对（max 基线 = 修复前 `_all_results.json` 快照 /tmp/e3_task14_maxbaseline/）：
  - **零差异字段**：case / model / gt / best / hit / submitted / gold_pass_set / tools / cp1 / cp2 / cp3 / cp4 / cp5 / cp6 / process / gate / gate_failed_cps / task_score / path_validation_mode（15/15 点、19 字段全零差异——fixed 判分与门控结论不受影响）。
  - **按修复目标变化的诊断字段**（任务 #21 M5 数字订正：原 §4 写"8 点 / 11 点（8 清空、3 换边）"与 §2.1 明细 9 条自相矛盾，影响面评审程序化比对实测为下列数字；本轮重放复核一致）：no_path_violation **9 点 False→None**（antibiotic×bailian/kimi/qwen、diazepam×qwen、theophylline×kimi/qwen、sotalol×bailian、metoprolol×bailian/qwen）、path_violations **13 点边清单变化（9 清空、4 换边）**——换边 4 点均由 cp1→cp2 换为 cp2→cp3（diazepam×bailian、theophylline×bailian、sotalol×qwen 另含保留边、metoprolol×kimi）；gate 仍为 False（15 点 gate 全 False，由 cp 阈值卡死，与既往结论一致）。分数字段零差异结论不变，本项仅订正诊断字段计数。
- 与任务 #8 基线（/tmp/e3_task8_baseline/）再比：分数字段同样零差异。
- `_all_results.json` 已按 min 口径重新生成入库（分数不变、诊断字段按新口径更新，基线账本口径自本 commit 起为 min）。

## 5. strict 模式 npv 分布（替代"零差异"的判别力证据）

| 口径 | strict trace 数 | compat trace 数 | open_exempt |
|---|---|---|---|
| min（修复后） | 0 | 65 | 3 |

min 口径下 compat 65 条的 npv 分布：

| npv | 条数 | 含义 |
|---|---|---|
| False | 29 | 真逆序（含 min 新暴露 3 条） |
| None | 36 | 相等/缺证据降级（compat 不卡 gate） |
| True | 0 | 全序合法 |

**strict 口径判别力证据**：全仓 68 条历史 trace 均为 legacy harness 格式（compat），strict（网关录像）分布为空——既往"15 点零差异"恰因全 compat 且 gate 已被 cp 阈值卡死，属巧合证据（监工指出）。strict 判别力改由单测 path_ 组 6 用例直接承载（strict 模式真实跑）：合法序 / 无 compare 合法（fallback prio 破平局）/ 回访迭代必过 / 逆序必红 ×2 / 跳步必红——其中回访用例改前红、改后绿，证明 min 口径生效且有判别力。

## 6. 治理登记（两文件在 E3 写作区外，本轮不碰，供修复七回写）

- INTERFACE_CONTRACT §3.3 L266 `evidence_time(cp)` 行"满足该 cp 所需录像事件的**最晚**时刻"
- INTERFACE_CONTRACT §4 L351 "cp3 = 最后一次相关 get_property 的时刻；cp2 = 最后一次 search_candidates"
- ADR-0008（governance/adr/0008-判分符号不删补接口层.md）§6 L56 "no_path_violation → 由 evidence_time(cp) 定义（录像最晚事件时刻，逐边比较）"

三处均需改为"首次建立时刻（min 聚合）"口径，修订请求拟文见任务 #14 完成汇报。
