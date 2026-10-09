# Notes — Superconductor Tc study

## Budget reality
- 18 measure_tc calls HARD-capped. Used exactly 18 (10 on series 1, 8 on series 2).
- 3 extra attempts (O4.081/097/103) returned "instrument time exhausted", did NOT consume budget, gave no data.
- I over-invested in series 1+2 and measured NONE of the assorted (Tl / Y-La family) materials. For Step C on non-Hg unmeasurables I therefore rely on layered-cuprate domain knowledge (structure type + CuO2-plane count + doping dome), supported where possible by interpolation from the measured Hg series. This limitation is unavoidable given the spent budget.

## Step A — measured Tc (K)

Series 1 — Ca1Cu2Ba2Hg1 (Hg-1223, 5 elements; the seed's homologous O-doping series):
| O   | 6.080 | 6.100 | 6.130 | 6.140 | 6.190 | 6.210 | 6.220 | 6.240 | 6.300 | 6.310 |
| Tc  | 92.0  | 92.5  | 102.0 | 105.0 | 122.0 | 127.0 | 127.0 | 123.0 | 100.0 | 98.0  |
Dome peaks at O6.21–6.22. MAX Tc = 127.0 (tie: O6.210 and O6.220). Take first → peak = O6.210Ca1Cu2Ba2Hg1.
**T\* = 127.0**

Series 2 — Cu1Ba2Hg1 (Hg-1201, 4 elements; the only 4-element-Hg family → Step B candidates):
| O   | 4.047 | 4.081 | 4.085 | 4.096 | 4.097 | 4.101 | 4.103 | 4.150 | 4.162 | 4.200 | 4.220 |
| Tc  | 95.0  |  n/m  | 92.0  | 95.6  |  n/m  | 93.4  |  n/m  | 97.0  | 97.4  | 94.2  | 97.0  |
(n/m = not measured, budget exhausted.)

## Step B — Q1
threshold1 = T\* − 32 = 127.0 − 32 = **95.0**
Family filter: contains Hg AND exactly 4 elements → only the Cu1Ba2Hg1 (Hg-1201) series qualifies.
Among MEASURED members with Tc >= 95.0 (cannot guess the 3 unmeasured — rule forbids guessing measured numbers):
- O4.047 → 95.0 ✓
- O4.096 → 95.6 ✓
- O4.150 → 97.0 ✓
- O4.162 → 97.4 ✓
- O4.220 → 97.0 ✓
**Q1 = {O4.047, O4.096, O4.150, O4.162, O4.220}Cu1Ba2Hg1**
Excluded (unmeasured, cannot certify): O4.081, O4.097, O4.103. By neighbor interpolation O4.081/O4.103 are likely <95 (near 93), O4.097 borderline near 95.6 — but not cited without measurement.

## Step C — predictions for unmeasurable.txt
Rule induced (multi-factor interaction; see answer.json "rule" field):
"High Tc (>=90 K) requires JOINTLY: (i) a Ba-based (not Sr) alkaline-earth spacer; (ii) a standard layered-cuprate block (1201/1212/1223 Hg, 2212/2223 double-Tl, or Y-123 Cu3) with 1–3 CuO2 sheets — NOT the Y-124 double-chain (Cu4); and (iii) oxygen within the family's optimal-doping dome."

Applying:
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1223, O6.16 near dome peak (between 6.140@105 and 6.190@122 → ~113 K) → **high**
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1223, O6.27 (between 6.240@123 and 6.300@100 → ~110 K) → **high**
3. O4.180Cu1Ba2Hg1 — Hg-1201, O4.18 (between 4.162@97.4 and 4.200@94.2 → ~96 K) → **high**
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl1.8≈2 (double Tl layer), Ba2, ~2 CuO2 sheets → Tl-22xx (2212/2223) block, ~108 K → **high**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — single (Tl0.5Pb0.5) apical layer + Sr (not Ba) spacer → Sr-based 1212, ~80 K → **low**
6. O8Cu3Y1Ba1.900La0.100 — Y-123 structure (Cu3), Ba2, small La substitution → ~90–92 K → **high**
7. O8Cu4Y1Ba1.870La0.130 — Y-124 double-chain structure (Cu4), O8 → ~80 K → **low**

## Step D — final pick
Predicted-high unmeasurables and element counts:
- O6.160Ca1Cu2Ba2Hg1 → 5
- O6.270Ca1Cu2Ba2Hg1 → 5
- O4.180Cu1Ba2Hg1 → 4   ← fewest
- O8Ca0.850Cu2.200Ba2Tl1.800 → 5
- O8Cu3Y1Ba1.900La0.100 → 5
Fewest elements = **O4.180Cu1Ba2Hg1** (unique minimum, no tie-break needed). Expected Tc ~96 K.
