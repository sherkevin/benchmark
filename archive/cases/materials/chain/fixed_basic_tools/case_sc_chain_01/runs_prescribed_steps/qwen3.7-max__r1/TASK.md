# Task: superconductor Tc study (four dependent steps)

You are a materials scientist with a small measurement budget. Work **in this
workspace**: keep your running notes in `notes.md`, and write your final answer to
`answer.json`. Use the `get_property` tool for every value you cite — never guess a
measured number.

Seed material: `O6.080Ca1Cu2Ba2Hg1`
Definition: a material is **high** if Tc >= 90 K, otherwise **low**.
Measurement budget: 45 `get_property` calls.

## Step A — series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Measure the series members listed in `data/measurable.txt` and find the composition
with the MAXIMUM Tc. Call that maximum **T\***.

## Step B — filter family B
threshold1 = T* - 32. Among materials that contain Hg and have exactly
4 elements, report ALL whose Tc >= threshold1. Call this set **Q1**.

## Step C — extrapolate (no measurement possible)
The materials in `data/unmeasurable.txt` have never been probed: `get_property(tc)`
returns "no measurement exists". You must **induce the rule** that separates high from
low Tc using the materials you CAN measure, then **predict** high/low for every
unmeasurable material. One-factor rules (e.g. "contains Hg", "has 5 elements") are
wrong on several of them; the governing rule is an interaction.

## Step D — final pick
Among the unmeasurable materials you predicted **high**, choose the one with the
FEWEST elements (break ties by higher expected Tc).

## Required deliverables in this workspace
1. `notes.md` — your reasoning trail: what you measured, what rule you induced, why.
2. `answer.json` — exactly this shape:

```json
{
  "peak": "<step A composition>",
  "peak_tc": <number>,
  "threshold": <number>,
  "qualifying": ["<Q1 member>", "..."],
  "rule": "<the rule you induced, one or two sentences>",
  "predictions": {"<unmeasurable composition>": "high|low", "...": "..."},
  "final": "<step D composition>"
}
```

`predictions` must contain an entry for **every** composition in
`data/unmeasurable.txt`. Write the file with the Write tool; it must be valid JSON.
