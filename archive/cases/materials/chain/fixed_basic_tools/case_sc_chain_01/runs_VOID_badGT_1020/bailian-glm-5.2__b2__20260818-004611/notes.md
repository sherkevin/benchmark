# Superconductor Tc study — notes

Budget: 34 total. Null ("no sample") returns do NOT consume budget.
DEFINITION: high if Tc >= 90 K, else low. (NOT 95 — 95 is only the Step-B threshold.)

## Step A — seed series O{x}Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
| O     | Tc    | class |
|-------|-------|-------|
| 6.080 | 92.0  | high  |
| 6.100 | 92.5  | high  |
| 6.130 | 102.0 | high  |
| 6.140 | 105.0 | high  |
| 6.160 | NO SAMPLE (unmeasurable) |
| 6.190 | 122.0 | high  |
| 6.210 | 127.0 | high  <-- MAX (T*) |
| 6.220 | 127.0 | high  <-- tie max |
| 6.240 | 123.0 | high  |
| 6.270 | NO SAMPLE (unmeasurable) |
| 6.300 | 100.0 | high  |
| 6.310 | 98.0  | high  |
T* = 127.0. Peak: O6.210Ca1Cu2Ba2Hg1 (first of the tie).

## Step B — 4-element Hg series O4.xxxCu1Ba2Hg1 (O,Cu,Ba,Hg)
threshold1 = 127 - 32 = 95.0
| O     | Tc    | >=95? |
|-------|-------|-------|
| 4.081 | 98.0  | yes |
| 4.085 | 92.0  | no  |
| 4.096 | 95.6  | yes |
| 4.097 | 95.3  | yes |
| 4.101 | 93.4  | no  |
| 4.103 | 92.5  | no  |
| 4.150 | 97.0  | yes |
| 4.162 | 97.4  | yes |
| 4.200 | 94.2  | no  |
| 4.220 | 97.0  | yes |
| 4.047 | NO SAMPLE | - |
| 4.180 | NO SAMPLE | - |
Q1 (Tc>=95): O4.081, O4.096, O4.097, O4.150, O4.162, O4.220  (6 members)

## Other materials (rule-induction examples)
| composition | Tc | class | notes |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | LOW | Tl + Cu1, O6 |
| O10Ca2Cu3Ba2Tl2 | 118.97 | HIGH | Tl + Cu3 |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | LOW | Tl + Cu1 |
| O7Cu3Ba2La1 | 89.6 | LOW | La present |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | LOW | La present |

## Rule induction (multi-factor)
Clean qualitative rule: La present -> LOW; Tl present + Cu<2 -> LOW (Tl+Cu3 -> HIGH);
Hg-Cu-Ba (no Tl/La) -> all measured HIGH. BUT this would make all 6 unmeasurables HIGH,
and the task warns single-factor rules (e.g. "contains Hg", "has 5 elements") are WRONG on
several of them -> so some unmeasurables must be LOW. The only factor that both separates
measured high/low AND varies among the unmeasurables is oxygen doping.

Doping factor: Hg-12(n-1)n family base O = 2*Cu + 2 (Cu1->4, Cu2->6, Cu3->8).
Define oxygen excess  delta = O - (2*Cu + 2).
Measured HIGH (Hg-Cu-Ba family) delta values:
  Cu2: 0.080,0.100,0.130,0.140,0.190,0.210,0.220,0.240,0.300,0.310  (min 0.080, O6.080->92.0)
  Cu1: 0.081,0.085,0.096,0.097,0.101,0.103,0.150,0.162,0.200,0.220  (min 0.081, O4.081->98.0)
=> HIGH requires delta >= ~0.08 (sufficient hole doping). Boundary is sharp: min HIGH delta = 0.080.
Overdoped LOW: Tl+Cu1 compounds O6 (delta~2.0) -> LOW. La -> LOW. Tl+Cu3 -> HIGH.

## Step C — predictions for the 6 unmeasurables (all Hg-Cu-Ba, no Tl/La)
| composition | Cu | delta=O-(2Cu+2) | prediction |
|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | 2 | 0.160 | high |
| O6.270Ca1Cu2Ba2Hg1 | 2 | 0.270 | high |
| O4.180Cu1Ba2Hg1 | 1 | 0.180 | high |
| O4.047Cu1Ba2Hg1 | 1 | 0.047 | LOW (delta<0.08, underdoped) |
| O4.066Cu1Ba2Hg1 | 1 | 0.066 | LOW (delta<0.08, underdoped) |
| O4.160Cu1Ba2Hg1 | 1 | 0.160 | high |

Check vs warning: "contains Hg" (->all high) is wrong on #4,#5 (2). "has 5 elements"
(->#1,2 high, rest low) is wrong on #3,#6 (2). Both wrong on several -> consistent with
multi-factor rule. (The pure-qualitative rule would make "contains Hg" right on all 6, so
the doping factor is the needed interaction.)

## Step D — among predicted HIGH (#1,2,3,6) pick fewest elements, tie->higher expected Tc
#1 O6.160 (5 elem), #2 O6.270 (5 elem), #3 O4.180 (4 elem), #6 O4.160 (4 elem).
Tie at 4 elements: #3 vs #6. Expected Tc by interpolation:
  #6 O4.160: between O4.150(97.0) & O4.162(97.4) -> ~97.3
  #3 O4.180: between O4.162(97.4) & O4.200(94.2) -> ~95.9
#6 higher -> FINAL = O4.160Cu1Ba2Hg1
(Robust: even under all-high interpretation, #6 still has the highest expected Tc among 4-elem.)

Budget used: 25/34. (4 "no sample" calls did not consume budget.)
