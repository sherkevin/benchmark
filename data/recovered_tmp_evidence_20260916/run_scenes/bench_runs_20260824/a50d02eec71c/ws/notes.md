# Notes: QED Prediction and Lead Selection

## Job 1 — Predicting QED of m05

### Target molecule
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
- N-alkyl chain length: 11 carbons
- Status: never synthesized → `measure_qed` returns `probe_unavailable`

### Identifying the congeneric series
Using `describe_structure` (free), I confirmed that the following pool molecules share the same scaffold `COc1ccc(cc1)C(=O)N`:

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15    | 2         | COc1ccc(cc1)C(=O)NCC |
| m24    | 3         | COc1ccc(cc1)C(=O)NCCC |
| m28    | 4         | COc1ccc(cc1)C(=O)NCCCC |
| m17    | 5         | COc1ccc(cc1)C(=O)NCCCCC |
| m22    | 6         | COc1ccc(cc1)C(=O)NCCCCCC |

**congener_ids: m15, m24, m28, m17, m22**

### Measured QED values (5 measurements used)

| mol_id | chain_len | QED |
|--------|-----------|------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Observed trend
QED peaks at chain_len=3 (0.7919), then decreases steadily through chain_len=6 (0.7377). The relationship is non-linear: there's an initial rise from chain 2→3, followed by a monotonic decline.

### Fitted trend and extrapolation
I compared polynomial fits of different degrees using all 5 data points:

- **Degree 1 (linear):** predicts QED = 0.695 at chain 11 (RMSE = 0.0131)
- **Degree 2 (quadratic):** predicts QED = 0.5416 at chain 11 (RMSE = 0.0119) ← chosen
- **Degree 3 (cubic):** predicts QED = 2.13 at chain 11 → clearly overfitting (QED must be 0–1)
- **Degree 4:** predicts QED = −12.1 → extreme overfitting

The **quadratic (degree 2)** is the best choice:
- It captures the curvature (peak near chain 3 then decline)
- It has the best RMSE among sensible models
- The task explicitly says "Do not assume the relationship is a straight line"
- Coefficients: 0.7493 + 0.0170·x − 0.00326·x²

**Predicted QED for m05 (chain_len=11): 0.5416**

---

## Job 2 — Lead Compound Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free screen)
3. Not in the same series as m05

### Screening
- `screen_candidates()` returned 18 molecules with PSA ≥ 40
- None of the m05-series members (m15, m17, m22, m24, m28) appear in this list (their PSA = 38.33 < 40), so constraint 3 is automatically satisfied for all PSA-passing candidates
- From the 18 PSA-passing candidates, I focused on the nitrobenzene series (moderate logP, drug-like properties) with shorter chain lengths

### Measurements for lead selection (3 measurements used)

| mol_id | series | QED | QED ≥ 0.6? |
|--------|--------|------|------------|
| m02    | nitrobenzene, chain 3 | 0.6046 | ✓ |
| m12    | nitrobenzene, chain 4 | 0.4709 | ✗ |
| m32    | nitrobenzene, chain 5 | 0.4683 | ✗ |

### Selected lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: **0.6046** (≥ 0.6 ✓)
- PSA: **72.24** (≥ 40 ✓)
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzamide — different from m05's methoxybenzamide ✓)

---

## Budget summary
- Pages listed: 9 (budget units 1–9)
- Series measurements: 5 (m15, m24, m28, m17, m22; budget units 10–14)
- Lead candidate measurements: 3 (m02, m12, m32; budget units 15–17)
- **Total: 17/17 budget used**
