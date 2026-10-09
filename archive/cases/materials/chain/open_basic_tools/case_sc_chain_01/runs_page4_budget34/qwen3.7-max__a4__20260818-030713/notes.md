# Notes - Superconductor Tc Study

## Task Overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Budget Tracking
- Search "Hg" pages 1-7: 7 units
- Search "Cu" pages 1-6: 6 units
- Measurements: 21 units (19 successful + 2 that returned no sample but still cost)
  Wait - actually the unmeasurable ones did NOT cost budget (no budget_used field shown)
  Actually recounting: 1 (seed) + 10 (series) + 10 (4-elem) + 1 (Tl material) = 22 measurable attempts
  But O6.160, O6.270, O4.047, O4.066, O4.160, O4.180 returned no sample - 6 unmeasurable
  So 22 - 6 = 16 actual measurements that cost + 1 for O6Cu1Ba2Hg1Tl1 = ... let me just use the reported numbers
- Total budget used: 34/34

## Step A: Homologous Series (O{varying}Ca1Cu2Ba2Hg1)

Measured members:
| Composition | Tc (K) | High? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE | - |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE | - |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes |

Peak: T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220)
Dome shape: rises from 92 → 127, then falls back to 98

## Step B: 4-element Hg family (O{varying}Cu1Ba2Hg1)

threshold1 = 127.0 - 32 = 95.0 K

Measured members:
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | Yes ✓ |
| O4.085Cu1Ba2Hg1 | 92.0 | No |
| O4.096Cu1Ba2Hg1 | 95.6 | Yes ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | Yes ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | No |
| O4.103Cu1Ba2Hg1 | 92.5 | No |
| O4.150Cu1Ba2Hg1 | 97.0 | Yes ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | Yes ✓ |
| O4.200Cu1Ba2Hg1 | 94.2 | No |
| O4.220Cu1Ba2Hg1 | 97.0 | Yes ✓ |

Unmeasurable: O4.047, O4.066, O4.160, O4.180

Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Other materials
- O6Cu1Ba2Hg1Tl1 (5 elem: O,Cu,Ba,Hg,Tl): Tc = 0.0 K (LOW!)

## Cu search - new material found
- O10Ca2Cu3Ba2Tl2 (5 elem: O,Ca,Cu,Ba,Tl): not measured (unknown measurability)
  Predicted: LOW (no Hg, has Tl)

## Step C: Rule Induction

Key observations:
1. ALL measurable Hg-containing materials WITHOUT Tl have Tc >= 90 (high)
2. The ONLY material with Tl (O6Cu1Ba2Hg1Tl1) has Tc = 0 (low)
3. Single-factor "has Hg → high" fails for O6Cu1Ba2Hg1Tl1

**Rule: Material is HIGH if it contains Hg AND does NOT contain Tl; otherwise LOW.**
(Multi-factor: considers both Hg and Tl presence)

### Unmeasurable materials and predictions:
1. O4.047Cu1Ba2Hg1 (4 elem, has Hg, no Tl) → HIGH
2. O4.066Cu1Ba2Hg1 (4 elem, has Hg, no Tl) → HIGH
3. O4.160Cu1Ba2Hg1 (4 elem, has Hg, no Tl) → HIGH
4. O4.180Cu1Ba2Hg1 (4 elem, has Hg, no Tl) → HIGH
5. O6.160Ca1Cu2Ba2Hg1 (5 elem, has Hg, no Tl) → HIGH
6. O6.270Ca1Cu2Ba2Hg1 (5 elem, has Hg, no Tl) → HIGH
7. O10Ca2Cu3Ba2Tl2 (5 elem, no Hg, has Tl) → LOW

## Step D: Final Pick

Unmeasurable predicted HIGH:
- O4.047Cu1Ba2Hg1 (4 elements)
- O4.066Cu1Ba2Hg1 (4 elements)
- O4.160Cu1Ba2Hg1 (4 elements)
- O4.180Cu1Ba2Hg1 (4 elements)
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

Fewest elements = 4 (four candidates tied)

Tie-breaking by expected Tc:
- O4.047: near O4.081 (98.0, highest in 4-elem family) → expected ~98+
- O4.066: between O4.047 and O4.081 → expected ~98
- O4.160: between O4.150(97.0) and O4.162(97.4) → expected ~97.3
- O4.180: between O4.162(97.4) and O4.200(94.2) → expected ~95.9

Final pick: O4.047Cu1Ba2Hg1 (4 elements, highest expected Tc near the series peak)
