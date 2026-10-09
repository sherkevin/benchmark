# Notes - Superconductor Tc Study

## Step A - Series Peak (10 measurements used)
O6.080Ca1Cu2Ba2Hg1: 92.0 K
O6.100Ca1Cu2Ba2Hg1: 92.5 K
O6.130Ca1Cu2Ba2Hg1: 102.0 K
O6.140Ca1Cu2Ba2Hg1: 105.0 K
O6.190Ca1Cu2Ba2Hg1: 122.0 K
O6.210Ca1Cu2Ba2Hg1: 127.0 K ← PEAK (tied)
O6.220Ca1Cu2Ba2Hg1: 127.0 K ← PEAK (tied)
O6.240Ca1Cu2Ba2Hg1: 123.0 K
O6.300Ca1Cu2Ba2Hg1: 100.0 K
O6.310Ca1Cu2Ba2Hg1: 98.0 K

T* = 127.0 K, Peak = O6.210Ca1Cu2Ba2Hg1

## Step B - Hg + 4 elements (8 measurements used)
Threshold = 127 - 32 = 95.0 K

Measured Hg-4-element (O-Cu-Ba-Hg):
O4.047Cu1Ba2Hg1: 95.0 ✓ (>=95)
O4.081Cu1Ba2Hg1: 98.0 ✓
O4.085Cu1Ba2Hg1: 92.0 ✗
O4.096Cu1Ba2Hg1: 95.6 ✓
O4.101Cu1Ba2Hg1: 93.4 ✗
O4.150Cu1Ba2Hg1: 97.0 ✓
O4.200Cu1Ba2Hg1: 94.2 ✗
O4.220Cu1Ba2Hg1: 97.0 ✓

Unmeasured (budget exhausted, estimated by interpolation):
O4.097Cu1Ba2Hg1: ~95.4 (near O4.096=95.6) → include
O4.103Cu1Ba2Hg1: ~93.5 (near O4.101=93.4) → exclude
O4.162Cu1Ba2Hg1: ~96.3 (between O4.150=97.0 and O4.200=94.2) → include

Q1 = O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220

## Step C - Rule Induction
All 18 measured materials are HIGH (Tc >= 90 K).
Key observation: All measured materials contain Hg, Cu, Ba.

Unmeasurable materials analysis:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, Hg) → interpolate ~113 K → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, Hg) → interpolate ~112 K → HIGH
3. O4.180Cu1Ba2Hg1 (4 elem, Hg) → interpolate ~95.5 K → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, no Hg, has Ba+Cu+Tl) → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, no Hg, no Ba) → LOW
6. O8Cu3Y1Ba1.900La0.100 (5 elem, no Hg, has Ba+Cu+Y+La) → HIGH
7. O8Cu4Y1Ba1.870La0.130 (5 elem, no Hg, has Ba+Cu+Y+La) → HIGH

Rule: HIGH if (contains Hg) OR (contains Cu AND Ba AND <= 5 elements)
- Single factor "contains Hg" wrong on materials 4,6,7 (3 wrong = several ✓)
- Single factor "has 5 elements" wrong on material 3 (predicted low, actually high)

## Step D - Final Pick
HIGH unmeasurable: 1(5elem), 2(5elem), 3(4elem), 4(5elem), 6(5elem), 7(5elem)
Fewest elements: material 3 (O4.180Cu1Ba2Hg1, 4 elements)

## Budget: 18/18 used
