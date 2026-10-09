# Notes

## Task Overview
- Task 1: Report QED of m05 (never synthesized, need to predict from congeneric series)
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, not in same congeneric series as m05
- Budget: 12 units total (shared between list_pool and measure_qed)

## Progress

### Reading the pool
Read all 33 molecules (9 pages, budget used: 9). Remaining budget: 3.

### m05 details
- mol_id: m05
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: COc1ccc(cc1)C(=O)N- (methoxy-benzamide)
- Descriptors: mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1
- m05 has psa=38.33 which is < 40, so m05 itself doesn't qualify for Task 2 anyway

### Identifying congeneric series with m05
m05 scaffold: methoxy-benzamide (COc1ccc(cc1)C(=O)N-alkyl)
Same series molecules (same scaffold, different alkyl chain length):
- m15: COc1ccc(cc1)C(=O)NCC (ethyl chain)
- m17: COc1ccc(cc1)C(=O)NCCCCC (pentyl)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (hexyl)
- m24: COc1ccc(cc1)C(=O)NCCC (propyl)
- m28: COc1ccc(cc1)C(=O)NCCCC (butyl)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (dodecyl - 12 carbons)

For Task 2, I need to avoid these molecules.

### Molecules with psa >= 40 (Task 2 constraint)
Looking at the pool:
- psa=110.02: m01, m03, m07, m14, m18, m19, m21, m25, m27 (sugar-amide series)
- psa=72.24: m02, m06, m09, m10, m12, m29, m31, m32, m33 (nitro-benzamide series)
- psa=38.33: m05, m15, m17, m22, m24, m28 (methoxy-benzamide - same series as m05, EXCLUDED)
- psa=29.1: m04, m08, m11, m13, m16, m20, m23, m26, m30 (biphenyl-amide series)

So candidates with psa >= 40 are: m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

### Measurements made
Used all 3 remaining budget units on m05 congeneric series:
- m15 (2C chain): QED = 0.7621
- m17 (5C chain): QED = 0.7498
- m24 (3C chain): QED = 0.7919
- m28: budget exhausted, no measurement

### Task 1: m05 QED prediction
Trend from congeneric series:
- m24 (3C): 0.7919
- m15 (2C): 0.7621
- m17 (5C): 0.7498

The trend shows QED peaks around 3C and decreases with longer chains. m05 has a 12C chain with:
- High alogp (4.956) - penalizes QED
- High rotb (12) - penalizes QED
- mw=305.46 - slightly high but acceptable

Extrapolating the trend and considering the property penalties, m05's QED is estimated around 0.50-0.55.

### Task 2: Lead molecule selection
Budget exhausted before measuring any candidate. Candidates with psa >= 40 and not in m05 series:
- Nitro-benzamides: m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar-amides: m01, m03, m07, m14, m18, m19, m21, m25, m27

Best candidate based on drug-like properties: m09 (mw=194, alogp=1.345, rotb=3, psa=72.24)
Expected QED: likely >= 0.6 based on favorable properties.

STRATEGIC ERROR: Should have measured 2 from m05 series and 1 candidate for Task 2.
Instead measured 3 from m05 series, leaving 0 for Task 2.

### Final answers submitted
- Task 1 (m05 QED): 0.52 (estimated from congeneric series trend)
- Task 2 (lead): m09 (selected based on drug-like properties, but no measurement obtained)
- lead_qed_measured: null (could not measure due to budget exhaustion)

### Strategic reflection
With 12 budget units:
- 9 units needed to read all 33 molecules
- 3 units remaining for measurements

Optimal strategy would be:
- 2 measurements for m05 congeneric series (to establish trend for Task 1)
- 1 measurement for a Task 2 candidate (to get measured QED >= 0.6)

My actual strategy:
- 3 measurements for m05 series (m15, m17, m24)
- 0 measurements for Task 2 candidates

This was suboptimal. I should have measured only 2 from the m05 series and saved 1 for Task 2.

Given the measurements I have (m15=0.7621, m17=0.7498, m24=0.7919), the trend is clear enough with just 2 points. I could have skipped m24 and measured a Task 2 candidate instead.

### m05 QED prediction methodology
Using the congeneric series data:
- 2C (m15): 0.7621
- 3C (m24): 0.7919
- 5C (m17): 0.7498

The QED peaks around 3C and decreases with longer chains. For m05 with 12C:
- Chain length effect: significant decrease expected
- alogp=4.956 (high, optimal ~2-3): penalizes QED
- rotb=12 (high, optimal <5): penalizes QED

Estimated QED for m05: 0.50-0.55, with 0.52 as the point estimate.
