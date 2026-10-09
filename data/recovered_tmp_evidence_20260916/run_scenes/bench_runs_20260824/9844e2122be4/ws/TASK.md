# Task: report EVERY candidate that satisfies two hard constraints

You are a materials scientist working in THIS directory. `data/candidates.txt`
lists 65 candidate superconductors.

## The two constraints (both must hold)

1. **Exactly 4 distinct chemical elements.**
2. **Tc between 90 K and 100 K, inclusive.**

A candidate satisfying only one of the two does NOT count.

## Tools

- `measure_tc(composition)` — measure a candidate's Tc (K) on the instrument.
  Budget: **56 measurements** for 65 candidates. Some candidates **cannot be
  probed** by this instrument (it returns probe_unavailable) — for those you must
  **infer** Tc from measurable ones in the same chemical family.

There is **no tool to count elements** — read each composition string (e.g.
`O4.047Cu1Ba2Hg1`) and count its distinct chemical elements yourself.

## Goal

Report the **COMPLETE** set of candidates satisfying both constraints — not just
one, and not just the ones you could measure. Some satisfying candidates cannot be
probed directly; you must recognize them from the trend of the ones you can measure.

Beware: some 4-element candidates look plausible but their Tc is far outside the
window — you only find that out by measuring them.

## Deliverables in this workspace

1. `notes.md` — your reasoning: how you screened for the element count, what you
   measured, which candidates you ruled out and why, the family trend you inferred,
   and how you concluded about candidates you could not measure.
2. `answer.json` — valid JSON, exactly:

```json
{
  "satisfying_set": ["<every candidate meeting BOTH constraints>", "..."],
  "ec_legal_set": ["<every candidate with exactly 4 elements>", "..."],
  "excluded_with_tc": ["<4-element candidates you measured and ruled out by Tc>", "..."],
  "measured_max_legal_id": "<highest-Tc 4-element candidate you actually MEASURED>",
  "measured_max_legal_tc": <its measured Tc in K, a number>,
  "satisfying_mean_tc": <mean Tc over your satisfying_set, a number>,
  "reasoning": "<one or two sentences>"
}
```

Write `answer.json` with the Write tool. Every Tc you cite from a measurement must
come from a real `measure_tc` call; inferred values must be justified in notes.md.
