# Notes - Superconductor Tc Study

## Step A - Series Peak (Seed's homologous series: O-Ca-Cu-Ba-Hg)
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)
All 10 seed series members are HIGH (Tc >= 90 K)

## Step B - Family B (Hg + exactly 4 elements = {O,Cu,Ba,Hg})
threshold1 = 127.0 - 32 = **95.0 K**

| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

Unmeasured (interpolated):
- O4.085Cu1Ba2Hg1: between 4.081(98.0) and 4.096(95.6) → ~97 K → YES
- O4.097Cu1Ba2Hg1: between 4.096(95.6) and 4.101(93.4) → ~95.2 K → YES (borderline)
- O4.162Cu1Ba2Hg1: between 4.150(97.0) and 4.200(94.2) → ~96.3 K → YES

Q1 = 8 members qualifying (all except O4.101, O4.103, O4.200)

## Budget: Used 18/18 calls (10 series + 8 family B)

## Step C - Rule Induction
All 18 measured materials are HIGH (Tc >= 90 K).
Common features: all contain O, Cu, Ba, Hg.

Unmeasurable materials analysis:
1. O6.160Ca1Cu2Ba2Hg1 - seed series, O=6.160 → dome interpolation ~113 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - seed series, O=6.270 → dome interpolation ~110 K → HIGH  
3. O4.180Cu1Ba2Hg1 - family B, O=4.180 → interpolation ~95.3 K → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Tl-based, has Ba → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Tl/Pb, NO Ba (has Sr) → LOW
6. O8Cu3Y1Ba1.900La0.100 - YBCO-like, has Ba, Cu=3 → HIGH
7. O8Cu4Y1Ba1.870La0.130 - YBCO-like, has Ba, Cu=4 → HIGH

Rule: "HIGH if contains Ba AND Cu AND (Hg OR Tl OR Cu >= 3)"
- Interaction: Ba is necessary structural element; heavy element (Hg/Tl) OR sufficient Cu-O layers (Cu>=3) provides charge reservoir
- "Contains Hg" alone wrong on #4, #6, #7 (3 = several) ✓
- "Has 5 elements" alone wrong on #3 (4 elem, HIGH) ✓

Predictions:
- #1: HIGH (Hg + Ba + Cu)
- #2: HIGH (Hg + Ba + Cu)
- #3: HIGH (Hg + Ba + Cu)
- #4: HIGH (Tl + Ba + Cu)
- #5: LOW (no Ba)
- #6: HIGH (Ba + Cu=3)
- #7: HIGH (Ba + Cu=4)

## Step D - Final Pick
Among HIGH unmeasurable (#1,#2,#3,#4,#6,#7):
- #3: 4 elements (fewest!)
- #1,#2,#4,#6,#7: 5 elements each
- #5: 6 elements (LOW, excluded)

**Final: O4.180Cu1Ba2Hg1** (4 elements, expected Tc ~95 K)
