# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90K, low Tc < 90K
- Budget: 45 units total
- Tools: measure_tc() and search_db()

## Step A: Series Peak
The seed belongs to a homologous doping series (same elements, varying oxygen).
Need to find series members and identify the composition with MAXIMUM Tc.

First, let me measure the seed material to establish a baseline:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K

Now I need to find other members of this series with different oxygen values:

From my search, I found these materials that belong to the same series (Ca, Cu, Ba, Hg):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (already measured)
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

Let me measure some of these to find the peak Tc:

Results:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K
- O6.160Ca1Cu2Ba2Hg1: No sample available (not synthesized)
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K
- O6.270Ca1Cu2Ba2Hg1: No sample available (not synthesized)
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K

Step A: The composition with MAXIMUM Tc is O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1 with Tc = 127.0K
So T* = 127.0K

## Step B: Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0

Materials containing Hg with exactly 4 elements that have Tc >= 95.0 (Q1):
- O4.081Cu1Ba2Hg1: Tc = 98.0K
- O4.096Cu1Ba2Hg1: Tc = 95.6K
- O4.097Cu1Ba2Hg1: Tc = 95.3K
- O4.150Cu1Ba2Hg1: Tc = 97.0K
- O4.162Cu1Ba2Hg1: Tc = 97.4K
- O4.220Cu1Ba2Hg1: Tc = 97.0K

So threshold1 = 95.0, and Q1 = [O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1]

## Step C: Extrapolate (no measurement possible)
Some materials in the database have never been synthesized — measure_tc returns "no sample exists" for them.
I need to induce a rule from measurable materials, then predict high/low for every unmeasurable one I find.

Materials I found that have no samples available:
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available
- O4.160Cu1Ba2Hg1: No sample available
- O4.180Cu1Ba2Hg1: No sample available

Now I need to analyze the measured materials to derive a rule for predicting high/low Tc based on composition.

Measured materials and their Tc values:
1. O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K (Low)
2. O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K (Low)
3. O6.130Ca1Cu2Ba2Hg1: Tc = 102.0K (High)
4. O6.140Ca1Cu2Ba2Hg1: Tc = 105.0K (High)
5. O6.190Ca1Cu2Ba2Hg1: Tc = 122.0K (High)
6. O6.210Ca1Cu2Ba2Hg1: Tc = 127.0K (High)
7. O6.220Ca1Cu2Ba2Hg1: Tc = 127.0K (High)
8. O6.240Ca1Cu2Ba2Hg1: Tc = 123.0K (High)
9. O6.300Ca1Cu2Ba2Hg1: Tc = 100.0K (High)
10. O6.310Ca1Cu2Ba2Hg1: Tc = 98.0K (High)
11. O4.081Cu1Ba2Hg1: Tc = 98.0K (High)
12. O4.085Cu1Ba2Hg1: Tc = 92.0K (Low)
13. O4.096Cu1Ba2Hg1: Tc = 95.6K (High)
14. O4.097Cu1Ba2Hg1: Tc = 95.3K (High)
15. O4.101Cu1Ba2Hg1: Tc = 93.4K (Low)
16. O4.103Cu1Ba2Hg1: Tc = 92.5K (Low)
17. O4.150Cu1Ba2Hg1: Tc = 97.0K (High)
18. O4.162Cu1Ba2Hg1: Tc = 97.4K (High)
19. O4.200Cu1Ba2Hg1: Tc = 94.2K (Low)
20. O4.220Cu1Ba2Hg1: Tc = 97.0K (High)

Looking at the patterns:
- Materials with the formula OxCa1Cu2Ba2Hg1 (6 elements): Higher oxygen content (around 6.19-6.24) tends to have very high Tc (122-127K), while lower oxygen content (6.08-6.14) has moderate Tc (92-105K)
- Materials with the formula Ox(Cu1Ba2Hg1) (4 elements): Tc seems to peak around certain oxygen values (around 4.08-4.10 and 4.15-4.16, and 4.22)

Rule derivation:
Looking at the 4-element compounds (Ox(Cu1Ba2Hg1)), Tc seems to depend on the oxygen content. High Tc (>90K) occurs roughly in ranges of 4.08-4.10 and 4.15-4.22.

For 6-element compounds (OxCa1Cu2Ba2Hg1), Tc appears to peak around oxygen content of 6.19-6.24, and materials with oxygen around 6.13-6.31 tend to have high Tc values.

However, looking more closely:
- In the Ca series (6 elements), oxygen content between ~6.13-6.31 gives high Tc
- In the simple Hg series (4 elements), oxygen content between ~4.08-4.10 and 4.15-4.22 gives high Tc

Actually, let me look at this differently. Looking at oxygen content:
- Low Tc (≤90K): None in our dataset
- Low Tc (90-95K): O4.085, O4.101, O4.103, O4.200, O6.080, O6.100
- High Tc (>95K): O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220

Wait, I notice that I classified incorrectly. The definition is: high if Tc >= 90K, low if Tc < 90K.
So everything except possibly some values around 90K is high Tc.

Actually re-examining:
- Low Tc (<90K): None in our measured dataset
- All measured samples have Tc >= 92K

Wait, let me reread the definition: "a material is **high** if Tc >= 90 K, otherwise **low**."

Actually, looking again at my data:
- O4.101Cu1Ba2Hg1: Tc = 93.4K -> HIGH (>= 90K)
- O4.103Cu1Ba2Hg1: Tc = 92.5K -> HIGH (>= 90K)
- O4.200Cu1Ba2Hg1: Tc = 94.2K -> HIGH (>= 90K)
- O4.085Cu1Ba2Hg1: Tc = 92.0K -> HIGH (>= 90K)
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0K -> HIGH (>= 90K)
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5K -> HIGH (>= 90K)

Actually, all measured materials in my dataset are HIGH Tc (>= 90K). So the rule might be complex.

Let me look at what makes Tc higher vs lower among the high Tc materials since all are technically "high":
- In 4-element compounds: Tc ranges from 92.0K to 98.0K
- In 6-element compounds: Tc ranges from 92.0K to 127.0K

The key insight might be looking at the difference between synthesized and non-synthesized materials. Let me focus on finding a rule that could distinguish between high and low for the unsynthesized materials.

Actually, wait. Maybe not all materials are high Tc. Let me go back and look for any materials with Tc < 90K by searching more broadly. But I've used up my budget, so I need to work with what I have.

Since all my measured samples are high Tc, I'll need to make predictions based on the pattern I observe. Looking at the trend:
- In the 6-element series (OxCa1Cu2Ba2Hg1), Tc peaks around O6.19-6.24 with values >120K
- In the 4-element series (Ox(Cu1Ba2Hg1)), Tc varies but generally stays above 92K

For the unsynthesized materials, I'll make predictions based on similarity to synthesized materials:
- O6.160Ca1Cu2Ba2Hg1: Between O6.140(105K) and O6.190(122K) → likely HIGH
- O6.270Ca1Cu2Ba2Hg1: Between O6.240(123K) and O6.300(100K) → likely HIGH
- O4.047Cu1Ba2Hg1: Lower oxygen than O4.081(98K) → maybe LOW
- O4.066Cu1Ba2Hg1: Lower oxygen than O4.081(98K) → maybe LOW
- O4.160Cu1Ba2Hg1: Between O4.150(97K) and O4.162(97.4K) → likely HIGH
- O4.180Cu1Ba2Hg1: Between O4.162(97.4K) and O4.200(94.2K) → likely HIGH

Actually, I should note that O4.200Cu1Ba2Hg1 has Tc=94.2K (still high), so O4.180 might also be high.

Refined predictions:
- O6.160Ca1Cu2Ba2Hg1: HIGH (interpolated between known high values)
- O6.270Ca1Cu2Ba2Hg1: HIGH (interpolated between known high values) 
- O4.047Cu1Ba2Hg1: LOW (lower oxygen content than the lowest oxygen material with measurement)
- O4.066Cu1Ba2Hg1: LOW (lower oxygen content than measured materials)
- O4.160Cu1Ba2Hg1: HIGH (between O4.150 and O4.162 which are both high)
- O4.180Cu1Ba2Hg1: HIGH (between high Tc materials)

Rule: Oxygen content seems to correlate with Tc, with intermediate oxygen levels giving highest Tc in both series. Very low oxygen (like O4.047, O4.066) likely gives lower Tc.

## Step D: Final pick
Among unmeasurable materials I predicted as high, choose the one with the fewest elements (break ties by higher expected Tc).

Unmeasurable materials predicted as HIGH:
- O6.160Ca1Cu2Ba2Hg1: 6 elements
- O6.270Ca1Cu2Ba2Hg1: 6 elements  
- O4.160Cu1Ba2Hg1: 4 elements
- O4.180Cu1Ba2Hg1: 4 elements

The ones with the fewest elements (4) are O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1.
Between these two, I expect both to have high Tc based on nearby measured values. 
O4.160 is between O4.150(Tc=97.0K) and O4.162(Tc=97.4K), so likely ~97K
O4.180 is between O4.162(Tc=97.4K) and O4.200(Tc=94.2K), so likely ~96K

So O4.160Cu1Ba2Hg1 has fewer elements (4) and higher expected Tc, making it the choice for step D.