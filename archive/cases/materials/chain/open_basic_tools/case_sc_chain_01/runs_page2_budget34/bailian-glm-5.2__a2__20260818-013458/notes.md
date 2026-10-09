# Superconductor Tc study — FINAL notes

## Budget: 34/34 used (exhausted). Unmeasurable measure_tc calls are free even at budget cap, but errored once exhausted.

## Step A — series (O,Ca,Cu,Ba,Hg; varying O)  [query "Ca1Cu2Ba2Hg1", 12 members]
| Composition | O | Tc | meas? |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92.0 | yes |
| O6.100Ca1Cu2Ba2Hg1 | 6.100 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 6.140 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | 6.160 | null | NO (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | 127.0 | yes ← T* (first max) |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | 127.0 | yes (also 127, tie) |
| O6.240Ca1Cu2Ba2Hg1 | 6.240 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | 6.270 | null | NO (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 6.300 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 6.310 | 98.0 | yes |

**T* = 127.0**, peak = O6.210Ca1Cu2Ba2Hg1. Series curve: bell peaking at ~6.210-6.220.

## Step B — family B (Hg + exactly 4 elements); threshold1 = 127.0 - 32 = 95.0
All 27 Hg materials accounted for: 12 series(5elem) + 14 O4.xxxCu1Ba2Hg1(4elem) + O6Cu1Ba2Hg1Tl1(5elem).
The ONLY 4-element Hg materials are the 14 O4.xxxCu1Ba2Hg1 (Hg-1201: O,Cu,Ba,Hg).

| Composition | O | Tc | meas? | ≥95? |
|---|---|---|---|---|
| O4.047Cu1Ba2Hg1 | 4.047 | null | NO | predict |
| O4.066Cu1Ba2Hg1 | 4.066 | null | NO | predict |
| O4.081Cu1Ba2Hg1 | 4.081 | 98.0 | yes | YES(Q1) |
| O4.085Cu1Ba2Hg1 | 4.085 | 92.0 | yes | no |
| O4.096Cu1Ba2Hg1 | 4.096 | 95.6 | yes | YES(Q1) |
| O4.097Cu1Ba2Hg1 | 4.097 | 95.3 | yes | YES(Q1) |
| O4.101Cu1Ba2Hg1 | 4.101 | 93.4 | yes | no |
| O4.103Cu1Ba2Hg1 | 4.103 | 92.5 | yes | no |
| O4.150Cu1Ba2Hg1 | 4.150 | 97.0 | yes | YES(Q1) |
| O4.160Cu1Ba2Hg1 | 4.160 | null | NO | predict |
| O4.162Cu1Ba2Hg1 | 4.162 | 97.4 | yes | YES(Q1) |
| O4.180Cu1Ba2Hg1 | 4.180 | null | NO | predict |
| O4.200Cu1Ba2Hg1 | 4.200 | ? | could not measure (budget exhausted before reaching it) | ? |
| O4.220Cu1Ba2Hg1 | 4.220 | 97.0 | yes | YES(Q1) |

O4.200: could NOT be measured (instrument time exhausted when attempted). Its status is unknown — treat carefully. Did I get its data? No — errored. So O4.200 is NOT confirmed measurable; it errored due to exhaustion, not "no sample". Status unknown. For Step B I only include CONFIRMED Tc≥95 measurable ones, plus unmeasurable ones per prediction (but Step B says "report ALL whose Tc >= threshold1" — unmeasurable ones can't have measured Tc; their inclusion depends on prediction). 

Step B wording: "Among materials that contains Hg and has exactly 4 elements, report ALL whose Tc >= threshold1. Call this set Q1." Unmeasurable materials don't have a measured Tc. The cleanest reading: Q1 = measurable 4-element Hg materials with measured Tc ≥ 95.0. Unmeasurable ones (O4.047, O4.066, O4.160, O4.180) go to Step C predictions, not Q1. O4.200 status unknown (couldn't measure).

Confirmed Q1 (measurable, Tc≥95.0):
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

## Step C — predict unmeasurable (rule induction)
Unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (series, 5 elem)
2. O6.270Ca1Cu2Ba2Hg1 (series, 5 elem)
3. O4.047Cu1Ba2Hg1 (4 elem)
4. O4.066Cu1Ba2Hg1 (4 elem)
5. O4.160Cu1Ba2Hg1 (4 elem)
6. O4.180Cu1Ba2Hg1 (4 elem)
Note: O4.200Cu1Ba2Hg1 — attempted but instrument exhausted; NOT confirmed unmeasurable (error was "instrument time exhausted", not "no sample exists"). So O4.200 is NOT a discovered unmeasurable material — exclude from predictions. Only true "no sample exists" results count as unmeasurable discovered.

O6Cu1Ba2Hg1Tl1: also errored "instrument time exhausted" — NOT confirmed unmeasurable. Exclude.

### Rule induction
Hg-1201 family Tc vs O (δ=O-4.0):
- 4.047 → ? (unmeas)
- 4.066 → ? (unmeas)
- 4.081 → 98.0
- 4.085 → 92.0
- 4.096 → 95.6
- 4.097 → 95.3
- 4.101 → 93.4
- 4.103 → 92.5
- 4.150 → 97.0
- 4.160 → ? (unmeas)
- 4.162 → 97.4
- 4.180 → ? (unmeas)
- 4.200 → ? (unknown)
- 4.220 → 97.0

The pattern is NON-monotonic & noisy — a single-factor rule (e.g. "Tc rises with O" or "Tc highest near δ=0.08") fails: 4.081(98) > 4.085(92) dips, 4.096-4.097 ~95, 4.101-4.103 ~92-93 (local min), then 4.150-4.220 all ~97. The values cluster around 92-98. Almost ALL measured Hg-1201 values are ≥92, and most are ≥95 except a dip near 4.085-4.103.

For unmeasurable 4-elem predictions, interpolate by O:
- O4.047: neighbors 4.066(?) & nothing lower; but 4.081→98. Extrapolate low-O side. Far below measured. Hg-1201 at very low δ (underdoped) → low Tc. Predict LOW (<90). Real Hg-1201 Tc drops sharply when underdoped (δ<0.05). O4.047 δ=0.047 → likely LOW.
- O4.066: δ=0.066, between 4.047(?) and 4.081(98). Near peak region. ~95-97. Predict HIGH.
- O4.160: between 4.150(97.0) and 4.162(97.4) → ~97. Predict HIGH.
- O4.180: between 4.162(97.4) and 4.200(unknown)/4.220(97.0) → ~97. Predict HIGH.

Series (5-elem) unmeasurable:
- O6.160: between 6.140(105) & 6.190(122) → interp ~113-114 → HIGH.
- O6.270: between 6.240(123) & 6.300(100) → interp ~111-112 → HIGH.

### Rule (multi-factor, avoids single-factor trap):
"Tc is predicted by LOCAL INTERPOLATION along the oxygen-doping series within the SAME element set (homologous family), not by oxygen content alone. For each unmeasurable composition, estimate Tc by interpolating between its two nearest measured neighbors in the same family (same element set, oxygen varying). Declare HIGH if the interpolated Tc ≥ 90 K, else LOW."
- Rationale why single-factor rules fail: Hg-1201 Tc is non-monotonic in O (98@0.081, 92@0.085, 95@0.096, 92.5@0.103, 97@0.150, 97.4@0.162, 97@0.220) — neither "higher O → higher Tc" nor "peak at fixed δ" holds. So interpolate locally rather than use a global O→Tc function.

Predictions:
- O6.160Ca1Cu2Ba2Hg1 → HIGH (interp ~113.5)
- O6.270Ca1Cu2Ba2Hg1 → HIGH (interp ~111.5)
- O4.047Cu1Ba2Hg1 → LOW (underdoped tail, extrapolated <90; nearest neighbor 4.066 also unmeas, next 4.081=98 but 4.047 is far underdoped → LOW)
- O4.066Cu1Ba2Hg1 → HIGH (between 4.047(low) & 4.081(98); local region near 98 → ~95-97 → HIGH)
- O4.160Cu1Ba2Hg1 → HIGH (between 4.150=97 & 4.162=97.4 → ~97.2)
- O4.180Cu1Ba2Hg1 → HIGH (between 4.162=97.4 & 4.220=97 → ~97.2)

## Step D — final pick
Among unmeasurable predicted HIGH, choose FEWEST elements (tie-break higher expected Tc).
HIGH predictions:
- 5-elem: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1
- 4-elem: O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
Fewest elements = 4 → candidates: O4.066, O4.160, O4.180.
Tie-break higher expected Tc:
- O4.066 ~95-97 (interp between low 4.047 and 98)
- O4.160 ~97.2
- O4.180 ~97.2
O4.160 and O4.180 both ~97.2. O4.160 sits between 97.0 and 97.4 → 97.2. O4.180 between 97.4 and 97.0 → 97.2. Tie between O4.160 & O4.180. 
Pick O4.160 (slightly more tightly bounded by two close high measurements 97.0/97.4; midpoint 97.2). O4.180 has one bound at unknown 4.200. O4.160 is the better-constrained → choose O4.160Cu1Ba2Hg1.

final = "O4.160Cu1Ba2Hg1"
