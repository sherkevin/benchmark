# Notes: Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K
- Budget: 34 measure_tc calls

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying O)
| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | N/A | no sample (also in unmeasurable.txt) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured ** PEAK ** |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured (tied peak) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | N/A | no sample (also in unmeasurable.txt) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

**T* = 127.0 K, Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Hg-containing 4-element materials with Tc >= 95.0 K
threshold1 = 127.0 - 32 = 95.0 K

| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.180Cu1Ba2Hg1 | N/A | no sample |
| O4.047Cu1Ba2Hg1 | N/A | no sample |

**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

## Other measured materials
| Composition | Tc (K) | Elements | High/Low |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | O,Cu,Ba,Hg,Tl (5) | LOW |
| O10Ca2Cu3Ba2Tl2 | 118.97 | O,Ca,Cu,Ba,Tl (5) | HIGH |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | O,Cu,Cd,Ba,Tl (5) | LOW |
| O7Cu3Ba2La1 | 89.6 | O,Cu,Ba,La (4) | LOW |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | O,Cu,Y,Ba,La (5) | LOW |

## Step C: Rule Induction

### All HIGH materials (Tc >= 90):
- O-Ca-Cu-Ba-Hg (10 materials): all have Ca, have Hg
- O-Cu-Ba-Hg (10 materials): no Ca, have Hg, no Tl
- O-Ca-Cu-Ba-Tl (1 material): has Ca, no Hg, has Tl

### All LOW materials (Tc < 90):
- O-Cu-Ba-Hg-Tl: no Ca, has Hg, has Tl → 0.0 K
- O-Cu-Cd-Ba-Tl: no Ca, no Hg → 88.0 K
- O-Cu-Ba-La: no Ca, no Hg → 89.6 K
- O-Cu-Y-Ba-La: no Ca, no Hg → 81.0 K

### Induced Rule:
**HIGH if contains Ca OR (contains Hg AND does NOT contain Tl)**

Verification:
- All O-Ca-Cu-Ba-Hg: has Ca → HIGH ✓
- All O-Cu-Ba-Hg: no Ca, Hg yes, no Tl → HIGH ✓
- O-Ca-Cu-Ba-Tl: has Ca → HIGH ✓
- O-Cu-Ba-Hg-Tl: no Ca, Hg+Tl → LOW ✓
- O-Cu-Cd-Ba-Tl: no Ca, no Hg → LOW ✓
- O-Cu-Ba-La: no Ca, no Hg → LOW ✓
- O-Cu-Y-Ba-La: no Ca, no Hg → LOW ✓

### Predictions for unmeasurable.txt:
1. O6.160Ca1Cu2Ba2Hg1: O,Ca,Cu,Ba,Hg → has Ca → HIGH
2. O6.270Ca1Cu2Ba2Hg1: O,Ca,Cu,Ba,Hg → has Ca → HIGH
3. O4.180Cu1Ba2Hg1: O,Cu,Ba,Hg → no Ca, Hg+no Tl → HIGH
4. O4.047Cu1Ba2Hg1: O,Cu,Ba,Hg → no Ca, Hg+no Tl → HIGH
5. O4.066Cu1Ba2Hg1: O,Cu,Ba,Hg → no Ca, Hg+no Tl → HIGH
6. O4.160Cu1Ba2Hg1: O,Cu,Ba,Hg → no Ca, Hg+no Tl → HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800: O,Ca,Cu,Ba,Tl → has Ca → HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: O,Ca,Cu,Sr,Tl,Pb → has Ca → HIGH
9. O8Cu3Y1Ba1.900La0.100: O,Cu,Y,Ba,La → no Ca, no Hg → LOW
10. O8Cu4Y1Ba1.870La0.130: O,Cu,Y,Ba,La → no Ca, no Hg → LOW

## Step D: Final Pick
Predicted HIGH:
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Fewest elements = 4. Four candidates tied at 4 elements.
Tie-break: higher expected Tc.
- O4.160Cu1Ba2Hg1: closest to O4.162 (97.4 K) → expected ~97 K → HIGHEST
- O4.066Cu1Ba2Hg1: near O4.081 (98.0 K peak) → expected ~96-97 K
- O4.180Cu1Ba2Hg1: between O4.162 (97.4) and O4.200 (94.2) → expected ~95-96 K
- O4.047Cu1Ba2Hg1: below O4.081 → uncertain

**Final answer: O4.160Cu1Ba2Hg1**

## Budget
- Step A: 10 successful + 2 failed = 10 budget used
- Step B: 10 successful + 2 failed = 10 budget used
- Other: 5 successful = 5 budget used
- Total: 25 budget used, 9 remaining
