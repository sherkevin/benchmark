# Superconductor Tc Study Notes

## Budget Usage
- 15 search pages + 3 successful measurements = 18 units (2 failed measurements didn't cost)
- Searches were too extensive - should have saved more budget for measurements

## Step A: Series Peak

### Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
All members found via Ca and Hg searches:
1. O6.080Ca1Cu2Ba2Hg1 - **Tc = 92K** (measured)
2. O6.100Ca1Cu2Ba2Hg1 - not measured
3. O6.130Ca1Cu2Ba2Hg1 - not measured
4. O6.140Ca1Cu2Ba2Hg1 - not measured
5. O6.160Ca1Cu2Ba2Hg1 - **NO SAMPLE** (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 - not measured
7. O6.210Ca1Cu2Ba2Hg1 - not measured
8. O6.220Ca1Cu2Ba2Hg1 - not measured
9. O6.240Ca1Cu2Ba2Hg1 - **Tc = 123K** (measured) ← highest measured
10. O6.270Ca1Cu2Ba2Hg1 - **NO SAMPLE** (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 - **Tc = 100K** (measured)
12. O6.310Ca1Cu2Ba2Hg1 - not measured

### Parabolic Fit
Using three data points: (6.080, 92), (6.240, 123), (6.300, 100)
Let x = oxygen - 6.0:
Tc(x) = -2623.1*x² + 1033.1*x + 26.2

Peak at x = 0.197 → oxygen ≈ 6.197, Tc ≈ 128K
Nearest member: O6.190Ca1Cu2Ba2Hg1 (predicted Tc ≈ 127.8K)

However, the HIGHEST MEASURED value is O6.240 at 123K.
Going with measured peak: **O6.240Ca1Cu2Ba2Hg1, T* = 123K**

## Step B: Filter Family B (Hg + exactly 4 elements)
threshold1 = T* - 32 = 123 - 32 = 91K

### 4-element Hg materials (O, Cu, Ba, Hg):
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1
6. O4.097Cu1Ba2Hg1
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1
10. O4.160Cu1Ba2Hg1
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.200Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1

None of these were measured (budget exhausted).
Cannot determine qualifying set without measurements.

## Step C: Unmeasurable Materials Predictions
Confirmed unmeasurable:
- O6.160Ca1Cu2Ba2Hg1: predicted Tc ≈ 124K → **high**
- O6.270Ca1Cu2Ba2Hg1: predicted Tc ≈ 114K → **high**

Rule: Parabolic Tc dome vs oxygen content within each homologous series.

Warning says single-factor rules are wrong on several - but with only 3 measured data points (all same series), can't build multi-factor model.

## Step D: Final Pick
Both unmeasurable predicted-high materials have 5 elements.
Tie-break by higher expected Tc: O6.160 (predicted ~124K) > O6.270 (predicted ~114K).
**Final: O6.160Ca1Cu2Ba2Hg1**

## Other Materials Discovered
- O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl)
- O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl)

Unknown measurability - couldn't check with exhausted budget.
