# Superconductor Research Notes

Starting date: 2026-08-17

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 45 units (each measurement and each database page costs 1 unit)
- High material: Tc >= 90 K
- Low material: Tc < 90 K

## Step A - Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to find and measure the series members to identify the composition with MAXIMUM Tc (= T*).

Found series members (same elements Ca, Cu, Ba, Hg, varying oxygen):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: No sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX!)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX!)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: No sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K

So T* = 127.0 K, achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B - Filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Among materials that contain Hg and have exactly 4 elements, report ALL whose Tc >= threshold1 (95.0 K).
Call this set Q1.

From searching Hg-containing materials, I identified these with exactly 4 elements (O, Cu, Ba, Hg):
Materials with 4 elements (O, Cu, Ba, Hg) that were measurable:
- O4.047Cu1Ba2Hg1: No sample available
- O4.066Cu1Ba2Hg1: No sample available
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (>= 95.0 K) ✓
- O4.085Cu1Ba2Hg1: Tc = 92.0 K (< 95.0 K)
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (>= 95.0 K) ✓
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (>= 95.0 K) ✓
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (< 95.0 K)
- O4.103Cu1Ba2Hg1: Tc = 92.5 K (< 95.0 K)
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0 K) ✓
- O4.160Cu1Ba2Hg1: No sample available
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (>= 95.0 K) ✓
- O4.180Cu1Ba2Hg1: No sample available
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (< 95.0 K)
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (>= 95.0 K) ✓

So Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1}

## Step C - Extrapolate (no measurement possible)
Some materials in the database have never been synthesized. I need to induce a rule from measurable materials, then predict high/low for every unmeasurable one.

Unmeasurable materials I found:
- O6.160Ca1Cu2Ba2Hg1 (from the Ca series)
- O6.270Ca1Cu2Ba2Hg1 (from the Ca series)
- O4.047Cu1Ba2Hg1 (from the 4-element series)
- O4.066Cu1Ba2Hg1 (from the 4-element series)
- O4.160Cu1Ba2Hg1 (from the 4-element series)
- O4.180Cu1Ba2Hg1 (from the 4-element series)

Let me analyze measurable materials to find a pattern:

Looking at the Ca-Cu-Ba-Hg series (varying oxygen):
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

Looking at the O-Cu-Ba-Hg series (varying oxygen):
- O4.081Cu1Ba2Hg1: Tc = 98.0 K (High)
- O4.085Cu1Ba2Hg1: Tc = 92.0 K (Low)
- O4.096Cu1Ba2Hg1: Tc = 95.6 K (High)
- O4.097Cu1Ba2Hg1: Tc = 95.3 K (High)
- O4.101Cu1Ba2Hg1: Tc = 93.4 K (Low)
- O4.103Cu1Ba2Hg1: Tc = 92.5 K (Low)
- O4.150Cu1Ba2Hg1: Tc = 97.0 K (High)
- O4.162Cu1Ba2Hg1: Tc = 97.4 K (High)
- O4.200Cu1Ba2Hg1: Tc = 94.2 K (Low)
- O4.220Cu1Ba2Hg1: Tc = 97.0 K (High)

I notice that in both series, there appears to be an optimal oxygen range where Tc peaks.
For the Ca series, peak Tc occurs around O6.21-O6.22, then drops again.
For the O-Cu-Ba-Hg series, higher Tc values seem to occur in the middle ranges (around O4.08-O4.16).

Looking more closely:
In the Ca series (O6.x range), materials with oxygen content around 6.19-6.24 have high Tc.
In the Cu-Ba-Hg series (O4.x range), materials with oxygen content around 4.08-4.10 and 4.15-4.16 have higher Tc.

A potential rule could be based on oxygen content relative to optimal ranges:
- For Ca-Cu-Ba-Hg series: O ~6.19-6.24 is optimal
- For Cu-Ba-Hg series: O ~4.08-4.10 and ~4.15-4.16 seems better

However, since O6.160 is close to the optimal range for the Ca series but lower than peak, and O6.270 is past the peak but still in a higher range than initial values, I'd predict:
- O6.160Ca1Cu2Ba2Hg1: High (in the upper half of the range where most values are high)
- O6.270Ca1Cu2Ba2Hg1: High (though past the peak, still in high Tc region)

For the O4.x series, since O4.047 and O4.066 are lower than the first high Tc value (O4.081), and O4.160, O4.180 are in the range where we had mixed results:
- O4.047Cu1Ba2Hg1: Low (lower than the first high Tc value)
- O4.066Cu1Ba2Hg1: Low (lower than the first high Tc value)
- O4.160Cu1Ba2Hg1: High (based on O4.162 being high)
- O4.180Cu1Ba2Hg1: Low (higher than the high Tc range)

Wait, let me reconsider. Looking at the actual pattern in the O4.x series more carefully:
- O4.081: 98.0K (High) - highest in low range
- O4.085: 92.0K (Low)
- O4.096: 95.6K (High)
- O4.097: 95.3K (High)
- O4.101: 93.4K (Low)
- O4.103: 93.4K (Low)
- O4.150: 97.0K (High)
- O4.162: 97.4K (High)
- O4.200: 94.2K (Low)
- O4.220: 97.0K (High)

Actually, looking at this again, it seems like there might be multiple peaks or a complex relationship.
Let me try to think of another pattern...

Maybe it's related to the number of oxygen atoms or some formula. Actually, let me try a simple interpolation approach:
For O6.160Ca1Cu2Ba2Hg1: Between O6.140 (105.0K) and O6.190 (122.0K), so probably high.
For O6.270Ca1Cu2Ba2Hg1: Between O6.240 (123.0K) and O6.300 (100.0K), so probably high (closer to the 123K side).

For O4.047Cu1Ba2Hg1: Lower than O4.081 (98.0K), possibly decreasing trend as we move away from optimal, likely low.
For O4.066Cu1Ba2Hg1: Lower than O4.081 (98.0K), likely low.
For O4.160Cu1Ba2Hg1: Close to O4.162 (97.4K), likely high.
For O4.180Cu1Ba2Hg1: Between O4.162 (97.4K) and O4.200 (94.2K), likely high (closer to the high side).

Actually, let me try to be more systematic. Let me look for a rule based on the oxygen coefficient:

Rule: Based on interpolation between known values and the observation that Tc tends to be higher in certain oxygen ranges:

For Ca-Cu-Ba-Hg series:
- O6.160 is between O6.140(105K) and O6.190(122K) → likely high
- O6.270 is between O6.240(123K) and O6.300(100K) → likely high (closer to 123K)

For Cu-Ba-Hg series:
- O4.047 is less than O4.081(98K) → likely low
- O4.066 is less than O4.081(98K) → likely low  
- O4.160 is close to O4.162(97.4K) → likely high
- O4.180 is between O4.162(97.4K) and O4.200(94.2K) → likely high (closer to high side)

## Step D - Final pick
Among unmeasurable materials predicted to be high, choose the one with the fewest elements (break ties by higher expected Tc).

Unmeasurable materials predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1: 5 elements (predicted high) 
- O6.270Ca1Cu2Ba2Hg1: 5 elements (predicted high)
- O4.160Cu1Ba2Hg1: 4 elements (predicted high)
- O4.180Cu1Ba2Hg1: 4 elements (predicted high)

The ones with the fewest elements (4) are O4.160Cu1Ba2Hg1 and O4.180Cu1Ba2Hg1.
Between these, I expect O4.160 to have higher Tc (similar to O4.162 at 97.4K) vs O4.180 which I predicted high but closer to a lower Tc value.

So the final pick would be O4.160Cu1Ba2Hg1.