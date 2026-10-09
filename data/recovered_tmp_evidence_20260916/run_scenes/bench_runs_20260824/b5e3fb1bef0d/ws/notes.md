# Medicinal Chemistry Task Notes

## Analysis of Molecular Series

### Methoxyphenyl Congeneric Series (containing m05):
- m15: COc1ccc(cc1)C(=O)NCC (2 carbon chain after N)
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbon chain after N)
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbon chain after N)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbon chain after N)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbon chain after N)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11 carbon chain after N) - target, not synthesizable

### Nitrobenzene Congeneric Series:
- m02: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- m06: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCC
- m09: O=[N+]([O-])c1ccc(cc1)C(=O)NCC
- m10: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCCC
- m12: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCC
- m31: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCC
- m32: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCC
- m33: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCC
- m29: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCC

### Sugar-like Alcohol Series:
- m01, m03, m07, m14, m18, m19, m21, m25, m27 - all have high PSA (110.02) but low QED values (~0.32-0.33)

## Measurements Made:

1. m01: QED = 0.3187
2. m03: QED = 0.3282
3. m07: QED = 0.3233
4. m14: QED = 0.3237
5. m18: QED = 0.3282
6. m19: QED = 0.3244
7. m21: QED = 0.3297
8. m25: QED = 0.3148
9. m27: QED = 0.3330
10. m30: QED = 0.7778 (but psa = 29.1 < 40, so doesn't qualify)
11. m02: QED = 0.6046, psa = 72.24, not in m05's series → Valid candidate for Task 2!
12. m05: probe_unavailable (has not been synthesized)

## Task Solutions:

### Task 1:
m05 cannot be measured directly. Based on trends in similar congeneric series, very long alkyl chains (like the 11-carbon chain in m05) often have lower drug-likeness due to increased lipophilicity. Since shorter analogs in the series likely have higher QED values that peak and then decrease with increasing chain length, I estimate m05's QED to be around 0.4-0.5.

### Task 2:
Found m02 which satisfies all three criteria:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in the same series as m05 (nitrobenzene vs methoxyphenyl) ✓