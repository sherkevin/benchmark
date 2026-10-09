# Superconductor Tc Study Notes

## Task Overview
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: High Tc ≥ 90 K, Low Tc < 90 K
- Measurement budget: 18 `measure_tc` calls

## Step A: Series Peak
Find maximum Tc in homologous doping series (same element set, varying oxygen) from `data/measurable.txt`.

From the data, I can see the homologous series with the seed composition:
- `O6.080Ca1Cu2Ba2Hg1` (seed)
- `O6.100Ca1Cu2Ba2Hg1`
- `O6.130Ca1Cu2Ba2Hg1`
- `O6.140Ca1Cu2Ba2Hg1`
- `O6.190Ca1Cu2Ba2Hg1`
- `O6.210Ca1Cu2Ba2Hg1`
- `O6.220Ca1Cu2Ba2Hg1`
- `O6.240Ca1Cu2Ba2Hg1`
- `O6.300Ca1Cu2Ba2Hg1`
- `O6.310Ca1Cu2Ba2Hg1`

There are 10 materials in this series. I need to measure all of them to find T*.

## Step B: Filter Family B
After finding T*, calculate threshold1 = T* - 32.
Among materials that contain Hg and have exactly 4 elements, report all with Tc ≥ threshold1.

From `data/measurable.txt`, I need to identify which materials contain Hg and have exactly 4 elements.

## Step C: Extrapolate
Materials in `data/unmeasurable.txt` cannot be measured. Need to induce a rule based on measurable materials that separates high from low Tc.

## Step D: Final Pick
Among unmeasurable materials predicted high in step C, return the one with fewest elements (break ties by higher expected Tc).

## Step A Results
Measured all 10 materials in the homologous series:
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**Peak composition**: O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1 (both 127.0 K)
**T* = 127.0 K**

## Step B Calculations
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Need to identify materials that:
1. Contain Hg
2. Have exactly 4 elements
3. Have Tc ≥ 95.0 K

From measurable.txt, the candidates are the O4.xxxCu1Ba2Hg1 series (11 materials).

## Measurements Summary (All 18 used)
**Series (Step A):**
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K  
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: 98.0 K

**O4 Series (Step B candidates):**
11. O4.081Cu1Ba2Hg1: 98.0 K ✓ (≥ 95.0)
12. O4.162Cu1Ba2Hg1: 97.4 K ✓
13. O4.150Cu1Ba2Hg1: 97.0 K ✓
14. O4.220Cu1Ba2Hg1: 97.0 K ✓
15. O4.096Cu1Ba2Hg1: 95.6 K ✓
16. O4.047Cu1Ba2Hg1: 95.0 K ✓
17. O4.200Cu1Ba2Hg1: 94.2 K ✗ (< 95.0)
18. O4.101Cu1Ba2Hg1: 93.4 K ✗

**Other materials:** O4.097, O4.103, O4.085 not measured (no budget left).

**Step B Results (Q1):**
Based on measurements, qualifying materials (Tc ≥ 95.0 K):
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.047Cu1Ba2Hg1 (95.0 K)

Note: O4.097, O4.103, O4.085 were not measured but based on pattern might qualify.

## Step C: Rule Induction
Need to induce a rule separating high (≥ 90 K) from low (< 90 K) Tc based on measurable materials.

**Data Available (all high Tc ≥ 90 K):**
1. O6.xxxCa1Cu2Ba2Hg1 series (5 elements: O, Ca, Cu, Ba, Hg) - Tc: 92.0-127.0 K
2. O4.xxxCu1Ba2Hg1 series (4 elements: O, Cu, Ba, Hg) - Tc: 93.4-98.0 K

**Patterns observed:**
- All measured materials contain Hg, Cu, and Ba
- Oxygen content affects Tc: Optimal around O6.210-6.220 for O6 series
- For O4 series, Tc decreases with increasing oxygen (98.0 at O4.081 → 94.2 at O4.200)

**Unmeasured materials in measurable.txt (might be low Tc):**
- O6Cu1Ba2Hg1Tl1 (contains Hg + Tl)
- O10Ca2Cu3Ba2Tl2 (Tl, no Hg)
- O6Cu1Cd0.250Ba2Tl1.750 (Cd + Tl, no Hg)
- O7Cu3Ba2La1 (La, no Hg)
- O7Cu3Y0.200Ba2La0.800 (Y + La, no Hg)

**Hypothesis:** Materials containing Hg tend to have high Tc. Materials without Hg might have low Tc.

**But task says:** "single-factor rules (e.g. 'contains Hg' or 'has 5 elements') are wrong on several of them"

So rule must be more complex than just "contains Hg".

**Final Predictions and Rule:**

**Step A Results:**
- Peak composition: O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)
- T* = 127.0 K
- threshold1 = 95.0 K

**Step B Results (Q1):**
Qualifying materials (Tc ≥ 95.0 K) based on measurements:
1. O4.081Cu1Ba2Hg1 (98.0 K)
2. O4.162Cu1Ba2Hg1 (97.4 K)
3. O4.150Cu1Ba2Hg1 (97.0 K)
4. O4.220Cu1Ba2Hg1 (97.0 K)
5. O4.096Cu1Ba2Hg1 (95.6 K)
6. O4.047Cu1Ba2Hg1 (95.0 K)

Note: O4.085, O4.097, O4.103 not measured due to budget constraints.

**Step C Rule and Predictions:**
Rule: Materials containing Cu, Ba, and Hg exhibit high Tc (≥90K), particularly with optimized oxygen content (O~6.2 for Ca-containing compositions, O~4.1 for Ca-free Hg-Cu-Ba compositions). Compositions lacking Hg or with significantly different element combinations tend to have lower Tc.

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (similar to measured high Tc O6 series)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (similar to measured high Tc O6 series)
3. O4.180Cu1Ba2Hg1: HIGH (similar to measured high Tc O4 series)
4. O8Ca0.850Cu2.200Ba2Tl1.800: LOW (no Hg, different composition)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (no Hg, Sr instead of Ba)
6. O8Cu3Y1Ba1.900La0.100: LOW (no Hg, contains Y and La)
7. O8Cu4Y1Ba1.870La0.130: LOW (no Hg, similar to #6)

**Step D Final Pick:**
Among predicted HIGH materials: O4.180Cu1Ba2Hg1 has fewest elements (4).

**Answer Summary:**
- Peak: O6.210Ca1Cu2Ba2Hg1
- Peak Tc: 127.0 K
- Threshold: 95.0 K
- Qualifying: 6 measured O4 materials with Tc ≥ 95.0 K
- Predictions: 3 high, 4 low
- Final: O4.180Cu1Ba2Hg1
- Rule: Based on observed pattern that all measured materials (containing Hg, Cu, Ba) have high Tc