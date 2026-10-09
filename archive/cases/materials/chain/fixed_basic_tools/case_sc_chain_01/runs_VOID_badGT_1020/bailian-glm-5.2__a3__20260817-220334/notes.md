# Superconductor Tc study — notes

## Task structure
- Seed: `O6.080Ca1Cu2Ba2Hg1`; High = Tc >= 90 K; Budget = 34 measure_tc calls.
- Steps A (series peak) → B (filter Hg & 4-elem, Tc >= T*-32) → C (induce rule, predict unmeasurable) → D (fewest-elem high prediction, tie→higher expected Tc).

## Data inventory
measurable.txt (30): Series A = O6.0xx Ca1Cu2Ba2Hg1 (12, seed's doping series); Family-B candidates = O4.0xx Cu1Ba2Hg1 (12, Hg + exactly 4 elem); 6 "other" calibration materials (Hg/Tl, Tl, Cd/Tl, La, Y/La).
unmeasurable.txt (10): 4 overlap measurable-but-unsynthesized (O6.160, O6.270, O4.180, O4.047); 2 new Hg-1201 (O4.066, O4.160); 2 Tl-Cu2 (O8...Tl1.8, O7...Tl0.5Pb0.5); 2 Y-La-Ba-Cu-O (O8Cu3Y1..., O8Cu4Y1...).

## Step A — series peak (12 calls; 10 returned Tc, 2 no-sample)
O6.080→92.0 | 6.100→92.5 | 6.130→102.0 | 6.140→105.0 | 6.160→NO SAMPLE
6.190→122.0 | 6.210→127.0 | 6.220→127.0 | 6.240→123.0 | 6.270→NO SAMPLE
6.300→100.0 | 6.310→98.0
**T* = 127.0** (max; tie O6.210 / O6.220). Report peak = O6.210Ca1Cu2Ba2Hg1.
threshold1 = 127.0 − 32 = **95.0**.
Dome shape 92→92.5→102→105→(gap)→122→127→127→123→(gap)→100→98; peak ~6.2 O apfu.

## Step B — Hg & exactly 4 elements, Tc >= 95.0
Measurable Hg+4-elem set = 10 synthesized O4.0xx Cu1Ba2Hg1 (O,Cu,Ba,Hg). Tc vs threshold 95.0:
- O4.081→98.0 ✓ | O4.162→97.4 ✓ | O4.150→97.0 ✓ | O4.220→97.0 ✓ | O4.096→95.6 ✓ | O4.097→95.3 ✓
- O4.200→94.2 ✗ | O4.101→93.4 ✗ | O4.103→92.5 ✗ | O4.085→92.0 ✗
- O4.180 & O4.047: NO SAMPLE → excluded (no Tc to test; Step B is about measured materials).
**Q1 = {O4.081, O4.162, O4.150, O4.220, O4.096, O4.097}** (all Cu1Ba2Hg1).

## Calibration (other measured materials)
- O6Cu1Ba2Hg1Tl1 (Hg+Tl, 5e, Cu1) → 0.0 LOW
- O10Ca2Cu3Ba2Tl2 (Tl, 5e, Cu3) → 118.97 HIGH
- O6Cu1Cd0.250Ba2Tl1.750 (Tl+Cd, 5e, Cu1) → 88.0 LOW
- O7Cu3Ba2La1 (La, 5e, Cu3, no Hg/Tl) → 89.6 LOW
- O7Cu3Y0.200Ba2La0.800 (Y+La, 6e, Cu3, no Hg/Tl) → 81.0 LOW

## Step C — induced rule (multi-factor interaction)
Single-factor failure check (per task warning):
- "contains Hg": O6Cu1Ba2Hg1Tl1 has Hg → Tc=0 ⇒ fails on this measured point.
- "contains Tl": Cu1-Tl → 88/0 LOW vs Cu3-Tl → 119 HIGH ⇒ fails.
- "#Cu": Hg-1201 Cu1→98 HIGH vs Tl Cu1→0 LOW ⇒ fails.
- "#elements": 5-elem O6Cu1Ba2Hg1Tl1→0 vs 5-elem O10Ca2Cu3Ba2Tl2→119 ⇒ fails.

Governing interaction = (Hg/Tl chemistry) × (Cu-layer count), validated on ALL 25 measured points:
**HIGH iff [ (contains Hg AND NOT contains Tl)  OR  (contains Tl AND total Cu ≥ 2) ];  otherwise LOW.**
Validation: Hg-1201 (Cu1) HIGH✓; Hg-1212 (Cu2) HIGH✓; O6Cu1Ba2Hg1Tl1 (Hg+Tl,Cu1) → neither branch ⇒ LOW✓ (Tc=0); O10Ca2Cu3Ba2Tl2 (Tl,Cu3) HIGH✓; O6Cu1Cd0.25Ba2Tl1.75 (Tl,Cu1) LOW✓; O7Cu3Ba2La1 & O7Cu3Y0.2Ba2La0.8 (no Hg/Tl) LOW✓. All fit.
Why "no Tl" on the Hg branch: O6Cu1Ba2Hg1Tl1 (stuffed Hg/Tl-1201) measured 0, so Hg-alone⇒HIGH is false. Tl threshold Cu≥2 because Cu1-Tl materials are LOW and Cu3-Tl is HIGH; the 2-Cu Tl-1212/2212 family is the smallest high-Tc Tl cuprate (Cu2 unmeasured but physically HIGH).

### Predictions for unmeasurable (10)
1. O6.160Ca1Cu2Ba2Hg1 — Hg, no Tl ⇒ **HIGH** (series-bracketed 105–122)
2. O6.270Ca1Cu2Ba2Hg1 — Hg, no Tl ⇒ **HIGH** (series-bracketed 123–100)
3. O4.180Cu1Ba2Hg1 — Hg-1201, no Tl ⇒ **HIGH** (~96, bracketed 97.4↘94.2)
4. O4.047Cu1Ba2Hg1 — Hg-1201, no Tl ⇒ **HIGH** (family HIGH; low-O tail, ≥90)
5. O4.066Cu1Ba2Hg1 — Hg-1201, no Tl ⇒ **HIGH** (~92–95)
6. O4.160Cu1Ba2Hg1 — Hg-1201, no Tl ⇒ **HIGH** (~97.2, bracketed 97.0–97.4)
7. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl, no Hg, Cu2.2≥2 ⇒ **HIGH** (Tl-2212 family)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl(+Pb), no Hg, Cu2≥2 ⇒ **HIGH** (Tl/Pb-1212)
9. O8Cu3Y1Ba1.900La0.100 — no Hg, no Tl ⇒ **LOW** (Y/La-Ba-Cu-O analogs measured 89.6/81)
10. O8Cu4Y1Ba1.870La0.130 — no Hg, no Tl ⇒ **LOW** (Y-124-like, no Hg/Tl)

Note: predictions for #4/5 (low-O Hg-1201 tail) and #7/8 (Tl Cu2) carry extrapolation risk, but they do NOT affect Step D — see below.

## Step D — final pick
Predicted HIGH: #1,2 (5 elem), #3,4,5,6 (4 elem), #7 (5 elem), #8 (6 elem).
Fewest elements = 4 ⇒ candidates {O4.180, O4.047, O4.066, O4.160} (Hg-1201).
Tie-break = higher expected Tc (Hg-1201 measured dome, O4.0xx→Tc):
081→98.0, 085→92.0, 096→95.6, 097→95.3, 101→93.4, 103→92.5, 150→97.0, 162→97.4, 200→94.2, 220→97.0.
- O4.160: bracketed by 4.150(97.0) & 4.162(97.4) ⇒ ~97.2 (best-bracketed, highest)
- O4.180: bracketed by 4.162(97.4) & 4.200(94.2) ⇒ ~96
- O4.066: below lowest measured 4.085(92.0) ⇒ ~90–92 (extrapolated down)
- O4.047: far below 4.085 ⇒ ~85–90 (lowest, most uncertain)
**FINAL = O4.160Cu1Ba2Hg1** (4 elements, highest well-bracketed expected Tc ~97.2).
Robustness: even if #4/5 or #7/8 predictions are wrong, they cannot beat a 4-element Hg-1201 candidate, so the final is unaffected.

## Budget
25/34 used. No-sample calls (O6.160, O6.270, O4.180, O4.047) and the one typo-error call (O6Cu1Ba2Tl1→unknown) did not charge budget. All 25 synthesized materials measured; the 10 unmeasurable have no physical samples, so remaining budget cannot refine predictions.
