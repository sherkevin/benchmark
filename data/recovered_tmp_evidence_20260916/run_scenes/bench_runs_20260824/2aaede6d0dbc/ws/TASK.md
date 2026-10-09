# Task: find the highest-Tc superconductor under an element-count constraint

You are a materials scientist working in THIS directory. A superconductor database
holds **68 candidate compositions**, but you are **NOT given the list** — you must
discover candidates yourself with `search_db`.

## The constraint (hard)

The final answer MUST contain **exactly 4 distinct chemical elements**.
A candidate with any other element count is disqualified no matter how high its Tc.

## Tools

- `search_db(query, page)` — search the catalog by a substring (e.g. an element
  symbol like "Hg"). Returns at most a few matches per page — **identifiers only, no
  Tc** — use `page` to paginate. **Each page costs 1 measurement-budget unit.**
- `get_element_count(composition)` — how many distinct elements a candidate has. Free.
- `measure_tc(composition)` — measure a candidate's Tc (K). Some candidates **cannot
  be probed** (returns probe_unavailable) — for those you must **infer** Tc from
  measurable ones in the same chemical family. Shares one budget with search_db.

Compositions have a format you cannot guess (e.g. `C1O5Cu1Sr2`), so measuring or
submitting an id you never discovered will fail. One composition to get you started:
**C1O5Cu1Sr2**

BUDGET: **35 units total**, shared by `measure_tc` (1 per measurement) and
`search_db` (1 per page). `get_element_count` is free. Spend deliberately.

## Goal

Report the 4-element candidate with the highest Tc — **including candidates that
cannot be measured directly**. If the true optimum is a composition the instrument
cannot probe, recognize it from the pattern of the ones you can measure, and still
report it.

## Deliverables in this workspace

1. `notes.md` — your reasoning: what you searched, which candidates are legal
   (4-element), what you measured, the family trend you inferred, and why the final
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

Write `answer.json` with the Write tool. Every Tc you cite from a measurement must
come from a real `measure_tc` call; inferred values must be justified in notes.md.
