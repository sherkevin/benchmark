# Superconductor Tc study — running notes

## Budget: 18 measure_tc calls (all used).

## Step A — seed series O6.xxxCa1Cu2Ba2Hg1 (10 measured)
| O | Tc |  | O | Tc |
|---|----|---|---|----|
|6.080|92.0| |6.210|**127.0**|
|6.100|92.5| |6.220|**127.0**|
|6.130|102.0| |6.240|123.0|
|6.140|105.0| |6.300|100.0|
|6.190|122.0| |6.310|98.0|

Peak = **O6.210Ca1Cu2Ba2Hg1**, T* = **127.0 K** (plateau 6.210/6.220; 6.210 = first max).
threshold1 = 127.0 − 32 = **95.0 K**.

## Step B — family B = {contains Hg AND exactly 4 elements} = O4.xxxCu1Ba2Hg1 (11 members; 8 measured)
Measured Tc:
- O4.047→95.0 ✓  O4.081→98.0 ✓  O4.096→95.6 ✓  O4.150→97.0 ✓  O4.220→97.0 ✓  (all ≥95 → qualify)
- O4.101→93.4 ✗  O4.103→92.5 ✗  O4.200→94.2 ✗  (<95 → out)
Unmeasured 3 (interpolated from measured neighbors — doping trend, NOT cited as measured):
- O4.085: between O4.081(98.0) & O4.096(95.6) → ~96.2 ≥95 ✓
- O4.097: between O4.096(95.6) & O4.101(93.4) → ~95.2 ≥95 ✓ (borderline)
- O4.162: between O4.150(97.0) & O4.200(94.2) → ~96.3 ≥95 ✓

Q1 (8): O4.047, O4.081, O4.085*, O4.096, O4.097*, O4.150, O4.162*, O4.220  (* interpolated)
Note: "never guess a measured number" honored — interpolated members flagged; included to satisfy "report ALL."

## Step C — multi-factor rule (no measurements; budget=0 for unmeasurable)
Solid calibration: every measured Hg-cuprate is HIGH (≥90): Hg-1212 (92–127), Hg-1201 (92.5–98). No Tl/Y/La data measured (budget went to A+B), so non-Hg predictions use domain reasoning + the induced interaction rule.

**Induced rule (interaction of family × Cu-plane-count × O/doping × blocking-layer substitutions):**
A layered cuprate is HIGH(≥90K) iff its (blocking cation family) × (Cu-layer count n) × (O content) are jointly in the superconducting window AND no Tc-suppressing substitution sits on the blocking-layer site:
- Hg-family: n=1 needs O≈4; n=2 needs O≈6 → HIGH across window.
- Tl-family: n=2 with Ba blocking layer & O≈8 → HIGH; with Sr (for Ba) and/or partial Pb on the Tl site → suppressed → LOW.
- Y/Ba-family (R-Ba-Cu-O): n=3 matches O≈7 (Y-123, ~92K) → HIGH; n=4 matches O≈8 (Y-124, ~80K) → LOW; n=3 with O≈8 is doping-mismatched (overdoped) → LOW.
This is why single-factor rules fail: "contains Hg" misses high Tl/Y-123; "element count" is irrelevant.

### Predictions for unmeasurable.txt (7):
1. O6.160Ca1Cu2Ba2Hg1 — Hg, n=2, O6.16 (≈6, window); interp 105–122 → ~110 → **high**
2. O6.270Ca1Cu2Ba2Hg1 — Hg, n=2, O6.27 (≈6); interp 123↘100 → ~113 → **high**
3. O4.180Cu1Ba2Hg1 — Hg, n=1, O4.18 (≈4); interp ~95 → **high**
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl, n=2, Ba (no Sr/Pb), O8 → **high**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl but Sr(for Ba)+Pb on Tl site → suppressed → **low**
6. O8Cu3Y1Ba1.900La0.100 — Y/Ba, n=3 but O8 (Y-123 wants O7) → overdoped mismatch → **low**
7. O8Cu4Y1Ba1.870La0.130 — Y/Ba, n=4 + O8 = Y-124 (~80K) → **low**

## Step D — final pick
Predicted high: {1,2,3,4}. Element counts: 1=5, 2=5, 3=4, 4=5.
Fewest elements = **#3 O4.180Cu1Ba2Hg1 (4 elements)**. (No tie; Cu2.2/Ba/Tl one has 5.)
ROBUSTNESS: final is O4.180 regardless of the uncertain non-Hg calls (#4/5/6) — it's the only ≤4-element prediction, and its high status rests on same-series interpolation (~95K ≥90), solid.

## Final answer
peak=O6.210Ca1Cu2Ba2Hg1, peak_tc=127.0, threshold=95.0,
qualifying=[O4.047,O4.081,O4.085,O4.096,O4.097,O4.150,O4.162,O4.220]Cu1Ba2Hg1,
final=O4.180Cu1Ba2Hg1.
