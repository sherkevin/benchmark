# Superconductor Tc study — notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- "high" = Tc >= 90 K, else "low"
- Budget: 24 measure_tc calls
- Step A: find max Tc (T*) among the homologous doping series (same elements as seed, vary O)
- Step B: threshold1 = T* - 32; Q1 = materials containing Hg AND exactly 4 elements, with Tc >= threshold1
- Step C: induce a multi-factor rule separating high/low; predict for every unmeasurable composition
- Step D: among unmeasurable predicted "high", pick fewest elements (tie-break higher expected Tc)

## Budget plan
- Step A: seed series = lines 1-10 of measurable.txt (O,Ca,Cu,Ba,Hg; O 6.080-6.310) → 10 calls
- Step B: 4-element + Hg materials = lines 11-21 (O4.xCu1Ba2Hg1, elements O,Cu,Ba,Hg = 4) → 11 calls
- Remaining: 24 - 10 - 11 = 3 calls for diverse labeled examples (rule induction for Step C)

## Element counts (for "exactly 4 elements" filter)
- Lines 1-10 (O6.xCa1Cu2Ba2Hg1): O,Ca,Cu,Ba,Hg = 5 elements
- Lines 11-21 (O4.xCu1Ba2Hg1): O,Cu,Ba,Hg = 4 elements  ← Hg + 4 elements
- Line 22 O6Cu1Ba2Hg1Tl1: O,Cu,Ba,Hg,Tl = 5
- Line 23 O10Ca2Cu3Ba2Tl2: O,Ca,Cu,Ba,Tl = 5, no Hg
- Line 24 O6Cu1Cd0.250Ba2Tl1.750: O,Cu,Cd,Ba,Tl = 5, no Hg
- Line 25 O7Cu3Ba2La1: O,Cu,Ba,La = 4, no Hg
- Line 26 O7Cu3Y0.200Ba2La0.800: O,Cu,Y,Ba,La = 5, no Hg

## Unmeasurable (cannot measure — must predict)
1. O6.160Ca1Cu2Ba2Hg1  (Hg-1212, δ=0.160) — same series as Step A → interpolate
2. O6.270Ca1Cu2Ba2Hg1  (Hg-1212, δ=0.270) — same series → interpolate
3. O4.180Cu1Ba2Hg1     (Hg-1201, δ=0.180) — same series as Step B → interpolate
4. O8Ca0.850Cu2.200Ba2Tl1.800  (Tl-1212-like, 2 Cu, Ba) 
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  (Tl/Pb-1212, Sr-based)
6. O8Cu3Y1Ba1.900La0.130      (Y-123-like, 3 Cu)
7. O8Cu4Y1Ba1.870La0.130      (Y-124-like, 4 Cu)

## Step A — Hg-1212 series O6.xCa1Cu2Ba2Hg1 (5 elements, NOT in Q1)
| δ | Tc |
|---|---|
| 0.080 | 92.0 |
| 0.100 | 92.5 |
| 0.130 | 102.0 |
| 0.140 | 105.0 |
| 0.190 | 122.0 |
| 0.210 | **127.0** ← T* |
| 0.220 | **127.0** ← T* (tie) |
| 0.240 | 123.0 |
| 0.300 | 100.0 |
| 0.310 | 98.0 |
→ T* = 127.0 K. Peak composition: O6.210Ca1Cu2Ba2Hg1 (first to reach max).
threshold1 = 127 - 32 = 95.0 K.

## Step B — Hg-1201 series O4.xCu1Ba2Hg1 (Hg + exactly 4 elements: O,Cu,Ba,Hg)
| δ | Tc | ≥95? |
|---|---|---|
| 0.047 | 95.0 | ✓ |
| 0.081 | 98.0 | ✓ |
| 0.085 | 92.0 | — |
| 0.096 | 95.6 | ✓ |
| 0.097 | 95.3 | ✓ |
| 0.101 | 93.4 | — |
| 0.103 | 92.5 | — |
| 0.150 | 97.0 | ✓ |
| 0.162 | 97.4 | ✓ |
| 0.200 | 94.2 | — |
| 0.220 | 97.0 | ✓ |
Q1 (≥95) = {O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} Cu1Ba2Hg1 (7 members).
(No other measurable material is both Hg-containing AND 4-element: lines 1-10 have 5 elems, line 22 has 5.)

## Step C — rule induction
Unmeasurable (cannot be measured; must predict):
1. O6.160Ca1Cu2Ba2Hg1  — Hg-1212, δ=0.160. Interpolate Step A dome (0.140→105, 0.190→122): ~115 → HIGH
2. O6.270Ca1Cu2Ba2Hg1  — Hg-1212, δ=0.270. Interpolate (0.240→123, 0.300→100): ~108 → HIGH
3. O4.180Cu1Ba2Hg1     — Hg-1201, δ=0.180. Interpolate Step B (0.162→97.4, 0.200→94.2): ~96 → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl1.8≈double-Tl, 2 Cu, Ba2, Ca. ~Tl-2212 (Tl2Ba2CaCu2O8) ~108 → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  — Tl0.5+Pb0.5=1 (single spacer site), 2 Cu, Sr2 (smaller than Ba) → LOW
6. O8Cu3Y1Ba1.900La0.130     — Y-123 (3Cu) La-doped on Ba site, O8 overdoped → borderline → LOW
7. O8Cu4Y1Ba1.870La0.130     — Y-124 (4Cu, double CuO chain) ~80 K → LOW

Anchoring measurements (3 calls, lines 23/24/25): Tl-2223, Tl-2201-ish, La-123.

## Anchoring results
| composition | Tc | structural reading |
|---|---|---|
| O10Ca2Cu3Ba2Tl2 | 118.97 | Tl₂Ba₂Ca₂Cu₃O₁₀ — Tl-2223, two TlO spacer layers + 3 CuO₂ → HIGH (confirms 2-spacer+≥3Cu family) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | (Tl,Cd)Ba₂CuO₆ — Tl-1201-like, single spacer + 1 CuO₂ → LOW (confirms single-Tl+1Cu is low) |
| O7Cu3Ba2La1 | 89.6 | LaBa₂Cu₃O₇ — 123-like, La on Ba site, O7 (underdoped vs 8) → LOW (La-123 underdoped is low) |

These three pin down the rule with both high and low examples.

## Final rule (Step C)
Multi-factor — high Tc requires BOTH (a) enough CuO₂ planes (≥2) AND (b) a double-layer (rock-salt) spacer that gives a large enough c-axis / carrier reservoir: either a heavy 5d spacer bilayer (Hg or Tl₂ ≈ Tl_total≥1.5 in the Tl site) on Ba₂, OR the 1212/2212 (M,Ba)₂CaCu₂ motif at near-optimal oxygen (δ ≈ 0.10–0.30). Single-layer Tl (Tl_total≈1, possibly Pb-substituted), single CuO₂ (1201-type), La-on-Ba 123 at O7, and 124 (4-Cu) at O8 are all low. Falling below any one factor drops Tc below ~90 K.

## Predictions for unmeasurable
1. O6.160Ca1Cu2Ba2Hg1  → high (Hg-1212, δ=0.160 on the measured dome → ~115 K)
2. O6.270Ca1Cu2Ba2Hg1  → high (Hg-1212, δ=0.270 → ~108 K)
3. O4.180Cu1Ba2Hg1     → high (Hg-1201, δ=0.180 → ~96 K, 4-element Hg family per Step B)
4. O8Ca0.850Cu2.200Ba2Tl1.800 → high (Tl≈1.8 double-Tl spacer + 2 Cu + Ba + Ca → Tl-2212/2223 family, ~108 K)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  → low (Tl+Pb=1 → single spacer; Sr₂ instead of Ba₂) 
6. O8Cu3Y1Ba1.900La0.130     → low (Y-123 La-substituted; anchoring La-123@O7 = 89.6 low)
7. O8Cu4Y1Ba1.870La0.130     → low (Y-124 double-chain, ~80 K)

High predictions: #1, #2, #3, #4.

## Step D — final pick
High set: {O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800}
Element counts:
- O6.160Ca1Cu2Ba2Hg1: O,Ca,Cu,Ba,Hg = 5
- O6.270Ca1Cu2Ba2Hg1: 5
- O4.180Cu1Ba2Hg1: O,Cu,Ba,Hg = 4 ← fewest
- O8Ca0.850Cu2.200Ba2Tl1.800: O,Ca,Cu,Ba,Tl = 5
Final = O4.180Cu1Ba2Hg1 (4 elements; expected ~96 K).

## Budget audit
10 (Step A) + 11 (Step B) + 3 (anchors) = 24/24. ✓ Done.


