# Task: predict an unmeasurable QED, and pick a lead meeting three constraints

You are a medicinal chemist. A compound collection of 33 molecules is available through
the tools below — **its catalogue is not given to you**; page through it with `list_pool`
to see `mol_id`, `smiles` and five cheap descriptors (`mw`, `alogp`, `psa`, `rotb`,
`arom`) that come free with the catalogue.

The collection contains several **congeneric series**: within one series every molecule
shares the same scaffold and differs only in the length of the N-alkyl chain. QED changes
systematically along a series, but **each scaffold has its own curve** — the shape and
the level both differ between series.

## Tools

- `measure_qed(mol_id)` — measure a molecule's QED (drug-likeness, 0–1) on a real sample.
  Budget: **17 measurements in total, shared by both jobs below**, so plan ahead.
  Molecules that were never made return `probe_unavailable` and cost nothing.

The five descriptors are free, but they are **not** the QED — QED weighs eight molecular
properties, three of which are not in the table.

## Job 1 — predict the QED of `m05`

Molecule **`m05`** (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has **never been synthesized**, so
`measure_qed` returns `probe_unavailable` for it. Predict its QED:

1. Work out which pool molecules belong to the **same series as `m05`** (same scaffold,
   chain length differs) — read the SMILES to decide.
2. Measure that series and read off how QED moves as the chain grows. The pool only holds
   **short-chain** members of the target's series, so you are extrapolating beyond the
   measured range, not interpolating inside it.
3. Do not assume the relationship is a straight line, and do not just quote the nearest
   measured molecule. Fit the trend you actually observe.

## Job 2 — pick a backup lead compound

Pick **one** molecule from the pool that satisfies **all three** constraints at once:

1. **measured QED ≥ 0.6** — the catalogue does not list QED, so the only way to know
   this is to measure the molecule.
2. **`psa` ≥ 40** — free, returned by `list_pool`.
3. **not in the same series as `m05`** — free, decide from the SMILES.

A molecule that fails any one of the three does not count, however good it looks
otherwise. Constraints 2 and 3 cost nothing to check, so check them **before** spending
measurements.

## Deliverables in this workspace

1. `notes.md` — your reasoning: which molecules you measured and why, the trend you fitted
   (state it explicitly), how you extrapolated, and how you narrowed down the lead.
2. `answer.json` — **already in this directory, pre-filled with the exact keys and
   empty values. Fill in the values; do not rename keys, do not add or remove keys.**

```json
{
  "target_qed": <your predicted QED for m05, a number between 0 and 1>,
  "lead_mol_id": "<the mol_id of your chosen lead compound>",
  "lead_qed_measured": <the QED you measured for that lead, a number>,
  "congener_ids": ["<every pool molecule in the SAME series as m05>", "..."]
}
```

Edit the existing `answer.json` (Edit or Write tool both work). Every QED you cite as
measured must come from a real `measure_qed` call.
