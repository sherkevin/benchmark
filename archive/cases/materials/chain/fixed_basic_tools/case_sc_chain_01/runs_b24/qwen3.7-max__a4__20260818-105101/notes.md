# Notes: Superconductor Tc Study

## Seed material
`O6.080Ca1Cu2Ba2Hg1` — elements: {O, Ca, Cu, Ba, Hg} (5 elements)

## Step A: Homologous series peak
Series: O6.xxxCa1Cu2Ba2Hg1 (element set {O,Ca,Cu,Ba,Hg}, varying oxygen)

| Composition | Tc (K) | High? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Y |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Y |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Y |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Y |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Y |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Y ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Y ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Y |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Y |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Y |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1, tied with O6.220)

## Step B: Hg + exactly 4 elements, threshold = 127 - 32 = 95.0 K

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | Y ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | Y ✓ |
| O4.150Cu1Ba2Hg1 | 97.0 | Y ✓ |
| O4.220Cu1Ba2Hg1 | 97.0 | Y ✓ |
| O4.096Cu1Ba2Hg1 | 95.6 | Y ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | Y ✓ |
| O4.047Cu1Ba2Hg1 | 95.0 | Y ✓ |
| O4.200Cu1Ba2Hg1 | 94.2 | N |
| O4.101Cu1Ba2Hg1 | 93.4 | N |
| O4.103Cu1Ba2Hg1 | 92.5 | N |
| O4.085Cu1Ba2Hg1 | 92.0 | N |

**Q1 = 7 compositions** with Tc >= 95.0 K

## Additional measurements (for rule induction)
| Composition | Elements | Tc (K) | High? |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | {O,Cu,Ba,Hg,Tl} 5elem | 0.0 | N |
| O10Ca2Cu3Ba2Tl2 | {O,Ca,Cu,Ba,Tl} 5elem | 118.97 | Y |
| O7Cu3Ba2La1 | {O,Cu,Ba,La} 4elem | 89.6 | N |

## Step C: Rule Induction

### Observed pattern across ALL 24 measurements:
- HIGH materials: all O6.xxxCa1Cu2Ba2Hg1 (Hg, no Tl, has Ca), all O4.xxxCu1Ba2Hg1 (Hg, no Tl, no Ca), O10Ca2Cu3Ba2Tl2 (no Hg, has Ca, has Tl)
- LOW materials: O6Cu1Ba2Hg1Tl1 (Hg AND Tl, no Ca), O7Cu3Ba2La1 (no Hg, no Ca)

### Induced Rule:
**HIGH if (contains Hg AND does NOT contain Tl) OR contains Ca**

This is an interaction of multiple factors (Hg×Tl interaction + Ca presence).
- Single-factor "contains Hg" fails: O6Cu1Ba2Hg1Tl1 has Hg but Tc=0
- Single-factor "contains Ca" fails: O4.xxxCu1Ba2Hg1 has no Ca but all high
- The rule correctly handles the Hg+Tl antagonism and the Ca rescue effect

### Verification against all measured data:
- 10 × O6.xxxCa1Cu2Ba2Hg1: Hg=T, Tl=F → HIGH ✓
- 11 × O4.xxxCu1Ba2Hg1: Hg=T, Tl=F → HIGH ✓
- O10Ca2Cu3Ba2Tl2: Ca=T → HIGH ✓
- O6Cu1Ba2Hg1Tl1: Hg=T but Tl=T (fails cond1), Ca=F (fails cond2) → LOW ✓
- O7Cu3Ba2La1: Hg=F (fails cond1), Ca=F (fails cond2) → LOW ✓

### Predictions for unmeasurable materials:
| Composition | Elements | Hg? | Tl? | Ca? | Rule result |
|---|---|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | {O,Ca,Cu,Ba,Hg} 5e | Y | N | Y | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | {O,Ca,Cu,Ba,Hg} 5e | Y | N | Y | HIGH |
| O4.180Cu1Ba2Hg1 | {O,Cu,Ba,Hg} 4e | Y | N | N | HIGH |
| O8Ca0.850Cu2.200Ba2Tl1.800 | {O,Ca,Cu,Ba,Tl} 5e | N | Y | Y | HIGH |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | {O,Ca,Cu,Sr,Tl,Pb} 6e | N | Y | Y | HIGH |
| O8Cu3Y1Ba1.900La0.100 | {O,Cu,Y,Ba,La} 5e | N | N | N | LOW |
| O8Cu4Y1Ba1.870La0.130 | {O,Cu,Y,Ba,La} 5e | N | N | N | LOW |

## Step D: Final pick
Among predicted HIGH, fewest elements:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: **4 elements** ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements

**Final answer: O4.180Cu1Ba2Hg1** (4 elements, expected Tc ~94-97 K based on analogous O4.xxx series)

## Budget used: 24/24
