# Notes for Superconductor Tc Study

## Task Summary
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, low otherwise
- Budget: 18 measure_tc calls

## Step A — Find series peak T*
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to measure series members in measurable.txt to find max Tc.

## Step B — Filter family B
threshold1 = T* - 32
Among materials with Hg and exactly 4 elements, find those with Tc >= threshold1.

## Step C — Extrapolate
Predict high/low for unmeasurable materials using an induced rule.
Warning: single-factor rules are wrong — need interaction of multiple factors.

## Step D — Final pick
Among predicted high unmeasurable materials, pick one with fewest elements (ties broken by higher expected Tc).

---

## Progress

### Step A: Measuring series members
Series members (same elements as seed O-Ca-Cu-Ba-Hg, varying oxygen):

| Composition | Tc |
|-------------|-----|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

### Step B: Filter family B
threshold1 = T* - 32 = 127.0 - 32 = **95.0**

Need materials with Hg AND exactly 4 elements, Tc >= 95.0

Results for 4-element Hg-containing materials (O-Cu-Ba-Hg):
| Composition | Tc | >= 95.0? |
|-------------|-----|----------|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | not measured |
| O4.103Cu1Ba2Hg1 | not measured |
| O4.085Cu1Ba2Hg1 | not measured |

**Q1 (qualifying materials)**: O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

### Step C: Predictions for unmeasurable

Induced rule: **Material is HIGH if it contains Hg, OR if it contains both Tl AND Ba**

This is a multi-factor rule: (has Hg) OR (has Tl AND has Ba)

Predictions:
1. O6.160Ca1Cu2Ba2Hg1 - HIGH (has Hg)
2. O6.270Ca1Cu2Ba2Hg1 - HIGH (has Hg)
3. O4.180Cu1Ba2Hg1 - HIGH (has Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 - HIGH (has Tl and Ba)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - LOW (has Tl but no Ba)
6. O8Cu3Y1Ba1.900La0.100 - LOW (no Hg, no Tl)
7. O8Cu4Y1Ba1.870La0.130 - LOW (no Hg, no Tl)

### Step D: Final pick

Among HIGH predictions: #1, #2, #3, #4
Element counts:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (O, Ca, Cu, Ba, Hg)
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements (O, Cu, Ba, Hg) ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

**Final answer: O4.180Cu1Ba2Hg1**
