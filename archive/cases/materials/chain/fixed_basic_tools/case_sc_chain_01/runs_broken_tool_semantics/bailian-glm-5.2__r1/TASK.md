# Task: find the best unexplored superconductor candidate

You are a materials scientist working in this directory. You have a measurement
budget of 45 `get_property` calls. Keep your running notes in
`notes.md` and write your final answer to `answer.json`. Every measured number you
cite must come from an actual `get_property` call.

## Goal

Among the materials in `data/unmeasurable.txt`, identify the single best candidate,
where "best" is defined as: **the fewest elements among those whose Tc is high**
(ties broken by higher expected Tc).

## Definitions you need

- **high** means Tc >= 90 K; otherwise low.
- The materials in `data/unmeasurable.txt` have **never been probed** — `get_property`
  returns "no measurement exists" for their Tc. Their Tc is therefore not obtainable
  by measurement; it can only be inferred from the materials you can measure
  (`data/measurable.txt`).
- Warning: one-factor rules (e.g. "contains Hg", "has 5 elements") are wrong on
  several of these materials. The behaviour is governed by an interaction.

## A second quantity we also want reported

Separately from the goal above, report the **qualifying set Q1**, defined as:
all materials that contain Hg and have exactly 4 elements, whose
Tc >= threshold1, where **threshold1 = T\* - 32** and **T\*** is the maximum Tc
within the homologous doping series of the seed material `O6.080Ca1Cu2Ba2Hg1`
(the series = same element set, varying oxygen content).

## Deliverables in this workspace

1. `notes.md` — your reasoning trail: what you measured and why, what rule you
   induced, how you arrived at the answer.
2. `answer.json` — valid JSON with exactly these keys:

```json
{
  "peak": "<composition with the maximum Tc in the seed's series>",
  "peak_tc": <that maximum Tc, i.e. T*>,
  "threshold": <threshold1>,
  "qualifying": ["<member of Q1>", "..."],
  "rule": "<the rule you induced, one or two sentences>",
  "predictions": {"<unmeasurable composition>": "high|low", "...": "..."},
  "final": "<your single best candidate>"
}
```

`predictions` must contain an entry for **every** composition in
`data/unmeasurable.txt`. Write the file with the Write tool.

How you sequence the work is up to you.
