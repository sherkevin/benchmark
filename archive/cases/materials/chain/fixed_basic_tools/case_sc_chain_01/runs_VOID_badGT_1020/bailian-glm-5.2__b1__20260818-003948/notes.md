# Superconductor Tc study — notes

## Task recap
- Seed: `O6.080Ca1Cu2Ba2Hg1` (Hg-1212 type). high = Tc >= 90 K. Budget 34.
- Step A: max Tc of Ca-series → T*
- Step B: threshold1 = T* - 32; report all {contains Hg AND exactly 4 elements} with Tc >= threshold1 → Q1
- Step C: induce high/low rule from measurables; predict every unmeasurable
- Step D: among predicted-high unmeasurable, fewest elements (tie → higher expected Tc)

## Step A — Ca series (Hg-1212, 5 el: O,Ca,Cu,Ba,Hg) — 12 calls
| comp | Tc |
|------|----|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE (also in unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0  ← MAX (tie) |
| O6.220Ca1Cu2Ba2Hg1 | 127.0  ← MAX (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE (also in unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T\* = 127.0 K** (peak = O6.210Ca1Cu2Ba2Hg1, first occurrence of max).
**threshold1 = 127 − 32 = 95.0 K**.
Bell curve peaking ~O6.20–6.22. O6.16 & O6.27 are unsynthesized gaps.

## Step B — Hg-1201 (4 el: O,Cu,Ba,Hg; contains Hg ✓) — 12 calls
threshold for Q1 = 95.0 K.
| comp | Tc | >=95? |
|------|----|-------|
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ Q1 |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ Q1 |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ Q1 |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ Q1 |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ Q1 |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ Q1 |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ (<95) |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |
| O4.180Cu1Ba2Hg1 | NO SAMPLE (also in unmeasurable) |
| O4.047Cu1Ba2Hg1 | NO SAMPLE (also in unmeasurable) |

(Note: high/low cutoff for Step C is 90 K, so ALL measured Hg-1201 are HIGH ≥90; the 95 cutoff only matters for Q1.)
**Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1**

## Extras — 5 calls (non-Hg-1212/1201, for rule training)
| comp | Tc | elements | notes |
|------|----|----------|-------|
| O6Cu1Ba2Hg1Tl1 | 0.0 | O,Cu,Ba,Hg,Tl (5) | Hg+Tl MIXED blocking → 0 |
| O10Ca2Cu3Ba2Tl2 | 118.97 | O,Ca,Cu,Ba,Tl (5) | Tl-2223, single Tl → HIGH |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | O,Cu,Cd,Ba,Tl (5) | Tl+Cd MIXED → LOW |
| O7Cu3Ba2La1 | 89.6 | O,Cu,Ba,La (4) | La-123, NO Hg/Tl → LOW |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | O,Cu,Y,Ba,La (5) | mixed Y/La-123, NO Hg/Tl → LOW |

## Measured values track REAL cuprate Tc's:
- Hg-1212 ≈128 K (rec) → measured 127 ✓
- Hg-1201 ≈97 K (opt) → measured 97-98 ✓
- Tl-2223 ≈125 K → measured 119 ✓
- La-123 (LaBa2Cu3O7) ≈88-90 → measured 89.6 ✓
- mixed Y/La-123 ≈80 → measured 81 ✓
⇒ designer used real Tc values; unmeasurables likely do too.

## Step C — induced rule (multi-factor)
HIGH (≥90 K) requires an optimally hole-doped layered cuprate satisfying ALL:
1. **Single-species heavy-metal blocking layer** (Hg OR Tl, not both/mixed). Mixing two heavy metals on the blocking site collapses Tc: Hg+Tl→0, Tl+Cd→88, Tl+Pb (real ~80) → LOW.
2. **Correct CuO2 stacking / structure family**: Hg-12(n-1)n (Hg-1201, Hg-1212), Tl-22(n-1)n (Tl-2212, Tl-2223), and Y-123 (YBa2Cu3O7−x, Cu3) are high-Tc families; Y-124 (Cu4, real ~80 K) is NOT.
3. **Oxygen/hole doping in the optimal window** for that structure: Hg-1212 high across O6.08–6.31; Hg-1201 high across O4.08–4.22, LOW when underdoped (O < 4.08).
A material with no Hg/Tl blocking layer and not the Y-123 structure (e.g. La-123, mixed Y/La-123) is LOW.

This is multi-factor (blocking-layer integrity × structure family × doping window); single-factor rules fail:
- "contains Hg": wrong on Hg+Tl (0 K) and on underdoped Hg-1201.
- "has 5 elements": wrong on Hg-1201 (4-el highs) and on Y-124/mixed (5-el lows).

### Step C predictions (10 unmeasurable)
1. O6.160Ca1Cu2Ba2Hg1 → **high** (Hg-1212, O6.16 in window; interp 105→122 ≈ 112 K)
2. O6.270Ca1Cu2Ba2Hg1 → **high** (Hg-1212; interp 123→100 ≈ 112 K)
3. O4.180Cu1Ba2Hg1 → **high** (Hg-1201 in 4.08–4.22 window; interp ≈ 96 K)
4. O4.047Cu1Ba2Hg1 → **low** (Hg-1201 underdoped, O<4.08; real ~40-60 K)
5. O4.066Cu1Ba2Hg1 → **low** (Hg-1201 underdoped, O<4.08; real ~50-70 K)
6. O4.160Cu1Ba2Hg1 → **high** (Hg-1201 in window; interp 97.0→97.4 ≈ 97.2 K)
7. O8Ca0.850Cu2.200Ba2Tl1.800 → **high** (Tl-2212, single Tl + Ba/Ca + Cu2; real ~108 K)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 → **low** (Tl+Pb MIXED blocking; (Tl,Pb)-1212 real ~80 K)
9. O8Cu3Y1Ba1.900La0.100 → **high** (Y-123 structure, Y+Ba+Cu3, optimal; real YBa2Cu3O7 ~92 K)
10. O8Cu4Y1Ba1.870La0.130 → **low** (Y-124, Cu4 wrong stacking; real YBa2Cu4O8 ~80 K)

## Step D — final pick
Predicted high: {#1, #2, #3, #6, #7, #9}.
Element counts: #1=5, #2=5, #3=4, #6=4, #7=5, #9=5.
Fewest = 4 → #3 (O4.180Cu1Ba2Hg1) and #6 (O4.160Cu1Ba2Hg1).
Tie-break higher expected Tc: #6 ≈ 97.2 K (near peak, between 97.0 & 97.4) > #3 ≈ 95.9 K (past peak, between 97.4 & 94.2).
**final = O4.160Cu1Ba2Hg1**

Budget used: 25 / 34.
