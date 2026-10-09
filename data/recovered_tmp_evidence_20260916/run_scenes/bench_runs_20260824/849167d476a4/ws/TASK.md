# Task: find which structural edit actually improves drug-likeness

You are a medicinal chemist working in THIS directory.

## The parent compound

SMILES `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`

| QED | MW | ALOGP | PSA | ROTB | AROM |
|---|---|---|---|---|---|
| **0.6136** | 309.45 | 5.444 | 29.1 | 9 | 2 |

Its drug-likeness needs improving. Diagnose **why** it scores poorly before deciding
what to change — the productive edit direction follows from which properties are out
of range and which are already fine.

## The candidates

A pool of single-point edits of the parent is available through the tools below —
**you are not given its catalogue**; you have to search it yourself.

## Tools

- `search_pool(query, page)` — find candidates whose SMILES contains `query`. Returns
  `edit_id`, SMILES and five free descriptors (`mw`, `alogp`, `psa`, `rotb`, `arom`),
  4 per page — **no QED**.
- `measure_qed(edit_id)` — measure one candidate's QED on a real sample.

**Both tools draw on one shared budget of 11 units**: every successful page of
`search_pool` costs 1, and every successful measurement costs 1. Paging blindly through
the whole pool will exhaust the budget before you measure anything — search with a
substructure instead.

**The descriptors do not determine QED** — QED weighs eight properties, three of which
are not returned. Do not assume an edit that improves one descriptor improves QED.

## What to deliver

1. **Which candidate has the highest QED** in the pool (`best_edit_id`), and the QED you
   measured for it (`best_qed_measured`).
2. **Pick one candidate as the optimised lead** (`pick_edit_id`). It must satisfy **all
   three** constraints at once:
   - **measured QED ≥ 0.8** — no tool lists QED, so the only way to know is to measure.
   - **`psa` ≤ 40** — free, returned by `search_pool`.
   - **`rotb` ≤ 9** — free, returned by `search_pool`.

   A candidate failing any one of the three does not count, however good it looks
   otherwise. The two free constraints cost nothing to check, so check them **before**
   spending measurements.
3. **Which edit direction turned out NOT to work** (`failed_direction`), in a few words.

## Deliverables in this workspace

1. `notes.md` — your reasoning: what you diagnosed as the parent's problem, which
   candidates you measured and why, what you found, and how you chose.
2. `answer.json` — **already in this directory, pre-filled with the exact keys and
   empty values. Fill in the values; do not rename keys, do not add or remove keys.**

```json
{
  "best_edit_id": "<edit_id of the highest-QED candidate>",
  "best_qed_measured": <the QED you measured for it, a number>,
  "pick_edit_id": "<edit_id satisfying all three constraints>",
  "failed_direction": "<the edit direction that did not work, a few words>"
}
```

Edit the existing `answer.json` (Edit or Write tool both work). Every QED you cite as
measured must come from a real `measure_qed` call.
