# Notes: Finding Optimal Drug-Likeness Edit

## Parent Compound Analysis
**SMILES:** `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`

**Properties:**
- QED: 0.6136 (poor - needs improvement)
- MW: 309.45
- ALOGP: 5.444
- PSA: 29.1
- ROTB: 9
- AROM: 2

## Initial Diagnosis

Looking at typical QED optimization targets:
1. **MW**: 309.45 - Slightly high but acceptable (target < 500)
2. **ALOGP**: 5.444 - Too high (ideal range 0-5)
3. **PSA**: 29.1 - Good (target < 140)
4. **ROTB**: 9 - At threshold (target ≤ 10)
5. **AROM**: 2 - Good

**Primary issue:** The parent's ALOGP (5.444) is too high. Lipophilicity should be reduced to improve QED.

**Secondary concerns:** MW is borderline high, ROTB is at the upper limit.

## Strategy
1. Search for edits that reduce lipophilicity (ALOGP)
2. Focus on substructures that might improve hydrophilicity
3. Check PSA ≤ 40 and ROTB ≤ 9 constraints before measuring QED
4. Use budget efficiently (11 total units: search pages + measurements)

## Initial Search Results (Page 1)
Search query: `C(=O)NCCCCCCCC` (amide chain region)
Found 15 total matches

**Candidates from page 1:**
1. **e01**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC(=O)N`
   - MW: 338.45, ALOGP: 3.909 (better!), PSA: 72.19 (too high > 40), ROTB: 10 (> 9)
   - Terminal amide addition reduces lipophilicity but increases PSA and ROTB

2. **e02**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC(=O)O`
   - MW: 339.44, ALOGP: 4.509 (better), PSA: 66.4 (> 40), ROTB: 10 (> 9)
   - Terminal carboxylic acid reduces lipophilicity but increases PSA and ROTB

3. **e03**: `O=[N+]([O-])c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 354.45, ALOGP: 5.352 (slightly better), PSA: 72.24 (> 40), ROTB: 10 (> 9)
   - Nitro group on aromatic ring, increases PSA significantly

4. **e05**: `Fc1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 327.44, ALOGP: 5.583 (worse!), PSA: 29.1 (good), ROTB: 9 (good)
   - Fluorine substitution increases lipophilicity

**Observations:**
- Terminal polar groups (e01, e02) reduce ALOGP but violate PSA and ROTB constraints
- Aromatic substitutions (e03, e05) may not help enough with ALOGP
- Need to check other pages for more candidates

## Search Results (Page 2)

**Candidates from page 2:**
5. **e06**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCC`
   - MW: 323.48, ALOGP: 5.834 (worse), PSA: 29.1 (good), ROTB: 10 (> 9)
   - Longer alkyl chain increases lipophilicity and ROTB

6. **e07**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCN`
   - MW: 324.47, ALOGP: 4.383 (better!), PSA: 55.12 (> 40), ROTB: 10 (> 9)
   - Terminal amine reduces ALOGP but increases PSA and ROTB

7. **e10**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCCC`
   - MW: 337.51, ALOGP: 6.224 (worse), PSA: 29.1 (good), ROTB: 11 (> 9)
   - Even longer alkyl chain worsens both ALOGP and ROTB

8. **e11**: `c1ccncc1-c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 310.44, ALOGP: 4.839 (better!), PSA: 41.99 (> 40 by 2), ROTB:, 9 (good)
   - Pyridine substitution reduces lipophilicity, PSA slightly above threshold

**Key insight from e11:** Heteroaromatic substitution (phenyl → pyridine) reduces ALOGP significantly (5.444 → 4.839). However, PSA is 41.99, just above the 40 threshold.

## Search Results (Page 3)

**Candidates from page 3:**
9. **e12**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCN(C)C`
   - MW: 352.52, ALOGP: 4.986 (better), PSA: 32.34 (good!), ROTB: 11 (> 9)
   - Terminal dimethyl amine reduces ALOGP, PSA good but ROTB too high

10. **e16**: `c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 233.35, ALOGP: 3.777 (much better!), PSA: 29.1 (good), ROTB: 8 (good), AROM: 1
   - One phenyl ring removed! MW reduced significantly, ALOGP improved
   - This looks promising - all free constraints met (PSA ≤ 40, ROTB ≤ 9)

11. **e17**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCO`
   - MW: 325.45, ALOGP: 4.416 (better), PSA: 49.33 (> 40), ROTB: 10 (> 9)
   - Terminal hydroxyl reduces ALOGP but increases PSA and ROTB

12. **e19**: `COc1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 339.48, ALOGP: 5.453 (similar), PSA: 38.33 (good!), ROTB: 10 (> 9)
   - Methoxy substitution gives good PSA but similar ALOGP, ROTB too high

**Promising candidate: e16** - Removing one phenyl ring dramatically improves ALOGP (3.777) while meeting PSA and ROTB constraints. MW also reduced significantly.

## Search Results (Page 4)

**Candidates from page 4:**
13. **e20**: `Clc1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 343.9, ALOGP: 6.097 (worse), PSA: 29.1 (good), ROTB: 9 (good)
   - Chlorine substitution increases lipophilicity

14. **e21**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 309.45, ALOGP: 5.444, PSA: 29.1, ROTB: 9, AROM: 2
   - This is the parent compound itself

15. **e22**: `Oc1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
   - MW: 325.45, ALOGP: 5.15 (slightly better), PSA: 49.33 (> 40), ROTB: 9 (good)
   - Hydroxyl substitution slightly reduces ALOGP but increases PSA

**Summary of all 15 candidates searched:**
- e16 looks most promising: ALOGP 3.777 (significantly improved), PSA 29.1 (≤ 40), ROTB 8 (≤ 9)
- Other candidates either don't improve ALOGP enough or violate PSA/ROTB constraints
- Need to measure QED for promising candidates

## New Search: Aromatic region modifications
Search query: `c1ccc(cc1)-c1ccc(cc1)` (biphenyl core)
Found 21 total matches

**Candidates from new search page 1:**
1. **e01**: Already seen - terminal amide
2. **e02**: Already seen - terminal carboxylic acid  
3. **e03**: Already seen - nitro substitution
4. **e04**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC`
   - MW: 253.34, ALOGP: 3.884 (much better!), PSA: 29.1 (good), ROTB: 5 (good)
   - Shorter alkyl chain (6 carbons instead of 8) - reduces lipophilicity significantly
   - All constraints met: PSA ≤ 40, ROTB ≤ 9

**New promising candidate: e04** - Shorter chain reduces ALOGP dramatically while meeting all constraints.

**Candidates from new search page 2:**
5. **e05**: Already seen - fluorine substitution
6. **e06**: Already seen - longer chain
7. **e07**: Already seen - terminal amine
8. **e08**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCC`
   - MW: 239.32, ALOGP: 3.493 (best so far!), PSA: 29.1 (good), ROTB: 4 (good)
   - Very short alkyl chain (3 carbons) - dramatically reduces lipophilicity
   - All constraints met: PSA ≤ 40, ROTB ≤ 9

**New best candidate: e08** - Shortest chain gives best ALOGP (3.493) while meeting all constraints.

## QED Measurements

**e08 measurement:** QED = 0.869 ✓
- Well above target of討論 0.8
- All constraints satisfied: PSA 29.1 ≤ 40, ROTB 4 ≤ 9
- Currently the best candidate found

**e04 measurement:** QED = 0.8048 ✓
- Above target of 0.8 but lower than e08
- All constraints satisfied: PSA 29.1 ≤ 40, ROTB 5 ≤ 9

**e16 measurement:** QED = 0.6802 ✗
- Below target of 0.8 despite good ALOGP (3.777)
- Shows QED depends on more than just the five visible descriptors
- Removing one phenyl ring didn't work as expected

**Current ranking:**
1. e08: QED 0.869 (best so far)
2. e04: QED 0.8048
3. e16: QED 0.6802 (not viable)

## New Search Results (Page 3)

**Candidates from page 3:**
9. **e09**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCC`
   - MW: 225.29, ALOGP: 3.103 (best lipophilicity!), PSA: 29.1 (good), ROTB: 3 (good)
   - Extremely short chain (2 carbons) - could have even higher QED than e08
   - All constraints met

10. **e10**: Already seen - very long chain
11. **e12**: Already seen - terminal dimethyl amine
12. **e13**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCC`
   - MW: 295.43, ALOGP: 5.054 (worse than parent), PSA: 29.1 (good), ROTB: 8 (good)
   - 7-carbon chain - not promising

**New candidate e09** looks even better than e08 based on ALOGP (3.103 vs 3.493).

**Remaining budget:** 1 unit (used 10 so far: 7 searches + 3 measurements)

## Final QED Measurements

**e09 measurement:** QED = 0.854 ✓
- Above target of 0.8 but lower than e08 (0.869)
- All constraints satisfied: PSA 29.1 ≤ 40, ROTB 3 ≤ 9

**Final ranking of viable candidates (QED ≥ 0.8, PSA ≤ 40, ROTB ≤ 9):**
1. **e08**: QED 0.869 (best) - SMILES: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCC`
2. **e09**: QED 0.854 - SMILES: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCC`
3. **e04**: QED 0.8048 - SMILES: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC`

**Failed direction:** Removing one phenyl ring (e16) did NOT work despite improving ALOGP (3.777). QED was only 0.6802, below target. This shows that QED depends on more factors than just the five visible descriptors.

**Budget usage:** 11 units total (7 search pages + 4 measurements)

**Conclusion:** Shortening the alkyl chain from 8 carbons to 2-6 carbons successfully improves drug-likeness. The optimal length appears to be 3 carbons (e08) giving QED 0.869. The failed direction was removing one phenyl ring, which unexpectedly lowered QED despite improving lipophilicity.