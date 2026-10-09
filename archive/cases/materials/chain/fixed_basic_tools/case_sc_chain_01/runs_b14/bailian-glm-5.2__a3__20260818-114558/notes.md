# Superconductor Tc study — running notes

## Task overview
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- Definition: **high** if Tc >= 90 K, else **low**.
- Budget: 14 `measure_tc` calls total.
- 26 measurable, 7 unmeasurable.

## Step A — series peak (seed family {O,Ca,Cu,Ba,Hg}, varying O)
Series members (measurable lines 1-10) and measured Tc:
| composition | Tc |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0**  ← max (tie) |
| O6.220Ca1Cu2Ba2Hg1 | **127.0**  ← max (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0**, peak composition = O6.210Ca1Cu2Ba2Hg1 (first occurrence of the max; plateau O6.21–6.22).
All series members are HIGH (>=90). This is Hg-1212 (HgBa2CaCu2O6+δ), real Tc ~127 K ✓.

Budget used so far: 10/14. Remaining: 4.

## Step B — filter (contains Hg AND exactly 4 elements), Tc >= threshold1
threshold1 = T* - 32 = 127.0 - 32 = **95.0**.

Candidates (contain Hg, exactly 4 elements) in measurable.txt = the O4.xxxCu1Ba2Hg1 series
(elements {O,Cu,Ba,Hg}, 4 elements), lines 11-21:
O4.047, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220.
This is Hg-1201 (HgBa2CuO4+δ), real ambient Tc ~94 K → likely ALL just under 95.0 (Q1 empty?).
Need to measure to confirm. (Unmeasurable O4.180Cu1Ba2Hg1 also qualifies on filter but has no sample.)

## Step C — induce rule for unmeasurable (high/low)
Unmeasurable set + predicted physics:
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1212 (2Cu,Hg). Interpolate seed: between O6.14(105)&O6.19(122) → ~113. HIGH.
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1212 (2Cu,Hg). Between O6.24(123)&O6.30(100) → ~110. HIGH.
3. O4.180Cu1Ba2Hg1 — Hg-1201 (1Cu,Hg). ~94? → HIGH if >=90. KEY PIVOT for Step D. Measure O4 bracket.
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl-1212-ish (2Cu,Tl). Real ~100 → HIGH (no measurable analog; by rule).
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl/Pb-1212 (2Cu,Tl). → HIGH (by rule).
6. O8Cu3Y1Ba1.900La0.100 — YBCO-123 (3Cu,Y). Real ~92 → HIGH. Measure to confirm.
7. O8Cu4Y1Ba1.870La0.130 — Y-248 (4Cu,Y). Real ~80 → LOW. Measure (critical low anchor).

## Step D — among predicted HIGH, fewest elements (tie→higher expected Tc)
If #3 (O4.180, 4 elements) is HIGH → it has fewest elements → final = O4.180Cu1Ba2Hg1.
If #3 is LOW → high candidates are 5-6 elements; tiebreak by highest expected Tc (Hg-1212 ~113-118 wins).

## Remaining measurement plan (4 calls)
Pivotal unknown = O4.180 (unmeasurable). Bracket it with measurable O4.162 & O4.200.
Also need Y-248 low anchor and YBCO-123 high anchor.
- Call 11: O4.162Cu1Ba2Hg1 (bracket below 4.180, + peak region)
- Call 12: O4.200Cu1Ba2Hg1 (bracket above 4.180, near peak)
- Call 13: O8Cu4Y1Ba1.870La0.130 (Y-248, low anchor for #7 + rule)
- Call 14 (reserve): decide after seeing O4 results — likely O8Cu3Y1Ba1.900La0.100 (YBCO-123 #6) unless O4 needs a 3rd point.

## Measurement log
1. O6.080Ca1Cu2Ba2Hg1 → 92.0
2. O6.100Ca1Cu2Ba2Hg1 → 92.5
3. O6.130Ca1Cu2Ba2Hg1 → 102.0
4. O6.140Ca1Cu2Ba2Hg1 → 105.0
5. O6.190Ca1Cu2Ba2Hg1 → 122.0
6. O6.210Ca1Cu2Ba2Hg1 → 127.0
7. O6.220Ca1Cu2Ba2Hg1 → 127.0
8. O6.240Ca1Cu2Ba2Hg1 → 123.0
9. O6.300Ca1Cu2Ba2Hg1 → 100.0
10. O6.310Ca1Cu2Ba2Hg1 → 98.0
