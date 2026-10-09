# Notes: Finding the Best Structural Edit for Drug-likeness

## Parent Compound Analysis
SMILES: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`

Properties:
- QED: 0.6136 (needs improvement to ≥0.8)
- MW: 309.45 (reasonable)
- ALOGP: 5.444 (HIGH - ideal is ~0-5)
- PSA: 29.1 (OK, under 40)
- ROTB: 9 (at limit)
- AROM: 2

**Diagnosis**: The main problems are:
1. High lipophilicity (ALOGP 5.444) - the long C8 alkyl chain is too hydrophobic
2. ROTB at 9 - the long flexible chain contributes to high rotatable bonds

**Strategy**: Look for edits that shorten the alkyl chain to reduce ALOGP and ROTB.

## Search Results

Searched pool with various queries. Found these candidates:

### Candidates passing psa ≤ 40 AND rotb ≤ 9:
- **e04**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC` - psa=29.1, rotb=5, alogp=3.884 (C4 chain)
- **e05**: `Fc1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC` - psa=29.1, rotb=9, alogp=5.583 (F substitution)
- **e08**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCC` - psa=29.1, rotb=4, alogp=3.493 (C3 chain)

### Candidates failing constraints:
- e01: psa=72.19 (>40), rotb=10 (>9) - amide terminal, FAILS
- e02: psa=66.4 (>40), rotb=10 (>9) - carboxylic acid terminal, FAILS
- e03: psa=72.24 (>40), rotb=10 (>9) - nitro group, FAILS
- e06: psa=29.1 (OK), rotb=10 (>9) - longer chain C9, FAILS
- e07: psa=55.12 (>40), rotb=10 (>9) - amine terminal, FAILS
- e10: psa=29.1 (OK), rotb=11 (>9) - much longer chain C10, FAILS
- e11: psa=41.99 (>40), rotb=9 (OK) - pyridine ring, FAILS psa

## Budget Issue
Unfortunately, I exhausted the budget (11/11) on search operations before measuring QED. 
The measure_qed call returned budget_exhausted=true.

## Analysis
The parent's main problem was high ALOGP (5.444). Candidates that shorten the chain:
- e08 (C3 chain): alogp=3.493 - largest reduction
- e04 (C4 chain): alogp=3.884 - good reduction

Since the parent's QED was limited by high lipophilicity, e08 with the lowest alogp (3.493) 
and lowest rotb (4) should have the highest QED improvement.

**Failed direction**: Lengthening the alkyl chain (e06, e10) made properties worse - 
higher alogp and higher rotb. Also, adding polar terminal groups (e01, e02, e07) 
increased PSA too much and often increased rotb.

## Conclusion
- **Best candidate (likely)**: e08 - shortest chain (C3), lowest alogp (3.493), lowest rotb (4)
- **Pick candidate**: e08 - passes psa=29.1≤40 and rotb=4≤9; QED likely ≥0.8 due to much improved alogp
- **Failed direction**: "Lengthening alkyl chain" - candidates e06 (C9) and e10 (C10) both failed with rotb > 9 and worse alogp

Note: Could not measure QED due to budget exhaustion. Conclusions based on descriptor analysis.
The parent's high alogp (5.444) was the main QED limiter. e08 reduces alogp to 3.493, which should
significantly improve QED. e05 (fluorine substitution) does NOT improve alogp (5.583 > 5.444), so
it's unlikely to achieve high QED despite passing psa/rotb constraints.
