# Task E3-03 — 接匿名映射表与材料侧身份键 + 补跑 e2e

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E3-03 / `task/E3-tools敲定与MCP统一/` |
| **类型** | Implementation（接入 + 收尾） |
| **状态** | ✅ **已交付**（§1/§2/§3 全部完成，三维评审修复已收口；复核 HEAD=b33e3a0 全 PASS：143 passed + 1 skipped 零回退。收口章节见 report.md §9，任务 #40 状态对齐） |
| **负责人** | E3（工具线） |
| **预计起止** | 2026-08-06 ~ （约半天） |
| **上游来源** | E1-03 的匿名映射表（已落盘）· E1-04 的材料侧身份键（同期交付）· task-02 三次验收意见 |
| **下游消费方** | **E5**（open 题的 grounding 判定）· **E6**（跑测前置） |

---

## 一句话目标

> 把 E1 交付的匿名映射表与材料侧身份键接进来，让 grounding 的第三条判据在真实数据上可判；并把因缺包而 skip 的 e2e 测试补跑。

---

## 0. task-02 三次验收通过

5 项致命 + 新增 1 项全部修完。实测：`@mcp.tool` 23→31、9 个契约工具全部暴露且走 `_contract_call` 统一转调、dummy atom 规范化正确（`[2*][1*]` → `[1*][2*]`、`a2b` 返 None）、占位直通残留 0 处、`evaluate.py` 的 KeyError 已修。测试基线 31 → **126 passed + 1 skipped**。

两处值得说：**dummy atom 那项你顺手把"解析失败原样返回"的静默 fallback 也修了**（我上次只当副作用提了一句）；**隔离证据改成指向 `rehearsal_logs` 实跑留痕并主动声明"本文件不构成隔离已验证依据"**——这是照 E1 的做法学的，对。

**一个提醒**：交付后状态行又落后于实际（标着"致命 2/3 未修"而实际全修完）。**收尾时把状态行一并更新**。

---

## 1. 接匿名映射表（主任务）【✅ 已完成：commit 4acdd60（任务 #36）；评审修复随 df93eca；证据：rehearsal_logs/grounding_condition3_realdata.log 三判据全 True + test_gateway_rdkit_grounding.py 两实跑用例】

E1 已落盘 `dag_poc/data/anonymization_mapping.json`。**你不要自建映射**，按约定接口取用。

现在的问题是：dummy atom 的规范化能力已经验证可用（`[1*]CC` 能解析、`[2*][1*]` 能规范化），但**还没在真实数据上跑通端到端**——A 侧实体现在的 `formula` 长这样：`[16*]15[19*]25[22*]3[81*]1`。

**要做**：
1. `edit_molecule` / `normalize_structure` 在真实匿名数据上跑通——输入是 A 侧的匿名结构，输出能与 `G_B` 做 `==` 判定；
2. grounding 条件③（`normalize(edit_op) == answer`）在真实数据上验证一次，**给出实跑证据**；
3. 契约里把条件③从"占位、当前无鉴别力"的标注**改为已实现**（前提是真的验过了）。

---

## 2. 接材料侧身份键【✅ 已完成：commit d3b7d4a（任务 #37）；证据：rehearsal_logs/material_identity_key_realdata.log（5,771/5,771 等价、唯一键 5,740、31 组碰撞全为上游重复录入）+ test_material_identity_grounding.py 4 用例；判别力充分，无需报监工裁定】

E1-04 同期交付材料侧身份键（`pymatgen` 的 `reduced_formula` + 空间群，在匿名域计算）。5,771 条晶体没有 SMILES，分子侧那套完全用不上。

**要做**：让 grounding 判定对材料侧也能工作，走 E1 的身份键接口。

**注意**：E1 可能报"空间群从现有快照取不到"并给替代键（如 `reduced_formula` + tc 分档）。**按它实际交付的键接**，不要假设一定有空间群。若键的判别力不足以支撑 grounding，**报我裁定**——按 `goal.md` §2b「质量门两条任一不成立即废弃」，可能要影响 15 道 open 材料题的存废。

---

## 3. 补跑 e2e 测试【✅ 已完成：任务 #38（环境项，零代码改动）；python3.10 独立 venv 装 mcp==1.29.0 + rdkit 2026.3.5，e2e 由 1 skipped → 2 passed（test_gateway_mcp_e2e.py，MCP_APP 内存通道实调）】

`mcp` 包未装导致 1 个 e2e 测试是 skip 状态。装包后补跑，确认 **9 个工具通过真实 MCP 通道可调用**（不只是装饰器在）。

---

## 4. 范围与非目标

**不做**：不建匿名映射表（E1 的）、不改 `dag_poc/data/**`（E1 的）、不改 `query_spec.json`/`query_lint.py`（E2 的）、不改 `templates/**`（E5 的）、不改共享账本。

---

## 5. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | 匿名映射表接入 | `edit_molecule`/`normalize_structure` 在真实匿名数据上跑通，**有实跑证据** |
| 2 | grounding 条件③ | 在真实数据上验证一次；契约标注从"占位无鉴别力"改为已实现 |
| 3 | 材料侧身份键接入 | grounding 对材料侧可判；若键判别力不足则报监工 |
| 4 | e2e 补跑 | 装 `mcp` 包后 skip 转 pass，确认 9 工具经真实 MCP 通道可调 |
| 5 | report 收口 + **状态行更新** | — |

## 6. 约束

grounding 条件③**只有在真实数据上验过之后**才能把标注改为已实现——不要因为代码写了就改。需要立 ADR 的报监工。进度写自己的 `progress.md`。
