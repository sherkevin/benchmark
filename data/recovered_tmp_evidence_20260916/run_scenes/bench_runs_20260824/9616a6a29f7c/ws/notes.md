# Notes: Drug-likeness Optimization Task

## Parent Compound Diagnosis

Parent SMILES: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`

| Property | Value | Assessment |
|----------|-------|------------|
| QED | 0.6136 | Needs improvement |
| MW | 309.45 | Fine |
| ALOGP | 5.444 | **Too high** (ideal ~0-5) |
| PSA | 29.1 | Fine |
| ROTB | 9 | At upper limit |
| AROM | 2 | Fine |

**Diagnosis:** The parent compound has **excessive lipophilicity** (ALOGP 5.444). The main strategy should be to reduce lipophilicity while maintaining other properties.

## Candidates Meeting Free Constraints (psa ≤ 40, rotb ≤ 9)

From the 23 candidates, these 12 pass both free filters:
- e04: psa=29.1, rotb=5, alogp=3.884 (shortened chain)
- e05: psa=29.1, rotb=9, alogp=5.583 (F substitution)
- e08: psa=29.1, rotb=4, alogp=3.493 (shorter chain)
- e09: psa=29.1, rotb=3, alogp=3.103 (even shorter chain)
- e13: psa=29.1, rotb=8, alogp=5.054 (slightly shorter chain)
- e14: psa=29.1, rotb=2, alogp=2.713 (very short chain)
- e15: psa=29.1, rotb=7, alogp=4.664 (medium chain)
- e16: psa=29.1, rotb=8, alogp=3.777 (removed one aromatic ring)
- e18: psa=38.33, rotb=9, alogp=4.29 (ether oxygen in chain)
- e20: psa=29.1, rotb=9, alogp=6.097 (Cl substitution - worse lipophilicity)
- e21: psa=29.1, rotb=9, alogp=5.444 (same as parent)
- e23: psa=29.1, rotb=6, alogp=4.274 (shorter chain)

## Strategy

Since the parent's problem is high ALOGP, candidates with **lower ALOGP** should have better QED:
- e14 (alogp=2.713) - very short chain
- e09 (alogp=3.103) - short chain
- e08 (alogp=3.493) - short chain
- e04 (alogp=3.884) - medium-short chain
- e16 (alogp=3.777) - removed aromatic ring

Candidates with high ALOGP (likely worse):
- e20 (alogp=6.097) - Cl makes it more lipophilic
- e05 (alogp=5.583) - F substitution
- e21 (alogp=5.444) - same as parent

## Measurement Plan

Budget: 8 measurements for 12 eligible candidates.

Priority order (lowest alogp first, expecting better QED):
1. e14 (alogp=2.713) - shortest chain
2. e09 (alogp=3.103)
3. e08 (alogp=3.493)
4. e16 (alogp=3.777)
5. e04 (alogp=3.884)
6. e23 (alogp=4.274)
7. e18 (alogp=4.29) - has ether, higher PSA
8. e15 (alogp=4.664)

Will measure these and track results.

## Measurement Results

| edit_id | alogp | psa | rotb | QED measured | Meets all constraints? |
|---------|-------|-----|------|--------------|------------------------|
| e14 | 2.713 | 29.1 | 2 | 0.8126 | YES |
| e09 | 3.103 | 29.1 | 3 | 0.854 | YES |
| e08 | 3.493 | 29.1 | 4 | 0.869 | YES |
| e16 | 3.777 | 29.1 | 8 | 0.6802 | NO (QED < 0.8) |
| e04 | 3.884 | 29.1 | 5 | 0.8048 | YES |
| e23 | 4.274 | 29.1 | 6 | 0.7778 | NO (QED < 0.8) |
| e18 | 4.29 | 38.33 | 9 | 0.7016 | NO (QED < 0.8) |
| e15 | 4.664 | 29.1 | 7 | 0.7345 | NO (QED < 0.8) |

## Analysis

**Best QED:** e08 with 0.869

**Candidates meeting ALL three constraints (QED ≥ 0.8, psa ≤ 40, rotb ≤ 9):**
- e08: QED=0.869, psa=29.1, rotb=4 ✓
- e09: QED=0.854, psa=29.1, rotb=3 ✓
- e14: QED=0.8126, psa=29.1, rotb=2 ✓
- e04: QED=0.8048, psa=29.1, rotb=5 ✓

**Failed direction:** e16 (removing one aromatic ring) gave QED=0.6802, which is only slightly better than parent (0.6136). Despite having a reasonable alogp of 3.777, the loss of aromatic character hurt drug-likeness. This edit direction (removing aromatic ring) did not work.

**Pick for optimized lead:** e08 - highest QED (0.869), well within psa (29.1 ≤ 40) and rotb (4 ≤ 9) constraints.
