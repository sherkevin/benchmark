# Notes for Medicinal Chemistry Task

## Summary
- Predicted QED for m05 (chain length 11): 0.5416 using quadratic fit
- Selected lead compound: m02 with QED 0.6046
- Congeneric series of m05: m15, m24, m28, m17, m22

## Job 1: Predict QED of m05

Target molecule m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
.- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11

**Found congeneric series members (same scaffold):**
- m15: chain length 2 (SMILES: `COc1ccc(cc1)C(=O)NCC`)
- m24: chain length 3 (SMILES: `COc1ccc(cc1)C(=O)NCCC`)
- m28: chain length 4 (SMILES: `COc1ccc(cc1)C(=O)NCCCC`)
- m17: chain length 5 (SMILES: `COc1ccc(cc1)C(=O)NCCCCC`)
- m22: chain length 6 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCC`)

These are all the short-chain members in the pool. m05 has chain length 11, so we need to extrapolate beyond the measured range.

**Job 1 Measurements:**
————————

Chain Length | mol_id | QED
-------------|--------|-----
2 | m15 | 0.7621
3 | m24 | 0.7919
4 | m28 | 0.7518
5 | m17 | 0.7498
6 | m22 | 0.7377

**Final Decisions:**

**Job 1 - Prediction for m05 (chain length 11):**
- Used quadratic fit (degree 2) to capture curvature in data
- QED = 0.74932 + 0.017024*x - 0.003264*x²
- Prediction: 0.5416
- Rationale: Data shows peak at chain length 3, then decline. Quadratic fit captures this curvature better than linear fit.

**Job 2 - Lead compound selection:**
- Selected m02 as lead compound
- QED measured: 0.6046 (≥ 0.6 constraint)
- psa: 72.24 (≥ 40 constraint)
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (not in m05's series)
- Alternative m09 had QED 0.5826 (< 0.6)
- Polyol series (e.g., m01) had low QED (0.3187)

**Congeneric series members (same as m05):**
- m15 (chain length 2)
- m24 (chain length 3)
- m28 (chain length 4)
- m17 (chain length 5)
- m22 (chain length 6)

**Budget usage:** Used all 17 measurements (9 for list_pool, 8 for measure_qed).

**Job 2: Pick backup lead compound**

**Candidates with psa ≥ 40:** 18 molecules from `screen_candidates()`

**Filtered to not in m05's series:** All 18 pass (none are m15, m24, m28, m17, m22)

**Measurements made:**
- m02 (nitro series, chain 3): QED = 0.6046 ✓ (≥ 0.6)
- m01 (polyol series, chain 4): QED = 0.3187 ✗ (< 0.6)
- m09 (nitro series, chain 2): QED = 0.5826 ✗ (< 0.6)

**Best candidate found:** m02
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` (not m05's series) ✓

**Budget:** Used all 17 measurements.

**Plan for Job 2:**
1. Use `screen_candidates()` to get molecules with psa ≥ 40
2. Filter out molecules in m05's series (m15, m24, m28, m17, m22)
3. Measure QED of promising candidates