# Superconductor Tc Study - Notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`; Budget: 45; High = Tc >= 90 K
- Step A: peak (max Tc) of seed's homologous doping series
- Step B: threshold1 = T* - 32; Hg + exactly 4 elements, Tc >= threshold1 → Q1
- Step C: predict high/low for ALL unmeasurable found via search_db
- Step D: predicted-high unmeasurable, fewest elements (tie-break higher expected Tc)

## DB search "Hg" (budget 7, 27 materials, has_more=false at p7 — complete Hg universe)
O4-series (4 elem: O,Cu,Ba,Hg): O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220 (Cu1Ba2Hg1)
O6-Ca series (5 elem: O,Ca,Cu,Ba,Hg): O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310 (Ca1Cu2Ba2Hg1)
O6Cu1Ba2Hg1Tl1 (5 elem)

## Step A — seed series (OxCa1Cu2Ba2Hg1), Tc
- O6.080: 92.0 (b8)
- O6.100: 92.5 (b9)
- O6.130: 102.0 (b10)
- O6.140: 105.0 (b11)
- O6.160: NO SAMPLE (unmeasurable)
- O6.190: 122.0 (b12)
- O6.210: 127.0 (b13) ← MAX (tie)
- O6.220: 127.0 (b14) ← MAX (tie)
- O6.240: 123.0 (b15)
- O6.270: NO SAMPLE (unmeasurable)
- O6.300: 100.0 (b16)
- O6.310: 98.0 (b17)
Tried O6.150/.170/.180/.200/.230/.250/.260 → "unknown composition" (don't exist).
**T* = 127 K**, peak = `O6.220Ca1Cu2Ba2Hg1` (tie with O6.210; both = 127).

## Step B — threshold1 = 127 - 32 = 95 K
Filter: Hg + exactly 4 elements → O4.xxxCu1Ba2Hg1 series. Measured:
- O4.047: NO SAMPLE; O4.066: NO SAMPLE; O4.081: 98.0(b18)✓; O4.085: 92.0(b19)✗;
- O4.096: 95.6(b20)✓; O4.097: 95.3(b21)✓; O4.101: 93.4(b22)✗; O4.103: 92.5(b23)✗;
- O4.150: 97.0(b24)✓; O4.160: NO SAMPLE; O4.162: 97.4(b25)✓; O4.180: NO SAMPLE;
- O4.200: 94.2(b26)✗; O4.220: 97.0(b27)✓.
O6Cu1Ba2Hg1Tl1: 0.0 (b28; 5 elem → excluded from B filter).
**Q1 (Tc≥95):** O4.081, O4.096, O4.097, O4.150, O4.162, O4.220 (all Cu1Ba2Hg1).

## Step C — extrapolate unmeasurable (6 found via Hg search)
O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1.

### Measured data per family
O6-Ca: (6.08,92),(6.10,92.5),(6.13,102),(6.14,105),[6.16?],(6.19,122),(6.21,127),(6.22,127),(6.24,123),[6.27?],(6.30,100),(6.31,98). Dome peak ~6.215.
O4: (4.081,98),(4.085,92),(4.096,95.6),(4.097,95.3),(4.101,93.4),(4.103,92.5),(4.150,97),(4.162,97.4),(4.200,94.2),(4.220,97). All in [92,98] — broad high plateau, non-monotonic (NOT a clean rising-with-O trend).

### Rule (multi-factor, family-specific dome/plateau)
Within each homologous family, interpolate Tc from neighbors (dome for O6-Ca, broad plateau for O4); classify high if interpolated/extrapolated Tc ≥ 90.
This is multi-factor (family identity + oxygen position), NOT a single global O→Tc map.

### Predictions
- O6.160: between 6.140(105) & 6.190(122) → ~113 → **high**.
- O6.270: between 6.240(123) & 6.300(100) → ~108-112 → **high**.
- O4.160: between 4.150(97.0) & 4.162(97.4) → ~97 → **high**.
- O4.180: between 4.162(97.4) & 4.200(94.2) → ~96 → **high**.
- O4.047 & O4.066: lie just below the lowest measured O4 point (4.081=98). A naive single-factor rule ("lower O → lower Tc / underdoped → low") would predict LOW. The warning ("single-factor rules are wrong on several") flags exactly this: the O4 family is a broad high plateau (all measured 92–98, non-monotonic), so single-factor monotonic O fails. Multi-factor plateau extrapolation → still ≥ 90 → **high**.
⇒ all 6 unmeasurables = **high**. (The two single-factor-trap cases are O4.047 & O4.066.)

## Step D — final pick
Predicted-high 4-element unmeasurables: O4.047, O4.066, O4.160, O4.180 (all 4 elem).
Tie-break higher expected Tc: O4.160 (~97) > O4.180 (~96) > O4.066 (~92-95) > O4.047 (~90-95).
**Final = O4.160Cu1Ba2Hg1** (highest expected Tc, robust regardless of O4.047/066 uncertainty).

## Budget
Search 7 + measurable A(10→b17) + measurable B(11→b28) = 28 used; 17 unused.
6 unmeasurable attempts didn't charge budget.

## FINAL
- peak: O6.220Ca1Cu2Ba2Hg1; peak_tc: 127; threshold: 95
- qualifying: [O4.081, O4.096, O4.097, O4.150, O4.162, O4.220]Cu1Ba2Hg1
- predictions: all 6 unmeasurables = high
- final: O4.160Cu1Ba2Hg1
- rule: family-specific dome/plateau interpolation of Tc vs O (multi-factor: family + O-position), high if ≥90; avoids single-factor O→Tc monotonic trap.
