# Three-figure paper integration QA — 2026-10-07

**历史 AI 三图整稿试排，已被决议 0012 的绘图分工替代。** 其中 Figure 3
不再是当前候选；10 页结果只对应这份历史副本，不评价新的程序数据图。

## Candidate build

The source copy `paper/aamas2027/main_ai_jointlock_20261007.tex` replaces the
old Figure 1/2 paths with the AI joint-lock candidates and inserts Figure 3
v05 after the controlled-drift result. It was compiled with the same template,
BibTeX style, and three-pass sequence as the paper Makefile.

Build directory:
`paper/aamas2027/build/ai_jointlock_20261007_bib/`

Candidate PDF:
`main_ai_jointlock_20261007.pdf` — 10 pages total.

## Result

The figure assets themselves render and the candidate compiles, but the
10-page output does not satisfy the current AAMAS body budget (the existing
formal build is 9 pages total: 8 body pages plus references). Therefore this
candidate is **not** switched into `main.tex`.

This is a layout-budget failure only. It does not invalidate the three AI
images or the 16/16 joint-lock record. Reducing the page count requires a
deliberate paper-layout decision, such as replacing or compressing existing
result material; that work is outside the current figure-only scope and was
not performed here.
