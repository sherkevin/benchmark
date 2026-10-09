# Independent visual review — submission_20261008_114253

2026-10-08. Read-only inspection after the coordinator confirmed the build had
completed. No LaTeX, figure source, main PDF, promotion record or TODO was edited.

## Disposition

**The requested 8-page body / page-9 References boundary, new results table,
abstract, and pages 5–8 layout pass.** All nine pages were rendered at 150 dpi
with Poppler and actually inspected. One minor Figure 1 label overrun is
recorded below; this is not a claim that the entire document has zero cosmetic
defects.

- PDF SHA-256: `be5e1f50da9ba5313ef5bd2159c288d2a2e9de84ddd271569900562a5c6b3cdc`.
- Current main source SHA-256: `0377e70fe9aec39158651b25bff6a898dbc9f4f13957cd478ec2cc1b02226930`.
- Actual size: 9 pages, each 612 × 792 PDF points.
- Body conclusion finishes on page 8. The only References heading is on page
  9, which contains references and no stranded body table.
- Renders: `independent_visual_renders/page-1.png` through `page-9.png`.

## Page-by-page observations

| Page | Actual visual check |
|---|---|
| 1 | Title, anonymous author line, abstract, CCS, introduction and license block are legible and separated. Abstract includes 128 orders, fixed priors/seeds, +4.74 pp versus Online SGD, [+4.32,+5.16], 125 wins and natural −0.29 pp. No clipping or footer collision. |
| 2 | Figure 1 and its caption are legible, and the two-column related-work/problem text flows normally. Minor first-panel label overrun described below. |
| 3 | Equations and method subsections fit their columns. Subscripts, fractions and display equations are readable; no page-edge clipping. |
| 4 | Comparator Table 1, baseline discussion and evaluation protocol fit; table and note do not overlap the next section. |
| 5 | Figure 2, experimental map, natural/drift discussion and arrival study are readable. The controlled paragraph flows from the left column to the right. +4.74 pp, MCSE 0.212, 125/128, the binomial interval and natural −0.29 pp remain visible. |
| 6 | Table 3 has ten method rows and five columns, with two clear grouped condition headers. Original-order and 128-order means are distinguishable. Figure 3 and Table 4 are separately captioned and do not overlap. Development text begins below them and remains inside the page. |
| 7 | Tables 5 and 6 use consistent readable typography; both formulas fit. The development discussion flows across the columns, followed by the post-call subsection. No material is clipped at the bottom. |
| 8 | Post-call Table 7 is now at the top of the body page, followed by Table 8 and the cost/limitations sections. The continuation sentence from the post-call subsection remains on this page. Conclusion is complete in the right column. Some ordinary float whitespace remains below Table 8; it causes no collision or overflow. |
| 9 | References start at the top, in two clear columns. Entries and long URLs stay inside the text area; no body float or conclusion text is displaced here. |

## New table and numerical presentation

Table 3's Natural / Controlled drift groups and Original / 128-order mean
subheaders align with the numeric columns. Group rules distinguish anchors,
online methods, credit permutation and the evaluator-only fixed reference.
Dash entries are explicitly explained, and the note states that order 0 is
excluded from the 128-order mean.

- Natural mean: Frozen 89.81 and MIRA-Relay 89.52, consistent with the stated
  −0.29 pp difference in the abstract and page-5 paragraph.
- Controlled mean: Online SGD 82.22 and MIRA-Relay 86.96, consistent with the
  stated +4.74 pp primary contrast.
- Original-order values remain separate: Frozen 73.38 and MIRA-Relay 87.56 in
  controlled drift. Figure 3 and Table 4 explicitly retain original-order scope.
- The 128-order uncertainty paragraph distinguishes pooled correctness from
  the equal-scaffold Local−Permutation contrast and includes the retrospective,
  fixed-cohort limitation.

PyMuPDF span inspection gives **8.9664 PDF points** for Table 3's headers,
method names and numerical cells, equivalent to **9 TeX points**. The inspected
Table 5–7 cells have the same size. Bold cells use the bold font at that size;
superscript daggers are appropriately smaller. This confirms the table was not
shrunk to fit the new columns. The 150 dpi page renders confirm readability.

## Minor cosmetic item: Figure 1, page 2

The first panel's bottom action line, “score eligible providers → choose one
a_t”, extends past the gray panel's right border by approximately 9 PDF
points. The final characters remain visible in the white gap; they are not
cropped and do not obscure the next panel. This does not affect the new table,
statistical statements or 8+1 page gate, but the panel boundary is visibly
crossed and should not be described as zero-overflow figure layout.

A 216 dpi detail is saved as
`independent_visual_renders/figure1_first_panel_detail.png`. A shorter label or
line wrap would remove this small defect; this reviewer has not edited it.

## Scope

This review checks the final rendered presentation and targeted numerical
consistency. It does not rerun experiments or verify citation metadata. The
separate independent arrival audit supplies the scientific numerical binding;
promotion remains the coordinator's action.
