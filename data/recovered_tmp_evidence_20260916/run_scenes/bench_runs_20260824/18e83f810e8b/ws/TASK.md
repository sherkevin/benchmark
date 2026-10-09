# Task: predict the QED of a molecule that cannot be measured

You are a medicinal chemist. A compound collection is available through the tools below
— **you are not given its catalogue**; you have to search it yourself.

The collection contains several **congeneric series**: within one series every molecule
shares the same scaffold and differs only in the length of the N-alkyl chain. QED changes
systematically along a series, but **each scaffold has its own curve** — the shape and
the level both differ between series.

## The target

Molecule **`m05`** (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has **never been synthesized**, so
`measure_qed` returns `probe_unavailable` for it. Its QED must be **predicted**.

## Tools

- `search_pool(query, page)` — find pool molecules whose SMILES contains `query`.
  Returns `mol_id`, SMILES and five free descriptors (`mw`, `alogp`, `psa`, `rotb`,
  `arom`), 4 per page — **no QED**.
- `measure_qed(mol_id)` — measure a molecule's QED (drug-likeness, 0–1) on a real sample.

**Both tools draw on one shared budget of 12 units**: every successful page of
`search_pool` costs 1, and every successful measurement costs 1. Paging blindly through
the whole collection will exhaust the budget before you measure anything — search with a
substructure of the target instead.

The five descriptors are free, but they are **not** the QED — QED weighs eight molecular
properties, three of which are not returned.

## Goal

1. Find the molecules in the **same series as `m05`** (same scaffold, chain length
   differs). A scaffold substring of the target's SMILES is the cheap way to do this.
2. Measure that series and read off how QED moves as the chain grows. The collection only
   holds **short-chain** members of the target's series, so you are extrapolating beyond
   the measured range, not interpolating inside it.
3. Predict the QED of `m05`. Do not assume the relationship is a straight line, and do
   not just quote the nearest measured molecule. Fit the trend you actually observe.

## Deliverables in this workspace

1. `notes.md` — your reasoning: what you searched for and why, which molecules you
   measured, the trend you fitted (state it explicitly), and how you extrapolated.
2. `answer.json` — **already in this directory, pre-filled with the exact keys and
   empty values. Fill in the values; do not rename keys, do not add or remove keys.**

```json
{
  "target_qed": <your predicted QED for m05, a number between 0 and 1>,
  "congener_ids": ["<every pool molecule you found in the SAME series as m05>", "..."]
}
```

Edit the existing `answer.json` (Edit or Write tool both work). Every QED you cite as
measured must come from a real `measure_qed` call.
