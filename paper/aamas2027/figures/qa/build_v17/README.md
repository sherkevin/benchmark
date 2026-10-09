# Figure QA build v17

- Figure 1: `../../generated/figure1/v21_ai.png`
- Figure 2: `../../generated/figure2/v26_ai.png`
- Purpose: seven-inch paper-scale check of the current candidate pair after the role-separation revision.
- Compile: `pdflatex -interaction=nonstopmode -halt-on-error .../ai_structures_fullwidth_20261007_v17.tex`
- Render: `pdftoppm -png -r 140 ...pdf page`
- PDF: `ai_structures_fullwidth_20261007_v17.pdf`
- Independent Figure 1 review: `../../external_review/20261007_figure1_role_v31_independent_review.md` (15/16, no hard error, recommended final candidate).
- Independent Figure 2 review: `../../external_review/20261007_figure2_role_v27_independent_review.md` (15/16, role and causal path pass; secondary labels slightly small).
- This build is a candidate QA artifact. It does not replace `../main.pdf` until the user selects the pair.
