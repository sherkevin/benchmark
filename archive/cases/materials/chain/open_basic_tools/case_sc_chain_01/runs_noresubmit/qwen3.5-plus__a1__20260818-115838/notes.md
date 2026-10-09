# Notes on Superconductor Tc Study

Budget: 18 units total (shared between measure_tc and search_db)

## Step A — Find series peak

Seed: `O6.080Ca1Cu2Ba2Hg1`

This belongs to a homologous series with same elements but varying oxygen.
Need to find members with different O content and measure Tc to find maximum.

### Plan:
1. Search for materials with similar composition (Hg, Cu, Ba, Ca)
2. Identify the series members (varying O)
3. Measure Tc for each to find peak (T*)

## Step B — Filter family B

Family B: contains Hg AND exactly 4 elements
threshold1 = T* - 32
Find all materials in this family with Tc >= threshold1

## Step C — Extrapolate for unmeasurable materials

Some materials return "no sample exists" - need to induce a prediction rule.
Single-factor rules are wrong on several materials.

## Step D — Final pick

Among unmeasurable materials predicted HIGH, pick the one with FEWEST elements.
Tie-breaker: higher expected Tc.

---

## Progress Log
