# Notes for Superconductor Tc Study

## Seed Material
- Composition: `O6.080Ca1Cu2Ba2Hg1`
- Contains elements: O, Ca, Cu, Ba, Hg (5 elements)
- Tc = 92.0 K (HIGH, since >= 90 K)

## Budget
- Total: 34 units - EXHAUSTED

## Step A - Homologous Series Peak
Series: O-Ca-Cu-Ba-Hg (same element set as seed, varying oxygen)

Measurements:
| Composition | Tc (K) | Sample? |
|-------------|--------|---------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | - | No |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | - | No |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes |

**Peak T* = 127.0 K at O6.210Ca1Cu2Ba2Hg1**

## Step B - Filter Family B (Hg, exactly 4 elements, Tc >= threshold)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

4-element Hg materials (O-Cu-Ba-Hg):
| Composition | Tc (K) | >= 95? |
|-------------|--------|--------|
| O4.047Cu1Ba2Hg1 | - (no sample) | ? |
| O4.066Cu1Ba2Hg1 | - (no sample) | ? |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | - (no sample) | ? |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | - (no sample) | ? |
| O4.200Cu1Ba2Hg1 | not measured | ? |
| O4.220Cu1Ba2Hg1 | not measured | ? |

**Q1 (qualifying measured materials):**
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)

## Step C - Unmeasurable Materials
Found via search_db but no sample exists:

From Hg search (4-element O-Cu-Ba-Hg):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

From 5-element series (O-Ca-Cu-Ba-Hg):
- O6.160Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1

Need to induce a rule to predict high/low for these.

## Rule Induction
Looking at measurable materials to find a pattern...

5-element series (O-Ca-Cu-Ba-Hg):
- O6.080: 92.0 (HIGH)
- O6.100: 92.5 (HIGH)
- O6.130: 102.0 (HIGH)
- O6.140: 105.0 (HIGH)
- O6.190: 122.0 (HIGH)
- O6.210: 127.0 (HIGH)
- O6.220: 127.0 (HIGH)
- O6.240: 123.0 (HIGH)
- O6.300: 100.0 (HIGH)
- O6.310: 98.0 (HIGH)

All 5-element series materials are HIGH (>= 90 K).

4-element series (O-Cu-Ba-Hg):
- O4.081: 98.0 (HIGH)
- O4.085: 92.0 (HIGH)
- O4.096: 95.6 (HIGH)
- O4.097: 95.3 (HIGH)
- O4.101: 93.4 (HIGH)
- O4.103: 92.5 (HIGH)
- O4.150: 97.0 (HIGH)
- O4.162: 97.4 (HIGH)

All measured 4-element O-Cu-Ba-Hg are also HIGH!

But the task warns "single-factor rules are wrong on several of them". Let me look for other materials in the database...

From Ca search, I found:
- O10Ca2Cu3Ba2Tl2 (5 elements: O, Ca, Cu, Ba, Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl)

I haven't measured these. Let me check if there are more materials by searching other elements.
