# Notes on QED Prediction Task - Final

## Task Summary
- Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - never synthesized
- Pick a backup lead compound with: QED ≥ 0.6, psa ≥ 40, NOT in same series as m05
- Budget: 12 units total (search pages + measurements)

## Results

### Job 1: Predict m05 QED - COMPLETED

Congeners found (same series as m05, scaffold `COc1ccc(cc1)C(=O)N`):

| mol_id | Chain Length | QED (measured) | psa |
|--------|--------------|----------------|-----|
| m15    | C2           | 0.7621         | 38.33 |
| m24    | C3           | 0.7919         | 38.33 |
| m28    | C4           | 0.7518         | 38.33 |
| m17    | C5           | 0.7498         | 38.33 |
| m22    | C6           | 0.7377         | 38.33 |
| m05    | C11          | ??? (predict)  | 38.33 |

**Trend**: QED peaks at C3 (0.7919), then decreases with increasing chain length.

**Extrapolation method**: Linear regression on C4-C6 data
- Equation: QED = 0.78165 - 0.00705 × chain_length
- For C11: QED = 0.78165 - 0.00705×11 = 0.7041

**Predicted QED for m05: 0.70**

### Job 2: Pick Backup Lead - FAILED

Constraints:
1. QED ≥ 0.6
2. psa ≥ 40
3. Not in same series as m05

The m05 series molecules all have psa = 38.33 (< 40), so they fail constraint 2.

I attempted to find molecules from different series but exhausted my budget on unproductive searches. All searches for other scaffold patterns returned "budget_exhausted": true.

**Result**: Could not find a valid backup lead due to budget exhaustion.

## Budget Breakdown
- Search m05 series (2 pages): 2 units
- Measure 5 congeners: 5 units
- Searches for other scaffolds (7 searches): 5 units (budget exhausted after 5 searches)
- **Total: 12/12 units**

## Mistake Analysis
I should have:
1. First searched for multiple scaffolds to identify different series
2. Checked psa values from search results (free) to filter for psa ≥ 40
3. Only then measured QED for promising candidates

Instead, I:
1. Measured all m05 series congeners first (5 units)
2. Then tried to search for other series (budget exhausted before finding any)

## Final Answer
```json
{
  "target_qed": 0.70,
  "lead_mol_id": null,
  "lead_qed_measured": null,
  "congener_ids": ["m05", "m15", "m17", "m22", "m24", "m28"]
}
```

**Note**: Could not find valid backup lead due to suboptimal budget allocation. The task requires finding a molecule with psa ≥ 40 from a different series, but I exhausted my budget before finding any such molecule.
