# Task: predict an unmeasurable QED, and pick a lead meeting three constraints

You are a medicinal chemist. A compound collection is available through the tools below
— **you are not given its catalogue**; you have to search it yourself.

The collection contains several **congeneric series**: within one series every molecule
shares the same scaffold and differs only in the length of the N-alkyl chain. QED changes
systematically along a series, but **each scaffold has its own curve** — the shape and
the level both differ between series.

## Tools

- `search_pool(query, page)` — find pool molecules whose SMILES contains `query`.
  Returns `mol_id`, SMILES and five free descriptors (`mw`, `alogp`, `psa`, `rotb`,
  `arom`), 4 per page — **no QED**.
- `measure_qed(mol_id)` — measure a molecule's QED (drug-likeness, 0–1) on a real sample.

**Both tools draw on one shared budget of 12 units**: every successful page of
`search_pool` costs 1, and every successful measurement costs 1. Paging blindly through
the whole collection will exhaust the budget before you measure anything — search with a
substructure instead.

The five descriptors are free, but they are **not** the QED — QED weighs eight molecular
properties, three of which are not returned.

## Job 1 — predict the QED of `m05`

Molecule **`m05`** (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has **never been synthesized**, so
`measure_qed` returns `probe_unavailable` for it. Predict its QED:

1. Find the molecules in the **same series as `m05`** (same scaffold, chain length
   differs). A scaffold substring of the target's SMILES is the cheap way to do this.
2. Measure that series and read off how QED moves as the chain grows. The collection only
   holds **short-chain** members of the target's series, so you are extrapolating beyond
   the measured range, not interpolating inside it.
3. Do not assume the relationship is a straight line, and do not just quote the nearest
   measured molecule. Fit the trend you actually observe.

## Job 2 — pick a backup lead compound

Pick **one** molecule from the collection that satisfies **all three** constraints at once:

1. **measured QED ≥ 0.6** — no tool lists QED, so the only way to know this is to
   measure the molecule.
2. **`psa` ≥ 40** — returned free by `search_pool`.
3. **not in the same series as `m05`** — decide from the SMILES.

A molecule that fails any one of the three does not count, however good it looks
otherwise. Constraints 2 and 3 cost no measurements once you have searched, so check them
**before** spending measurements.

## Deliverables in this workspace

1. `notes.md` — your reasoning: what you searched for and why, which molecules you
   measured, the trend you fitted (state it explicitly), how you extrapolated, and how you
   narrowed down the lead.
2. `answer.json` — valid JSON, exactly these keys:

```json
{
  "target_qed": <your predicted QED for m05, a number between 0 and 1>,
  "lead_mol_id": "<the mol_id of your chosen lead compound>",
  "lead_qed_measured": <the QED you measured for that lead, a number>,
  "congener_ids": ["<every pool molecule you found in the SAME series as m05>", "..."]
}
```

Write `answer.json` with the Write tool. Every QED you cite as measured must come from a
real `measure_qed` call.
