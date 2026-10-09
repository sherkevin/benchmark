# Task

You are a medicinal chemist. A compound collection of 33 molecules is available through
the tools below — **its catalogue is not given to you**. Each entry has `mol_id`, `smiles`
and five descriptors (`mw`, `alogp`, `psa`, `rotb`, `arom`). **The catalogue does not
list QED.**

## Tools

- `list_pool(page)` — list the catalogue, 4 entries per page.
- `measure_qed(mol_id)` — measure a molecule's QED (drug-likeness, 0–1) on a real sample.
  A molecule that has never been synthesized returns `probe_unavailable` and costs nothing.

**Both tools draw on one shared budget of 17 units**, covering both tasks below:
each successful page costs 1 and each successful measurement costs 1.

## Task 1

Report the QED of **`m05`** (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`). It has never been synthesized, so
`measure_qed` returns `probe_unavailable` for it.

## Task 2

Pick **one** molecule from the pool that satisfies **all three** constraints at once:

1. measured QED ≥ 0.6
2. `psa` ≥ 40
3. not in the same congeneric series as `m05` — two molecules are in the same series
   when they share the same scaffold and differ only in the length of the N-alkyl chain

A molecule that fails any one of the three does not count, however good it looks otherwise.

## Deliverables in this workspace

1. `notes.md` — a record of what you did.
2. `answer.json` — **already in this directory, pre-filled with the exact keys and empty
   values. Fill in the values; do not rename keys, do not add or remove keys.**

```json
{
  "target_qed": <your QED for m05, a number between 0 and 1>,
  "lead_mol_id": "<the mol_id of the molecule you picked in Task 2>",
  "lead_qed_measured": <the QED you measured for it, a number>
}
```

Edit the existing `answer.json` (Edit or Write tool both work). Every QED you report as
measured must come from a real `measure_qed` call.
