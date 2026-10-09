# Notes

## Setup
- 33-molecule pool, listed via `list_pool(page)` (4/page, 9 pages). Budget 17 shared.
- Full catalogue paged (budget_used 9 after listing). 8 units left for measures.

## Congeneric series (same scaffold = Ar-C(=O)N-(alkyl), differ only in N-alkyl chain)

**Methoxy series (m05's series): `COc1ccc(cc1)C(=O)N-[alkyl]`, psa=38.33**
| mol | chain | mw | alogp | rotb | QED |
|-----|-------|----|------|------|-----|
| m15 | C2 | 179.22 | 1.445 | 3 | 0.7621 |
| m24 | C3 | 193.25 | 1.835 | 4 | 0.7919 (peak) |
| m28 | C4 | 207.27 | 2.225 | 5 | 0.7518 |
| m17 | C5 | 221.30 | 2.615 | 6 | 0.7498 |
| m22 | C6 | 235.33 | 3.005 | 7 | 0.7377 |
| m05 | C11 | 305.46 | 4.956 | 12 | (unsynth → predict) |

**Nitro series: `O=[N+]([O-])c1ccc(cc1)C(=O)N-[alkyl]`, psa=72.24**
| mol | chain | alogp | rotb | QED |
|-----|------|------|------|-----|
| m09 | C2 | 1.345 | 3 | 0.5826 |
| m12 | C4 | 2.125 | 5 | — |
| m02 | C3 | 1.735 | 4 | 0.6046 |
| m32 | C5 | 2.515 | 6 | — |
| m33 | C6 | 2.905 | 7 | — |
| m29 | C7 | 3.295 | 8 | — |
| m31 | C8 | 3.685 | 9 | — |
| m06 | C9 | 4.075 |10 | — |
| m10 | C10| 4.465 |11 | — |

**Biphenyl series: `c1ccc(-c1ccc)C(=O)N-[alkyl]`, psa=29.1** (all psa<40, never valid for Task 2)

**Sugar series: `OCC(O)C(O)C(O)C(=O)N-[alkyl]`, psa=110.02**
- C10 m25 measured: QED 0.3148 (best chain, still far below 0.6 → whole series fails)

## Task 1 — QED of m05
m05 is methoxy-series C11. Real measured methoxy series:
C2 0.7621 → C3 0.7919 → C4 0.7518 → C5 0.7498 → C6 0.7377.
Peak at C3, then steady decline (~0.018/CH₂). m05 = C11 has alogp 4.956 (far past the
3–4 drug-like optimum) and rotb 12 (past the QED rotb penalty threshold ~10), so the
decline accelerates. Extrapolating the measured trend with the rotb/alogp penalty:
**predicted m05 QED ≈ 0.62**. Reported target_qed = 0.62.

## Task 2 — lead molecule
Constraints: QED≥0.6 AND psa≥40 AND not methoxy series (m05's series).
- psa≥40 excludes biphenyl (29.1) and methoxy (38.33) series entirely.
- Sugar series: m25 (best member) = 0.3148 < 0.6. Eliminated.
- Only nitro series (psa 72.24) remains. m09 C2 = 0.5826 (<0.6). m02 C3 = 0.6046 ≥ 0.6. ✓
- Nitro series ≠ methoxy series (different aromatic substituent: NO2 vs OMe).
**Lead = m02, measured QED = 0.6046.**

## Budget ledger
- 9 listing pages (1–9) = 9
- 8 measures (m15,m24,m28,m17,m22,m09,m25,m02) = 8
- Total = 17/17. Zero waste.

## answer.json
- target_qed: 0.62 (predicted from methoxy-series trend)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046 (real measure_qed call)
