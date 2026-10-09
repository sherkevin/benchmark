# Notes: Drug-likeness optimization

## Parent compound diagnosis
- SMILES: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
- QED: 0.6136 (needs improvement)
- **ALOGP = 5.444** → Way too high (ideal ~1-3). This is the main problem.
- **ROTB = 9** → Borderline high. Reducing flexible bonds helps QED.
- PSA = 29.1 → Below optimal (~40-100) but acceptable
- MW = 309.45 → Fine
- AROM = 2 → Fine

**Diagnosis**: The parent is too lipophilic (ALOGP 5.444) and has too many rotatable bonds (9). The long octyl chain (CCCCCCCC) is the main culprit for both. Shortening the chain should reduce ALOGP and ROTB simultaneously, improving QED.

## Filtering candidates for pick constraints (PSA ≤ 40, ROTB ≤ 9)

Passing candidates:
- e04: MW 253, ALOGP 3.884, PSA 29.1, ROTB 5 (chain shortened to CCCC)
- e05: MW 327, ALOGP 5.583, PSA 29.1, ROTB 9 (F added to ring)
- e08: MW 239, ALOGP 3.493, PSA 29.1, ROTB 4 (chain shortened to CCC)
- e09: MW 225, ALOGP 3.103, PSA 29.1, ROTB 3 (chain shortened to CC)
- e13: MW 295, ALOGP 5.054, PSA 29.1, ROTB 8 (chain shortened to CCCCCCCC→CCCCCCC)
- e14: MW 211, ALOGP 2.713, PSA 29.1, ROTB 2 (chain shortened to C)
- e15: MW 281, ALOGP 4.664, PSA 29.1, ROTB 7 (chain shortened to CCCCCC)
- e16: MW 233, ALOGP 3.777, PSA 29.1, ROTB 8, AROM 1 (removed one phenyl ring)
- e18: MW 311, ALOGP 4.29, PSA 38.33, ROTB 9 (ether in chain)
- e20: MW 344, ALOGP 6.097, PSA 29.1, ROTB 9 (Cl added)
- e21: MW 309, ALOGP 5.444, PSA 29.1, ROTB 9 (same as parent!)
- e23: MW 267, ALOGP 4.274, PSA 29.1, ROTB 6 (chain shortened to CCCCC)

Failing candidates (PSA > 40 or ROTB > 9):
e01, e02, e03, e06, e07, e10, e11, e12, e17, e19, e22

## Measurement strategy (8 measurements for 23 candidates)

Most promising for QED improvement (chain shortening = ALOGP reduction + ROTB reduction):
1. e14 (ALOGP 2.713, ROTB 2) - strongest improvement expected
2. e09 (ALOGP 3.103, ROTB 3) - very strong
3. e08 (ALOGP 3.493, ROTB 4) - strong
4. e04 (ALOGP 3.884, ROTB 5) - good
5. e16 (ALOGP 3.777, ROTB 8, AROM 1) - ring removal, good ALOGP
6. e23 (ALOGP 4.274, ROTB 6) - moderate
7. e15 (ALOGP 4.664, ROTB 7) - moderate
8. e18 (ALOGP 4.29, PSA 38.33, ROTB 9) - ether, higher PSA might help QED

Skipping: e05 (ALOGP 5.583 - worse), e20 (ALOGP 6.097 - worse), e21 (same as parent), e13 (ALOGP 5.054 - still high), and all failing candidates.

## Measurement results (all 8 budget used)

| edit_id | QED | PSA | ROTB | Qualifies (QED≥0.8, PSA≤40, ROTB≤9)? |
|---------|-------|-------|------|----------------------------------------|
| e08 | 0.869 | 29.1 | 4 | YES |
| e09 | 0.854 | 29.1 | 3 | YES |
| e14 | 0.8126 | 29.1 | 2 | YES |
| e04 | 0.8048 | 29.1 | 5 | YES |
| e23 | 0.7778 | 29.1 | 6 | NO (QED < 0.8) |
| e15 | 0.7345 | 29.1 | 7 | NO (QED < 0.8) |
| e18 | 0.7016 | 38.33 | 9 | NO (QED < 0.8) |
| e16 | 0.6802 | 29.1 | 8 | NO (QED < 0.8) |

## Analysis

### Chain-shortening trend (clear sweet spot at C3):
- Parent (C8): QED 0.6136
- e15 (C6): QED 0.7345
- e23 (C5): QED 0.7778
- e04 (C4): QED 0.8048
- **e08 (C3): QED 0.869** ← peak
- e09 (C2): QED 0.854
- e14 (C1): QED 0.8126

Shortening the chain reduces both ALOGP and ROTB, which are the parent's two main problems. The QED peaks at C3 (e08), then starts declining as the molecule becomes too small/light.

### Ring deletion (e16) failed
Removing one phenyl ring (biphenyl → phenyl) was a dramatic structural change that barely improved QED (0.6802 vs parent 0.6136). While AROM dropped from 2 to 1, the remaining properties (ALOGP 3.777, ROTB 8) were still not good enough. This was the least effective edit direction.

### Ether insertion (e18) underperformed
Adding an ether oxygen in the chain (QED 0.7016) was only marginally better than parent. The ALOGP reduction (4.29 vs 5.444) wasn't sufficient.

## Final answers

- **best_edit_id**: e08 (QED 0.869) — highest QED measured. Unmeasured candidates all have higher ALOGP or structural issues that make beating 0.869 unlikely.
- **best_qed_measured**: 0.869
- **pick_edit_id**: e08 — satisfies all three constraints: QED 0.869 ≥ 0.8, PSA 29.1 ≤ 40, ROTB 4 ≤ 9
- **failed_direction**: "ring deletion" — removing an aromatic ring (e16) produced only marginal QED improvement (0.6802) despite being a major structural change
