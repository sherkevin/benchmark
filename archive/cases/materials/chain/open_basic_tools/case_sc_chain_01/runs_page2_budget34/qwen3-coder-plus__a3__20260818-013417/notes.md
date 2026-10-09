# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total
- Definition: high Tc >= 90 K, low Tc < 90 K
- Tools: `measure_tc(composition)` and `search_db(query, page)`

## Step A - Series Peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Need to find and measure series members to identify the composition with MAXIMUM Tc (= T*).

## Materials in the homologous series and their Tc values:
1. O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K)
2. O6.100Ca1Cu2Ba2Hg1 (Tc = 92.5 K)
3. O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0 K)
4. O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0 K)
5. O6.160Ca1Cu2Ba2Hg1 (not measurable - no sample exists)
6. O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0 K)
7. O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K) - PEAK
8. O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0 K) - PEAK
9. O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0 K)
10. O6.270Ca1Cu2Ba2Hg1 (not measurable - no sample exists)
11. O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0 K)
12. O6.310Ca1Cu2Ba2Hg1 (Tc = 98.0 K)

## Results:
- Peak Tc (T*) = 127.0 K
- Peak compositions: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1

## Step B - Filter Family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Among materials that contain Hg and have exactly 4 elements, report ALL whose Tc >= 95.0 K.

Materials with Hg and exactly 4 elements (Cu, Ba, Hg, and O):
- O4.047Cu1Ba2Hg1 (not measurable - no sample exists)
- O4.066Cu1Ba2Hg1 (not measurable - no sample exists)
- O4.081Cu1Ba2Hg1 (Tc = 98.0 K) ✓
- O4.085Cu1Ba2Hg1 (Tc = 92.0 K) - below threshold
- O4.096Cu1Ba2Hg1 (Tc = 95.6 K) ✓
- O4.097Cu1Ba2Hg1 (not measurable - out of budget)
- O4.101Cu1Ba2Hg1 (not measurable - out of budget)
- O4.103Cu1Ba2Hg1 (not measurable - out of budget)
- O4.150Cu1Ba2Hg1 (not measurable - out of budget)
- O4.160Cu1Ba2Hg1 (not measurable - out of budget)
- O4.162Cu1Ba2Hg1 (not measurable - out of budget)
- O4.180Cu1Ba2Hg1 (not measurable - out of budget)
- O4.200Cu1Ba2Hg1 (not measurable - out of budget)
- O4.220Cu1Ba2Hg1 (not measurable - out of budget)

So Q1 = {O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1}

## Step C - Extrapolate
I need to induce a rule from measurable materials to predict high/low for unmeasurable ones.

Measured materials with Tc values:
- O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K) - Low
- O6.100Ca1Cu2Ba2Hg1 (Tc = 92.5 K) - Low
- O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0 K) - High
- O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0 K) - High
- O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0 K) - High
- O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K) - High
- O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0 K) - High
- O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0 K) - High
- O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0 K) - High
- O6.310Ca1Cu2Ba2Hg1 (Tc = 98.0 K) - High
- O4.081Cu1Ba2Hg1 (Tc = 98.0 K) - High
- O4.085Cu1Ba2Hg1 (Tc = 92.0 K) - Low
- O4.096Cu1Ba2Hg1 (Tc = 95.6 K) - High

Unmeasurable materials found:
- O6.160Ca1Cu2Ba2Hg1 (from homologous series)
- O6.270Ca1Cu2Ba2Hg1 (from homologous series)
- O4.047Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.066Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.097Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.101Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.103Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.150Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.160Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.162Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.180Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.200Cu1Ba2Hg1 (from 4-element Hg materials)
- O4.220Cu1Ba2Hg1 (from 4-element Hg materials)
- O6Cu1Ba2Hg1Tl1 (from Hg search)

Looking at the pattern, it seems that oxygen content might be a factor. Let me analyze:
- O6.080Ca1Cu2Ba2Hg1 (Tc = 92.0 K) - Low
- O6.100Ca1Cu2Ba2Hg1 (Tc = 92.5 K) - Low
- O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0 K) - High
- O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0 K) - High
- O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0 K) - High
- O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K) - High
- O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0 K) - High
- O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0 K) - High
- O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0 K) - High
- O6.310Ca1Cu2Ba2Hg1 (Tc = 98.0 K) - High

There seems to be a pattern where Tc increases with oxygen content up to a point (around O6.210-O6.220) and then decreases again. For O4.xCu1Ba2Hg1:
- O4.081Cu1Ba2Hg1 (Tc = 98.0 K) - High
- O4.085Cu1Ba2Hg1 (Tc = 92.0 K) - Low
- O4.096Cu1Ba2Hg1 (Tc = 95.6 K) - High

For the Ca-free compounds (4 oxygen vs 6 oxygen), it seems like the optimal oxygen content varies based on the other elements present. Let me look for a more complex rule.

Actually, looking more closely at the data, I notice that in the main series, Tc starts low at O6.080 and O6.100 (< 90K), increases significantly around O6.130 and beyond (> 90K). The peak is at O6.210 and O6.220. Then it decreases but remains above 90K.

For the 4-element compounds:
- O4.081Cu1Ba2Hg1 (Tc = 98.0 K) - High
- O4.085Cu1Ba2Hg1 (Tc = 92.0 K) - Low
- O4.096Cu1Ba2Hg1 (Tc = 95.6 K) - High

Let me try to develop a rule based on the relationship between oxygen content and the presence of Ca. In the series with Ca, high Tc seems to occur around oxygen content of 6.130-6.310, while in the series without Ca, high Tc seems to occur around oxygen content of 4.080-4.100.

Rule: A material is high Tc if it has an oxygen content that falls within the optimal range for its specific elemental composition. For Ca1Cu2Ba2Hg1 materials, high Tc occurs when oxygen content is between ~6.130-6.310. For Cu1Ba2Hg1 materials, high Tc appears to occur when oxygen content is around ~4.080-4.100.

Based on this rule:
- O6.160Ca1Cu2Ba2Hg1: Oxygen 6.160 is in the high Tc range → High
- O6.270Ca1Cu2Ba2Hg1: Oxygen 6.270 is in the high Tc range but past the peak → Likely High (though lower than peak)
- O4.047Cu1Ba2Hg1: Oxygen 4.047 is below optimal range → Low
- O4.066Cu1Ba2Hg1: Oxygen 4.066 is likely below optimal range → Low
- O4.097Cu1Ba2Hg1: Oxygen 4.097 is in the high Tc range → High
- O4.101Cu1Ba2Hg1: Oxygen 4.101 is likely in the high Tc range → High
- O4.103Cu1Ba2Hg1: Oxygen 4.103 is likely in the high Tc range → High
- O4.150Cu1Ba2Hg1: Oxygen 4.150 might be past the optimal range for this system → Low
- O4.160Cu1Ba2Hg1: Oxygen 4.160 might be past the optimal range for this system → Low
- O4.162Cu1Ba2Hg1: Oxygen 4.162 might be past the optimal range for this system → Low
- O4.180Cu1Ba2Hg1: Oxygen 4.180 might be past the optimal range for this system → Low
- O4.200Cu1Ba2Hg1: Oxygen 4.200 might be past the optimal range for this system → Low
- O4.220Cu1Ba2Hg1: Oxygen 4.220 might be past the optimal range for this system → Low
- O6Cu1Ba2Hg1Tl1: This has 5 elements (O, Cu, Ba, Hg, Tl) so doesn't fit the 4-element criteria, and the oxygen value is unclear

## Step D - Final Pick
Among unmeasurable materials I predicted as high, choose the one with the fewest elements (break ties by higher expected Tc).

Predicted as high:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)
- O4.097Cu1Ba2Hg1 (4 elements) ← Fewest elements
- O4.101Cu1Ba2Hg1 (4 elements)
- O4.103Cu1Ba2Hg1 (4 elements)

Between the 4-element materials, I would expect O4.097Cu1Ba2Hg1 to have a high Tc close to O4.096Cu1Ba2Hg1 (95.6K) and O4.101Cu1Ba2Hg1/O4.103Cu1Ba2Hg1 to also have high Tcs. Since O4.097 is closest to the known high Tc material O4.096, it would likely have a slightly lower Tc than O4.101 or O4.103. So O4.101 or O4.103 would be better candidates.

Of the 4-element materials predicted as high, I'll select O4.101Cu1Ba2Hg1 as the final pick since oxygen 4.101 is closer to the optimal range.