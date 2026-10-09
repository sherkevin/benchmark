# Notes

## Task 1: Predict QED for m05

m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has chain_len=11 in the scaffold `COc1ccc(cc1)C(=O)N`.

Since m05 cannot be measured directly (probe_unavailable), I measured all other members of its congeneric series:

| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Used `fit_trend` with degree=2 polynomial to extrapolate to chain_len=11:
- **Predicted QED for m05: 0.5416**

## Task 2: Find a qualifying molecule

Requirements:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in the same congeneric series as m05

From pool.tsv, molecules with psa ≥ 40 are:
- Nitro series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

Measured candidates:

| mol_id | series   | psa   | QED    | Qualifies? |
|--------|----------|-------|--------|------------|
| m02    | nitro    | 72.24 | 0.6046 | YES ✓      |
| m09    | nitro    | 72.24 | 0.5826 | No (QED<0.6) |
| m12    | nitro    | 72.24 | 0.4709 | No         |
| m01    | sugar    | 110.02| 0.3187 | No         |
| m14    | sugar    | 110.02| 0.3237 | No         |
| m27    | sugar    | 110.02| 0.333  | No         |

**Selected: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Not in m05's series (nitro vs COc1ccc...) ✓

## Budget used: 12/12 measurements
- 5 for m05's series (m15, m24, m28, m17, m22)
- 6 for Task 2 candidates (m02, m09, m12, m01, m14, m27)
- 1 remaining, not needed
