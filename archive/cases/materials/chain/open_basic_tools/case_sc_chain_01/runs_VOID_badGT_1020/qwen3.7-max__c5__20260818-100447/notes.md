# Notes - Superconductor Tc Study

## Budget Tracking
- Used: 34 / 34 (exhausted)
- 9 search_db pages (Hg: 9 pages), 2 Tl pages, 2 Ca pages = 13 search units
- Measurements: 12 seed series + 14 four-element + 1 Tl material = 21 measurement units
- Unmeasurable attempts: 8 (free - no budget cost)

## All Discovered Materials (27 Hg + 4 non-Hg = 31 total)

### Hg-containing (27):
**Series 1: O-Cu-Ba-Hg (4 elements)** - 14 materials
O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1,
O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.101Cu1Ba2Hg1, O4.103Cu1Ba2Hg1,
O4.150Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.180Cu1Ba2Hg1,
O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

**Series 2: O-Ca-Cu-Ba-Hg (5 elements, seed series)** - 12 materials
O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1,
O6.160Ca1Cu2Ba2Hg1, O6.190Ca1Cu2Ba2Hg1, O6.210Ca1Cu2Ba2Hg1, O6.220Ca1Cu2Ba2Hg1,
O6.240Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1, O6.310Ca1Cu2Ba2Hg1

**Other Hg:** O6Cu1Ba2Hg1Tl1 (5 elements: O,Cu,Ba,Hg,Tl)

### Non-Hg materials (from Tl search):
O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl) - HAS SAMPLE (couldn't measure)
O6Cu1Cd0.250Ba2Tl1.750 (5 elements: O,Cu,Cd,Ba,Tl) - HAS SAMPLE (couldn't measure)
O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb) - NO SAMPLE
O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl) - NO SAMPLE

## Step A: Series Peak Results
| Composition | Tc (K) | Sample? | High/Low |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes | High |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes | High |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes | High |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes | High |
| O6.160Ca1Cu2Ba2Hg1 | - | NO | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes | High |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes | High ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes | High ← PEAK |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes | High |
| O6.270Ca1Cu2Ba2Hg1 | - | NO | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes | High |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes | High |

**T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1** (tied with O6.220)
Dome shape: rises from 92 K to 127 K peak, then drops back to 98 K.

## Step B: Filter Family B
- threshold1 = 127.0 - 32 = 95.0 K
- 4-element Hg materials (O-Cu-Ba-Hg) measured:

| Composition | Tc (K) | Sample? | >= 95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.066Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.081Cu1Ba2Hg1 | 98.0 | Yes | ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | Yes | ✗ |
| O4.096Cu1Ba2Hg1 | 95.6 | Yes | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | Yes | ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | Yes | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | Yes | ✗ |
| O4.150Cu1Ba2Hg1 | 97.0 | Yes | ✓ |
| O4.160Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.162Cu1Ba2Hg1 | 97.4 | Yes | ✓ |
| O4.180Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.200Cu1Ba2Hg1 | 94.2 | Yes | ✗ |
| O4.220Cu1Ba2Hg1 | 97.0 | Yes | ✓ |

**Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} (6 members)**

## Step C: Rule Induction

### Measured data summary:
- ALL 10 measurable seed-series (5 elem, O-Ca-Cu-Ba-Hg): HIGH (92-127 K)
- ALL 10 measurable 4-elem (O-Cu-Ba-Hg): HIGH (92-98 K)
- O6Cu1Ba2Hg1Tl1 (5 elem, O-Cu-Ba-Hg-Tl): LOW (Tc = 0 K!)
- Only Low material has Tl and lacks Ca

### Rule: "Low if contains Tl AND lacks Ca; High otherwise"
- Single-factor "Tl→Low" would be wrong because some Tl materials also have Ca (known Tl-Ca cuprate families are high-Tc)
- The Ca presence protects against Tl-induced suppression

### Unmeasurable materials & predictions:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, no Tl) → **High**
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, no Tl) → **High**
3. O4.047Cu1Ba2Hg1 (4 elem, no Tl) → **High**
4. O4.066Cu1Ba2Hg1 (4 elem, no Tl) → **High**
5. O4.160Cu1Ba2Hg1 (4 elem, no Tl) → **High**
6. O4.180Cu1Ba2Hg1 (4 elem, no Tl) → **High**
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, has Tl + Ca) → **High**
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, has Tl + Ca) → **High**

## Step D: Final Pick
All 8 predicted High.
- 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Fewest elements = 4 (four candidates).
Tie-break by expected Tc:
- O4.160: between O4.150(97.0) and O4.162(97.4) → ~97.3 K
- O4.180: between O4.162(97.4) and O4.200(94.2) → ~95.8 K
- O4.066: near O4.081(98.0) → ~96-98 K (uncertain)
- O4.047: below measured range → ~94-97 K (uncertain)

**Best pick: O4.160Cu1Ba2Hg1 (4 elements, ~97.3 K expected)**
