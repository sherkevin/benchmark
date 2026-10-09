# Superconductor Tc Study — Notes

## Setup
- Budget: 34 units (shared between measure_tc and search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg = 5 elements)
- "high" if Tc >= 90 K, else "low"

## Plan
- Step A: find homologous doping series (same element set, varying oxygen) around seed; measure to find max Tc = T*
- Step B: threshold1 = T* - 32; among Hg + exactly 4 elements, report all with Tc >= threshold1 = Q1
- Step C: induce rule from measurable materials; predict high/low for unmeasurable ones
- Step D: among predicted-high unmeasurable materials, pick fewest elements (tie -> higher expected Tc)

## Budget tracking
Start: 34
Used on search (Hg pages 1-9): 9 → remaining 25

## All Hg materials found (27 total)
**O6 series (Ca1Cu2Ba2Hg1, 5 elements) — Step A series** (12 members):
- O6.080Ca1Cu2Ba2Hg1 (seed)
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

**O4 family (Cu1Ba2Hg1, 4 elements) — Step B candidates** (14 members):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

**5-element** (not in Step B):
- O6Cu1Ba2Hg1Tl1 (O,Cu,Ba,Hg,Tl)

## Budget math
9 (search) + measurements. Real-physics priors:
- O6 series = Hg-1212 (HgBa2CaCu2O6+δ): optimal Tc ~127 K, peak δ~0.2-0.3 → expect peak ~O6.20-6.27
- O4 family = Hg-1201 (HgBa2CuO4+δ): optimal ~94-97 K, peak at high δ → ~O4.18-4.22
=> T* ~127, threshold1 ~95 → O4 family likely mostly below threshold; only high-O end may qualify
=> Don't need to measure all 14 O4; focus on high-O end for Step B

## Step A measurements (spread to find peak)
| comp | Tc |
|------|-----|
| O6.080Ca1Cu2Ba2Hg1 (seed) | 92 |
| O6.190Ca1Cu2Ba2Hg1 | 122 |
| O6.210Ca1Cu2Ba2Hg1 | 127 |
| O6.220Ca1Cu2Ba2Hg1 | 127 ← peak (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123 |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE (unmeasurable → Step C) |
| O6.300Ca1Cu2Ba2Hg1 | 100 |
| O6.310Ca1Cu2Ba2Hg1 | 98 |

→ Unimodal bell: rises 92→122→127, falls 127→123→[gap]→100→98.
  Unmeasured 6.10/6.13/6.14/6.16 are on rising limb (92..122) ⇒ < 127. T* = 127.
  Unmeasured 6.27 on falling limb (123→100) ⇒ < 127. Confirmed.
**T* = 127 K at O6.220Ca1Cu2Ba2Hg1**
**threshold1 = 127 − 32 = 95 K**

Budget: 9 (search) + 7 (measures, O6.27 free) = 16 used; 18 remaining.
Note: no-sample measure_tc calls do NOT consume budget (verified: 9 search→9, O6.27 no-sample, then O6.24→10).

## Step B — O4 family (HgBa2CuO4+δ, 4 elements), threshold 95
| comp | Tc | >=95? |
|------|-----|-------|
| O4.047Cu1Ba2Hg1 | NO SAMPLE | (Step C) |
| O4.066Cu1Ba2Hg1 | NO SAMPLE | (Step C) |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | NO SAMPLE | (Step C) |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | (Step C) |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

**Q1 (measurable, Hg, 4 elements, Tc >= 95):**
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)
Note: the 4 no-sample O4 members are NOT in Q1 (unmeasurable). Q1 = measurable members meeting threshold.

## Unmeasurable (no sample) materials discovered (ALL via search_db)
**In O4 family (4 elements):**
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1
**In O6 series (5 elements):**
- O6.270Ca1Cu2Ba2Hg1
Also probed (not discovered via search but via measure) — still all found through search_db first.

## Budget: 9 (search) + 17 measures = 26; remaining 8. Stop measuring. Now induce rule.

## Step C — rule induction (extrapolate to unmeasurable)
Goal: predict high/low for the 5 unmeasurable materials (threshold for "high" = 90 K per task).

**Rule (induced):** Within a homologous oxygen-doping series (same cation set, only δ varies),
Tc is a smooth unimodal function of the oxygen content δ. Predict an unmeasured member's Tc
by interpolation between its two nearest measured neighbors along the δ axis, and classify
high (>=90) vs low (<90) from the interpolated value. Single-factor rules (e.g. "more O → higher
Tc", or "4-element → high") fail on several members (e.g. O4.20 has 4 el. but Tc 94.2 just
above 90; O6.080 seed only 92; O4.085 = 92 dips below neighbors) — the peak is non-monotonic.

### O6.270Ca1Cu2Ba2Hg1 (5 elements, falls between measured O6.240=123 and O6.300=100)
Linear interp δ=6.27: frac f=(6.27-6.24)/(6.30-6.24)=0.05/0.06... wait 6.27-6.24=0.03, /0.06=0.5
  → Tc ≈ 123 + 0.5*(100-123) = 123 - 11.5 = 111.5 K → **high** (>=90).
  (Confirms falling limb; far above 90.)

### O4 family interpolations (4 elements each; "high" = Tc>=90)
Series δ → Tc (measured): 0.081→98, 0.085→92, 0.096→95.6, 0.097→95.3, 0.101→93.4,
0.103→92.5, 0.150→97, 0.162→97.4, 0.200→94.2, 0.220→97.
The whole measurable O4 band sits in ~92-98 K (all HIGH). Non-monotonic but bounded well above 90.

- **O4.047Cu1Ba2Hg1**: left of first measured (0.081→98). Low-δ side of the band.
  Real Hg-1201 at very low δ (underdoped) drops sharply below Tc. Extrapolate from 0.081=98:
  trend on low side is rising into the band, so at 0.047 (further underdoped) Tc is lower.
  Spacing 0.047→0.081 = 0.034 (≈ gap to next 0.085). If slope ~ (98-92)/0.004 on the steep
  low side... but 0.047 is far left. Conservative: underdoped regime, Tc likely < 90 → **low**.
  Risk noted: only the two leftmost points (0.081,0.085) constrain this. Single-factor "low-δ→low"
  is exactly the trap; but real Hg-1201 underdoped does go non-superconducting → low.
  → predict **low**.

- **O4.066Cu1Ba2Hg1**: between 0.047(unmeas) and 0.081(98). Closer to band; extrapolate from
  0.081=98 and the rising low-δ edge. 0.066 is 0.015 below 0.081. If slope near 0.081 is
  roughly the local rising edge, Tc still plausibly ~85-92 — borderline.
  Given the band is firmly HIGH and 0.066 is just 0.015 from a measured 98, but on the
  underdoped (low-δ) tail where Tc falls: **low** (just below 90) is the consistent call.
  → predict **low**. (If wrong it's a near-miss; this is the warning case "single-factor wrong".)
  Reconsider: the warning says single-factor rules are wrong on SEVERAL. O4.047 & O4.066 are
  exactly the underdoped tail — using "low δ → low" as a single factor could be the trap.
  But I have no measurement between 0.047 and 0.081 to refute a smooth rise. With only 98 at
  0.081 and nothing lower measured, the safest physics-grounded call: underdoped Hg-1201 goes
  non-superconducting → **low** for both. Keep both LOW.

- **O4.160Cu1Ba2Hg1**: between 0.150(97) and 0.162(97.4). Interp ≈ 97.1 → **high**.
- **O4.180Cu1Ba2Hg1**: between 0.162(97.4) and 0.200(94.2). frac=(0.180-0.162)/(0.200-0.162)=
  0.018/0.038=0.474 → 97.4 + 0.474*(94.2-97.4)=97.4-1.5=95.9 → **high**.

### Predictions summary
| material | elements | predicted |
|----------|---------|-----------|
| O4.047Cu1Ba2Hg1 | 4 | low |
| O4.066Cu1Ba2Hg1 | 4 | low |
| O4.160Cu1Ba2Hg1 | 4 | high |
| O4.180Cu1Ba2Hg1 | 4 | high |
| O6.160Ca1Cu2Ba2Hg1 | 5 | high (interp 6.14=105→6.19=122: ~112) |
| O6.270Ca1Cu2Ba2Hg1 | 5 | high (interp 6.24=123→6.30=100: ~112) |

Probe-verification pass (free for no-sample, costs for sample):
O6.100=92.5, O6.130=102, O6.140=105 (all measurable, rising limb), O6.160=NO SAMPLE.
Confirms rising limb 92.5→102→105→[6.16 gap]→122 — O6.16 high.
Budget now 29 used (these 3 cost 1 each; O6.16 free). 5 remaining.

## Step D — final pick
Predicted-high unmeasurable: O4.160(4 el), O4.180(4 el), O6.160(5 el), O6.270(5 el).
Fewest elements = 4. Tie between O4.160 and O4.180 → higher expected Tc:
  O4.160 ≈ 97.1, O4.180 ≈ 95.9 → **O4.160Cu1Ba2Hg1** (higher expected Tc).


