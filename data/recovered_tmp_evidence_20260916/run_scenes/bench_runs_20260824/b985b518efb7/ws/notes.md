Medicinal chemistry task notes

Task 1: Need to find the QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC), which cannot be measured directly as it's not synthesized.

Task 2: Need to find a molecule that meets all three criteria:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in the same congeneric series as m05

MY APPROACH WAS FLAWED:
I used all 12 budget units on listing pages (7 actual pages + additional attempts), leaving 0 budget for QED measurements. This means I have no actual QED values to work with, which makes completing the tasks impossible.

CORRECT APPROACH SHOULD HAVE BEEN:
1. Use 2-3 budget units to list some pages to understand the compound pool
2. Use 1-2 budget units to measure QEDs of compounds from m05's series to establish a trend
3. Use remaining budget to measure promising candidates for Task 2

For Task 1: I needed to measure several compounds from the same series as m05 (scaffold COc1ccc(cc1)C(=O)N with varying chain lengths) and use fit_trend to predict m05's QED. The series includes: m15 (chain_len: 2), m24 (chain_len: 3), m28 (chain_len: 4), m17 (chain_len: 5), m22 (chain_len: 6), and m05 (chain_len: 11).

For Task 2: I needed to identify compounds with psa ≥ 40 (from my listings I saw that compounds with scaffold O=[N+]([O-])c1ccc(cc1)C(=O)N have psa=72.24 and compounds with scaffold OCC(O)C(O)C(O)C(=O)N have psa=110.02) that are not in the same series as m05, then measure their QED to find one ≥ 0.6.

Since I have no QED measurements, my answer.json contains guesses which is not a proper solution to the tasks.