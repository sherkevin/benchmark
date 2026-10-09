# Task: predict the Tc curve of a not-yet-synthesized material family

You are a materials scientist working in THIS directory. You study
`RE(1-x) Pr(x) Ba2 Cu3 O7` cuprates: substituting Pr for the rare earth (RE)
suppresses the critical temperature Tc. How fast Tc falls with Pr content x
depends on which RE is used.

## What you already have

- `data/synthesized.txt` — **19 compositions you have already synthesized**
  (two RE families across a range of Pr). Each has a physical sample, so you can
  measure its real Tc (K) by calling `measure_tc` on it.
- `data/to_predict.txt` — **11 compositions of a THIRD RE family that has NOT
  been synthesized yet.** There is no sample, so `measure_tc` cannot return a value
  for them. You must **predict** each one's Tc.

## Goal

For **every** composition in `data/to_predict.txt`, predict its Tc (K). Measure as
many of the synthesized compositions as you need to pin down how Tc depends on Pr
content and on the RE, then reason out where the un-synthesized family must fall.

## Deliverables in this workspace

1. `notes.md` — your running reasoning: what you measured, the pattern you inferred
   (how Tc falls with Pr, how the RE families compare), and how you extrapolated to
   the un-synthesized family.
2. `answer.json` — valid JSON, exactly:

```json
{
  "predictions": {"<composition from to_predict.txt>": <predicted Tc in K, a number>, "...": 0},
  "reasoning": "<one or two sentences on how you derived the curve>"
}
```

`predictions` MUST contain an entry for **every** composition in
`data/to_predict.txt` (a number in K; use 0 for a composition you believe is no
longer superconducting). Write `answer.json` with the Write tool. Every Tc you cite
from a measurement must come from a real `measure_tc` call.
