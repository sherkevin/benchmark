# Task: superconductor Tc study (four dependent steps)

You are a materials scientist with a limited measurement budget. Work **in this
workspace**: keep your running notes in `notes.md`, and write your final answer to
`answer.json`. Use the `measure_tc` tool for every Tc value you cite — never guess
a measured number.

Seed material: `O6.080Ca1Cu2Ba2Hg1`
Definition: a material is **high** if Tc >= 90 K, otherwise **low**.
Measurement budget: 34 `measure_tc` calls.

## Step A — series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Measure the series members in `data/measurable.txt` and find the composition with
the MAXIMUM Tc. Call that maximum **T***.

## Step B — filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32. Among materials that contains Hg and has exactly 4 elements, report ALL whose
Tc >= threshold1. Call this set **Q1**.

## Step C — extrapolate (no measurement possible)
The materials in `data/unmeasurable.txt` have never been synthesized — there is no
physical sample to put in the instrument. `measure_tc` will tell you "no sample
exists" for them. You must **induce a rule** that separates high from low Tc using
the materials you CAN measure, then **predict** high/low for every unmeasurable
material. Warning: single-factor rules (e.g. "contains Hg" or "has 5 elements") are
wrong on several of them — the governing rule is an interaction of multiple factors.

## Step D — final pick
Among the unmeasurable materials you predicted **high** in step C, return the one
with the FEWEST elements (break ties by higher expected Tc).

## Answer format
Write `answer.json` with exactly these keys:
```json
{
  "peak": "<step A composition>",
  "peak_tc": <T* number>,
  "threshold": <threshold1 number>,
  "qualifying": ["<Q1 member>", ...],
  "predictions": {"<unmeasurable composition>": "high" or "low", ...},
  "final": "<step D answer>",
  "rule": "<the rule you induced in step C>"
}
```
`predictions` MUST contain an entry for EVERY composition in `data/unmeasurable.txt`.
