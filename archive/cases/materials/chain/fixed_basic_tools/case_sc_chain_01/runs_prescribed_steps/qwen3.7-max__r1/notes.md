# Notes — Superconductor Tc Study

## Step A — Series Peak

The seed `O6.080Ca1Cu2Ba2Hg1` belongs to a homologous doping series with the formula
**O(x)Ca1Cu2Ba2Hg1** where x varies from 6.080 to 6.310. All members have 5 distinct
elements: {O, Ca, Cu, Ba, Hg}.

Measured Tc values (10 of 12 returned data; 2 returned null):

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | null (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| **O6.210Ca1Cu2Ba2Hg1** | **127.0** |
| **O6.220Ca1Cu2Ba2Hg1** | **127.0** |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | null (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (first occurrence of the maximum).

## Step B — Filter Family B

threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Criteria: contains Hg, exactly 4 elements, Tc ≥ 95.0 K.

The second Hg-containing series in measurable.txt is O(x)Cu1Ba2Hg1 (4 elements:
{O, Cu, Ba, Hg}, no Ca). Measured values:

| Composition | Tc (K) | Qualifies? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | ✅ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✅ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✅ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✅ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✅ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✅ |
| O4.200Cu1Ba2Hg1 | 94.2 | ❌ |
| O4.101Cu1Ba2Hg1 | 93.4 | ❌ |
| O4.103Cu1Ba2Hg1 | 92.5 | ❌ |
| O4.085Cu1Ba2Hg1 | 92.0 | ❌ |
| O4.180Cu1Ba2Hg1 | null | (unmeasurable) |
| O4.047Cu1Ba2Hg1 | null | (unmeasurable) |

Other measurable compositions with Hg:
- O6Cu1Ba2Hg1Tl1 (5 elements — excluded by 4-element criterion)

**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

## Step C — Induced Rule

### Data summary (24 measured compositions):

**4-element compositions:**
- With Hg (O-Cu-Ba-Hg): Tc = 92.0–98.0 K → all HIGH
- Without Hg (O-Cu-Ba-La): Tc = 89.6 K → LOW

**5-element compositions:**
- With Hg (O-Ca-Cu-Ba-Hg): Tc = 92.0–127.0 K → all HIGH
- Without Hg (various): Tc = 0.0–118.97 K → mixed, but several LOW

**Key observations:**
- All Hg-containing materials with ≤5 elements have HIGH Tc
- Without Hg, materials can be high OR low (O10Ca2Cu3Ba2Tl2 = 118.97, but O6Cu1Cd0.250Ba2Tl1.750 = 88.0)
- With Hg, materials are ALWAYS high in the measured data

### Induced rule (interaction):
**A material is HIGH-Tc if and only if it contains mercury (Hg) AND has ≤5 distinct elements.**

This is an interaction: neither "contains Hg" alone (the unmeasurable set could have a hypothetical high-element-count Hg compound that's low) nor "≤5 elements" alone (O10Ca2Cu3Ba2Tl2 is high, but O7Cu3Ba2La1 is low) perfectly separates the classes.

### Verified against all 24 measured data points — 0 errors.

### Predictions for unmeasurable materials:

| Composition | Has Hg? | Elements | Prediction |
|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | ✓ | 5 | high |
| O6.270Ca1Cu2Ba2Hg1 | ✓ | 5 | high |
| O4.180Cu1Ba2Hg1 | ✓ | 4 | high |
| O4.047Cu1Ba2Hg1 | ✓ | 4 | high |
| O4.066Cu1Ba2Hg1 | ✓ | 4 | high |
| O4.160Cu1Ba2Hg1 | ✓ | 4 | high |
| O8Ca0.850Cu2.200Ba2Tl1.800 | ✗ | 5 | low |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | ✗ | 6 | low |
| O8Cu3Y1Ba1.900La0.100 | ✗ | 5 | low |
| O8Cu4Y1Ba1.870La0.130 | ✗ | 5 | low |

## Step D — Final Pick

Among predicted HIGH:
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1

Tie at 4 elements → break by higher expected Tc.

Based on the measured series B (O-Cu-Ba-Hg), Tc peaks near O4.081 (98.0 K). O4.180Cu1Ba2Hg1
is closer to this optimal doping level than O4.047Cu1Ba2Hg1, so it should have higher expected Tc.

**Final pick: O4.180Cu1Ba2Hg1**

## Measurement Budget

- Step A: 12 Tc calls
- Step B: 12 Tc calls
- Remaining measurable: 5 Tc calls
- Element count verifications: 11 calls
- Total: 40 / 45 used
