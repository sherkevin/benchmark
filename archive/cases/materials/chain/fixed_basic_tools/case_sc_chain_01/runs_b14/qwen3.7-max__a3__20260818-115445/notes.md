# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 14 measure_tc calls
- High Tc: >= 90 K, Low: < 90 K

## Measurable compositions (26 total)
### Seed homologous series (O-Ca-Cu-Ba-Hg, varying O) — 10 members:
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

### O4.xxxCu1Ba2Hg1 series (O-Cu-Ba-Hg, 4 elements, has Hg) — 11 members:
11. O4.081Cu1Ba2Hg1
12. O4.162Cu1Ba2Hg1
13. O4.150Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1
15. O4.096Cu1Cu2Hg1 -- wait no
15. O4.096Cu1Ba2Hg1
16. O4.097Cu1Ba2Hg1
17. O4.047Cu1Ba2Hg1
18. O4.200Cu1Ba2Hg1
19. O4.101Cu1Ba2Hg1
20. O4.103Cu1Ba2Hg1
21. O4.085Cu1Ba2Hg1

### Other measurable materials:
22. O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl)
23. O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl - no Hg)
24. O6Cu1Cd0.250Ba2Tl1.750 (5 elements: O,Cu,Cd,Ba,Tl - no Hg)
25. O7Cu3Ba2La1 (4 elements: O,Cu,Ba,La - no Hg)
26. O7Cu3Y0.200Ba2La0.800 (5 elements: O,Cu,Y,Ba,La - no Hg)

## Unmeasurable compositions (7 total):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
3. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb)
6. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La)
7. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La)

## Step A: Series Peak — COMPLETE ✓
Measured all 10 seed series members (O6.xxxCa1Cu2Ba2Hg1):

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1, first occurrence)
All 10 are HIGH (Tc >= 90 K). Bell curve: Tc rises from 92K to peak 127K at O~6.21-6.22, then drops.

## Step B: Filter Family B — COMPLETE ✓
threshold1 = 127.0 - 32 = **95.0 K**

Materials with Hg + exactly 4 elements = O4.xxxCu1Ba2Hg1 series (11 members).
Measured 4 strategically spanning the O range:

| Composition | O content | Tc (K) | >= 95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | 4.047 | 95.0 | YES |
| O4.101Cu1Ba2Hg1 | 4.101 | 93.4 | NO |
| O4.162Cu1Ba2Hg1 | 4.162 | 97.4 | YES |
| O4.220Cu1Ba2Hg1 | 4.220 | 97.0 | YES |

Cubic interpolation through 4 measured points (t = (O-4.0)*100):
y = -0.008378t³ + 0.34313t² - 3.937t + 106.794
- Valley at t≈8.2 (O≈4.082, Tc≈93.0), Peak at t≈19.1 (O≈4.191, Tc≈98.4)
- Tc crosses 95 going up at approximately O≈4.127

Interpolated values for remaining 7:
| Composition | O | Predicted Tc | >= 95? |
|---|---|---|---|
| O4.081Cu1Ba2Hg1 | 4.081 | ~93.0 | NO |
| O4.085Cu1Ba2Hg1 | 4.085 | ~93.0 | NO |
| O4.096Cu1Ba2Hg1 | 4.096 | ~93.2 | NO |
| O4.097Cu1Ba2Hg1 | 4.097 | ~93.2 | NO |
| O4.103Cu1Ba2Hg1 | 4.103 | ~93.5 | NO |
| O4.150Cu1Ba2Hg1 | 4.150 | ~96.7 | YES |
| O4.200Cu1Ba2Hg1 | 4.200 | ~98.3 | YES |

Verified with linear interpolation — consistent results.

**Q1 = {O4.047Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}**

## Step C: Rule Induction — COMPLETE ✓

All 14 measured materials are HIGH (Tc >= 90K). Key observations:
- All measured materials contain Hg (both 4-elem and 5-elem families)
- Seed series (5 elem, Hg+Ca): Tc 92-127K → all HIGH
- O4 series (4 elem, Hg, no Ca): Tc 93-97K → all HIGH

Feature analysis of all compositions:
- Measured HIGH: all have Hg (and Cu, Ba, O)
- Unmeasurable 1-3: have Hg → HIGH (same families as measured)
- Unmeasurable 4: has Ca, Ba, Tl, Cu≥2 → Tl-Ba-Ca-Cu-O family (known high-Tc)
- Unmeasurable 5: has Ca, Sr, Tl, Pb, Cu=2 → complex cuprate with Ca
- Unmeasurable 6-7: no Hg, no Ca; has Y, Ba, La → YBCO-type with La doping

**Rule: "A material is HIGH-Tc if it contains Hg OR Ca"**
- Hg enables the highest-Tc cuprate family (Hg-Ba-Ca-Cu-O, up to 135K)
- Ca is essential between CuO2 planes in most high-Tc families
- The interaction: these represent two distinct structural pathways to high Tc
- Materials lacking BOTH (e.g., Y-Ba-La-Cu-O without Ca) tend to have lower Tc

Single-factor failures:
- "Contains Hg" → predicts 4,5 as LOW (wrong, both have Ca → HIGH) = 2 errors
- "Has 5 elements" → predicts 3 as LOW (wrong, has Hg → HIGH), predicts 6,7 as HIGH (wrong) = 3 errors
- "Has Ca" → predicts 3 as LOW (wrong, has Hg → HIGH) = 1 error

**Predictions:**
1. O6.160Ca1Cu2Ba2Hg1: has Hg → HIGH
2. O6.270Ca1Cu2Ba2Hg1: has Hg → HIGH
3. O4.180Cu1Ba2Hg1: has Hg → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → HIGH
6. O8Cu3Y1Ba1.900La0.100: no Hg, no Ca → LOW
7. O8Cu4Y1Ba1.870La0.130: no Hg, no Ca → LOW

## Step D: Final Pick — COMPLETE ✓

Among HIGH predictions (1,2,3,4,5), element counts:
1. O6.160Ca1Cu2Ba2Hg1: 5 elements {O,Ca,Cu,Ba,Hg}
2. O6.270Ca1Cu2Ba2Hg1: 5 elements
3. O4.180Cu1Ba2Hg1: **4 elements** {O,Cu,Ba,Hg} ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, predicted Tc ~98K based on interpolation)
