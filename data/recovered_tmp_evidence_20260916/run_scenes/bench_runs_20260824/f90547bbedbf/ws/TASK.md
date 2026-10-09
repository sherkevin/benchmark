# Task

You are a medicinal chemist. `data/leads.tsv` lists a family of benzamide lead compounds
you can work with (each row: `smiles` and five descriptors `mw`, `alogp`, `psa`, `rotb`,
`arom`). **It does not list QED.**

Your goal: **propose ONE molecule that meets this specification**

- QED ≥ 0.85
- no structural alerts (RDKit QED ALERTS = 0)
- it must be a real catalogued compound in the answer library (verify with the tool)

## Tools

- `measure_qed(smiles)` — measure a molecule's QED (drug-likeness, 0–1) on a real sample.
  Only the lead family can be measured; anything else returns `probe_unavailable`.
  Costs 1 of 12 measurement-budget units.
- `check_molecule_exists(smiles)` — asks whether a structure is in the answer library.
  Returns only yes/no (no properties). You have at most 10 checks.

## Deliverable in this workspace

1. `notes.md` — a record of what you measured and why you chose your molecule.
2. `answer.json` — **already in this directory, pre-filled with the exact keys and empty
   values. Fill in the values; do not rename keys, do not add or remove keys.**

```json
{
  "proposed_smiles": "<SMILES of the molecule you propose>",
  "anchor_smiles": "<SMILES of the measurable lead you built on>",
  "edit_op": "<the structural change from anchor to your molecule>",
  "cited_values": [{"smiles": "<a molecule you measured>", "prop": "qed", "value": <number>}]
}
```

Every value you cite as measured must come from a real `measure_qed` call.
