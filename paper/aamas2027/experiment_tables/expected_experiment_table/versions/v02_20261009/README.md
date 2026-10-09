# Expected experiment matrix v02

This directory contains the clean expected matrix for the current canonical paper PDF.

- Binding source: \`paper/aamas2027/main.tex\`
- Binding PDF: \`paper/aamas2027/main.pdf\`
- Matrix structure: Tables 1--8 and the Figure 3 delay data, in paper order
- Status: planning targets only; no cell is evidence or a closure result

The v01 file was not promoted because it added Table 8b, a future sealed Table 9, and a Table 10 companion; it also mixed Replay-SGD into a roster that is absent from the paper, reversed the documented negative E3 diagnostic, and bound an old source hash. v02 keeps the exact paper roster, E1--E6 map, row order, column semantics, and delay-gain metric.

The Guide v09 reviews remain HOLD/PENDING. In particular, its primary-ablation and paired-CI requirements are not all represented by the current paper tables; they must not be inferred from this planning matrix.