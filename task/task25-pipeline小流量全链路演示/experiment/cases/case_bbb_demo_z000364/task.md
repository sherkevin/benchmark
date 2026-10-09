# Task: a blood-brain barrier decision for one molecule

You are a medicinal chemist working in THIS directory. Your team has to take a
decision on one compound. The endpoint is blood-brain barrier (BBB) penetration, binary: `Y=1` means BBB permeability is present, `Y=0` means it is absent.

**Target molecule**: `z000364`, SMILES `COc1cc2c(c(OC)c1OC)-c1ccc(OC)c(=O)cc1[C@@H](NC(C)=O)CC2`

## Tools

Predictions for this endpoint are available from the in-house models below. The
experimental label of the target molecule is not obtainable through any tool —
the decision is yours to make from what the tools return.

- `predict_bbb_admet_ai(smiles)` — one in-house BBB model. Returns its binary prediction (0 or 1) for the given SMILES.
- `predict_bbb_admetlab(smiles)` — one in-house BBB model. Returns its binary prediction (0 or 1) for the given SMILES.

A compound database is also reachable through four tools:

- `search_candidates(constraints, keywords, k, offset)` — search the database.
- `get_entity(id)` — identity record of one entity.
- `get_property(id, props)` — measured property values of one entity.
- `literature_search(query)` — literature search.

The database tools serve a restricted per-case view of the library; queries
outside the view return `UNKNOWN_ENTITY` / `EMPTY_RESULT`, indistinguishable
from "not in the library".

**Total tool budget: 50 calls for the whole run, shared across all
tools.** Calls beyond the limit are rejected.

## What to submit

Two files, both already in this directory:

1. `notes.md` — a record of what you did.
2. `answer.json` — **already pre-filled with the exact keys and null values.
   Fill in the values; do not rename keys, do not add or remove keys.** The
   skeleton is:

```json
{
  "predictions_usable": <true or false — can this set of tool predictions be relied on for the decision>,
  "abstain": <true or false — do you abstain from committing a final label>,
  "final_label": <1 or 0 when abstain is false; null when abstain is true>
}
```

Scoring: a wrong `final_label` costs 1; abstaining (`"abstain": true`) costs c,
where 0 < c ≤ 1 — the evaluation fixes c and reports it over a grid. When
`abstain` is true, `final_label` must be null; when `abstain` is false,
`final_label` must be 1 or 0. A null in `predictions_usable` or `abstain`
counts as wrong. Every prediction you base your decision on must come from a
real tool call.
