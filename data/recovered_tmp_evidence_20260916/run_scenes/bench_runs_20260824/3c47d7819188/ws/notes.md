# Notes

## Setup
- Budget: 12 measurements total, shared by both tasks.
- 4 scaffolds (congeneric series), each varying only N-alkyl chain length.

## Scaffold groups
1. `COc1ccc(cc1)C(=O)N` (psa=38.33): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, unsynthesized)**
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa=72.24): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(O)C(=O)N` (psa=110.02): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa=29.1): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1 — predict QED of m05 (chain_len=11)
m05 is in scaffold #1. Measure the 5 synthesizable series members, fit trend, evaluate at x=11.

Measurements (m05 series):
| chain_len | mol_id | qed |
|---|---|---|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

Trend: peaks near chain_len=3, declines slowly after. Extrapolate to 11.

## Task 2 — pick one molecule: QED>=0.6 AND psa>=40 AND not in m05's series
- m05's series = scaffold #1 (psa=38.33, <40 anyway). Exclude scaffold #1 and #4 (psa=29.1<40).
- Eligible by psa>=40: scaffold #2 (NO2, psa=72.24) and #3 (OCC, psa=110.02).
- Scaffold #3 has very low alogp and psa=110 (low PSA desirability) → likely low QED.
- Scaffold #2 (NO2): shorter chains likely higher QED. m32(5)=0.4683, m33(6)=0.4591 (low). Try short chains m09(2), m02(3), m12(4).

## Task 1 — prediction
Quadratic fit through the 5 series points (chain_len 2..6) evaluated at x=11:
- prediction = **0.5416**, rmse=0.01188, coeffs [0.74932, 0.017024, -0.003264] (a·x² + b·x + c).
Trend: QED peaks ~chain_len 3 then slowly declines; at chain_len 11 (extrapolation) → 0.54.

## Task 2 — analysis of which series can reach 0.6
- Scaffold #1 (m05's): psa=38.33 <40 AND excluded by constraint 3. Out.
- Scaffold #4 (biphenyl): psa=29.1 <40. Out.
- Scaffold #2 (NO2, psa=72.24): measured m32=0.4683, m33=0.4591.
  qed_components(m32): MW0.69, ALOGP0.99, HBA0.98, HBD0.99, PSA0.93, ROTB0.69, AROM0.83, **ALERTS0.0817**.
  → Nitro group is a fixed structural alert across the WHOLE series; ALERTS≈0.08 tanks overall
    QED regardless of chain. Even best NO2 (m09, alogp≈1.35, rotb=3) capped ~0.55-0.57 < 0.6. OUT.
- Scaffold #3 (OCC sugar-amide, psa=110.02): no nitro → ALERTS≈1.0. Eligible. ROTB is the punisher
  (f_ROTB ~1.0 at rotb≤4, →0 by rotb≈10.4). So short chains best: m14(rotb5) is the best OCC bet;
  long chains (m07 rotb12, m25 rotb13) get ROTB≈0 → dead.
  Decision: measure **m14** (chain_len=2, mw193, alogp-2.802, rotb5, psa110).

## Task 2 — second round of measurements
- m14 (OCC, chain2): QED=0.3237. qed_components: MW0.406, ALOGP0.0494,
  HBA0.7262, HBD0.0555, PSA0.5829, ROTB0.8476, AROM0.4725, ALERTS0.8425.
  → OCC scaffold has 4 OH donors → HBD≈0.055 FIXED across the whole series. Caps OCC low.
- m03 (OCC, chain6): 0.3282.  m07 (OCC, chain9): 0.3233.
  → OCC series FLAT ~0.32 across chains 2/6/9. HBD drag dominates; chain length irrelevant.
  OCC series ceiling ≈ 0.33. DEAD for Task 2 (need ≥0.6).

## Refined NO2 ceiling analysis
m32 measured 0.4683 with ALERTS=0.0817. The nitro alert is FIXED across the whole NO2
series. Using Bickerton weights, compare short-chain NO2 members to m32 (only MW & ROTB
change meaningfully; HBA/HBD/PSA/AROM/ALERTS fixed by scaffold, ALOGP near-peak for all):
  m12 (chain4, mw222, rotb5): MW~0.60, ROTB~0.85  → ΔQED vs m32 ≈ +0.003  (~0.471)
  m02 (chain3, mw208, rotb4): MW~0.50, ROTB~1.00  → ≈ m32             (~0.468)
  m09 (chain2, mw194, rotb3): MW~0.41, ROTB~0.95  → MW loss > ROTB gain  (~0.453)
  m31 (chain8, mw278, rotb9): MW~0.95, ROTB~0.40  → ROTB loss dominates  (~0.446)
Crux: any QED formula reproducing m32=0.468 must weight the ALERTS(0.0817) term heavily;
since short chains share that fixed ALERTS drag and their MW/ROTB trade ~cancels, NO NO2
member can reach 0.6. Best NO2 ≈ 0.47.

## CONCLUSION so far
- Scaffold #1 (m05's): excluded by constraint 3 AND psa=38.33<40.
- Scaffold #4 (biphenyl): psa=29.1<40, excluded.
- Scaffold #2 (NO2, psa72): nitro ALERTS caps it ~0.47. <0.6.
- Scaffold #3 (OCC, psa110): HBD drag caps it ~0.33. <0.6.
No molecule obviously satisfies all three. But task framing implies one exists, so the
formula may be kinder than my m32-extrapolation. DECISIVE TEST: measure the best NO2
short chain m12 (best non-alert profile: MW0.60+ALOGP1.0+ROTB0.85) directly.

## Task 2 — round 3
- m12 (NO2, chain4): QED=0.4709. Confirms NO2 ceiling ~0.47 < 0.6. DEAD.
- Budget left: 1.

## Final deliberation
Eligible scaffolds (psa≥40, not m05's series #1):
- #2 NO2: capped ~0.47 by fixed ALERTS(nitro)=0.0817. Verified m32=0.468, m12=0.471.
- #3 OCC: capped ~0.33 by fixed HBD(4×OH)=0.055. Verified m14=0.324, m03=0.328, m07=0.323.
Both sub-0.6 by construction (a QED desirability near 0 dominates the geometric-ish weighted mean).
Constraint 3 ("not same series as m05") excludes ALL of scaffold #1 anyway, and #4 fails psa<40.

So no pool member satisfies all three constraints simultaneously. The constraints are
mutually exclusive given the catalogue: every psa≥40 molecule is either an NO2 nitro
(alert drag) or an OCC polyol (HBD drag), and both drag their QED below 0.6.

Task 2's answer is the closest achievable: the highest-QED measured molecule that
satisfies psa≥40 and the series constraint. That is **m12** (NO2, chain4):
  QED 0.4709 (measured), psa 72.24 ≥40, series #2 ≠ m05's series #1.
It does NOT reach 0.6, but it is the best eligible molecule — no better one can exist
given the fixed structural ceilings of the two eligible scaffolds.

## Decision
- lead_mol_id = m12, lead_qed_measured = 0.4709 (real measurement, mol_id m12).
- target_qed (m05) = 0.5416 (trend prediction at chain_len 11).
