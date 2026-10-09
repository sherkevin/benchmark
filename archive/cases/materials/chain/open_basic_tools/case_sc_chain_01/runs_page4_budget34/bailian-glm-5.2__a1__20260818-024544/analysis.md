# Step C — rule induction analysis

## Data tables

### Family 4E (O,Cu,Ba,Hg), only Cu/Ba/Hg fixed at 1/2/1, O varies
| O | Tc | status |
|---|---|---|
| 0.047 | ? | unmeas |
| 0.066 | ? | unmeas |
| 0.081 | 98.0 | high |
| 0.085 | 92.0 | high |
| 0.096 | 95.6 | high |
| 0.097 | 95.3 | high |
| 0.101 | 93.4 | high |
| 0.103 | 92.5 | high |
| 0.150 | 97.0 | high |
| 0.160 | ? | unmeas |
| 0.162 | 97.4 | high |
| 0.180 | ? | unmeas |
| 0.200 | 94.2 | high |
| 0.220 | 97.0 | high |

Observation: ALL measured 4E samples are high (>= 90). Range 92.0–98.0. The whole measured 4E family is high. So a naive single-factor rule "4-element Hg cuprate → high" would predict all 4 unmeasurable 4E as high.

### Family 5E-series (O,Ca,Cu,Ba,Hg), Ca/Cu/Ba/Hg fixed 1/2/2/1, O6.x varies
| O6.x | Tc | status |
|---|---|---|
| 0.080 | 92.0 | high |
| 0.100 | 92.5 | high |
| 0.130 | 102.0 | high |
| 0.140 | 105.0 | high |
| 0.160 | ? | unmeas |
| 0.190 | 122.0 | high |
| 0.210 | 127.0 | high (peak) |
| 0.220 | 127.0 | high |
| 0.240 | 123.0 | high |
| 0.270 | ? | unmeas |
| 0.300 | 100.0 | high |
| 0.310 | 98.0 | high |

Observation: ALL measured 5E-series samples are high. So naive rule would predict O6.160 and O6.270 high.

## Check single-factor rules — the warning says several are wrong.
If "contains Hg" → high: then everything (incl O6Cu1Ba2Hg1Tl1 which measured 0.0) would be high. O6Cu1Ba2Hg1Tl1 measured 0.0 = LOW. So "contains Hg" is wrong for that one. But that's measurable, not in Step C.

Actually the Step C set is only the 6 unmeasurable materials. The warning: "single-factor rules are wrong on several of them." So among the 6 unmeasurables, a single-factor rule misclassifies several. We need a multi-factor rule that distinguishes.

## Thinking about what could make an unmeasurable material LOW:
- O6Cu1Ba2Hg1Tl1 (measurable, Tc=0.0) is LOW. It has Tl and 5 elements. But Tl1 with O6 (round) → 0.0. Interesting: it has Cu1 (not Cu2) and Ba2Hg1, plus Tl1. So composition O6 Cu1 Ba2 Hg1 Tl1.
  - This is like a "1221" (CuBa2HgTl = 1,2,1,1) layer with O6.
- The 4E family is O4.x Cu1 Ba2 Hg1 — "1211" structure with ~4 oxygens. All high.
- The 5E-series is O6.x Ca1 Cu2 Ba2 Hg1 — "2212" structure (Cu2Ba2Ca1Hg1) with ~6 oxygens. All high.

## Real physics intuition (Hg-based cuprates):
- Hg-1201 (HgBa2CuO4): one CuO2 layer, Tc ~ 90-98 K. → 4E family matches (Cu1Ba2Hg1 + O4).
- Hg-1212 (HgBa2CaCu2O6): two CuO2 layers, Tc ~ 120-127 K. → 5E series matches (Cu2Ba2Ca1Hg1 + O6).
- The number of CuO2 layers (n) drives Tc: n=1 → ~95K, n=2 → ~125K, n=3 → ~135K.

In this data:
- 4E (Cu1 = 1 CuO2 layer): ~92-98 K ✓ matches n=1
- 5E-series (Cu2 = 2 CuO2 layers): ~92-127 K ✓ matches n=2

So both families are structurally high-Tc cuprates. Every member measured is high. The unmeasurable ones, by structure (same layer count), should also be high — they're just oxygen off-stoichiometry variations within a known-high family.

## But the warning insists single-factor rules fail on several of the 6.
What single-factor rules might one try, and where do they fail?

Let me reconsider. Perhaps the intended single-factor traps:
(a) "Hg cuprate → high" — but maybe some unmeasurable is actually low.
(b) "higher O → higher Tc" — for 5E series: 0.080→92, rises to 127 at 0.21, then falls to 98 at 0.31. Non-monotonic dome. Linear extrapolation would fail.
(c) For 5E: O6.270 sits between 0.240 (123) and 0.300 (100) — interpolating → ~110, still high. O6.160 between 0.140(105) and 0.190(122) → ~113, high. Both high.
(d) For 4E: it's a dome too. Peak near O4.081 (98) and O4.162 (97.4). 
   - O4.047, O4.066 (below 0.081): extrapolating the rising edge... 0.081=98, 0.085=92 (already dropping fast). Actually 0.081→0.085 drops 98→92, steep. Below 0.081 likely lower. At 0.047/0.066, could be below 90? Possibly LOW.
   - Hmm, this is where single-factor (O value alone) gets tricky.

## Let me reconsider the 4E dome shape carefully:
O: 0.081→98.0, 0.085→92.0, 0.096→95.6, 0.097→95.3, 0.101→93.4, 0.103→92.5, 0.150→97.0, 0.162→97.4, 0.200→94.2, 0.220→97.0
This is noisy but generally: low-O region around 0.08 has a spike (98 at 0.081), then mid (~0.09-0.10) dips to ~92-93, then 0.15-0.16 back up to ~97, 0.20-0.22 ~94-97.

There seem to be TWO local maxima: ~0.081 (98) and ~0.162 (97.4). Could indicate two phases / oxygen ordering. Noisy.

## Pragmatic, defensible multi-factor rule:
A material is predicted HIGH if it shares the structural signature of a known-high Hg-cuprate family:
 (1) It contains Hg, AND
 (2) Its cation framework (Cu:Ba:Hg plus optional Ca/Tl) matches a measured-high family with the same number of CuO2 layers (Cu count = layer count), AND
 (3) Its oxygen content falls within the measured high-Tc window of that family (i.e., not extrapolated beyond where the family stays high).

Apply:
- O4.047 (4E, Cu1): O=0.047. The 4E measured window where high is confirmed is O ∈ [0.081, 0.220]. 0.047 is BELOW the confirmed window. → can't confirm high by window rule → predict LOW.
- O4.066 (4E, Cu1): O=0.066 < 0.081. Below window → LOW.
- O4.160 (4E, Cu1): O=0.160, within [0.081,0.220] and very near 0.162(97.4)/0.150(97.0). → HIGH.
- O4.180 (4E, Cu1): O=0.180, within window, between 0.162(97.4) and 0.200(94.2). → HIGH.
- O6.160 (5E, Cu2): O6.160 within [0.080,0.310], between 0.140(105) & 0.190(122). → HIGH.
- O6.270 (5E, Cu2): O6.270 within window, between 0.240(123) & 0.300(100). → HIGH.

This gives 4 HIGH, 2 LOW (O4.047, O4.066). The "single-factor rules are wrong on several" — a single-factor "all Hg cuprates high" would mark all 6 high (wrong on the 2 low ones). A single-factor "O value threshold" without structure would also be inconsistent. Multi-factor (structure family + oxygen window) resolves it.

## Step D — among predicted-HIGH unmeasurables, pick FEWEST elements (tie → higher expected Tc):
Predicted HIGH unmeasurables:
- O4.160Cu1Ba2Hg1 — 4 elements, expected ~97.4 (interp 0.162)
- O4.180Cu1Ba2Hg1 — 4 elements, expected ~96 (interp)
- O6.160Ca1Cu2Ba2Hg1 — 5 elements, expected ~113
- O6.270Ca1Cu2Ba2Hg1 — 5 elements, expected ~111

Fewest elements = 4-element ones: O4.160 and O4.180 (tie at 4 elements).
Tiebreak → higher expected Tc:
- O4.160: between 0.150(97.0) and 0.162(97.4) → ~97.2-97.4
- O4.180: between 0.162(97.4) and 0.200(94.2) → ~96
→ O4.160Cu1Ba2Hg1 has higher expected Tc.

FINAL = O4.160Cu1Ba2Hg1
