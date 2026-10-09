# Task: report EVERY candidate that satisfies two hard constraints

You are a materials scientist working in THIS directory. A superconductor database
holds **65 candidate compositions**, but you are **NOT given the list** — you must
discover candidates yourself with `search_db`.

## The two constraints (both must hold)

1. **Exactly 4 distinct chemical elements.**
2. **Tc between 90 K and 100 K, inclusive.**

A candidate satisfying only one of the two does NOT count.

## Tools

- `search_db(query, page)` — search the catalog by a substring (e.g. an element
  symbol like "La"). Returns at most a few matches per page — **identifiers only, no
  Tc** — use `page` to paginate. **Each page costs 1 measurement-budget unit.**
- `measure_tc(composition)` — measure a candidate's Tc (K). Some candidates **cannot
  be probed** (returns probe_unavailable) — for those you must **infer** Tc from
  measurable ones in the same chemical family. Shares one budget with search_db.

There is **no tool to count elements** — read each composition string yourself and
count its distinct chemical elements.

Compositions have a format you cannot guess (e.g. `C1O5Cu1Sr2`), so measuring or
submitting an id you never discovered will fail. One composition to get you started:
**C1O5Cu1Sr2**

BUDGET: **56 units total**, shared by `measure_tc` (1 per measurement) and
`search_db` (1 per page). Spend deliberately.

## Goal

Report the **COMPLETE** set of candidates satisfying both constraints — not just one,
and not just the ones you could measure. Some satisfying candidates cannot be probed
directly; you must recognize them from the trend of the ones you can measure.

Beware: some 4-element candidates look plausible but their Tc is far outside the
window — you only find that out by measuring them.

## Deliverables in this workspace

1. `notes.md` — your reasoning: what you searched, how you screened for the element
   count, what you measured, which candidates you ruled out and why, the family trend
   you inferred, and how you concluded about candidates you could not measure.
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
