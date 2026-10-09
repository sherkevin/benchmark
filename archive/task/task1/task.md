# Task 1 — 提问方式对比实验：一次全量 vs. 分步提问

## 1. 背景与被测系统

- **被测平台**：DAMO Nobel（NobelClaw）面向材料与药物研发的科研智能体平台
  - 前端：http://nobelclaw.aliyun-inc.com/
  - 后端 API：`https://mlflow-debug-ymwhij-058852-8001.cloud-ide.alibaba-inc.com/api/v1`
- **Agent 能力（skills）**：
  - `drug_discovery`（mock，快）：validate_target / search_drug_literature / generate_molecules / predict_admet / search_molecule_database / predict_binding / simulate_virtual_cell / generate_drug_report
  - `elements`（真实端点）：crystal_predict / crystal_generate(慢, ~360s) / compare_materials / literature_search / generate_report
  - `protein`（mock）：predict_structure / detect_binding_sites / generate_structure_image
  - `literature`（mock）：fetch_bib_info / download_poscar / screen_literature
  - `embodied`（mock）：plan_manipulation_task / check_motion_feasibility / simulate_execution
- **交互机制**：
  - 建会话：`POST /sessions` body `{project_id}`
  - 对话：`POST /projects/{pid}/sessions/{sid}/chat` body `{message, deep_thinking}`，返回 SSE 流
    （事件：task_start / eta_warning / tool_call / message / ping）
  - 任务分解确认：复杂任务时 agent 会调用 `propose_decomposition` 并等待 `{message:"", confirmed_tasks:[...]}`
  - 读消息：`GET /sessions/{sid}/messages`

## 2. 实验目标（GOAL）

> 验证：在同一个 session 里**一次性输入全部 prompt（全量）**，与**把大 prompt 拆成步骤级、一段一段输入、一问一答（分步）**，
> 哪种提问方式能让 agent 给出**更好的回复效果**。
> 持续运行多组配对对比，直到能以**高置信度**得出结论为止。

## 3. 实验设计（受控变量法）

对每个复杂任务 T，先拆成有序的原子子指令 `[s1, s2, …, sN]`，二者使用**完全相同**的子指令、目标**同一最终交付物**，唯一变量是「投递方式」：

- **条件 A — 全量（all-at-once）**：新建 session，**一条**用户消息包含全部 N 条子指令（编号列出）。
  若 agent 提出任务分解则确认一次，之后不再人工干预，让其跑到结束。
- **条件 B — 分步（step-by-step）**：新建 session，发 **N 条**用户消息，第 k 条只含 `s_k`；
  每条都等 agent 完整回答后再发下一条。

每组对比放在 `session_i/` 文件夹下，含两个文件：
- `A_all_at_once.md`：全量提问的 prompt 与完整回复
- `B_step_by_step.md`：分步提问的逐条 prompt 与逐条回复
- （另存 `eval.md` 记录该组打分与判定）

## 4. 评估维度（什么叫"更好"）

每组对 A / B 在以下维度打分（1–5）并给出判定（A 胜 / B 胜 / 平）：

1. **子步覆盖度**：N 个子步是否都被真正执行/回答
2. **工具调用正确性**：是否调用了正确的 skill 工具、调用次数是否合理
3. **数值完整性与自洽**：关键数值（Tc、形成能、Kd、ADMET 等）是否齐全且前后一致
4. **最终综合质量**：是否产出整合性的结论/报告（而非碎片）
5. **稳健性**：是否中途卡住、跳步、漏步、编造结果、需额外催促
6. **效率**：总耗时、总工具调用数、轮次

## 5. 任务集（覆盖不同 skill，避开慢的 crystal_generate）

- **session_1**：药物发现全流程（GLP-1R 多肽，drug_discovery，5+ 步）
- **session_2**：材料性质预测（带 CIF 路径跳过慢生成：Tc + 形成能 + 文献 + 报告）
- **session_3**：材料对比（两种材料 Tc/形成能/稳定性对比）
- **session_4**：蛋白结构（predict_structure + binding_sites + 图）
- **session_5+**：按需补充（另一药物靶点 / 另一材料体系），直到结论稳定

## 6. 高置信度停止准则

当满足以下任一条件即停止：
- 累计 ≥4 组配对，且 ≥75% 的组指向同一方向（A 或 B 一致更优），且能给出机制性解释；或
- 出现明确且可复现的失败模式，足以定性区分两种方式。
若结果摇摆则继续追加 session。

## 7. 方法选择说明

采用 **curl/HTTP API**（而非 Chrome 点击）：更快、可脚本化、拿到的 transcript（含 tool_call/tool_result/数值）更完整、可复现。Chrome 仅用于初始勘察界面。
