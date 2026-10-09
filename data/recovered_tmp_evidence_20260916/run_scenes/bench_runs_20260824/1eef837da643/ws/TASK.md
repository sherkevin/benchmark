# Task: predict the Tc curve of a not-yet-synthesized material family

You are a materials scientist working in THIS directory. You study
`RE(1-x) Pr(x) Ba2 Cu3 O7` cuprates: substituting Pr for the rare earth (RE)
suppresses the critical temperature Tc, and how fast it falls depends on the RE.

The **Gd** member of this family (`... Gd ...`) has **not been synthesized yet**, so
its Tc cannot be measured. Your job is to **predict the Tc of every Gd-series
composition** in the materials database.

## Tools (you are NOT given any candidate list — discover it yourself)

- `search_db(query)` — search the materials catalog; returns the composition
  identifiers whose formula contains `query` (e.g. an element symbol). It returns
  **identities only, no Tc**. Use it to find which compositions exist.
- `measure_tc(composition)` — measure the real Tc of a **synthesized** sample.
  Compositions that have not been synthesized (the whole Gd series) return
  `sample_available: false` — they cannot be measured, only predicted.

## How to proceed (your plan is up to you)

Discover the family members with `search_db`, measure the ones that are synthesized
to learn how Tc depends on Pr content and on the RE, then infer where the
un-synthesized Gd series must fall and predict it.

## Deliverables in this workspace

1. `notes.md` — your running reasoning: what you searched and measured, the pattern
   you inferred, and how you extrapolated to the Gd series.
2. `answer.json` — valid JSON, exactly:

```json
{
  "predictions": {"<Gd composition>": <predicted Tc in K, a number>, "...": 0},
  "reasoning": "<one or two sentences on how you derived the curve>"
}
```

`predictions` MUST contain an entry for **every** Gd-series composition in the
database (find them all via `search_db`); use 0 for one you believe is no longer
superconducting. Write `answer.json` with the Write tool. Every measured Tc you
cite must come from a real `measure_tc` call.
