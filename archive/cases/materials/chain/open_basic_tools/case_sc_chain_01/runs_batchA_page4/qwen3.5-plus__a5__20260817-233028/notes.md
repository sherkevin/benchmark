# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Budget: 45 units total (shared between measure_tc and search_db)

## Step A — Find series peak (homologous doping series)
The seed has elements: O, Ca, Cu, Ba, Hg
Need to find materials with SAME elements but varying oxygen content.

Plan: Search for materials with these elements to find the homologous series.

## Step B — Filter family B (contains Hg, exactly 4 elements)
threshold1 = T* - 32
Find all materials with Hg and exactly 4 elements where Tc >= threshold1

## Step C — Extrapolate (unmeasurable materials)
Find materials where measure_tc returns "no sample exists"
Induce a rule from measurable materials to predict high/low

## Step D — Final pick
Among unmeasurable materials predicted "high", choose one with FEWEST elements
Tie-break: higher expected Tc

---

## Progress Log

### Step A: Finding the homologous series

