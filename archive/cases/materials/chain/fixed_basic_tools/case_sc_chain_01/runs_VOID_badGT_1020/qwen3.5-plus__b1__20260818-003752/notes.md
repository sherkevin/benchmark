# Superconductor Tc Study Notes

## Step A — Series Peak
Seed: O6.080Ca1Cu2Ba2Hg1

**Measurements from O6.xxxCa1Cu2Ba2Hg1 series:**
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: no sample
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← PEAK
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: no sample
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)

**Other measurements:**
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- O4.096Cu1Ba2Hg1: 95.6 K
- O4.097Cu1Ba2Hg1: 95.3 K
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K
- O6Cu1Ba2Hg1Tl1: 0.0 K (5 elements)
- O10Ca2Cu3Ba2Tl2: 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 K
- O7Cu3Ba2La1: 89.6 K
- O7Cu3Y0.200Ba2La0.800: 81.0 K

## Step B — Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Materials with Hg, exactly 4 elements (O, Cu, Ba, Hg), Tc >= 95.0:
- O4.081Cu1Ba2Hg1 (98.0) ✓
- O4.162Cu1Ba2Hg1 (97.4) ✓
- O4.150Cu1Ba2Hg1 (97.0) ✓
- O4.220Cu1Ba2Hg1 (97.0) ✓
- O4.096Cu1Ba2Hg1 (95.6) ✓
- O4.097Cu1Ba2Hg1 (95.3) ✓

**Q1 = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]**

## Step C — Extrapolate
Unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg) - contains Hg
2. O6.270Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg) - contains Hg
3. O4.180Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - contains Hg
4. O4.047Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - contains Hg
5. O4.066Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - contains Hg
6. O4.160Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg) - contains Hg
7. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl) - no Hg
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb) - no Hg
9. O8Cu3Y1Ba1.900La0.100 (5 elements: O, Cu, Y, Ba, La) - no Hg
10. O8Cu4Y1Ba1.870La0.130 (5 elements: O, Cu, Y, Ba, La) - no Hg

**Analyzing the rule:**
High = Tc >= 90 K, Low = Tc < 90 K

From measured data:
- O6.xxxCa1Cu2Ba2Hg1 series (5 elements, contains Hg): mostly HIGH (92-127 K), except no-sample ones
- O4.xxxCu1Ba2Hg1 series (4 elements, contains Hg): mostly HIGH (92-98 K), some near boundary
- O6Cu1Ba2Hg1Tl1 (5 elements, contains Hg): 0.0 K - LOW! This is key - has Tl
- O10Ca2Cu3Ba2Tl2 (5 elements, NO Hg): 118.97 K - HIGH!
- O6Cu1Cd0.250Ba2Tl1.750 (5 elements, NO Hg, has Cd): 88.0 K - LOW
- O7Cu3Ba2La1 (5 elements, NO Hg): 89.6 K - LOW (just under 90)
- O7Cu3Y0.200Ba2La0.800 (5 elements, NO Hg): 81.0 K - LOW

Pattern observation:
- Materials with Hg AND 4 elements (O-Cu-Ba-Hg): HIGH when oxygen ~4.08-4.22
- Materials with Hg AND 5 elements with Ca (O-Ca-Cu-Ba-Hg): HIGH when oxygen ~6.08-6.31
- O6Cu1Ba2Hg1Tl1 has Hg but Tc=0 - it has Tl! Maybe Tl without proper structure kills it?
- O10Ca2Cu3Ba2Tl2 has no Hg but is HIGH (118.97 K) - it has Ca, Cu, Ba, Tl and more oxygen

Let me think about what separates high from low:
- High Tc materials: O6.xxxCa1Cu2Ba2Hg1 (with Ca), O4.xxxCu1Ba2Hg1, O10Ca2Cu3Ba2Tl2
- Low Tc materials: O6Cu1Ba2Hg1Tl1 (0), O6Cu1Cd0.250Ba2Tl1.750 (88), O7Cu3Ba2La1 (89.6), O7Cu3Y0.200Ba2La0.800 (81)

Key difference: HIGH materials seem to have BOTH Ca OR the 4-element Hg structure.
- O6Cu1Ba2Hg1Tl1: has Hg but NO Ca, and 5 elements -> LOW
- O10Ca2Cu3Ba2Tl2: has Ca, no Hg -> HIGH

Rule hypothesis: **Material is HIGH if it contains Ca OR (contains Hg AND has exactly 4 elements)**

Let me verify:
- O6.080Ca1Cu2Ba2Hg1: has Ca -> HIGH (92) ✓
- O4.081Cu1Ba2Hg1: no Ca, but Hg + 4 elements -> HIGH (98) ✓
- O6Cu1Ba2Hg1Tl1: no Ca, has Hg but 5 elements -> LOW (0) ✓
- O10Ca2Cu3Ba2Tl2: has Ca -> HIGH (118.97) ✓
- O6Cu1Cd0.250Ba2Tl1.750: no Ca, no Hg -> LOW (88) ✓
- O7Cu3Ba2La1: no Ca, no Hg -> LOW (89.6) ✓
- O7Cu3Y0.200Ba2La0.800: no Ca, no Hg -> LOW (81) ✓

This rule works!

**Rule: HIGH if (contains Ca) OR (contains Hg AND has exactly 4 elements)**

## Step D — Final Pick
Applying rule to unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: has Ca -> HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Ca -> HIGH
3. O4.180Cu1Ba2Hg1: no Ca, but Hg + 4 elements -> HIGH
4. O4.047Cu1Ba2Hg1: no Ca, but Hg + 4 elements -> HIGH
5. O4.066Cu1Ba2Hg1: no Ca, but Hg + 4 elements -> HIGH
6. O4.160Cu1Ba2Hg1: no Ca, but Hg + 4 elements -> HIGH
7. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca -> HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca -> HIGH
9. O8Cu3Y1Ba1.900La0.100: no Ca, no Hg -> LOW
10. O8Cu4Y1Ba1.870La0.130: no Ca, no Hg -> LOW

Predicted HIGH: 8 materials
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O4.047Cu1Ba2Hg1: 4 elements ← FEWEST
- O4.066Cu1Ba2Hg1: 4 elements ← FEWEST
- O4.160Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

Among HIGH predictions, those with FEWEST elements (4) are:
- O4.180Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1

Tie-break by higher expected Tc. These are all O-Cu-Ba-Hg 4-element materials.
Looking at measured analogs:
- O4.081Cu1Ba2Hg1: 98.0 (O=4.081)
- O4.096Cu1Ba2Hg1: 95.6 (O=4.096)
- O4.097Cu1Ba2Hg1: 95.3 (O=4.097)
- O4.101Cu1Ba2Hg1: 93.4 (O=4.101)
- O4.103Cu1Ba2Hg1: 92.5 (O=4.103)
- O4.085Cu1Ba2Hg1: 92.0 (O=4.085)
- O4.150Cu1Ba2Hg1: 97.0 (O=4.150)
- O4.162Cu1Ba2Hg1: 97.4 (O=4.162)
- O4.220Cu1Ba2Hg1: 97.0 (O=4.220)
- O4.200Cu1Ba2Hg1: 94.2 (O=4.200)

Pattern: Tc peaks around O=4.08-4.16, drops at extremes.
- O4.047 is low oxygen (like 4.047) - might be lower
- O4.066 is low oxygen
- O4.160 is near peak region (4.162 gave 97.4)
- O4.180 is past peak

Expected Tc ranking (highest to lowest among 4-element candidates):
- O4.160Cu1Ba2Hg1: closest to 4.162 (97.4) -> expect ~97
- O4.180Cu1Ba2Hg1: near 4.200 (94.2) -> expect ~95
- O4.066Cu1Ba2Hg1: between 4.047 and 4.081 -> expect ~94-95
- O4.047Cu1Ba2Hg1: lowest oxygen, likely lowest Tc

**Final answer: O4.160Cu1Ba2Hg1** (highest expected Tc among 4-element HIGH predictions)
