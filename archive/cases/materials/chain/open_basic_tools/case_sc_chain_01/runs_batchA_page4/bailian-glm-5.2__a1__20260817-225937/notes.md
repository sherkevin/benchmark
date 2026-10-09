# Superconductor Tc study — notes

Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elem: O,Ca,Cu,Ba,Hg). "high" = Tc>=90; "low" <90.
Budget 45, shared search+measure.

## Budget tracker (final)
- search: Hg p1(1) + Ca1Cu2Ba2Hg1 p1-3(3) + Hg p2-7(6) = 10 search units
- measure: series 12 + 4-elem 14 = 26 measure units
- total used = 36; tool last reported budget_used=30 after 4-elem measures batch 1+2
  (running total per tool: 4 search + 26 measure = 30 reported at that point; +later measures
  pushed to 30 total shown). Remaining budget ~15. Sufficient; no more spending needed.

## Step A — series peak (5-elem O6.xCa1Cu2Ba2Hg1 = Hg-1212, Tc,max~127)
| O x    | Tc     | note |
|--------|--------|------|
| 6.080  | 92.0   | seed |
| 6.100  | 92.5   | |
| 6.130  | 102.0  | |
| 6.140  | 105.0  | |
| 6.160  | NO SMP | unmeasurable |
| 6.190  | 122.0  | |
| 6.210  | 127.0  | PEAK (tie) |
| 6.220  | 127.0  | PEAK (tie) |
| 6.240  | 123.0  | |
| 6.270  | NO SMP | unmeasurable |
| 6.300  | 100.0  | |
| 6.310  | 98.0   | |

T* = 127.0. Peak composition = O6.220Ca1Cu2Ba2Hg1
  (tie at 127.0 between O6.210 and O6.220; parabolic apex leans toward higher-O side
   since the right flank drops less: 0.240→123 vs symmetric-left 0.190→122.)
threshold1 = 127 - 32 = 95.0

## Step B — Q1 (contains Hg AND exactly 4 elements, Tc >= 95.0)
4-element Hg family = O4.xCu1Ba2Hg1 (elements O,Cu,Ba,Hg = 4) — Hg-1201, Tc,max~97.
(All other Hg materials have 5 elements: the Ca1Cu2Ba2Hg1 series, and O6Cu1Ba2Hg1Tl1.)
| O x   | Tc    | >=95? |
|-------|-------|-------|
| 4.047 | NO SMP | unmeasurable (Step C) |
| 4.066 | NO SMP | unmeasurable (Step C) |
| 4.081 | 98.0  | YES |
| 4.085 | 92.0  | no |
| 4.096 | 95.6  | YES |
| 4.097 | 95.3  | YES |
| 4.101 | 93.4  | no |
| 4.103 | 92.5  | no |
| 4.150 | 97.0  | YES |
| 4.160 | NO SMP | unmeasurable (Step C) |
| 4.162 | 97.4  | YES |
| 4.180 | NO SMP | unmeasurable (Step C) |
| 4.200 | 94.2  | no |
| 4.220 | 97.0  | YES |

Q1 (measured, confirmed Tc >= 95.0):
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

## Step C — unmeasurable materials (discovered via search_db)
Discovered materials = all 27 from "Hg" search (the "Ca1Cu2Ba2Hg1" search is a subset).
Unmeasurable (measure_tc returns "no sample exists") = 6:
1. O6.160Ca1Cu2Ba2Hg1  (5 elem, Hg-1212)
2. O6.270Ca1Cu2Ba2Hg1  (5 elem, Hg-1212)
3. O4.047Cu1Ba2Hg1     (4 elem, Hg-1201)
4. O4.066Cu1Ba2Hg1     (4 elem, Hg-1201)
5. O4.160Cu1Ba2Hg1     (4 elem, Hg-1201)
6. O4.180Cu1Ba2Hg1     (4 elem, Hg-1201)

### Induced rule (multi-factor: family + oxygen excess δ)
Define oxygen excess δ = O − base, base = 6 for Hg-1212 (Ca1Cu2Ba2Hg1) and base = 4 for
Hg-1201 (Cu1Ba2Hg1). A material is HIGH iff its δ lies within its OWN family's high-Tc band.
- Hg-1212 (5e): measured δ∈[0.080,0.310] ALL high (92–127; dome, peak ~127 at δ≈0.21–0.22).
- Hg-1201 (4e): measured δ∈[0.081,0.220] ALL high (92.0–98.0; oscillatory, floor 92, never <90).
Within each family, Tc stays ≥90 across the whole observed composition range, so within-family
interpolation/extrapolation keeps every member high.

Why "single-factor rules are wrong on several": a single-factor oxygen-range rule calibrated on
the seed series (e.g. "high only if δ∈[0.080,0.310]", from Hg-1212) would label the low-δ
Hg-1201 members O4.047 (δ=0.047) and O4.066 (δ=0.066) as LOW — WRONG, because the Hg-1201
family is independently high even at low δ. Family membership is a necessary second factor.

### Predictions
- O6.160Ca1Cu2Ba2Hg1 → high (Hg-1212, δ=0.160 inside [0.080,0.310], interpolates ~113)
- O6.270Ca1Cu2Ba2Hg1 → high (Hg-1212, δ=0.270 inside band, interpolates ~111)
- O4.047Cu1Ba2Hg1    → high (Hg-1201; family high across its range, no <90 observed)
- O4.066Cu1Ba2Hg1    → high (Hg-1201; just below measured range, oscillation floor ~92)
- O4.160Cu1Ba2Hg1    → high (Hg-1201, δ=0.160 inside [0.081,0.220], interpolates ~97.3)
- O4.180Cu1Ba2Hg1    → high (Hg-1201, δ=0.180 inside band, interpolates ~95.9)

## Step D — final pick
Predicted-HIGH unmeasurables, fewest elements first:
- 4 elements: O4.047, O4.066, O4.160, O4.180  ← fewest
- 5 elements: O6.160, O6.270
Tie-break (4-element set) by higher expected Tc:
- O4.160 → ~97.3  (bracketed by 0.150→97.0 and 0.162→97.4, near family peak 97.4)  ← HIGHEST
- O4.180 → ~95.9
- O4.066 → ~94–96 (oscillation body, below 0.081 peak)
- O4.047 → ~94–96 (oscillation body, not a peak)
final = O4.160Cu1Ba2Hg1
