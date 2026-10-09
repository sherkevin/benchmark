# Superconductor Tc study — notes (FINAL, budget exhausted)

Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elem: O,Ca,Cu,Ba,Hg). high = Tc>=90K.
Budget 26, ALL USED (26/26).

## Step A — series peak (DONE)
Homologous series = `O#.###Ca1Cu2Ba2Hg1` (Hg-1212), 12 members, varying O.
| O | Tc | sample |
|---|---|---|
| 6.080 | 92.0 | yes |
| 6.100 | 92.5 | yes |
| 6.130 | — | n/m |
| 6.140 | 105.0 | yes |
| 6.160 | null | **NO SAMPLE** |
| 6.190 | 122.0 | yes |
| 6.210 | **127.0** | yes ← T* |
| 6.220 | 127.0 | yes |
| 6.240 | 123.0 | yes |
| 6.270 | — | n/m |
| 6.300 | — | n/m |
| 6.310 | 98.0 | yes |

**T* = 127.0, peak = `O6.210Ca1Cu2Ba2Hg1`** (O6.220 also 127 but 0.210 lower O / listed first).
**threshold1 = 127 − 32 = 95.0**

## Step B — Hg + exactly 4 elements, Tc >= 95 (Q1)
All Hg materials (27): 14×O4.xxxCu1Ba2Hg1 (4-elem), 12×O6.xxxCa1Cu2Ba2Hg1 (5-elem, the series), 1×O6Cu1Ba2Hg1Tl1 (5-elem).
Only the 14 `O4.xxxCu1Ba2Hg1` (O,Cu,Ba,Hg) have exactly 4 elements.
Measured (5):
| O | Tc | qual? |
|---|---|---|
| 4.085 | 92.0 | no |
| 4.096 | 95.6 | YES |
| 4.097 | 95.3 | YES |
| 4.101 | 93.4 | no |
| 4.150 | 97.0 | YES |
Unmeasured 4-elem: 4.047,4.066,4.081,4.103,4.160,4.162,4.180,4.200,4.220 (no budget).

Q1 (measured, >=95) = {O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1}.

## Step C — unmeasurable materials (predict high/low)
Unmeasurable discovered (sample_available=false): `O6.160Ca1Cu2Ba2Hg1`.
(No budget to test others; the only confirmed no-sample composition is O6.160.)

Rule induction: high = Tc>=90. From the measurable 5-elem series dome, O6.160 sits on the ascending side between 6.140(105) and 6.190(122). Its oxygen 6.160 is between 6.140 and 6.190. Interpolating the dome: 6.160 Tc ≈ between 105 and 122, well above 90 → **high**.
Single-factor (oxygen-only) rules are warned wrong; but for a single unmeasurable interpolated within a measured dome, interpolation is the soundest prediction. Expected Tc ≈ 113–115 K → high.

## Step D — among predicted-high unmeasurable, fewest elements (tie: higher expected Tc)
Only one unmeasurable (O6.160Ca1Cu2Ba2Hg1), predicted high. **Final = O6.160Ca1Cu2Ba2Hg1.**

## Caveat
Budget ran out before all 4-elem family measured and before searching for more unmeasurables. Answer reflects measured/predicted data within budget. predictions contains the one unmeasurable confirmed.
