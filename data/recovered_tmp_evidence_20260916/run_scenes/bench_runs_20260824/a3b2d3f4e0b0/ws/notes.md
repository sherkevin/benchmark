# Notes: QED prediction + lead pick

## Series identification (from SMILES)

Four congeneric series, each = fixed scaffold + variable N-alkyl chain `–C(=O)N-(CH2)n`:

1. **Methoxybenzamide** (m05's series) — `COc1ccc(cc1)C(=O)N-alkyl`, psa=38.33
   - m15(C2), m24(C3), m28(C4), m17(C5), m22(C6), **m05(C11, target, unmade)**
2. **Nitrobenzamide** — `O=[N+]([O-])c1ccc(cc1)C(=O)N-alkyl`, psa=72.24
   - m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **Biphenylbenzamide** — `c1ccc(cc1)-c1ccc(cc1)C(=O)N-alkyl`, psa=29.1
   - m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)
4. **Sugar/polyol amide** — `OCC(O)C(O)C(O)C(=O)N-alkyl`, psa=110.02
   - m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)

## Job 1 — predict m05 QED (chain C11, unmade → extrapolate)

Measured methoxy series (the target's own congeneric series), short-chain only:
- m15 (C2): 0.7621
- m24 (C3): 0.7919  ← peak
- m28 (C4): 0.7518
- m17 (C5): 0.7498
- m22 (C6): 0.7377

Trend: gentle rise C2→C3 then monotonic decay C3→C6. m05 is at C11, far beyond the
measured range, so I extrapolate the decay rather than interpolate.

The decay is driven by rotb (3→6 across C2→C6) and alogp (1.445→3.005). m05 has
rotb=12, alogp=4.956 — rotb is well into the regime where QED's rotb penalty
flattens to a floor. To anchor the long-chain regime I measured two rotb≈12
members of *other* series (allowed: same prediction philosophy, scaffolds differ):

- m07 (sugar, rotb=12, alogp=-0.072): **0.3233** — clean rotb=12 anchor
- m10 (nitro,  rotb=11, alogp=4.465):  **0.3724** — rotb=11, high-alogp anchor

At rotb=11–12 the QED is ~0.32–0.37 regardless of scaffold: the rotb penalty
floor dominates over the scaffold-specific level once the chain is long. m05
(rotb=12, alogp=4.956) sits between m07 and m10 on alogp and equals m07 on rotb.

Extrapolation of the methoxy decay curve lands in the 0.32–0.37 band; weighting
the rotb=12 anchor (m07=0.3233) most heavily and nudging down slightly for m05's
very high alogp (4.956 > m10's 4.465) but up for its low PSA/low-HBD level
(keeps it off the lowest floor), I predict **m05 QED ≈ 0.35**.

## Job 2 — lead (constraints: QED≥0.6 measured, psa≥40, not methoxy series)

psa≥40 eliminates biphenyl (29.1) and methoxy (38.33) series → candidates: nitro (72.24) + sugar (110.02).
Not-same-series-as-m05 → methoxy excluded anyway.
Sugar series has very negative alogp (-2.8…0.3) → likely QED-penalized → nitro series first.

Measured nitro short chains:
- m09 (C2, mw194, alogp1.35): 0.5826  ← <0.6
- m02 (C3, mw208, alogp1.74): 0.6046  ← ≥0.6 ✓
- m12 (C4, mw222, alogp2.13): 0.4709  ← <0.6

**Lead = m02**: measured QED 0.6046 ≥ 0.6, psa 72.24 ≥ 40, nitro series ≠ m05's methoxy series. ✓ all three.

## Budget ledger
12/12 used: m15,m24,m28,m17,m22 (Job1 series) + m07,m10 (rotb-12 anchors) + m09,m02,m12 (Job2 nitro scan).
