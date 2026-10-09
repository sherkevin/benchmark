# Notes for Molecular Drug-likeness Task

## Initial Analysis
- Need to find the QED of m05 (which cannot be measured directly)
- Need to find a molecule that meets 3 criteria:
  1. QED ≥ 0.6
  2. PSA ≥ 40
  3. Not in same congeneric series as m05

## Task 1: Finding m05's QED
- m05 has SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Cannot be measured directly (probe_unavailable)
- Belongs to scaffold: COc1ccc(cc1)C(=O)N with chain length 11
- Congeneric series includes: m15 (chain 2), m24 (chain 3), m28 (chain 4), m17 (chain 5), m22 (chain 6)
- Measured QEDs:
  - m15 (chain 2): 0.7621
  - m24 (chain 3): 0.7919
  - m28 (chain 4): 0.7518
  - m17 (chain 5): 0.7498
  - m22 (chain 6): 0.7377
- Predicted QED for m05 (chain 11): 0.5416

## Task 2: Finding suitable lead molecule
- Must meet all three constraints:
  1. QED ≥ 0.6
  2. PSA ≥ 40
  3. Not in same congeneric series as m05
- Molecules with PSA ≥ 40 are from scaffolds:
  - "O=[N+]([O-])c1ccc(cc1)C(=O)N": m02, m06, m09, m10, m12, m29, m31, m32, m33 (PSA = 72.24)
  - "OCC(O)C(O)C(O)C(=O)N": m01, m03, m07, m14, m18, m19, m21, m25, m27 (PSA = 110.02)
- Measured QEDs:
  - m02: 0.6046 (QED ≥ 0.6 ✓, PSA ≥ 40 ✓, different scaffold ✓) - perfect candidate!
  - m01: 0.3187 (QED < 0.6 ✗)
  - m12: 0.4709 (QED < 0.6 ✗)
- Selected m02 as the lead molecule