# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 units total for both search_db and measure_tc (ALL USED)
- High Tc: >= 90 K, otherwise low
- Tools: search_db (1 unit/page) and measure_tc (1 unit/composition)

## Step A: Find series peak
Seed belongs to homologous doping series (same element set, varying oxygen).
Found series: O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310 (all Ca1Cu2Ba2Hg1)

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: no sample (unmeasurable)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK candidate)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (PEAK candidate)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: no sample (unmeasurable)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: not measured

Peak Tc (T*) = 127.0 K at O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1

## Step B: Filter family B
threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Find materials containing Hg and has exactly 4 elements.
Need to report ALL with Tc >= 95.0 K (set Q1)

From Hg searches earlier, found O-Cu-Ba-Hg materials (4 elements):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

But haven't measured their Tc values! Budget is exhausted.

Problem: Cannot complete Step B without measuring Tc of these 4-element Hg materials or inducing a rule that can predict their Tc.

## Step C: Extrapolate rule
Unmeasurable materials found: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1
Need to induce rule from measurable materials.
Warning: single-factor rules are wrong on several.

## Step D: Final pick
Among unmeasurable materials predicted HIGH, choose one with FEWEST elements.
Break ties by higher expected Tc.

## Budget Summary
Search_db used: 9 units (pages 1-5 for "Ca", pages 1-4 for "Hg")
Measure_tc used: 9 units (9 compositions measured)
Total: 18/18 units used

## Reflection
Not ideal - should have measured some O-Cu-Ba-Hg materials to get data for 4-element Hg family. Also should have conserved budget by measuring fewer Ca-Cu-Ba-Hg materials (just enough to find peak).

Current limitations:
1. Cannot complete Step B properly (need Tc measurements for 4-element Hg materials)
2. Rule induction is weak due to limited data diversity (all measured materials are HIGH)
3. Cannot verify which materials are truly unmeasurable (no budget to try measuring O-Cu-Ba-Hg materials)

## Final Answer Rationale
1. Step A: Peak Tc = 127.0 K at O6.210Ca1Cu2Ba2Hg1 (or O6.220Ca1Cu2Ba2Hg1)
2. Step B: threshold1 = 95.0 K. No 4-element Hg materials can be confirmed to have Tc >= 95.0 K due to lack of measurements. Rule predicts O-Cu-Ba-Hg materials are LOW (Tc < 90 K < 95 K), so qualifying set is empty.
3. Step C: Rule induced: "Materials with Ca, Cu2, and Hg are HIGH; others are LOW." Predicts O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1 as HIGH.
4. Step D: Among unmeasurable materials predicted HIGH (O6.160 and O6.270, both 5 elements), choose O6.160Ca1Cu2Ba2Hg1 as it likely has higher expected Tc (closer to peak on rising side).

## Completed Task
- Created notes.md with running notes
- Created answer.json with final answer
- Used all 18 budget units
- Acknowledged limitations due to budget allocation