# Task: find the highest-Tc superconductor under an element-count constraint

You are a materials scientist working in THIS directory. You have a list of
68 candidate superconductors in `data/candidates.txt`.

## The constraint (hard)

The final answer MUST contain **exactly 4 distinct chemical elements**.
A candidate with any other element count is disqualified no matter how high its Tc.

## Tools

- `get_element_count(composition)` — how many distinct elements a candidate has.
  Free (does not use measurement budget).
- `measure_tc(composition)` — measure a candidate's Tc (K) on the instrument.
  You have a budget of **35 measurements** for 68 candidates, so choose
  wisely. Some candidates **cannot be probed** by this instrument (it returns
  probe_unavailable) — for those you must **infer** Tc from measurable ones in
  the same chemical family.

## Goal

Report the 4-element candidate with the highest Tc — **including candidates
that cannot be measured directly**. If the true optimum is a composition the
instrument cannot probe, you must recognize that from the pattern of the ones
you can measure, and still report it.

## Deliverables in this workspace

1. `notes.md` — your reasoning: which candidates are legal (4-element), what
   you measured, the family trend you inferred, and why you believe the final
   answer is optimal even if you could not measure it directly.
2. `answer.json` — valid JSON, exactly:

```json
{
  "final": "<the 4-element composition with highest Tc, measurable or not>",
  "legal_set": ["<every 4-element candidate you identified>", "..."],
  "legal_count": <how many 4-element candidates exist, a number>,
  "legal_mean_tc": <mean Tc over all legal candidates incl. inferred ones, a number>,
  "measured_max_id": "<the highest-Tc candidate you actually MEASURED>",
  "measured_max_tc": <its measured Tc in K, a number>,
  "reasoning": "<one or two sentences>"
}
```

Write `answer.json` with the Write tool. Every Tc you cite from a measurement
must come from a real `measure_tc` call; inferred values must be justified in notes.md.
