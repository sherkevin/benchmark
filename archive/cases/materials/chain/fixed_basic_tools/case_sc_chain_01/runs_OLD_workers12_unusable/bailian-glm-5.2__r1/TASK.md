# Task: pick the next composition worth synthesizing

You are a materials scientist. Two lists sit in `data/`:

- `data/synthesized.txt` — compositions you **have samples of**. You can put them in
  the instrument and measure Tc with the `measure_tc` tool.
- `data/not_synthesized.txt` — compositions nobody has made yet. **There is no sample**,
  so `measure_tc` cannot return a value for them. Their Tc can only be predicted.

You have 45 units of instrument time (1 unit per `measure_tc` call).
Keep your running notes in `notes.md` and write your final answer to `answer.json`.
Every measured number you cite must come from an actual `measure_tc` call.
(The number of distinct elements in a composition you can simply count from its formula.)

## Goal

Synthesis is expensive, so only one candidate gets made next. Among the compositions in
`data/not_synthesized.txt`, identify the single best one to synthesize, where "best"
is defined as: **the fewest distinct elements among those whose Tc would be high**
(ties broken by higher expected Tc).

## Definitions you need

- **high** means Tc >= 90 K; otherwise low.
- Warning: one-factor rules (e.g. "contains Hg", "has 5 elements") are wrong on
  several of these compositions. The behaviour is governed by an interaction.

## A second quantity we also want reported

Separately from the goal above, report the **qualifying set Q1**, defined as:
all compositions that contain Hg and have exactly 4 distinct elements, whose
Tc >= threshold1, where **threshold1 = T\* - 32** and **T\*** is the maximum Tc
within the homologous doping series of `O6.080Ca1Cu2Ba2Hg1`
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
  "predictions": {"<not-yet-synthesized composition>": "high|low", "...": "..."},
  "final": "<the one composition you would synthesize next>"
}
```

`predictions` must contain an entry for **every** composition in
`data/not_synthesized.txt`. Write the file with the Write tool.

How you sequence the work is up to you.
