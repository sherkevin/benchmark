# Task: find the highest-Tc composition under a measurement budget

You are a materials scientist working in THIS directory. A list of candidate
compositions is in `data/candidates.txt`. Each has a real critical temperature Tc (K)
that you can obtain by calling `measure_tc` on it.

## Your budget (this is the core constraint)

You have **12 measurement credits**. Each `measure_tc` call costs **1 credit**.
There are **30 candidates** — far more than your budget, so you
**cannot measure them all**. You must decide which ones are worth measuring.

## Goal

Report the single candidate you believe has the **highest Tc**. Because you cannot
measure everything, you will have to measure a few, infer how Tc varies across the
candidates, and use that to decide what to measure next — until your credits run out,
then commit to your best answer.

## Deliverables in this workspace

1. `notes.md` — your running reasoning: what you measured, what pattern you see, why
   you chose each next measurement, and why you finally picked your answer.
2. `answer.json` — valid JSON, exactly:

```json
{
  "final": "<the candidate composition you believe has the highest Tc>",
  "measured": {"<composition>": <Tc you measured>, "...": 0},
  "reasoning": "<one or two sentences on how you decided>"
}
```

`measured` should list every composition you actually measured with its Tc.
Write `answer.json` with the Write tool. Every Tc you cite must come from a real
`measure_tc` call. How you spend your credits is entirely up to you.
