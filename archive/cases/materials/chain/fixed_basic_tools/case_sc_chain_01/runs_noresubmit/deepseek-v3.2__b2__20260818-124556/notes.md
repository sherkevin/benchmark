# Superconductor Tc Study Notes

## Initial Observations
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Measurement budget: 18 `measure_tc` calls
- Goal: Find maximum Tc in homologous doping series, then analyze other materials

## Step A - Series Peak
Need to measure all materials in `data/measurable.txt` to find maximum Tc (T*).

From looking at the data:
- Lines 1-10 appear to be the homologous series with oxygen varying: O6.080Ca1Cu2Ba2Hg1 to O6.310Ca1Cu2Ba2Hg1
- Lines 11-21: Different series O4.xxxCu1Ba2Hg1
- Lines 22-27: Various other compositions

The seed belongs to a homologous doping series with same element set, varying oxygen. So we need to measure all materials that share the same element set as the seed: Ca1Cu2Ba2Hg1.

Looking at measurable.txt:
1. O6.080Ca1Cu2Ba2Hg1 (seed)
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

These are the 10 members of the homologous series. We need to measure all of them to find maximum Tc.

### Measurement Results for Homologous Series:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

**Maximum Tc (T*): 127.0 K**
**Composition with maximum Tc: O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1** (both have 127.0 K)

We need to pick one as the peak composition. Looking at oxygen values: O6.210 and O6.220. The maximum appears to be at O6.210-O6.220 range. O6.210 comes first in the list, so I'll use O6.210Ca1Cu2Ba2Hg1 as the peak.

### Step B - Filter Family B (contains Hg and has exactly 4 elements)
Threshold1 = T* - 32 = 127.0 - 32 =听95.0 K

Materials with exactly 4 elements that contain Hg (from measurable.txt lines 11-21):
11. O4.081Cu1Ba2Hg1: Tc = 98.0 K ✓ (>= 95.0)
12. O4.162Cu1Ba2Hg1: Tc = 97.4 K ✓
13. O4.150Cu1Ba2Hg1: Tc = 97.0 K ✓
14. O4.220Cu1Ba2Hg1: Tc = 97.0 K ✓
15. O4.096Cu1Ba2Hg1: Tc = 95.6 K ✓
16. O4.097Cu1Ba2Hg1: Tc = 95.3 K ✓
17. O4.047Cu1Ba2Hg1: Tc = 95.0 K ✓ (exactly 95.0)
18. O4.200Cu1Ba2Hg1: Tc = 94.2 K ✗ (below 95.0)
19. O4.101Cu1Ba2Hg1: Not measured (no budget left)
20. O4.103Cu1Ba2Hg1: Not measured (no budget left)
21. O4.085Cu1Ba2Hg1: Not measured (no budget left)

**Qualifying set Q1:** All materials with Tc >= 95.0 K that were measured:
- O4.081Cu1Ba2Hg1 (98.0 K)
- O4.162Cu1Ba2Hg1 (97.4 K)
- O4.150Cu1Ba2Hg1 (97.0 K)
- O4.220Cu1Ba2Hg1 (97.0 K)
- O4.096Cu1Ba2Hg1 (95.6 K)
- O4.097Cu1Ba2Hg1 (95.3 K)
- O4.047Cu1Ba2Hg1 (95.0 K)

Note: O4.200Cu1Ba2Hg1 has Tc = 94.2 K (below threshold). O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1, and O4.085Cu1Ba2Hg1 were not measured due to budget constraints.

### Step C - Extrapolate (induce rule)
All measured materials have high Tc (>= 90 K). Need to induce rule separating high from low Tc.

Measured materials all contain: Cu, Ba, and Hg (except homologous series also contains Ca).

Unmeasured materials in measurable.txt (lines 22-27) don't all contain Hg:
- O6Cu1Ba2Hg1Tl1: Has Hg and Tl
- O10Ca2Cu3Ba2Tl2: Has Tl but no Hg
- O6Cu1Cd0.250Ba2Tl1.750: Has Tl and Cd
- O7Cu3Ba2La1: Has La, no Hg
- O7Cu3Y0.200Ba2La0.800: Has Y and La, no Hg

Unmeasurable materials in unmeasurable.txt:
1. O6.160Ca1Cu2Ba2Hg1: Similar to homologous series (Hg present)
2. O6.270Ca1Cu2Ba2Hg1: Similar to homologous series (Hg present)
3. O4.180Cu1Ba2Hg1: Similar to O4.xxx series (Hg present)
4. O8Ca0.850Cu2.200Ba2Tl1.800: Has Tl instead of Hg
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Has Sr instead of Ba, has Tl and Pb
6. O8Cu3Y1Ba1.900La0.100: Has Y and La, no Hg
7. O8Cu4Y1Ba1.870La0.130: Has Y and La, no Hg

Rule induction: Since all measured materials (with Hg) have high Tc, and task says single-factor rules like "contains Hg" are wrong, rule must be more complex.

Observed pattern from measurements:
1. All materials with Hg, Ba, and Cu have high Tc (>= 90 K)
2. Oxygen content affects Tc magnitude but not high/low classification (within tested ranges)
3. Materials without Hg might still have high Tc if they contain Tl (thallium) as a substitute

Considering task warning: Single-factor rules wrong on several unmeasurable materials. So "contains Hg" alone is insufficient.

Induced rule: High Tc requires (Hg OR Tl) AND (Ba OR Sr) AND Cu, with appropriate oxygen content (typically 4-8). Materials lacking these combinations have low Tc.

Applying to unmeasurable materials:
- O6.160Ca1Cu2Ba2Hg1: Has Hg, Ba, Cu → high
- O6.270Ca1Cu2Ba2Hg1: Has Hg, Ba, Cu → high
- O4.180Cu1Ba2Hg1: Has Hg, Ba, Cu → high
- O8Ca0.850Cu2.200Ba2Tl1.800: Has Tl, Ba, Cu → high
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Has Tl, Sr, Cu → high (Sr substitutes for Ba)
- O8Cu3Y1Ba1.900La0.100: No Hg/Tl → low
- O8Cu4Y1Ba1.870La0.130: No Hg/Tl → low

But wait, if all except last two are high, then O4.180Cu1Ba2Hg1 still has fewest elements among highs.

### Step D - Final pick
Among unmeasurable materials predicted high: O4.180Cu1Ba2Hg1 has fewest elements (4 elements). Others predicted high have 5 elements.