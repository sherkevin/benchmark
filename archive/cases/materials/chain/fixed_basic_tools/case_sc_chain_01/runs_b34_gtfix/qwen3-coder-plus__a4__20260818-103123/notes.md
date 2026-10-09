# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Measurement budget: 34 calls
- Definition: High Tc ≥ 90 K, Low Tc < 90 K

## Step A - Series Peak
Need to measure all compositions in measurable.txt that belong to the same homologous series as the seed (same elements, varying oxygen).

Series compositions from measurable.txt:
- O6.080Ca1Cu2Ba2Hg1 (seed): Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

Maximum Tc (T*) = 127.0 K
Peak composition = O6.210Ca1Cu2Ba2Hg1 (choosing one of the two with max Tc)

Step B calculations:
- Threshold1 = T* - 32 = 127.0 - 32 = 95.0 K

Materials containing Hg with exactly 4 elements:
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (≥ 95.0 K) ✓
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (≥ 95.0 K) ✓
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (≥ 95.0 K) ✓
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (≥ 95.0 K) ✓
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (≥ 95.0 K) ✓
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (≥ 95.0 K) ✓
- O4.047Cu1Ba2Hg1: Tc = 95.0 K (≥ 95.0 K) ✓
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95.0 K) ✗
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (< 95.0 K) ✗
- O4.103Cu1Ba2Hg1: Tc = 92.5 K (< 95.0 K) ✗
- O4.085Cu1Ba2Hg1: Tc = 92.0 K (< 95.0 K) ✗

Set Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1}

Other compositions with different element sets:
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (Low) - has 5 elements
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (High) - has 5 elements
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (Low) - has 5 elements
- O7Cu3Ba2La1: Tc = 89.6 K (Low) - has 5 elements
- O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K (Low) - has 5 elements

All measured compositions with their Tc values and classification:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (Low)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K (Low)
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K (High)
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K (High)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K (High)
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (High)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (High)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K (High)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K (High)
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K (High)
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (High)
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (High)
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (High)
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (High)
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (High)
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (High)
- O4.047Cu1Ba2Hg1: Tc = 95.0 K (High)
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (High)
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (High)
- O4.103Cu1Ba2Hg1: Tc = 92.5 K (High)
- O4.085Cu1Ba2Hg1: Tc = 92.0 K (High)
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K (Low)
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K (High)
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K (Low)
- O7Cu3Ba2La1: Tc = 89.6 K (Low)
- O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K (Low)

Analyzing for a rule to separate high from low Tc materials:

Looking at the factors:
- Oxygen content: Higher oxygen content seems to correlate with higher Tc in some series (like O6.xCa1Cu2Ba2Hg1 series)
- Number of elements: Materials with more elements tend to have lower Tc (except for O10Ca2Cu3Ba2Tl2 which is high despite having 5 elements)
- Specific elements: Hg seems to be associated with higher Tc, but not always (O6Cu1Ba2Hg1Tl1 has Tc=0.0)
- Element ratios: Maybe specific ratios of certain elements matter

Let me examine more systematically:
High Tc materials (> 90 K):
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K
- O4.081Cu1Ba2Hg1: Tc = 98.0 K
- O4.162Cu1Ba2Hg1: Tc = 97.4 K
- O4.150Cu1Ba2Hg1: Tc = 97.0 K
- O4.220Cu1Ba2Hg1: Tc = 97.0 K
- O4.096Cu1Ba2Hg1: Tc = 95.6 K
- O4.097Cu1Ba2Hg1: Tc = 95.3 K
- O4.047Cu1Ba2Hg1: Tc = 95.0 K
- O4.200Cu1Ba2Hg1: Tc = 94.2 K
- O4.101Cu1Ba2Hg1: Tc = 93.4 K
- O4.103Cu1Ba2Hg1: Tc = 92.5 K
- O4.085Cu1Ba2Hg1: Tc = 92.0 K
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K

Low Tc materials (≤ 90 K):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (borderline - actually > 90, so High)
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K (Wait, 92.0 is > 90, so High!)
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K
- O7Cu3Ba2La1: Tc = 89.6 K
- O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K

Actually, correcting:
High Tc materials (≥ 90 K):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K
- O4.081Cu1Ba2Hg1: Tc = 98.0 K
- O4.162Cu1Ba2Hg1: Tc = 97.4 K
- O4.150Cu1Ba2Hg1: Tc = 97.0 K
- O4.220Cu1Ba2Hg1: Tc = 97.0 K
- O4.096Cu1Ba2Hg1: Tc = 95.6 K
- O4.097Cu1Ba2Hg1: Tc = 95.3 K
- O4.047Cu1Ba2Hg1: Tc = 95.0 K
- O4.200Cu1Ba2Hg1: Tc = 94.2 K
- O4.101Cu1Ba2Hg1: Tc = 93.4 K
- O4.103Cu1Ba2Hg1: Tc = 92.5 K
- O4.085Cu1Ba2Hg1: Tc = 92.0 K
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K

Low Tc materials (< 90 K):
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K
- O7Cu3Ba2La1: Tc = 89.6 K
- O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K

Looking for patterns:

Looking for a rule that separates high from low Tc:
- O6Cu1Ba2Hg1Tl1 (Low) vs others with Hg that are High: This one has Tl
- O6Cu1Cd0.250Ba2Tl1.750 (Low) has Cd and Tl
- O7Cu3Ba2La1 (Low) has La
- O7Cu3Y0.200Ba2La0.800 (Low) has Y and La

Maybe the rule is: Contains Hg AND does not contain La, Cd, or Tl? Let me check:
- O6Cu1Ba2Hg1Tl1: Has Hg, has Tl → Low (matches rule prediction)
- O6Cu1Cd0.250Ba2Tl1.750: No Hg → not applicable to this potential rule
- O7Cu3Ba2La1: No Hg → not applicable to this potential rule  
- O7Cu3Y0.200Ba2La0.800: No Hg → not applicable to this potential rule

Actually, let me reconsider:
- O6Cu1Cd0.250Ba2Tl1.750: Has Tl and Cd, no Hg → Low
- O7Cu3Ba2La1: Has La, no Hg → Low  
- O7Cu3Y0.200Ba2La0.800: Has Y and La, no Hg → Low

Looking again:
High Tc materials (≥ 90 K) all have Hg OR Ca:
- O6.080Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.100Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.130Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.140Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.190Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.210Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.220Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.240Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.300Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O6.310Ca1Cu2Ba2Hg1: Has both Ca and Hg
- O4.081Cu1Ba2Hg1: Has Hg
- O4.162Cu1Ba2Hg1: Has Hg
- O4.150Cu1Ba2Hg1: Has Hg
- O4.220Cu1Ba2Hg1: Has Hg
- O4.096Cu1Ba2Hg1: Has Hg
- O4.097Cu1Ba2Hg1: Has Hg
- O4.047Cu1Ba2Hg1: Has Hg
- O4.200Cu1Ba2Hg1: Has Hg
- O4.101Cu1Ba2Hg1: Has Hg
- O4.103Cu1Ba2Hg1: Has Hg
- O4.085Cu1Ba2Hg1: Has Hg
- O10Ca2Cu3Ba2Tl2: Has Ca, no Hg

Low Tc materials (< 90 K):
- O6Cu1Ba2Hg1Tl1: Has Hg and Tl
- O6Cu1Cd0.250Ba2Tl1.750: No Hg, has Cd and Tl
- O7Cu3Ba2La1: No Hg, has La
- O7Cu3Y0.200Ba2La0.800: No Hg, has Y and La

Interesting! So the only high Tc material without Hg is O10Ca2Cu3Ba2Tl2 which has Ca.
The only low Tc material with Hg is O6Cu1Ba2Hg1Tl1 which also has Tl.

So maybe the rule is: 
- Has Hg AND does not have Tl, OR
- Has Ca AND does not have La, Cd, Y

Let me refine: Looking at it differently:
- High Tc materials: All contain either Hg or Ca (or both)
- Low Tc materials: Don't contain Hg or Ca, OR contain problematic elements

Actually, let me try another approach:
- High Tc: O6.080Ca1Cu2Ba2Hg1 through O6.310Ca1Cu2Ba2Hg1 (all have both Ca and Hg)
- High Tc: O4.081Cu1Ba2Hg1 through O4.085Cu1Ba2Hg1 (all have Hg, no Ca)
- High Tc: O10Ca2Cu3Ba2Tl2 (has Ca, no Hg, but has Tl)
- Low Tc: O6Cu1Ba2Hg1Tl1 (has both Hg and Tg - this breaks a simple "has Hg" rule)
- Low Tc: O6Cu1Cd0.250Ba2Tl1.750 (no Hg, has Cd and Tl)
- Low Tc: O7Cu3Ba2La1 (no Hg, has La)
- Low Tc: O7Cu3Y0.200Ba2La0.800 (no Hg, has Y and La)

It seems like the combination of Hg with Tl results in low Tc.
Also, absence of both Hg and Ca with presence of La, Y, or Cd seems to result in low Tc.

Rule hypothesis: A material is HIGH Tc IF:
- It contains Hg AND does not contain Tl, OR
- It contains Ca AND does not contain both La and (Cd or Y)

Let me test this:
- O6Cu1Ba2Hg1Tl1: Has Hg but also has Tl → Low ✓ (doesn't meet first condition)
- O6Cu1Cd0.250Ba2Tl1.750: No Hg, no Ca → Low ✓ (doesn't meet either condition)
- O7Cu3Ba2La1: No Hg, no Ca → Low ✓ (doesn't meet either condition)
- O7Cu3Y0.200Ba2La0.800: No Hg, no Ca → Low ✓ (doesn't meet either condition)
- O10Ca2Cu3Ba2Tl2: Has Ca, has Tl, La, no Y → Meets second condition (has Ca, doesn't have both La and (Cd or Y)) because it doesn't have Cd. ✓

Wait, let me reconsider O10Ca2Cu3Ba2Tl2: Has Ca, has Tl and Tl2, has Ba, Cu. This is High Tc.
This doesn't fit my rule since it doesn't have La, Cd, or Y, but it has Ca, so maybe the second condition is just "Has Ca and no La or Cd or Y"?

Actually, let me look again at O10Ca2Cu3Ba2Tl2: It contains Ca, Ba, Cu, Tl. It has Tl but still has high Tc. So Tl isn't always bad if Ca is present.

Let me look more systematically:
Elements in high Tc materials: Ca, Cu, Ba, Hg, Tl (in O10Ca2Cu3Ba2Tl2)
Elements in low Tc materials: Cu, Ba, Hg, Tl (in O6Cu1Ba2Hg1Tl1), Cd, Tl, La (in others)

Wait, I see that O6Cu1Ba2Hg1Tl1 has Hg and Tl together and is low Tc, but O10Ca2Cu3Ba2Tl2 has Ca and Tl together and is high Tc.

So maybe the rule is:
- Contains Hg and NOT Tl → High
- Contains Ca and NOT La, Cd, Y → High
- Everything else → Low

Testing:
- O6Cu1Ba2Hg1Tl1: Has Hg but also has Tl → NOT (Hg and NOT Tl); No Ca → Low ✓
- O6Cu1Cd0.250Ba2Tl1.750: No Hg; No Ca → Low ✓
- O7Cu3Ba2La1: No Hg; No Ca → Low ✓
- O7Cu3Y0.200Ba2La0.800: No Hg; No Ca → Low ✓
- O10Ca2Cu3Ba2Tl2: No Hg; Has Ca, but has Tl (not La, Cd, Y) → IS (Ca and NOT La,Cd,Y) → High ✓
- All high Tc materials: They either have (Hg and NOT Tl) or (Ca and NOT La,Cd,Y)

This looks promising!

For the unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1: Has both Ca and Hg, no Tl → Has (Hg and NOT Tl) → High
2. O6.270Ca1Cu2Ba2Hg1: Has both Ca and Hg, no Tl → Has (Hg and NOT Tl) → High
3. O4.180Cu1Ba2Hg1: Has Hg, no Tl → Has (Hg and NOT Tl) → High
4. O8Ca0.850Cu2.200Ba2Tl1.800: Has Ca, has Tl (but not La, Cd, Y) → Has (Ca and NOT La,Cd,Y) → High
5. O7Ca1Cu2Sr2Tl0.500Pb0.500: Has Ca, has Tl (but not La, Cd, Y) → Has (Ca and NOT La,Cd,Y) → High
6. O8Cu3Y1Ba1.900La0.100: No Hg; No Tl but has Y and La → No (Hg and NOT Tl), No (Ca and NOT La,Cd,Y) → Low
7. O8Cu4Y1Ba1.870La0.130: No Hg; No Tl but has Y and La → No (Hg and NOT Tl), No (Ca and NOT La,Cd,Y) → Low