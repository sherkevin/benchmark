# Task: superconductor Tc study (four dependent steps)

You are a materials scientist with a limited measurement budget. Work **in this
workspace**: keep your running notes in `notes.md`, and write your final answer to
`answer.json`.

You have two tools:
- `measure_tc(composition)` — measure Tc of a synthesized sample (costs 1 budget unit)
- `search_db(query, page)` — search the materials database (costs 1 budget unit per page)

**Both tools share the same budget of 18 units total.**

Seed material: `O6.080Ca1Cu2Ba2Hg1`
Definition: a material is **high** if Tc >= 90 K, otherwise **low**.

## How to discover materials

You do NOT have a list of all materials. Use `search_db` to find them:
- `search_db("Hg", page=1)` finds materials containing "Hg"
- `search_db("Cu", page=2)` gets the second page of Cu-containing materials
- Each page shows at most 4 results and tells you if more pages exist

Plan your searches carefully — every page costs 1 budget unit.

## Step A — series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Find and measure the series members to identify the composition with MAXIMUM Tc (= T*).

## Step B — filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32. Among materials that contains Hg and has exactly 4 elements, report ALL whose
Tc >= threshold1. Call this set **Q1**.

## Step C — extrapolate (no measurement possible)
Some materials in the database have never been synthesized — `measure_tc` returns
"no sample exists" for them. Induce a rule from measurable materials, then predict
high/low for every unmeasurable one you find. Warning: single-factor rules are wrong
on several of them.

## Step D — final pick
Among unmeasurable materials you predicted **high**, choose the one with the
FEWEST elements (break ties by higher expected Tc).

## Answer format
Write `answer.json`:
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
`predictions` should contain ALL unmeasurable materials you discovered via search_db.
