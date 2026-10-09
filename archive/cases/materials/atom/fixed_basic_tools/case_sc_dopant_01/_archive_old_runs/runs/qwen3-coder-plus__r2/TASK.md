# Task: predict Tc across an unsynthesized substitution series

You are a materials scientist. Two lists sit in `data/`:

- `data/synthesized.txt` — compositions you **have samples of**. Put them in the
  instrument with `measure_tc` to read their critical temperature Tc (K).
- `data/targets.txt` — compositions **nobody has made yet**. There is no sample, so
  `measure_tc` returns no value for them. Their Tc can only be predicted.

You have 14 units of instrument time (1 unit per `measure_tc` call).
Keep your reasoning in `notes.md`; write your final answer to `answer.json`.
Every measured number you cite must come from an actual `measure_tc` call.

## Goal

Predict the critical temperature Tc for **every** composition in `data/targets.txt`.
A composition with Tc = 0 is not superconducting at all.

You also need two summary quantities about the target series:

1. `max_pr_above_80K` — the **highest Pr content** in the target series whose Tc is
   still at or above **80 K**.
2. `critical_pr` — the **highest Pr content** in the target series that is still
   superconducting at all (Tc > 0).

## What you know

All compositions here share the layout RE(1-x)Pr(x)Ba2Cu3O7, where RE is a rare-earth
element and x is the Pr content. The samples you can measure cover **two different RE
elements**; every target uses a **third** RE (Gd). Tc depends on both x and which RE
is used, so measuring the series you do have should tell you how Tc behaves.

## Deliverables

1. `notes.md` — what you measured, the relationship you inferred, and how you used it.
2. `answer.json` — valid JSON with exactly these keys:

```json
{
  "law": "<the relationship you inferred, one or two sentences>",
  "predictions": {"<target composition>": <predicted Tc in K>, "...": 0},
  "max_pr_above_80K": <highest Pr content in the target series with Tc >= 80 K>,
  "critical_pr": <highest Pr content in the target series still superconducting>
}
```

`predictions` must contain a numeric entry for **every** composition in
`data/targets.txt`. Write the file with the Write tool.

How you sequence the work is up to you.
