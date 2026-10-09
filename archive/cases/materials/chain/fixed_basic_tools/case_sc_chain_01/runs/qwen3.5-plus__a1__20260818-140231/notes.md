# Notes for Superconductor Tc Study

## Task Summary
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low Tc: < 90 K
- Budget: 18 measure_tc calls (EXHAUSTED)

## Step A — Series Peak (COMPLETE)
Measured O6.xxxCa1Cu2Ba2Hg1 series:
- O6.080: 92.0 K
- O6.100: 92.5 K
- O6.130: 102.0 K
- O6.140: 105.0 K
- O6.190: 122.0 K
- O6.210: 127.0 K ← **T* (PEAK)**
- O6.220: 127.0 K
- O6.240: 123.0 K
- O6.300: 100.0 K
- O6.310: 98.0 K

**T* = 127.0 K**
**threshold1 = 127.0 - 32 = 95.0 K**

## Step B — Filter Family B (Hg + 4 elements, Tc >= 95.0)
Measured O4.xxxCu1Ba2Hg1 series:
- O4.047: 95.0 ✓ (>= 95)
- O4.081: 98.0 ✓
- O4.096: 95.6 ✓
- O4.097: 95.3 ✓
- O4.101: NOT MEASURED
- O4.103: NOT MEASURED
- O4.085: NOT MEASURED
- O4.150: 97.0 ✓
- O4.162: 97.4 ✓
- O4.200: 94.2 ✗ (< 95)
- O4.220: 97.0 ✓

**Q1 (confirmed):** O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1

## Step C — Predict Unmeasurable Materials
Unmeasurable compositions:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
3. O4.180Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb)
6. O8Cu3Y1Ba1.900La0.100 (5 elements: O, Cu, Y, Ba, La)
7. O8Cu4Y1Ba1.870La0.130 (5 elements: O, Cu, Y, Ba, La)

### Analysis of measured data to find rule:

**O6.xxxCa1Cu2Ba2Hg1 series (5 elements with Hg):**
All measured values: 92.0 to 127.0 K - ALL HIGH (>= 90)

**O4.xxxCu1Ba2Hg1 series (4 elements with Hg):**
- O4.047: 95.0 HIGH
- O4.081: 98.0 HIGH
- O4.096: 95.6 HIGH
- O4.097: 95.3 HIGH
- O4.150: 97.0 HIGH
- O4.162: 97.4 HIGH
- O4.200: 94.2 LOW
- O4.220: 97.0 HIGH

Most are HIGH, one is LOW (O4.200 at 94.2)

**Other measured materials (not measured yet - need to analyze):**
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

Wait - I didn't measure these! I used all 18 calls on the two series. Let me reconsider.

Actually I need to induce a rule from what I DO know. The pattern seems related to:
- Presence of Hg
- Number of elements
- Oxygen content relative to other elements

Looking at high Tc materials:
- O6.xxxCa1Cu2Ba2Hg1: All HIGH (92-127 K) - has Hg, 5 elements
- O4.xxxCu1Ba2Hg1: Mostly HIGH (95-98 K), one LOW (94.2) - has Hg, 4 elements

The warning says single-factor rules like "contains Hg" or "has 5 elements" are wrong.

Let me think about what interaction of factors could matter...

Looking at the unmeasurable list and comparing to measured:
- O6.160Ca1Cu2Ba2Hg1: Same formula pattern as O6.xxx series → likely HIGH
- O6.270Ca1Cu2Ba2Hg1: Same formula pattern as O6.xxx series → likely HIGH
- O4.180Cu1Ba2Hg1: Similar to O4.xxx series, 0.180 is between 0.162 (97.4) and 0.200 (94.2) → could be borderline
- O8Ca0.850Cu2.200Ba2Tl1.800: Has Tl, no Hg, 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements, has Tl and Pb, no Hg
- O8Cu3Y1Ba1.900La0.100: Has Y and La, no Hg, 5 elements
- O8Cu4Y1Ba1.870La0.130: Has Y and La, no Hg, 5 elements

Key observation: All materials I measured that have Hg are HIGH (except O4.200 at 94.2 which is just below 95 but still >= 90 so HIGH by task definition).

Wait - the task definition says HIGH is Tc >= 90 K. O4.200 at 94.2 is still HIGH (94.2 >= 90)!

So ALL measured materials with Hg are HIGH (>= 90 K).

For materials WITHOUT Hg, I haven't measured any. But the warning says "contains Hg" alone is wrong for some unmeasurable materials.

Let me think differently. The unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 - has Hg → likely HIGH
2. O6.270Ca1Cu2Ba2Hg1 - has Hg → likely HIGH
3. O4.180Cu1Ba2Hg1 - has Hg → likely HIGH (O4.200 was 94.2, O4.162 was 97.4, so 0.180 might be around 95-96)
4. O8Ca0.850Cu2.200Ba2Tl1.800 - no Hg, has Tl
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - no Hg, has Tl and Pb
6. O8Cu3Y1Ba1.900La0.100 - no Hg, has Y and La (rare earth)
7. O8Cu4Y1Ba1.870La0.130 - no Hg, has Y and La (rare earth)

The warning says the rule is an "interaction of multiple factors". 

Possible rule: Materials with Hg AND certain element count or oxygen ratio are HIGH.

Or: Materials containing BOTH Hg AND Ba are HIGH? Or Hg + Cu + Ba?

Looking at what makes something HIGH:
- All my measured samples have Hg, Cu, Ba
- The O6.xxx series (with Ca) has higher Tc (92-127)
- The O4.xxx series (without Ca) has lower but still high Tc (94-98)

For unmeasurable without Hg:
- O8Ca0.850Cu2.200Ba2Tl1.800: Has Ca, Cu, Ba, Tl (similar to O6.xxx but with Tl instead of Hg)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Has Ca, Cu, Sr, Tl, Pb (different - has Sr instead of Ba)
- O8Cu3Y1Ba1.900La0.100: Has Cu, Y, Ba, La (Y and La are rare earths)
- O8Cu4Y1Ba1.870La0.130: Has Cu, Y, Ba, La

Hypothesis: HIGH if contains Hg OR (contains both Ba AND a rare earth like Y/La)?

Actually, let me look at element counts and compositions more carefully:

Materials with known HIGH Tc all have: Hg + Ba + Cu
- O6.xxxCa1Cu2Ba2Hg1: 5 elements, Hg present → HIGH
- O4.xxxCu1Ba2Hg1: 4 elements, Hg present → mostly HIGH (all >= 90)

For materials without Hg, I need to predict based on other factors.

Another angle: Maybe the rule is about specific element combinations that enable high-Tc superconductivity.

Looking at known high-Tc superconductor families:
- Hg-based cuprates (like what I measured) are known high-Tc
- Tl-based cuprates can also be high-Tc
- YBa2Cu3O7 (YBCO) is a famous high-Tc superconductor (~92 K)

So O8Cu3Y1Ba1.900La0.100 and O8Cu4Y1Ba1.870La0.130 are similar to YBCO - they have Y, Ba, Cu. These might be HIGH.

O8Ca0.850Cu2.200Ba2Tl1.800 has Tl which is also known for high-Tc compounds.

O7Ca1Cu2Sr2Tl0.500Pb0.500 has Sr instead of Ba - this might be different.

Let me try a rule based on known superconductor chemistry:
**HIGH if: (contains Hg) OR (contains Y AND Ba AND Cu) OR (contains Tl AND Ba AND Cu)**

Testing on unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 - has Hg → HIGH ✓
2. O6.270Ca1Cu2Ba2Hg1 - has Hg → HIGH ✓
3. O4.180Cu1Ba2Hg1 - has Hg → HIGH ✓
4. O8Ca0.850Cu2.200Ba2Tl1.800 - has Tl, Ba, Cu → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - has Tl but NO Ba (has Sr instead) → LOW
6. O8Cu3Y1Ba1.900La0.100 - has Y, Ba, Cu → HIGH
7. O8Cu4Y1Ba1.870La0.130 - has Y, Ba, Cu → HIGH

But wait, the warning says single-factor rules are wrong. My rule has multiple factors (element combinations).

Let me reconsider. Maybe I need to look at this differently.

Actually, I realize I should look at the data more systematically. Let me think about what distinguishes HIGH from LOW among measured samples.

ALL measured samples are HIGH (>= 90 K):
- O6.xxx series: 92-127 K (all >= 90)
- O4.xxx series: 94.2-98 K (all >= 90, even O4.200 at 94.2)

So all measured materials with Hg + Cu + Ba are HIGH.

For unmeasurable without Hg, I need to predict. The key insight might be:
- Hg-containing cuprates with Ba are HIGH
- Tl-containing cuprates with Ba might be HIGH
- Y-containing cuprates with Ba (YBCO-like) are HIGH
- Materials without the right combination (like Sr instead of Ba) might be LOW

Rule: **HIGH if contains (Hg OR Tl OR Y) AND contains Ba AND contains Cu**

Testing:
1. O6.160Ca1Cu2Ba2Hg1 - Hg + Ba + Cu → HIGH
2. O6.270Ca1Cu2Ba2Hg1 - Hg + Ba + Cu → HIGH
3. O4.180Cu1Ba2Hg1 - Hg + Ba + Cu → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 - Tl + Ba + Cu → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - Tl but Sr not Ba → LOW
6. O8Cu3Y1Ba1.900La0.100 - Y + Ba + Cu → HIGH
7. O8Cu4Y1Ba1.870La0.130 - Y + Ba + Cu → HIGH

This gives 6 HIGH, 1 LOW.

## Step D — Final Pick
Among predicted HIGH unmeasurable, pick one with FEWEST elements.

HIGH predictions:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements (O, Ca, Cu, Ba, Hg)
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements
3. O4.180Cu1Ba2Hg1 - 4 elements (O, Cu, Ba, Hg) ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements
5. O8Cu3Y1Ba1.900La0.100 - 5 elements
6. O8Cu4Y1Ba1.870La0.130 - 5 elements

O4.180Cu1Ba2Hg1 has only 4 elements - that's the fewest!

**Final answer: O4.180Cu1Ba2Hg1**
