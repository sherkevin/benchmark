# Notes: Superconductor Tc Study

## Step A — Series Peak (Homologous Doping Series)

Seed: O6.080Ca1Cu2Ba2Hg1

Measured all members of the O6.xxxCa1Cu2Ba2Hg1 series from `data/measurable.txt`:

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | null (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| **O6.210Ca1Cu2Ba2Hg1** | **127.0** |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | null (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**Peak T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (tie with O6.220, picking O6.210 as first).

The Tc curve follows a dome shape vs oxygen content, peaking at O≈6.21.

## Step B — Filter Family B (Hg + 4 elements + Tc >= T*-32)

Threshold = 127.0 - 32 = **95.0 K**

Materials with Hg and exactly 4 elements from measurable.txt:
- O4.xxxCu1Ba2Hg1 series (4 elements: O, Cu, Ba, Hg)
- O6Cu1Ba2Hg1Tl1 has 5 elements → excluded

Measured O4.xxxCu1Ba2Hg1 Tc values:

| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |
| O4.180Cu1Ba2Hg1 | null (unmeasurable) | N/A |
| O4.047Cu1Ba2Hg1 | null (unmeasurable) | N/A |

**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

## Step C — Rule Induction

### Measured data split

**High Tc (≥ 90 K):**
- O6.xxxCa1Cu2Ba2Hg1: 92.0–127.0 K (10 materials, 5 elements: O,Ca,Cu,Ba,Hg)
- O4.xxxCu1Ba2Hg1: 92.0–98.0 K (10 materials, 4 elements: O,Cu,Ba,Hg)
- O10Ca2Cu3Ba2Tl2: 118.97 K (5 elements: O,Ca,Cu,Ba,Tl)

**Low Tc (< 90 K):**
- O6Cu1Ba2Hg1Tl1: 0.0 K (5 elements: O,Cu,Ba,Hg,Tl)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K (5 elements: O,Cu,Cd,Ba,Tl)
- O7Cu3Ba2La1: 89.6 K (4 elements: O,Cu,Ba,La)
- O7Cu3Y0.200Ba2La0.800: 81.0 K (5 elements: O,Cu,Y,Ba,La)

### One-factor rules tested (all fail on measurable data):
- "contains Ba" → ALL measured have Ba (no discrimination)
- "contains Hg" → fails on O6Cu1Ba2Hg1Tl1 (has Hg, Tc=0)
- "contains Cu" → ALL measured have Cu
- "contains Ca" → fails on O6Cu1Cd0.250Ba2Tl1.750 (has Ca, Tc=88)

### Interaction rule discovered

**Rule: "Contains Ba AND NOT La AND NOT Y"**

Verification on measured data:
- All 6 materials with Ba but no La/Y: Tc = 92.0–127.0 K → all HIGH ✓
- Both materials with La or Y: Tc = 81.0–89.6 K → all LOW ✓
- Perfect 8/8 classification on measured data

Physical rationale: In cuprate superconductors, Ba provides the alkaline-earth spacer layer essential for the charge-reservoir/CuO₂-plane structure. La and Y are rare-earth elements that substitute into or disrupt the CuO₂ planes, suppressing hole doping and reducing Tc. The interaction is that Ba enables high-Tc structure while La/Y actively suppress it; neither factor alone determines the outcome.

### Predictions for unmeasurable materials

| # | Composition | Elements | Ba? | La? | Y? | Prediction |
|---|---|---|---|---|---|---|
| 1 | O6.160Ca1Cu2Ba2Hg1 | 5 | ✓ | ✗ | ✗ | **high** |
| 2 | O6.270Ca1Cu2Ba2Hg1 | 5 | ✓ | ✗ | ✗ | **high** |
| 3 | O4.180Cu1Ba2Hg1 | 4 | ✓ | ✗ | ✗ | **high** |
| 4 | O4.047Cu1Ba2Hg1 | 4 | ✓ | ✗ | ✗ | **high** |
| 5 | O4.066Cu1Ba2Hg1 | 4 | ✓ | ✗ | ✗ | **high** |
| 6 | O4.160Cu1Ba2Hg1 | 4 | ✓ | ✗ | ✗ | **high** |
| 7 | O8Ca0.850Cu2.200Ba2Tl1.800 | 5 | ✓ | ✗ | ✗ | **high** |
| 8 | O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 | ✗ | ✗ | ✗ | **low** (no Ba) |
| 9 | O8Cu3Y1Ba1.900La0.100 | 5 | ✓ | ✓ | ✓ | **low** (has La, Y) |
| 10 | O8Cu4Y1Ba1.870La0.130 | 5 | ✓ | ✓ | ✓ | **low** (has La, Y) |

Wait — let me reconsider #7 (O8Ca0.850Cu2.200Ba2Tl1.800). It has Ba, no La, no Y → predicted high. But it contains Tl and has 8 oxygens. Measured Tl-containing materials: O10Ca2Cu3Ba2Tl2 (Tc=118.97, high) and O6Cu1Ba2Hg1Tl1 (Tc=0, low) and O6Cu1Cd0.250Ba2Tl1.750 (Tc=88, low). The Tl effect is inconsistent. Under my rule, #7 is predicted high because it has Ba and lacks La/Y.

Actually, let me reconsider my rule more carefully. Let me test an alternative: "Hg AND Ba AND NOT Tl".

**Alternative rule: "Hg AND Ba AND NOT Tl"**

Verification on measured data:
- Materials with Hg, Ba, no Tl: O6.xxxCa1Cu2Ba2Hg1 (92-127K) and O4.xxxCu1Ba2Hg1 (92-98K) → all HIGH ✓
- O6Cu1Ba2Hg1Tl1 (Hg, Ba, has Tl): Tc=0 → LOW ✓
- O10Ca2Cu3Ba2Tl2 (no Hg): Tc=118.97 → predicted LOW ✗ (false negative)
- O6Cu1Cd0.250Ba2Tl1.750 (no Hg): Tc=88 → LOW ✓
- O7Cu3Ba2La1 (no Hg): Tc=89.6 → LOW ✓
- O7Cu3Y0.200Ba2La0.800 (no Hg): Tc=81 → LOW ✓

Score: 7/8 = 87.5% (1 false negative: O10Ca2Cu3Ba2Tl2)

Both rules are 87.5% on measured data (each misses 1). Let me compare:
- "Ba AND NOT La AND NOT Y": fails on O7Cu3Ba2La1 (predicted high because Ba+no Y, but has La → actually wait, La IS excluded). Let me re-verify.

O7Cu3Ba2La1: Ba=✓, La=✓, Y=✗ → has La → condition "NOT La AND NOT Y" fails → predicted LOW. Actual Tc=89.6 → LOW ✓

So "Ba AND NOT La AND NOT Y" is actually 8/8 = 100% on measured data! Let me recheck all 8:

1. O6.080Ca1Cu2Ba2Hg1: Ba✓, La✗, Y✗ → high. Actual 92.0 ✓
2. O4.081Cu1Ba2Hg1: Ba✓, La✗, Y✗ → high. Actual 98.0 ✓
3. O6Cu1Ba2Hg1Tl1: Ba✓, La✗, Y✗ → high. Actual 0.0 ✗!!
4. O10Ca2Cu3Ba2Tl2: Ba✓, La✗, Y✗ → high. Actual 118.97 ✓
5. O6Cu1Cd0.250Ba2Tl1.750: Ba✓, La✗, Y✗ → high. Actual 88.0 ✗!!
6. O7Cu3Ba2La1: Ba✓, La✓ → low. Actual 89.6 ✓
7. O7Cu3Y0.200Ba2La0.800: Ba✓, La✓, Y✓ → low. Actual 81.0 ✓

Wait — that's 25 measured materials total but I'm grouping. Let me be precise:

ALL measured materials with Ba, no La, no Y:
- 10× O6.xxxCa1Cu2Ba2Hg1: Tc = 92.0-127.0 → HIGH
- 10× O4.xxxCu1Ba2Hg1: Tc = 92.0-98.0 → HIGH
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 → LOW (counterexample!)
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 → HIGH
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 → LOW (counterexample!)

So "Ba AND NOT La AND NOT Y" gives:
- 23 correct (10+10+1+2=23 high correct, wait no...)

Hmm, let me count properly:
- High correctly predicted: 10 (O6 series) + 10 (O4 series) + 1 (O10Ca2Cu3Ba2Tl2) = 21
- Low incorrectly predicted as high: O6Cu1Ba2Hg1Tl1 (0.0) and O6Cu1Cd0.250Ba2Tl1.750 (88.0) = 2 false positives
- Low correctly predicted: O7Cu3Ba2La1 (89.6) and O7Cu3Y0.200Ba2La0.800 (81.0) = 2

Total: 23/25 = 92%

Hmm, that's actually quite good. But "Hg AND Ba AND NOT Tl" gives:
- High correctly: 10+10 = 20
- Low incorrectly as high: 0
- High incorrectly as low: O10Ca2Cu3Ba2Tl2 = 1
- Low correctly: O6Cu1Ba2Hg1Tl1, O6Cu1Cd0.250Ba2Tl1.750, O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800 = 4

Total: 24/25 = 96%

So "Hg AND Ba AND NOT Tl" is actually 24/25 = 96%!

Let me double-check "Ba AND NOT La AND NOT Y":
- High predicted: all with Ba, no La, no Y = 10+10+1+1+1 = 23 materials
  - Correct: 10+10+1 = 21
  - Incorrect (actually low): O6Cu1Ba2Hg1Tl1 (0.0), O6Cu1Cd0.250Ba2Tl1.750 (88.0) = 2
- Low predicted: materials without Ba or with La/Y = 2
  - Correct: 2

Total: 23/25 = 92%

So "Hg AND Ba AND NOT Tl" at 96% is better! Let me use this rule.

**FINAL RULE: "Contains Hg AND Ba AND NOT Tl"**

Re-doing predictions:

| # | Composition | Hg? | Ba? | Tl? | Prediction |
|---|---|---|---|---|---|
| 1 | O6.160Ca1Cu2Ba2Hg1 | ✓ | ✓ | ✗ | **high** |
| 2 | O6.270Ca1Cu2Ba2Hg1 | ✓ | ✓ | ✗ | **high** |
| 3 | O4.180Cu1Ba2Hg1 | ✓ | ✓ | ✗ | **high** |
| 4 | O4.047Cu1Ba2Hg1 | ✓ | ✓ | ✗ | **high** |
| 5 | O4.066Cu1Ba2Hg1 | ✓ | ✓ | ✗ | **high** |
| 6 | O4.160Cu1Ba2Hg1 | ✓ | ✓ | ✗ | **high** |
| 7 | O8Ca0.850Cu2.200Ba2Tl1.800 | ✗ | ✓ | ✓ | **low** |
| 8 | O7Ca1Cu2Sr2Tl0.500Pb0.500 | ✗ | ✗ | ✓ | **low** |
| 9 | O8Cu3Y1Ba1.900La0.100 | ✗ | ✓ | ✗ | **low** |
| 10 | O8Cu4Y1Ba1.870La0.130 | ✗ | ✓ | ✗ | **low** |

## Step D — Final Pick

Predicted high: #1-6
- 5 elements: #1 (O6.160Ca1Cu2Ba2Hg1), #2 (O6.270Ca1Cu2Ba2Hg1)
- 4 elements: #3 (O4.180Cu1Ba2Hg1), #4 (O4.047Cu1Ba2Hg1), #5 (O4.066Cu1Ba2Hg1), #6 (O4.160Cu1Ba2Hg1)

Fewest elements = 4 → candidates: #3, #4, #5, #6

Tiebreak by higher expected Tc:
From the measured O4.xxxCu1Ba2Hg1 series, Tc peaks at O4.081 (98.0 K) and is lowest at O4.085 (92.0 K). Higher oxygen content in the O4 range roughly correlates with slightly higher Tc.
- O4.180: O=4.180, expected Tc near the peak (~97 K based on O4.162=97.4, O4.220=97.0)
- O4.160: O=4.160, expected Tc ~97 K (similar to O4.162=97.4)
- O4.066: O=4.066, expected Tc ~94 K (between O4.081=98.0 and O4.047=extrapolated low)
- O4.047: O=4.047, expected Tc <92 K (below measured range)

**Final pick: O4.180Cu1Ba2Hg1** (4 elements, highest expected Tc among unmeasurable 4-element candidates)
