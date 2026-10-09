# E1-03 §2.1 匿名映射表 · 实跑留痕（任务板 Task #22）

日期：2026-08-06。全部输出为脚本实跑原样摘录，可复跑复现：
`python3 src/data_build/build_anonymization_mapping.py` 与 `python3 src/data_build/check_mapping_nonmonotonic.py`。

## 1. R4 非单调断言实跑输出（check_mapping_nonmonotonic.py，退出码 0）

```
载入映射表：/Users/jingwu/work/benchmark/dag_poc/data/anonymization_mapping.json（88 个标签键，其中真实元素 87 种，载体=RDKit dummy atom）
[断言1] 周期序非严格单调：通过 ✓
[断言1] 字母序非严格单调：通过 ✓
[断言2] 周期序 Spearman ρ = +0.0323（|ρ| < 0.5）：通过 ✓
[断言2] 字母序 Spearman ρ = +0.0029（|ρ| < 0.5）：通过 ✓
[断言3] 相邻原子序 label 差符号混合（正 44 / 负 42）：通过 ✓

[留痕] 周期序前 10 元素 → label：
  H（Z=1）→ [19*]
  He（Z=2）→ [61*]
  Li（Z=3）→ [72*]
  Be（Z=4）→ [46*]
  B（Z=5）→ [45*]
  C（Z=6）→ [16*]
  N（Z=7）→ [81*]
  O（Z=8）→ [22*]
  F（Z=9）→ [7*]
  Ne（Z=10）→ [30*]

R4 断言全部通过：映射与周期序/字母序均非单调相关 ✓
```

断言口径（与生成期 r4_quick_check 一致，E2 query_lint 将来可照抄）：
判定对象仅为真实元素（保留标签键 `*` 排除）；①严格单调检查（周期序/字母序
label 序列不得严格递增/递减）；②Spearman |ρ| < 0.5（秩归一，对 label 空位稳健）；
③相邻原子序 label 差不得全部同号。

## 2. ADR-0017 事实二复跑（build 脚本运行时打印，全 ✓）

```
[ADR-0017 事实二复跑]
  '[1*]CC' → '[1*]CC' ✓
  '[2*][1*]' → '[1*][2*] ✓
  'a2b' → None ✓
  '[Aa]CC' → None ✓
  'FFO' → None ✓
```

## 3. dummy atom 编号上限实测（RDKit 2025.09.2）

```
[dummy atom 编号上限实测]
  N=1: parse OK / isotope 读回 1 → 稳定
  N=87: parse OK / isotope 读回 87 → 稳定
  N=118: parse OK / isotope 读回 118 → 稳定
  N=255: parse OK / isotope 读回 255 → 稳定
  N=999: parse OK / isotope 读回 999 → 稳定
  N=1000: parse OK / isotope 读回 1000 → 稳定
  N=9999: parse OK / isotope 读回 9999 → 稳定
  N=65535: parse OK / isotope 读回 65535 → 稳定
  N=100000: parse OK / isotope 读回 34464 → 溢出！读回 34464（16 位截断）
```

结论：**安全上限 N ≤ 65535**（RDKit isotope 为 16 位存储，N=100000 被静默截断为
100000 mod 65536 = 34464）。本表仅需 88 个 label（87 元素 + 保留标签 `*`），
距上限三个数量级以上，无风险。build 脚本在元素并集 +1 > 65535 时直接退出防呆。

## 4. 全量自 canonical 验证与重跑不变证明

```
元素并集：87 种（来源：entity_registry.jsonl 分子 formula + 晶体 formula_sc）
匿名 SMILES 全量自 canonical 验证：通过 74941 / 解析失败 0 / 非自 canonical 0

映射表落盘 → dag_poc/data/anonymization_mapping.json
  88 元素 / judge 段：分子 74941 + 晶体 5771
  15775493 bytes / md5=a77b6ef8ce3be434eba36e08a5bb1401
R4 生成期自检：映射与周期序/字母序均非单调相关 ✓
```

连续两次运行 build 脚本，落盘产物 md5 均为 `a77b6ef8ce3be434eba36e08a5bb1401`
（15,775,493 bytes）——重跑产物逐字节不变 ✓。

### 自 canonical 修复留痕（意外发现 → 修复）
- 首轮实测：就地置换后直接 MolToSmiles，14298/74941 条匿名输出非自 canonical——
  根因是就地置换的 Mol 对象保留源分子 chiral tag，匿名域中元素身份消失后这些
  手性中心失效，首次输出仍原样带 `@`/`@@`。
- 修复：输出前迭代"重解析 → canonical"至不动点（上限 3 轮）。修复后仍余 18 条
  单次重解析不收敛（配位化合物/方向双键复杂结构），迭代 2 轮全部收敛。
- 终态：74941/74941 自 canonical 通过，且 build 脚本将"非自 canonical ≠ 0"设为
  阻断条件（不得落盘映射表）。
