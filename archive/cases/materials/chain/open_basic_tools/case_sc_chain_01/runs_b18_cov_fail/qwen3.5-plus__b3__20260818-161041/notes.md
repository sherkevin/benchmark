# Superconductor Tc Study Notes

## Seed Material
- Seed: `O6.080Ca1Cu2Ba2Hg1`

## Step A — Find series peak (T*)
The seed belongs to a homologous doping series with same element set but varying oxygen.
Elements in seed: O, Ca, Cu, Ba, Hg (5 elements)

### Homologous series members found (O-Ca-Cu-Ba-Hg):
| Composition | Tc (K) | Sample? |
|-------------|--------|---------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | - | No (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | - | No (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

**threshold1 = T* - 32 = 95.0 K**

## Step B — Family B (contains Hg, exactly 4 elements)
4-element Hg materials found from search_db("Hg"):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1

Budget exhausted before measuring these. Cannot determine Q1 definitively.

## Step C — Unmeasurable materials
Found 2 unmeasurable materials in the homologous series:
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

Need to induce a rule from measurable materials to predict high/low.

Looking at the pattern:
- Tc increases with oxygen up to ~O6.21-6.22, then decreases
- O6.160 is between O6.140 (105 K) and O6.190 (122 K) - likely high (>95 K, actually >90 K)
- O6.270 is between O6.240 (123 K) and O6.300 (100 K) - likely high (>90 K)

Rule: Tc follows a bell curve with oxygen content, peaking around O6.21-6.22.

For O6.160: interpolating between 105 and 122 → ~113-115 K → HIGH
For O6.270: interpolating between 123 and 100 → ~110-115 K → HIGH

## Step D — Final pick
Among unmeasurable materials predicted high, choose one with FEWEST elements.
Both have 5 elements. Tie-break by higher expected Tc.
O6.160 expected ~113-115 K, O6.270 expected ~110-115 K. Hard to say which is higher.

Looking more carefully at the curve:
- O6.140: 105
- O6.160: ? (predicted)
- O6.190: 122

The rise from 6.140 to 6.190 is 17 K over 0.05 oxygen units.
At 6.160 (0.02 above 6.140), expected ~105 + (0.02/0.05)*17 = 105 + 6.8 = 111.8 K

For O6.270:
- O6.240: 123
- O6.270: ? (predicted)
- O6.300: 100

The drop from 6.240 to 6.300 is 23 K over 0.06 oxygen units.
At 6.270 (0.03 above 6.240), expected ~123 - (0.03/0.06)*23 = 123 - 11.5 = 111.5 K

So O6.160 has slightly higher expected Tc (~112 K vs ~111.5 K).

But wait - I need to check if there are other unmeasurable materials in the database that I haven't found. I only searched for Hg and Ca. There might be other materials I haven't discovered.

## Budget used
Total: 18/18
- 7 searches (Hg pages 1-2, Ca pages 1-5)
- 12 measurements (10 successful, 2 unmeasurable)

Wait, let me recount:
- Searches: Hg(1), Ca(1), CaHg(1), Ca(2), Ca(3), Ca(4), Ca(5), Hg(2) = 8 searches
- Measurements: 12 compositions in homologous series = 12 measurements
Total: 8 + 12 = 20? That's more than 18.

Actually from the budget_used values:
- After all searches and measurements: budget_used = 18

Let me trust the tool's accounting. Final budget: 18/18.

## Final Answer Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold: 95.0 K
- Qualifying (Q1): [] (no budget to measure 4-element Hg materials)
- Predictions: O6.160Ca1Cu2Ba2Hg1 → high, O6.270Ca1Cu2Ba2Hg1 → high
- Final: O6.160Ca1Cu2Ba2Hg1 (5 elements, slightly higher expected Tc than O6.270)
- Rule: Bell curve interpolation based on oxygen content

Note: Budget was exhausted before completing Step B measurements. The qualifying array is empty because the 4-element Hg materials (O4.xxxCu1Ba2Hg1) were not measured.
