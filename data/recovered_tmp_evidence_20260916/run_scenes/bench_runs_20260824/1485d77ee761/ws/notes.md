# Notes — QED task

## Setup
- Pool: 33 molecules, 4 scaffolds (from `group_by_scaffold`, free).
- Budget: 12 total, shared (list_pool + measure_qed). Each page = 1, each successful measure = 1.
  `probe_unavailable` (unsynthesized) costs 0.

## Scaffolds (scaffold -> members by N-alkyl chain length)
1. `COc1ccc(cc1)C(=O)N` (anisoyl-amide) — m05's series:
   m15(2), m24(3), m28(4), m17(5), m22(6), m05(11=unsynthesized)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzoyl amide):
   m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(O)C(=O)N` (sugar amide):
   m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl amide):
   m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Descriptors seen on page 1 (psa is constant per scaffold — alkyl chain adds only CH2, no polar atoms)
- m01 (scaffold3, ch4): mw221.25 alogp-2.022 psa110.02 rotb7 arom0
- m02 (scaffold2, ch3): mw208.22 alogp1.735  psa72.24  rotb4 arom1
- m03 (scaffold3, ch6): mw249.31 alogp-1.242 psa110.02 rotb9 arom0
- m04 (scaffold4, ch4): mw253.34 alogp3.884  psa29.1   rotb5 arom2  -> psa<40, FAILS constraint 2

So per-scaffold PSA: scaffold2=72.24, scaffold3=110.02, scaffold4=29.1, scaffold1=m05's (need a member's value).
=> Task 2 must be scaffold 2 or 3 (psa>=40). Scaffold4 fails. Scaffold1 = m05's series, excluded by constraint 3.

## Strategy
- Task 1: measure m05's series (m15,m24,m28,m17,m22 at ch=2,3,4,5,6), fit QED vs chain_len,
  evaluate at ch=11. (5 budget)
- Task 2: pick from scaffold2 or scaffold3 with measured QED>=0.6. Scaffold2 (nitro) has
  alogp in sweet spot for ch~3-5, likely high QED IF nitro not a QED alert. Probe m12(ch4), m32(ch5).

## Budget plan
- page1 list: 1 (done)
- m05 probe: 0 (unsynthesized)
- Task1 measures (m15,m24,m28,m17,m22): 5
- Task2 probes: m12, m32, then more as needed
- Cumulative after first batch (m05+5task1+m12+m32): 1+0+5+2 = 8, leaving 4.

## Measurements

### Task 1 — m05 series (scaffold `COc1ccc(cc1)C(=O)N`), QED vs chain_len
| mol | ch | qed |
|-----|----|------|
| m05 | 11 | probe_unavailable (0 cost) — must predict |
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |
Trend: peaks ~ch3 then slowly declines. Fit polynomial, evaluate at ch=11.

### Task 2 — nitro scaffold2 probes (psa 72.24 ok, not m05 series ok)
| mol | ch | qed | pass? |
|-----|----|------|-------|
| m12 | 4 | 0.4709 | NO (<0.6) |
| m32 | 5 | 0.4683 | NO (<0.6) |
Both ~0.47 => nitro caps QED (alert group). Scaffold2 rejected for Task 2.

Budget after this batch: 8/12 used, 4 left.

### Task 2 — pivot to scaffold3 (sugar amide, psa 110.02, not m05 series)
PSA constant per scaffold (CH2 adds no polar atoms). Eligible scaffolds for Task2:
scaffold2 (nitro, QED capped ~0.47) and scaffold3 (sugar, unmeasured). => must be scaffold3.
Scaffold3 members: m14(2),m27(3),m01(4),m19(5),m03(6),m21(7),m18(8),m07(9),m25(10).
m01 desc (ch4): mw221 alogp-2.022 psa110 rotb7 arom0. m03(ch6): mw249 alogp-1.242 rotb9.
alogp very negative (penalized) but improves with chain; rotb<=9 up to ch6 (good); mw low (good);
hbd~4/hba~5 fine; no alerts. QED likely 0.5-0.7 — could clear 0.6.
Bracket with m14(ch2), m01(ch4), m03(ch6); keep 1 in reserve.

### Task 1 result
fit_trend deg2 on [2,3,4,5,6]->[0.7621,0.7919,0.7518,0.7498,0.7377], x=11:
prediction = 0.5416, rmse 0.01188, coeffs [0.74932,0.017024,-0.003264].
=> target_qed (m05) = 0.5416.

### Task 2 — scaffold3 (sugar) results
| mol | ch | qed |
|-----|----|------|
| m14 | 2 | 0.3237 |
| m01 | 4 | 0.3187 |
| m03 | 6 | 0.3282 |
Flat ~0.32 (sugar: too polar/hbd-heavy -> QED low). Fails >=0.6.
Budget now 11/12 used, 1 left.

### Eligibility recheck (PSA is constant per scaffold; chain is pure -CH2-)
- scaffold1 (anisoyl, m05's): EXCLUDED (same series as m05).
- scaffold2 (nitro): psa 72.24 OK, but QED capped ~0.47 (m12,m32). <0.6.
- scaffold3 (sugar): psa 110.02 OK, but QED ~0.32. <0.6.
- scaffold4 (biphenyl): psa 29.1 < 40 -> FAILS psa constraint.
=> No scaffold yields a molecule passing all 3 constraints.

### Last-unit decision
Task2 needs a real measure_qed for lead_qed_measured. Best chance of >=0.6 anywhere:
nitro scaffold2 likely higher at low MW/low rotb. m09(ch2) is the lightest, best lipophilicity,
and lowest rotb in scaffold2 -> best QED shot. Spend last unit on m09.
(Scaffold3 staying ~0.32 across ch2/4/6 => sugar scaffold caps low; no point there.)
Scaffold4 stays psa 29.1 regardless of chain (m04 measured it) -> ineligible for ANY member.

### Final batch — last unit
| mol | scaffold | ch | qed |
|-----|----------|----|------|
| m09 | nitro(2) | 2 | 0.5826 |
m09 is the highest nitro member and still <0.6. Budget exhausted (12/12).

### Conclusion
No molecule passes all 3 Task2 constraints:
- scaffold1 excluded (same series as m05).
- scaffold2 (nitro) max 0.5826 < 0.6.
- scaffold3 (sugar) max ~0.33 < 0.6.
- scaffold4 (biphenyl) psa 29.1 < 40.
=> Task 2 unsatisfiable within budget. Answer reports the situation honestly.
Best observed (closest, valid scaffold): m09, 0.5826 — but it does NOT satisfy constraint 1.
Per instructions, report measured values only; do not fabricate a passing molecule.

### Final answer values
- target_qed (m05) = 0.5416 (fit_trend deg2, 5-point series, rmse 0.01188).
- lead_mol_id = "m09" (best measured, same-series-excluded & psa>=40 satisfied; qed 0.5826 < 0.6 — does NOT fully satisfy constraints; no molecule in pool does).
- lead_qed_measured = 0.5826.
