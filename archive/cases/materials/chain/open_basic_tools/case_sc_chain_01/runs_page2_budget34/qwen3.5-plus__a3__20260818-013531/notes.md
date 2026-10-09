# Notes for Superconductor Tc Study

**Budget:** 34 units total (shared between measure_tc and search_db)

**Seed material:** O6.080Ca1Cu2Ba2Hg1

**Definition:** high if Tc >= 90 K, otherwise low

## Step A — Find series peak (T*)
- Seed belongs to homologous doping series (same element set, varying oxygen)
- Elements in seed: O, Ca, Cu, Ba, Hg
- Need to find series members with varying oxygen levels
- Measure each to find MAX Tc (= T*)

## Step B — Filter family B
- threshold1 = T* - 32
- Find materials with Hg AND exactly 4 elements
- Report all with Tc >= threshold1 (set Q1)

## Step C — Extrapolate for unmeasurable materials
- Some materials have no sample (measure_tc returns "no sample exists")
- Induce rule from measurable materials
- Predict high/low for unmeasurable ones
- Warning: single-factor rules are wrong on several

## Step D — Final pick
- Among unmeasurable materials predicted "high"
- Choose one with FEWEST elements
- Break ties by higher expected Tc

## Progress

### Step A: Finding series members
- Need to search for materials with same elements as seed but varying O
