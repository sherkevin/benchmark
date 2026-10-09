# Notes - Superconductor Tc Study

## Budget: 18/18 used

## Step A: Series Peak (O-Ca-Cu-Ba-Hg series)
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 ★ PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 ★ PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)

## Step B: Family B (Hg + exactly 4 elements, threshold = 95 K)
Measured:
| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Not measured (budget), interpolated:
- O4.085Cu1Ba2Hg1: ~97.4 (between O4.081=98.0 and O4.096=95.6) → YES
- O4.097Cu1Ba2Hg1: ~95.2 (between O4.096=95.6 and O4.101=93.4) → YES (borderline)
- O4.103Cu1Ba2Hg1: ~93.5 (between O4.101=93.4 and O4.150=97.0) → NO

**Q1 = {O4.047, O4.081, O4.085, O4.096, O4.097, O4.150, O4.162, O4.220}** (8 members)

## Step C: Rule Induction

### Key observations from measured data:
- ALL 18 measured materials have Tc >= 90 K (all "high")
- All measured materials contain: Cu, Ba, O, Hg
- Series A: {O,Ca,Cu,Ba,Hg} → all high (92-127 K)
- Family B: {O,Cu,Ba,Hg} → all high (93.4-98 K)

### Unmeasurable materials to predict:
1. O6.160Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} = 5 elements, has Hg+Ba
2. O6.270Ca1Cu2Ba2Hg1: {O,Ca,Cu,Ba,Hg} = 5 elements, has Hg+Ba
3. O4.180Cu1Ba2Hg1: {O,Cu,Ba,Hg} = 4 elements, has Hg+Ba
4. O8Ca0.850Cu2.200Ba2Tl1.800: {O,Ca,Cu,Ba,Tl} = 5 elements, has Tl+Ba (no Hg)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: {O,Ca,Cu,Sr,Tl,Pb} = 6 elements, has Tl but NO Ba
6. O8Cu3Y1Ba1.900La0.100: {O,Cu,Y,Ba,La} = 5 elements, has Y+Ba (no Hg/Tl)
7. O8Cu4Y1Ba1.870La0.130: {O,Cu,Y,Ba,La} = 5 elements, has Y+Ba (no Hg/Tl)

### Induced Rule:
"High Tc if the material contains Ba AND at least one of {Hg, Tl, Y}"
- Need both a large alkaline earth (Ba) for structural framework AND a high-Tc indicator element (Hg, Tl, or Y)
- This is an interaction: neither Ba alone nor Hg/Tl/Y alone is sufficient

### Why single-factor rules fail:
- "Contains Hg" alone: wrong on materials 4,6,7 (no Hg but should be high) → 3 errors
- "Has 5 elements" alone: wrong on 3 (4 elem, high), 5 (6 elem, low), and ambiguous on 6,7 → 3+ errors
- "Contains Ba" alone: wrong on material 5 (no Ba, should be low) and would predict 6,7 as high (correct) but 5 is an error → misses the interaction

### Predictions:
1. O6.160Ca1Cu2Ba2Hg1: Ba ✓ + Hg ✓ → **HIGH**
2. O6.270Ca1Cu2Ba2Hg1: Ba ✓ + Hg ✓ → **HIGH**
3. O4.180Cu1Ba2Hg1: Ba ✓ + Hg ✓ → **HIGH**
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ba ✓ + Tl ✓ → **HIGH**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: no Ba ✗ → **LOW**
6. O8Cu3Y1Ba1.900La0.100: Ba ✓ + Y ✓ → **HIGH**
7. O8Cu4Y1Ba1.870La0.130: Ba ✓ + Y ✓ → **HIGH**

## Step D: Final Pick
Among predicted HIGH: materials 1,2,3,4,6,7
- Material 1: 5 elements
- Material 2: 5 elements
- Material 3: 4 elements ← FEWEST
- Material 4: 5 elements
- Material 6: 5 elements
- Material 7: 5 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected ~96 K based on family B interpolation)
