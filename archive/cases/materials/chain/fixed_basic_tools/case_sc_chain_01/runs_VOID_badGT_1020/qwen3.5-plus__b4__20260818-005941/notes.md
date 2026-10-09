# Notes for Superconductor Tc Study

## Task Overview
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- High Tc definition: >= 90 K
- Measurement budget: 34 calls

## Step A — Find peak Tc in homologous series
**T* = 127.0 K** (from O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Hg1)
Peak composition: **O6.210Ca1Cu2Ba2Hg1**

## Step B — Filter family B
- threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**
- Materials with Hg, exactly 4 elements, and Tc >= 95.0:
  - O4.081Cu1Ba2Hg1: 98.0 ✓
  - O4.162Cu1Ba2Hg1: 97.4 ✓
  - O4.150Cu1Ba2Hg1: 97.0 ✓
  - O4.220Cu1Ba2Hg1: 97.0 ✓
  - O4.096Cu1Ba2Hg1: 95.6 ✓
  - O4.097Cu1Ba2Hg1: 95.3 ✓
  
**Q1** = [O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1]

## Step C — Predict high/low for unmeasurable materials
Unmeasurable list:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.180Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
4. O4.047Cu1Ba2Hg1 (4 elements)
5. O4.066Cu1Ba2Hg1 (4 elements)
6. O4.160Cu1Ba2Hg1 (4 elements)
7. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl - no Hg)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb - no Hg)
9. O8Cu3Y1Ba1.900La0.100 (5 elements: O, Cu, Y, Ba, La - no Hg)
10. O8Cu4Y1Ba1.870La0.130 (5 elements - no Hg)

### Rule Induction
Looking at measured data to find pattern:

**High Tc (>= 90 K) materials:**
- O6.080Ca1Cu2Ba2Hg1: 92.0 (5 elem, has Hg, has Ca)
- O6.100Ca1Cu2Ba2Hg1: 92.5 (5 elem, has Hg, has Ca)
- O6.130Ca1Cu2Ba2Hg1: 102.0 (5 elem, has Hg, has Ca)
- O6.140Ca1Cu2Ba2Hg1: 105.0 (5 elem, has Hg, has Ca)
- O6.190Ca1Cu2Ba2Hg1: 122.0 (5 elem, has Hg, has Ca)
- O6.210Ca1Cu2Ba2Hg1: 127.0 (5 elem, has Hg, has Ca)
- O6.220Ca1Cu2Ba2Hg1: 127.0 (5 elem, has Hg, has Ca)
- O6.240Ca1Cu2Ba2Hg1: 123.0 (5 elem, has Hg, has Ca)
- O6.300Ca1Cu2Ba2Hg1: 100.0 (5 elem, has Hg, has Ca)
- O6.310Ca1Cu2Ba2Hg1: 98.0 (5 elem, has Hg, has Ca)
- O4.081Cu1Ba2Hg1: 98.0 (4 elem, has Hg, no Ca)
- O4.162Cu1Ba2Hg1: 97.4 (4 elem, has Hg, no Ca)
- O4.150Cu1Ba2Hg1: 97.0 (4 elem, has Hg, no Ca)
- O4.220Cu1Ba2Hg1: 97.0 (4 elem, has Hg, no Ca)
- O4.096Cu1Ba2Hg1: 95.6 (4 elem, has Hg, no Ca)
- O4.097Cu1Ba2Hg1: 95.3 (4 elem, has Hg, no Ca)
- O4.200Cu1Ba2Hg1: 94.2 (4 elem, has Hg, no Ca)
- O4.101Cu1Ba2Hg1: 93.4 (4 elem, has Hg, no Ca)
- O4.103Cu1Ba2Hg1: 92.5 (4 elem, has Hg, no Ca)
- O4.085Cu1Ba2Hg1: 92.0 (4 elem, has Hg, no Ca)
- O10Ca2Cu3Ba2Tl2: 118.97 (5 elem, no Hg, has Ca, has Tl)

**Low Tc (< 90 K) materials:**
- O6Cu1Ba2Hg1Tl1: 0.0 (5 elem, has Hg, no Ca)
- O6Cu1Cd0.250Ba2Tl1.750: 88.0 (5 elem, no Hg, no Ca, has Cd)
- O7Cu3Ba2La1: 89.6 (4 elem, no Hg, no Ca, has La)
- O7Cu3Y0.200Ba2La0.800: 81.0 (5 elem, no Hg, no Ca, has Y, La)

Pattern analysis:
- All materials with Hg AND (has Ca OR is 4-element Cu-Ba-Hg-O type) are high
- O6Cu1Ba2Hg1Tl1 has Hg but Tc=0 - it has Tl and no Ca
- O10Ca2Cu3Ba2Tl2 has no Hg but is high - it has Ca and Tl

Looking more carefully:
- High Tc seems to require: (has Hg AND has Ca) OR (has Hg AND 4 elements with Cu/Ba) OR (has Ca AND has Tl)

Let me think differently. The warning says single-factor rules are wrong. Let me look at element combinations:

**High Tc patterns:**
- Has Hg + Has Ca: All high (92-127 K)
- Has Hg + No Ca + 4 elements (Cu, Ba, Hg, O): All high (92-98 K)
- Has Tl + Has Ca + No Hg: High (118.97 K for O10Ca2Cu3Ba2Tl2)

**Low Tc patterns:**
- Has Hg + No Ca + Has Tl: 0.0 K (O6Cu1Ba2Hg1Tl1)
- No Hg + No Ca: Low (81-89.6 K)
- Has Hg + No Ca + 5 elements: Need to check...

Wait, O6Cu1Ba2Hg1Tl1 has Hg but no Ca, and has 5 elements (O, Cu, Ba, Hg, Tl). It's 0.0 K.

So the rule might be:
**High if: (Has Hg AND Has Ca) OR (Has Hg AND exactly 4 elements AND no Tl) OR (Has Tl AND Has Ca AND no Hg)**

Actually simpler: **High if contains Hg AND (contains Ca OR has exactly 4 elements without Tl)**

Let me verify:
- O6.080Ca1Cu2Ba2Hg1: Hg + Ca → High ✓
- O4.081Cu1Ba2Hg1: Hg + 4 elem + no Tl → High ✓
- O6Cu1Ba2Hg1Tl1: Hg + no Ca + has Tl → Low ✓
- O10Ca2Cu3Ba2Tl2: No Hg, but has Ca + Tl → High (exception?)

Hmm, O10Ca2Cu3Ba2Tl2 doesn't fit the Hg rule. Let me reconsider.

Alternative rule: **High if (Has Hg AND NOT(Has Tl)) OR (Has Ca AND Has Tl)**

- O6.080Ca1Cu2Ba2Hg1: Hg, no Tl → High ✓
- O4.081Cu1Ba2Hg1: Hg, no Tl → High ✓
- O6Cu1Ba2Hg1Tl1: Hg, has Tl → Low ✓
- O10Ca2Cu3Ba2Tl2: No Hg, but Ca + Tl → High ✓
- O6Cu1Cd0.250Ba2Tl1.750: No Hg, no Ca → Low ✓
- O7Cu3Ba2La1: No Hg, no Ca → Low ✓
- O7Cu3Y0.200Ba2La0.800: No Hg, no Ca → Low ✓

This works! **Rule: High Tc if (contains Hg AND does NOT contain Tl) OR (contains both Ca AND Tl)**

## Step D — Final pick
Among predicted "high" unmeasurable, pick one with fewest elements.

Predicted High unmeasurable materials:
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Fewest elements = 4. Among the 4-element ones, tie-break by expected Tc:
- O4.160 is very close to O4.162 (97.4 K measured)
- O4.180 is between O4.162 (97.4) and O4.200 (94.2)
- O4.066 is near O4.081 (98.0) but data is noisy in that region
- O4.047 has no nearby measured point

**Final answer: O4.160Cu1Ba2Hg1** (closest to a well-measured high Tc value)

---

## Summary
- T* (peak Tc): 127.0 K from O6.210Ca1Cu2Ba2Hg1
- threshold1: 95.0 K
- Q1 (qualifying 4-element Hg materials >= 95 K): 6 materials
- Rule: High if (has Hg AND no Tl) OR (has Ca AND has Tl)
- Final: O4.160Cu1Ba2Hg1
