# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Budget
- Total: 34 units
- Both measure_tc and search_db (per page) cost 1 unit each

## Step A — Find series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Elements in seed: O, Ca, Cu, Ba, Hg

Need to find materials with same elements but varying oxygen content.

### Actions taken:
- 

## Step B — Filter family B
- Contains Hg AND exactly 4 elements
- threshold1 = T* - 32
- Find all with Tc >= threshold1 → set Q1

## Step C — Extrapolate
- Find unmeasurable materials (no sample exists)
- Induce rule from measurable materials
- Predict high/low for unmeasurable ones

## Step D — Final pick
- Among unmeasurable predicted "high"
- Choose one with FEWEST elements
- Break ties by higher expected Tc

## Answer JSON structure:
```json
{
  "peak": "<step A composition>",
  "peak_tc": <T* number>,
  "threshold": <threshold1 number>,
  "qualifying": ["<Q1 member>", ...],
  "predictions": {"<unmeasurable composition>": "high" or "low", ...},
  "final": "<step D answer>",
  "rule": "<the rule you induced>"
}
```
