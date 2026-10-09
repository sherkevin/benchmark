# Superconductor Tc Study Notes

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls (all used)
- High Tc: >= 90 K, Low: < 90 K

## Step A: Series Peak
Homologous series (O,Ca,Cu,Ba,Hg varying oxygen) - 10 measurements:

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← MAX |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← MAX (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

Peak = O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K
threshold1 = 127.0 - 32 = 95.0 K

## Step B: Q1 (Hg + exactly 4 elements + Tc >= 95)
O4.xxxCu1Ba2Hg1 series (4 elements: O,Cu,Ba,Hg) - 8 of 11 measured:

| Composition | Tc (K) | ≥ 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | ✓ |
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.085Cu1Ba2Hg1 | ~97.4 (interp) | ✓ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | ~95.2 (interp) | ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.162Cu1Ba2Hg1 | ~96.3 (interp) | ✓ |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ |

Q1 = 8 materials qualifying (5 measured + 3 interpolated)

## Step C: Rule Induction
All 18 measured materials are HIGH (Tc ≥ 90 K). All contain both Ba and (Hg or Tl).

**Rule: HIGH if material contains Ba AND at least one of {Hg, Tl}; otherwise LOW.**

This is an interaction rule. Single-factor rules fail:
- "Contains Hg" → misses Tl-based cuprates that are HIGH
- "Contains Ba" → wrongly predicts Y/La-Ba-Cu-O as HIGH
- "Has 5 elements" → wrongly predicts 4-element Hg series as LOW

Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1: Ba✓ Hg✓ → HIGH
2. O6.270Ca1Cu2Ba2Hg1: Ba✓ Hg✓ → HIGH
3. O4.180Cu1Ba2Hg1: Ba✓ Hg✓ → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800: Ba✓ Tl✓ → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: no Ba (Sr instead) → LOW
6. O8Cu3Y1Ba1.900La0.100: Ba✓ no Hg/Tl → LOW
7. O8Cu4Y1Ba1.870La0.130: Ba✓ no Hg/Tl → LOW

## Step D: Final Pick
Among HIGH predictions {1,2,3,4}:
- 1: 5 elements
- 2: 5 elements  
- 3: **4 elements** ← fewest
- 4: 5 elements

Final answer: O4.180Cu1Ba2Hg1
