# Notes for QED Prediction Task

## Task Overview
- Predict QED for m05 (never synthesized, probe_unavailable)
- Pick a backup lead compound with: QED ≥ 0.6, psa ≥ 40, NOT in same series as m05
- Budget: 12 measurements total

## Analysis of m05 Series (Congeners)
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (methoxy-phenyl-amide with C11 chain)

Same scaffold molecules in pool (methoxy-phenyl-amide series):
- m15: C2 chain
- m24: C3 chain
- m28: C4 chain
- m17: C5 chain
- m22: C6 chain

m05 has C11 chain - much longer than any in the pool. Need to extrapolate.

## Measured QED for m05 Congeners

| Mol ID | Chain Length | QED |
|--------|-------------|-----|
| m15 | C2 | 0.7621 |
| m24 | C3 | 0.7919 |
| m28 | C4 | 0.7518 |
| m17 | C5 | 0.7498 |
| m22 | C6 | 0.7377 |

## Trend Analysis for m05 QED Prediction

The QED vs chain length shows an inverted U-shape (parabolic) pattern:
- QED increases from C2 to C3 (peak at 0.7919)
- Then steadily decreases from C3 onwards
- C3→C4: -0.0401
- C4→C5: -0.0020
- C5→C6: -0.0121

Average decline from C3 to C6: ~0.018 per carbon

For extrapolation to C11 (5 carbons beyond C6):
- Linear extrapolation: 0.7377 - 5*0.018 ≈ 0.65
- But very long alkyl chains typically hurt drug-likeness (high logP, poor solubility)
- The quadratic nature suggests the decline may accelerate for longer chains

Fitting a quadratic model to the 5 data points and extrapolating to chain=11:
The curve peaks around C3 and declines thereafter. For C11, considering the lipophilicity penalty for very long chains, I estimate QED ≈ 0.58.

**Predicted QED for m05: 0.58**

## Backup Lead Selection

Candidates with psa ≥ 40 and NOT in m05 series:

**Polyol series (psa=110.02):** m01, m03, m07, m14, m18, m19, m21, m25, m27
**Nitro series (psa=72.24):** m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurements for Backup Candidates

| Mol ID | Series | psa | QED | Passes QED≥0.6? |
|--------|--------|-----|-----|-----------------|
| m01 | polyol | 110.02 | 0.3187 | No |
| m02 | nitro | 72.24 | 0.6046 | YES ✓ |
| m03 | polyol | 110.02 | 0.3282 | No |
| m09 | nitro | 72.24 | 0.5826 | No |
| m12 | nitro | 72.24 | 0.4709 | No |
| m14 | polyol | 110.02 | 0.3237 | No |
| m27 | polyol | 110.02 | 0.3330 | No |

### Selected Lead: m02

Verification:
1. **QED ≥ 0.6**: 0.6046 ✓
2. **psa ≥ 40**: 72.24 ✓
3. **NOT in m05 series**: m02 is nitro-phenyl-amide, m05 is methoxy-phenyl-amide ✓

m02 SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` (nitro-phenyl-amide with C3 chain)

## Measurements Summary
- Used all 12 measurements
- 5 for m05 congeners: m15, m24, m28, m17, m22
- 7 for backup candidates: m01, m02, m03, m09, m12, m14, m27

## Final Answer
- target_qed: 0.58
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
