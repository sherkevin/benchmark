# Three-figure AI joint-lock review — 2026-10-07

**历史记录，已由决议 0012 替代（2026-10-07）。** 两张 AI 结构图候选
继续保留，Figure 3 已改为 `figure3/v02_data/` 程序图；下文的 16/16 是
当时的视觉自检，不能证明 AI 数据图坐标精确，不再作为当前联合锁定依据。

## Scope

This review covers only the three current AI-generated figure candidates. It
does not approve experimental claims, the paper body, or a switch in
`main.tex`.

| Figure | Candidate | Canvas | SHA-256 |
|---|---|---:|---|
| Figure 1 overview | `generated/figure1/v05_ai.png` | 2079×756 | `5fd6838daa4b166fcaf5a99a166ba2166c2e2cfa59bcae3ea124f5c5e57c2355` |
| Figure 2 lifecycle | `generated/figure2/v22_ai.png` | 2117×743 | `d23b23d14fb06ca8509f484f383a4555ee1fe8b717f231ce25794fcb3692e31c` |
| Figure 3 evidence | `generated/figure3/v05_ai.png` | 2087×753 | `f019e48a7cdd1d06687bfa30350052f5334362b2ee2a957667396f4ecaa2c1a3` |

## Cross-figure invariants

1. **Method naming:** `MIRA-Relay` is spelled consistently in the overview,
   lifecycle update block, cost legend, and rescued/harmed categories.
2. **Color semantics:** navy/blue marks the selector or learned state, gray
   marks frozen/baseline/unobserved elements, amber marks terminal evaluation,
   and purple marks delayed feedback. No panel uses an extra semantic color.
3. **Information visibility:** Figures 1–2 show selected-only feedback and do
   not expose unselected outputs; Figure 3 reports only the receipt-bound
   result roles and does not imply unobserved counterfactual outputs.
4. **Visual language:** all three use white backgrounds, compact rounded
   modules or panel tabs, short labels, dark navy text, consistent arrows or
   axes, and restrained light-blue borders/grids.
5. **Paper scale:** all three candidates were checked in the same 7-inch
   full-width PDF QA render `qa/build_v05/ai_candidates_fullwidth_20261007_v05.pdf`;
   page 1, page 2, and page 3 remain readable.

## 16-point gate

| Figure | Semantic contract | Causal/data flow | Gallery style | Paper readability | Total | Hard-error check |
|---|---:|---:|---:|---:|---:|---|
| Figure 1 v05 | 4 | 4 | 4 | 4 | **16/16** | pass |
| Figure 2 v22 | 4 | 4 | 4 | 4 | **16/16** | pass |
| Figure 3 v05 | 4 | 4 | 4 | 4 | **16/16** | pass |

No stale comparator, queue bypass, input-to-feedback shortcut, repeated
selected-provider candidate, ambiguous `Other` legend, invented value, or
unobserved output is visible in the current chain. The panel (c) labels at
the lower-left and upper-right are close but remain distinguishable at the
paper-scale QA render.

## Decision

The three candidates pass the **joint visual/semantic lock candidate** gate and
are the only versions to carry forward:

- Figure 1 v05;
- Figure 2 v22;
- Figure 3 v05.

All previous versions remain archived. The formal paper path is intentionally
unchanged until the owner explicitly accepts the final three-figure switch;
this record does not silently modify `main.tex`.
