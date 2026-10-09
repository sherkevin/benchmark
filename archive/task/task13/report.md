# task13 — 完成汇报（修复 15 条 SMILES 错配）

| 字段 | 值 |
|---|---|
| **状态** | ✅ 完成 |
| **执行者** | 监工（本任务无人负责：六条 E 线的 prompt 均明确"不做 T-55"，而它是 E5 的硬前置） |
| **完成日期** | 2026-08-06 |
| **对应账本** | T-55 ｜ PR-25 |

## 一、结论先行

1. **验收硬标准已达成**：`verify_snapshot_identity.py --strict` 的**身份问题 16 项 → 0 项，退出码 1 → 0**。
2. **真错配精确为 15 条**——与 task11 的独立结论**完全吻合**（两条独立路径互证）。
3. **`aa`/`bh` 的 SMILES 重复已解除**：`aa`(Metoprolol)=`CC(C)NCC(COC1=CC=C(C=C1)CCOC)O`、`bh`(Propranolol)=`CC(C)NCC(COC1=CC=CC2=CC=CC=C21)O`。
4. **性质值零改动**——严守 task.md 锁定的范围（只改身份，不动 admet/crystals/literature）。
5. **12 个受影响 case 的 gt 身份已修正**，其真值恢复可信，E5 复用旧 case 的硬阻塞解除。

## 二、做了什么

| 动作 | 结果 |
|---|---|
| 用 PubChem 权威 `ConnectivitySMILES` 替换 `molecule_search[].smiles` | **39 条**（15 条真错配 + 24 条写法统一为权威形式，保证可复现） |
| 新增 `inchikey` 身份锚 | 40/40（比 SMILES 稳定，SMILES 有多种等价写法） |
| 新增 `pubchem_cid` 回查锚 | 40/40（可回溯 PubChem 原始记录） |
| 入库门（RDKit 算分子式 vs PubChem 权威分子式，不符则拒收） | **拒收 0 条**（全部通过） |

## 三、39 条替换的精确分类（RDKit 规范化 + 分子式双重比对）

| 类别 | 数量 | 处置 |
|---|---|---|
| 原本完全一致 | 1 | 不动 |
| **分子式不同＝真错配** | **15** | 替换（这是本 task 的核心目标） |
| 分子式相同、写法不同（立体化学/互变异构） | 7 | 一并统一为权威写法 |
| 规范化后等价（仅芳香式 vs Kekulé 写法差异） | 17 | 一并统一为权威写法 |

> **为何写法差异也替换**：统一为权威写法后，任何人重跑校验脚本都得到同一结果，避免"规范化后等价但字面不同"造成的反复困扰。

**15 条真错配典型例**（mock 原值算出的分子式 → 权威分子式）：

| ID | 分子 | 原分子式 | 权威分子式 |
|---|---|---|---|
| `aa` | Metoprolol | C16H21NO2 | **C15H25NO3** |
| `ah` | Digoxin | C18H16O10 | **C41H64O14** |
| `bf` | Astemizole | C22H21ClN2O | **C28H31FN4O** |
| `ei` | Mirtazapine | C18H13N | **C17H19N3** |

完整明细见 [`修复明细.json`](修复明细.json)（每条含 id / 真名 / 类别 / old / new）。

## 四、安全性核验（证明没有越界改动）

| 检查项 | 结果 |
|---|---|
| `admet` 性质值（bbb/herg/caco2/logS/logD/half_life_days/pass）改动 | **0 处** ✓ |
| `crystals` 改动 | 无 ✓ |
| `literature` 改动 | 无 ✓ |
| `molecule_search` 条数 | 40 → 40（无增删）✓ |
| 原文件备份 | `/tmp/mock_db.backup.json` |

> task.md 已用 24 项逐值核验证明"**性质值没有串，错的只有 SMILES**"，本次严守该范围。

## 五、验收门输出（修复后）

```
[A] SMILES 无法解析 —— 无
[B] 分子式与 PubChem 权威记录不符 —— 0 条
[C] canonical SMILES 重复（不同 ID 指向同一分子）—— 0 组
合计身份问题 0 项
退出码: 0
```

## 六、订正一条历史误记

此前 TODO 与 E1 prompt 中曾记载"`verify_snapshot_identity.py --strict` 有 bug，报错却返回 0"。**该记录有误**——实测该门一直有效（修复前退出码 1、报 16 项问题；修复后退出码 0）。已在相关文档中订正。

## 七、遗留

| # | 遗留 | 说明 |
|---|---|---|
| 1 | **ADMET 填充率仍为 80.4%**（193/240，47 个空格） | 本 task 只修身份，不补性质。补空格需拉 TDC ADMET（AqSolDB 已落盘可补 logS），归 E1 后端库构造范围 |
| 2 | **`bh` 不应出现在 BBB 约束型 case 中** | `证据_admet_gt.md` 脚注 †：Martins BBBP 数据集中 propranolol 有矛盾标签（盐型 Y=1 / 原型 Y=0），mock 取 1 是采用正确生物学事实。建议出 BBB 题时避开 `bh`——**需转告 E5** |
| 3 | **12 个 case 的可解性需重验** | gt 身份变了但性质未变，故合格集 \|G\| 理论上不变；但按"库一变就重验"的纪律，E5 复用这些 case 前应跑一次可解性穷举 |
