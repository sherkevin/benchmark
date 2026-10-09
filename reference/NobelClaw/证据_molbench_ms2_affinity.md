# MolBench MS-2 亲和力二选一题池（可复用 GT 题源）

> 来源：MolBench MS-2（实验 Ki，上海 AI Lab / InternScience）。每题给一个靶点 + 两个候选 SMILES，
> 问哪个亲和力更高/更低（更低/更高 Ki），GT 是唯一正确 SMILES。
> 这是一批自带实验真值、可直接判分的药物-亲和力题，曾用于 task5 早期设计（单步 vs 并行执行）。
> task5 后改为「stage-by-stage vs 直接回答」三领域评测后，此题池移到此处留作**可复用题源**，未删除。
> 适用：任何需要「药物-靶点亲和力比较、SMILES 级判分」的子任务。

## 题目（可直接粘贴）

### AF01 — Angiotensin-converting enzyme（问更低 Ki / 高亲和）
- A: `CCCC[C@H](S)C(=O)N[C@@H](Cc1ccc(-c2ccccc2)cc1)C(=O)O`
- B: `CC(C)(S)C(=O)N[C@@H](Cc1ccc(-c2ccccc2)cc1)C(=O)O`

### AF02 — Phosphodiesterase 5A（问更高 Ki / 低亲和）
- A: `COc1ccc(-c2cnc(N3Cc4c(nc5ccccc5c4O)C3c3ccc4c(c3)CCO4)nc2)cc1OC`
- B: `COc1ccc(-c2cnc(N3Cc4c(nc5ccccc5c4O)C3c3ccc(OC)c(OC)c3)nc2)cc1OC`

### AF03 — Serotonin 1d (5-HT1d) receptor（问更低 Ki）
- A: `Cc1ccc2c(OCCN3CCN(Cc4ccc(F)c([N+](=O)[O-])c4)CC3)cccc2n1`
- B: `Cc1ccc2c(OCCN3CCN(Cc4ccc(Cl)c(Cl)c4)CC3)cccc2n1`

### AF04 — Serotonin 3a (5-HT3a) receptor（问更低 Ki）
- A: `CN1CCN(c2nc3ccccc3nc2O)CC1`
- B: `CN1CCN(c2nc3ccccc3nc2Cl)CC1`

### AF05 — Cyclooxygenase-2（问更高 Ki）
- A: `COC(=O)[C@H](Cc1c[nH]c2ccccc12)NC(=O)CNC(=O)C(=O)c1cn(S(=O)(=O)c2ccc(C)cc2)c2ccccc12`
- B: `Cc1ccc(S(=O)(=O)n2cc(C(=O)C(=O)NCC(=O)N[C@@H](Cc3c[nH]c4ccccc34)C(=O)O)c3ccccc32)cc1`

### AF06 — Coagulation factor IX（问更低 Ki）
- A: `CC(Oc1cccc(-c2ccc3sc(C(=N)N)cc3c2)c1)c1ccc(F)cc1`
- B: `COC(=O)C(Oc1cccc(-c2ccc3sc(C(=N)N)cc3c2)c1)c1ccc(F)cc1`

### AF07 — Aminopeptidase N（问更高 Ki）
- A: `CC[C@H](C)[C@H](NC(=O)[C@H](CNC(=O)[C@@H](N)C(C)C)Cc1ccc(O)cc1)C(=O)N[C@@H](Cc1cnc[nH]1)C(=O)N1CCC[C@H]1C(=O)N[C@@H](Cc1ccccc1)C(=O)O`
- B: `CCCC[C@H](N)C(=O)N[C@@H](Cc1ccc(O)cc1)C(=O)N[C@H](C(=O)N[C@@H](Cc1cnc[nH]1)C(=O)N1CCC[C@H]1C(=O)N[C@@H](Cc1ccccc1)C(=O)O)[C@@H](C)CC`

### AF08 — Adenosine A3 receptor（问更低 Ki）
- A: `Cn1cc2c(nc(NCCc3ccccc3)n3nc(-c4ccco4)nc23)n1`
- B: `C[C@H](Nc1nc2nn(C)cc2c2nc(-c3ccco3)nn12)c1ccccc1`

### AF09 — Metabotropic glutamate receptor 3（问更低 Ki）
- A: `N[C@@]1(C(=O)O)C[C@H](NC(=O)c2cccc(Cl)c2)[C@H]2[C@H](C(=O)O)[C@H]21`
- B: `CC(=O)N[C@H]1C[C@@](N)(C(=O)O)[C@@H]2[C@@H](C(=O)O)[C@H]12`

### AF10 — Neurokinin 3 receptor（问更低 Ki）
- A: `C[C@@H]1c2nnc(-c3ccccn3)n2CCN1C(=O)c1ccc(F)cc1`
- B: `C[C@@H]1c2nnc(-c3nc(C(F)(F)F)ns3)n2CCN1C(=O)c1ccc(F)cc1`

### AF11 — Fibroblast growth factor receptor 1（问更高 Ki）
- A: `CN(c1cccc(N)c1)c1ccc2c(/C=C/c3ccccc3)n[nH]c2c1`
- B: `CC(=O)Nc1cccc(N(C)c2ccc3c(/C=C/c4ccccc4)n[nH]c3c2)c1`

### AF12 — Histone deacetylase 9（问更高 Ki）
- A: `CN1CCC(CNC(=O)c2cccc(-c3noc(C(F)(F)F)n3)c2)(c2nc(-c3ccccc3)cs2)CC1`
- B: `CN1CCC(CNC(=O)c2cccc(C(=O)NO)c2)(c2nc(-c3ccccc3)cs2)CC1`

### AF13 — Oligopeptide transporter small intestine isoform（问更低 Ki）
- A: `C[C@H](NC(=O)[C@@H](N)Cc1ccc(-c2ccccc2)cc1)C(=O)O`
- B: `C[C@H](NC(=O)[C@@H](N)Cc1ccc(Oc2ccccc2)cc1)C(=O)O`

### AF14 — Protein kinase C theta（问更低 Ki）
- A: `CCC(C)(C#N)c1cccc(-c2ccnc3[nH]ncc23)c1`
- B: `C#CCC(C)(CN)c1cccc(-c2ccnc3[nH]ncc23)c1`

### AF15 — Interleukin-8 receptor B（问更高 Ki）
- A: `CC(C)C[C@H](CO)Nc1nc(SCc2ccncc2)nc2nc(N)sc12`
- B: `CC(C)C[C@H](CO)Nc1nc(SCc2ccccn2)nc2nc(N)sc12`

## Ground Truth

| 题号 | 靶点 | 问法 | GT | GT SMILES |
|---|---|---|---|---|
| AF01 | Angiotensin-converting enzyme | 低 Ki | A | `CCCC[C@H](S)C(=O)N[C@@H](Cc1ccc(-c2ccccc2)cc1)C(=O)O` |
| AF02 | Phosphodiesterase 5A | 高 Ki | B | `COc1ccc(-c2cnc(N3Cc4c(nc5ccccc5c4O)C3c3ccc(OC)c(OC)c3)nc2)cc1OC` |
| AF03 | Serotonin 1d (5-HT1d) receptor | 低 Ki | A | `Cc1ccc2c(OCCN3CCN(Cc4ccc(F)c([N+](=O)[O-])c4)CC3)cccc2n1` |
| AF04 | Serotonin 3a (5-HT3a) receptor | 低 Ki | B | `CN1CCN(c2nc3ccccc3nc2Cl)CC1` |
| AF05 | Cyclooxygenase-2 | 高 Ki | A | `COC(=O)[C@H](Cc1c[nH]c2ccccc12)NC(=O)CNC(=O)C(=O)c1cn(S(=O)(=O)c2ccc(C)cc2)c2ccccc12` |
| AF06 | Coagulation factor IX | 低 Ki | B | `COC(=O)C(Oc1cccc(-c2ccc3sc(C(=N)N)cc3c2)c1)c1ccc(F)cc1` |
| AF07 | Aminopeptidase N | 高 Ki | A | `CC[C@H](C)[C@H](NC(=O)[C@H](CNC(=O)[C@@H](N)C(C)C)Cc1ccc(O)cc1)C(=O)N[C@@H](Cc1cnc[nH]1)C(=O)N1CCC[C@H]1C(=O)N[C@@H](Cc1ccccc1)C(=O)O` |
| AF08 | Adenosine A3 receptor | 低 Ki | B | `C[C@H](Nc1nc2nn(C)cc2c2nc(-c3ccco3)nn12)c1ccccc1` |
| AF09 | Metabotropic glutamate receptor 3 | 低 Ki | A | `N[C@@]1(C(=O)O)C[C@H](NC(=O)c2cccc(Cl)c2)[C@H]2[C@H](C(=O)O)[C@H]21` |
| AF10 | Neurokinin 3 receptor | 低 Ki | B | `C[C@@H]1c2nnc(-c3nc(C(F)(F)F)ns3)n2CCN1C(=O)c1ccc(F)cc1` |
| AF11 | Fibroblast growth factor receptor 1 | 高 Ki | A | `CN(c1cccc(N)c1)c1ccc2c(/C=C/c3ccccc3)n[nH]c2c1` |
| AF12 | Histone deacetylase 9 | 高 Ki | B | `CN1CCC(CNC(=O)c2cccc(C(=O)NO)c2)(c2nc(-c3ccccc3)cs2)CC1` |
| AF13 | Oligopeptide transporter small intestine isoform | 低 Ki | A | `C[C@H](NC(=O)[C@@H](N)Cc1ccc(-c2ccccc2)cc1)C(=O)O` |
| AF14 | Protein kinase C theta | 低 Ki | B | `C#CCC(C)(CN)c1cccc(-c2ccnc3[nH]ncc23)c1` |
| AF15 | Interleukin-8 receptor B | 高 Ki | A | `CC(C)C[C@H](CO)Nc1nc(SCc2ccncc2)nc2nc(N)sc12` |

GT 分布：8×A / 7×B；9 题问高亲和、6 题问低亲和；15 个不同靶点。
