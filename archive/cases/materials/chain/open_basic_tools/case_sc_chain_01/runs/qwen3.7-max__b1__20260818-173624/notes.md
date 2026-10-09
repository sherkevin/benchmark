# Notes

## Budget: 26 total, all used (11 measurements + 15 searches)

## Step A - Series Peak (Hg-Ba-Ca-Cu-O, varying oxygen)
Seed: O6.080Ca1Cu2Ba2Hg1 → Tc = 92.0 K

| Composition | Tc (K) | Status |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | measured |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | measured |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | measured |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | measured |
| O6.160Ca1Cu2Ba2Hg1 | — | no sample |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | measured |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | measured ← PEAK (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | measured |
| O6.270Ca1Cu2Ba2Hg1 | — | no sample |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | measured |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | measured |

T* = 127.0 K, peak composition = O6.210Ca1Cu2Ba2Hg1

## Step B - Filter family B (Hg + exactly 4 elements)
threshold1 = 127.0 - 32 = 95.0

4-element Hg materials found (14 total, all O-Cu-Ba-Hg):
O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1,
O4.085Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1,
O4.101Cu1Ba2Hg1 (measured: 93.4 K < 95.0), O4.103Cu1Ba2Hg1,
O4.150Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1,
O4.180Cu1Ba2Hg1 (unmeasurable), O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

Q1 = [] (no measured 4-element Hg material with Tc >= 95.0)

## Step C - Unmeasurable materials (5 found)
1. O4.180Cu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg) → predict HIGH
2. O6.160Ca1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg) → predict HIGH  
3. O6.270Ca1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg) → predict HIGH
4. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb) → predict HIGH
5. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl) → predict HIGH

Rule: "Contains Cu AND at least one of (Hg, Ba, Tl) → high; otherwise → low"
All measurable materials (11 tested) are HIGH. All 5 unmeasurable predicted HIGH.

Single-factor "has Hg" wrong on #4,#5 (no Hg but still high).
Single-factor "has Ba" wrong on #4 (no Ba but still high).

## Step D - Final pick
Among predicted HIGH: {O4.180(4el), O6.160(5el), O6.270(5el), O7...(6el), O8...(5el)}
Fewest elements: O4.180Cu1Ba2Hg1 (4 elements)
Final = O4.180Cu1Ba2Hg1
