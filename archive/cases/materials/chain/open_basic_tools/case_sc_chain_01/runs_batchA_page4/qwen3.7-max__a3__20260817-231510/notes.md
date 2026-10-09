# Notes - Superconductor Tc Study

## Budget: 44/45 used

## Complete Database (35 materials total, all contain Cu)

### Hg-1201 series (4 elements: O, Cu, Ba, Hg) - 14 members
| Composition | Tc (K) | Available | High? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.066Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.081Cu1Ba2Hg1 | 98.0 | yes | high |
| O4.085Cu1Ba2Hg1 | 92.0 | yes | high |
| O4.096Cu1Ba2Hg1 | 95.6 | yes | high |
| O4.097Cu1Ba2Hg1 | 95.3 | yes | high |
| O4.101Cu1Ba2Hg1 | 93.4 | yes | high |
| O4.103Cu1Ba2Hg1 | 92.5 | yes | high |
| O4.150Cu1Ba2Hg1 | 97.0 | yes | high |
| O4.160Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.162Cu1Ba2Hg1 | 97.4 | yes | high |
| O4.180Cu1Ba2Hg1 | - | NO | unmeasurable |
| O4.200Cu1Ba2Hg1 | 94.2 | yes | high |
| O4.220Cu1Ba2Hg1 | 97.0 | yes | high |

### Hg-1212 series (5 elements: O, Ca, Cu, Ba, Hg) - 12 members
| Composition | Tc (K) | Available | High? |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes | high |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes | high |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes | high |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes | high |
| O6.160Ca1Cu2Ba2Hg1 | - | NO | unmeasurable |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes | high |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes | high ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes | high ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes | high |
| O6.270Ca1Cu2Ba2Hg1 | - | NO | unmeasurable |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes | high |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes | high |

### Other Hg material (5 elements: O, Cu, Ba, Hg, Tl)
| O6Cu1Ba2Hg1Tl1 | 0.0 | yes | LOW |

### Non-Hg Tl-containing materials
| O10Ca2Cu3Ba2Tl2 | 118.97 | yes | high (5 elem: O,Ca,Cu,Ba,Tl) |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | yes | LOW (5 elem: O,Cu,Cd,Ba,Tl) |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | - | NO | unmeasurable (6 elem: O,Ca,Cu,Sr,Tl,Pb) |
| O8Ca0.850Cu2.200Ba2Tl1.800 | - | NO | unmeasurable (5 elem: O,Ca,Cu,Ba,Tl) |

### Y-La containing materials (no Hg, no Tl)
| O7Cu3Ba2La1 | 89.6 | yes | LOW (4 elem: O,Cu,Ba,La) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | yes | LOW (5 elem: O,Cu,Y,Ba,La) |
| O8Cu3Y1Ba1.900La0.100 | - | NO | unmeasurable (5 elem: O,Cu,Y,Ba,La) |
| O8Cu4Y1Ba1.870La0.130 | - | NO | unmeasurable (5 elem: O,Cu,Y,Ba,La) |

---

## Step A: Series Peak
- Seed: O6.080Ca1Cu2Ba2Hg1 (Hg-1212 series)
- Peak: O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K (tied with O6.220)

## Step B: Q1 (Hg + exactly 4 elements, Tc >= threshold1)
- threshold1 = 127.0 - 32 = 95.0
- Qualifying (measured, Hg, 4 elem, Tc >= 95.0):
  - O4.081Cu1Ba2Hg1 (98.0)
  - O4.096Cu1Ba2Hg1 (95.6)
  - O4.097Cu1Ba2Hg1 (95.3)
  - O4.150Cu1Ba2Hg1 (97.0)
  - O4.162Cu1Ba2Hg1 (97.4)
  - O4.220Cu1Ba2Hg1 (97.0)

## Step C: Rule Induction
Measurable materials analysis:
- ALL Hg-containing (no Tl) materials → HIGH
- O10Ca2Cu3Ba2Tl2 (has Ca, Tl, no Hg) → HIGH
- O6Cu1Ba2Hg1Tl1 (has Hg AND Tl, no Ca) → LOW (0.0)
- O6Cu1Cd0.250Ba2Tl1.750 (Tl, no Ca, no Hg) → LOW (88.0)
- O7Cu3Ba2La1 (no Hg, no Tl, no Ca) → LOW (89.6)
- O7Cu3Y0.200Ba2La0.800 (no Hg, no Tl, no Ca) → LOW (81.0)

**Rule: HIGH if (contains Hg AND does NOT contain Tl) OR contains Ca. LOW otherwise.**

This is a multi-factor rule (checks Hg, Tl, and Ca presence).
- Single-factor "contains Hg → high" FAILS for O6Cu1Ba2Hg1Tl1
- Single-factor "contains Tl → low" FAILS for O10Ca2Cu3Ba2Tl2
- Single-factor "contains Ca → high" works for measured data but is incomplete

### Predictions for unmeasurable materials:
1. O4.047Cu1Ba2Hg1: Hg yes, Tl no → HIGH
2. O4.066Cu1Ba2Hg1: Hg yes, Tl no → HIGH
3. O4.160Cu1Ba2Hg1: Hg yes, Tl no → HIGH
4. O4.180Cu1Ba2Hg1: Hg yes, Tl no → HIGH
5. O6.160Ca1Cu2Ba2Hg1: Hg yes, Tl no, Ca yes → HIGH
6. O6.270Ca1Cu2Ba2Hg1: Hg yes, Tl no, Ca yes → HIGH
7. O7Ca1Cu2Sr2Tl0.500Pb0.500: Ca yes → HIGH
8. O8Ca0.850Cu2.200Ba2Tl1.800: Ca yes → HIGH
9. O8Cu3Y1Ba1.900La0.100: no Hg, no Ca → LOW
10. O8Cu4Y1Ba1.870La0.130: no Hg, no Ca → LOW

## Step D: Final Pick
Among unmeasurable predicted HIGH, fewest elements:
- 4 elements: O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Tie-break among 4-element ones by expected Tc:
- O4.160: between O4.150(97.0) and O4.162(97.4) → ~97.3 (highest)
- O4.180: between O4.162(97.4) and O4.200(94.2) → ~95.9
- O4.066: near O4.081(98.0) → ~96
- O4.047: further from peak → ~93

**Final pick: O4.160Cu1Ba2Hg1** (4 elements, highest expected Tc ~97.3)
