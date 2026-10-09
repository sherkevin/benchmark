# Task: predict the QED of a molecule that cannot be measured

You are a medicinal chemist working in THIS directory. `data/pool.tsv` lists 33
molecules from a compound collection: `mol_id`, `smiles`, and five cheap descriptors
(`mw`, `alogp`, `psa`, `rotb`, `arom`) that came free with the catalogue.

## The target

Molecule **`m05`** — SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`.

It has **never been synthesized**, so there is no sample and `measure_qed` will
return `probe_unavailable` for it. Its QED must be **predicted**.

## Tools

- `measure_qed(mol_id)` — measure a molecule's QED (drug-likeness, 0–1) on a real
  sample. Budget: **8 measurements** for 33 molecules, so choose carefully.
  Molecules that were never made return `probe_unavailable` and cost nothing.

The five descriptors in `pool.tsv` are free, but they are **not** the QED — QED
weighs eight molecular properties, three of which are not in the table.

## Goal

The collection contains several **congeneric series**: within one series every
molecule shares the same scaffold and differs only in the length of the N-alkyl
chain. QED changes systematically along a series, but **each scaffold has its own
curve** — the shape and the level both differ between series.

So:

1. Work out which molecules in the pool belong to the **same series as `m05`**
   (same scaffold, chain length differs). Read the SMILES to decide.
2. Spend your measurement budget on that series, and read off how QED moves as the
   chain grows. Note that the pool only contains **short-chain** members of the
   target's series, while other series are represented over a wider range — so you
   are extrapolating beyond the measured range, not interpolating inside it.
3. Predict the QED of `m05`.

Do not assume the relationship is a straight line, and do not just quote the
nearest measured molecule. Fit the trend you actually observe.

## Deliverables in this workspace

1. `notes.md` — your reasoning: how you identified the target's series, which
   molecules you measured and why, the trend you fitted (state it explicitly), and
   how you extrapolated to `m05`.
2. `answer.json` — valid JSON, exactly:

```json
{
  "target_qed": <your predicted QED for m05, a number between 0 and 1>,
  "congener_ids": ["<every pool molecule in the SAME series as m05>", "..."],
  "trend": "<the trend you fitted, in one sentence>",
  "reasoning": "<one or two sentences>"
}
```

Write `answer.json` with the Write tool. Every QED you cite as measured must come
from a real `measure_qed` call; the predicted value must be justified in notes.md.
