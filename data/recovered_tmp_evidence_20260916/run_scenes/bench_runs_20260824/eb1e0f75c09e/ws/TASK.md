# Task

You are a medicinal chemist triaging a compound collection in THIS directory.
`data/candidates.tsv` lists 20 compounds: `mol_id`, `smiles`, and cheap descriptors that
came with the catalogue (`mw`, `clogp`, `tpsa`, `hbd`, `hba`, `rotb`, `rings`).
**The catalogue does not list solubility or brain penetration.**

## Tools

- `measure_admet(mol_id)` — run one assay on a pooled compound. Returns its measured
  aqueous solubility (`logS`, log10 mol/L) and brain penetration (`bbb`, 0 or 1).
  **Budget: 8 assays in total.** One call = one assay = 1 unit.

## Target profile

A compound qualifies only if it satisfies **both**:

1. measured `logS` between -3 and -2 inclusive
2. measured `bbb` = 1

## Deliverables in this workspace

1. `notes.md` — a record of what you did.
2. `answer.json` — **already in this directory, pre-filled with the exact key and an
   empty value. Fill in the value; do not rename the key, do not add or remove keys.**

```json
{
  "qualifying_ids": ["<mol_id>", "..."]
}
```

List **every** compound you have confirmed qualifies. Edit the existing `answer.json`
(Edit or Write tool both work). Every compound you report must have been confirmed by a
real `measure_admet` call.
