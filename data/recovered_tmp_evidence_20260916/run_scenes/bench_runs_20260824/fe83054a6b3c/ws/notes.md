# Task notes

## Target
- `m05`: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxy-benzoyl amide with an N-alkyl chain.
- Scaffold: `COc1ccc(cc1)C(=O)N` + chain of 11 carbons.
- Never synthesized; must predict QED by extrapolating its OWN series.

## Job 1 — same series as m05 (methoxy-benzoyl amide scaffold)
Candidates by eye from pool.tsv (all share `COc1ccc(cc1)C(=O)N` + alkyl):
- m15: `COc1ccc(cc1)C(=O)NCC` (C2)
- m24: `COc1ccc(cc1)C(=O)NCCC` (C3)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (C4)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (C5)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (C6)
- m05: target (C11) — unmeasurable

Will confirm scaffolds with describe_structure.

## Budget
12 measurements total, shared by both jobs. Plan carefully.

## Job 1 — measurements & fit
Series `COc1ccc(cc1)C(=O)N` (methoxy-benzoyl amide), QED vs chain length:
| chain | mol | QED |
|---|---|---|
| C2 | m15 | 0.7621 |
| C3 | m24 | 0.7919 (peak) |
| C4 | m28 | 0.7518 |
| C5 | m17 | 0.7498 |
| C6 | m22 | 0.7377 |
| C11 | m05 | ? (target, unmeasurable) |

Peak at C3, then steady decline. Non-linear → quadratic fit appropriate.

Quadratic fit (degree 2) at x=11:
- coefficients [a0,a1,a2] = [0.74932, 0.017024, -0.003264]
- vertex (peak) at -a1/(2a2) ≈ 2.6 (matches observed peak near C3)
- prediction = **0.5416**, RMSE on points = 0.01188
- Linear (degree 1) at x=11 → 0.695, RMSE 0.01308. Linear is worse fit and the task says don't assume a line.

Target QED prediction = **0.5416** (quadratic, the trend actually observed).
Note uncertainty: extrapolating 5 units past C6; quadratic and linear bracket ~0.54–0.70.

## Job 2 — lead
Constraints: measured QED ≥ 0.6, psa ≥ 40, NOT in m05 series.
- psa screen passes 18 molecules. m05-series members (m15,m17,m22,m24,m28) all have psa 38.33 < 40 → excluded, so constraint 3 auto-satisfied by all psa-passers. Good.
- Remaining non-target series (psa ≥ 40):
  - nitro-benzoyl amide `O=[N+]([O-])c1ccc(cc1)C(=O)N`: m02,m06,m09,m10,m12,m29,m31,m32,m33 (psa 72.24, alogp reasonable ~1.3-4.5)
  - polyol `OCC(O)C(O)C(O)C(=O)N`: m01,m03,m07,m14,m18,m19,m21,m25,m27 (psa 110.02, alogp very negative -2.8..0.3 → likely low QED)
- Short-chain members most likely ≥ 0.6 (near series peaks). Try shortest of each: m09 (nitro C2), m14 (polyol C2).
- Measurements used so far: 5 → 7 left.

### Lead measurements
- m09 (nitro, C2): QED 0.5826 — just under 0.6, reject.
- m14 (polyol, C2): QED 0.3237 — far too low, reject (polyol series confirmed low, stop measuring it).
- m02 (nitro, C3): QED 0.6046 — **passes ≥ 0.6.**
- m02 psa = 72.24 ≥ 40 ✓; scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's `COc1ccc(cc1)C(=O)N` ✓.

## Lead = m02 (measured QED 0.6046). Measurements used: 8 / 12.

