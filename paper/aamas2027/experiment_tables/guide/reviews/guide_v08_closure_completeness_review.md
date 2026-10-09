# Guide v08 closure completeness review

Date: 2026-10-08  
Scope: read-only comparison of `versions/v08_20261008/guide.tex` with effective Benchmark/Baseline contract v004, evaluation standard v006, and the Gate closure record. No Guide edits and no experiments.

## Executive verdict

**FAIL as a sufficient closure specification; PASS as a strong target matrix.**

If real experiments satisfy every row in v08, the resulting evidence would be sufficient to *seek* a fresh v006 scoring decision and would likely cover most H/B/Gate evidence. The Guide itself cannot close acceptance: v006 requires an independent PASS/FAIL scoring record, evidence paths/hashes, two-person or equivalent review, and separate Gate A/B/C decisions. Several v08 rows also add unapproved outcome thresholds or leave provenance/claim details ambiguous. Therefore the correct status is **HOLD until the following closure defects are resolved and the evidence is re-scored**.

## 1. Gate-level decision

| Gate | Verdict | Reason |
|---|---|---|
| Gate A | **CONDITIONAL / not self-closing** | v08 specifies truth, routing, split, lineage and family evidence well. It does not require the v006 score sheet with A1–A4 scores, evidence paths, hashes, reviewer identity and explicit H1–H4 PASS. The benchmark contract also needs the current authority documents and conflict receipts, not only a future Guide checkbox. |
| Gate B | **CONDITIONAL / not self-closing** | Baseline roles and same-contract requirements are explicit. However, “every primary baseline has raw run and recovery” is not a complete source/adapter fidelity receipt, and the BayesianRouter/LinTS row leaves the canonical implementation and prior unresolved. v006 requires all four Gate-B subitems ≥3, not merely a populated roster. |
| Gate C | **FAIL as written for closure** | E1–E6 and evidence fields are comprehensive, but v08 introduces outcome margins without a documented decision record and maps results to a future Table 9 that is absent from the current manuscript mapping. v006 requires E1–E5 Stage-A evidence first, then a separately frozen Stage-B E6 confirmation; v08's “E6按声明范围完成” plus “current target makes E6 mandatory” conflates a scope choice with the standard's stage transition. |

## 2. H1–H10 audit

| ID | v08 coverage | Verdict / gap |
|---|---|---|
| H1 | Truth source/version/hash and regeneration are required (sections 2, 4, 10). | **PASS target, evidence missing.** Must name the authoritative manifest and attach its hash in the eventual score sheet. |
| H2 | Duplicate, canonical conflict, alias, unknown rules are stated. | **PASS target, evidence missing.** Require raw conflict receipt and cleaned/raw denominators, not only a rule sentence. |
| H3 | Molecule/scaffold split, confirmation and provider exposure are listed. | **PASS target, evidence missing.** Must include provider training-member audit and overlap report paths. |
| H4 | Provider source, commit, checkpoint, environment and failure semantics. | **PASS target, evidence missing.** License and deterministic/independent receipt are in B4 but should be attached to H4 evidence. |
| H5 | Explicit selected-only visibility and no full-panel access. | **PASS target.** Independent leakage checker and rejection of illegal traces are required; a prose contract alone cannot pass. |
| H6 | Same input/menu/feature/prior/feedback/delay/budget/seed/compute. | **PASS target, conditional evidence.** “Compute” must be operationalized (wall-clock boundary and hardware); warm-start exceptions must be named per method. |
| H7 | Canonical, modern nearest, matched, fixed and evaluator controls. | **PASS target.** Retrospective Best Fixed must remain outside deployable ranking in every displayed result. |
| H8 | Freeze and one-shot confirmation described. | **PASS only for a completed Stage-B receipt.** v08 must record freeze timestamp/commit and prove no label return; current Guide cannot assert it. |
| H9 | Metrics, denominators, failures, unknowns and uncertainty are said to be frozen. | **CONDITIONAL.** The new E1–E6 numerical margins are themselves post-v006 additions; they need a dated ADR/preregistration before execution. |
| H10 | Raw JSONL/config/hash/verifier and regeneration are required. | **PASS target, evidence missing.** The score must point to each raw artifact and independent rerun result. |

## 3. B1–B15 audit

| ID | Verdict | Specific issue |
|---|---|---|
| B1 | Conditional PASS | Truth authority is described, but no concrete evidence path/hash is bound in v08. |
| B2 | Conditional PASS | Endpoint and label rules are present; require explicit TDC BBB archive identifier and regeneration receipt. |
| B3 | Conditional PASS | Split/overlap requirements are present; confirmation freshness must be proven, not asserted. |
| B4 | Conditional PASS | Lineage fields are named; every provider needs an actual checkpoint/training-member/independent rerun receipt. |
| B5 | PASS target | Machine contract is detailed; validator output remains to be produced. |
| B6 | PASS target | Visible history/release-time checker is required; no checker artifact is mapped to a named path. |
| B7 | PASS target | Delays and pressure are covered. Natural lifecycle/arrival is correctly optional by scope, but any claim must be separately declared. |
| B8 | Conditional PASS | Fairness dimensions are complete; the hard E5 `100 μs` kernel cap is not a v006 requirement and cannot replace the required common budget/compute evidence. |
| B9 | PASS target | Roster is broad and role-separated. |
| B10 | Conditional PASS | “BayesianRouter / LinTS” combines distinct method families and does not identify one canonical adapter. Split into separate rows or state which one is in-scope. |
| B11 | PASS target | Family macro/exposure/joint errors are required. |
| B12 | PASS target | Metrics and utility coverage is complete; utility must be declared primary/secondary before execution. |
| B13 | PASS target | Paired/scaffold/family uncertainty is specified; exact CI method and multiplicity policy need a preregistered script. |
| B14 | Conditional PASS | E1–E6 are mapped, but v006 Stage A requires E1–E5 before confirmation and does not require E6 unless Stage B is entered. v08 makes E6 mandatory without recording that as a project decision. |
| B15 | Conditional PASS | Reproducibility fields are complete; no actual freeze, command, environment or recovery paths exist in a target Guide. |

## 4. E1–E6 and threshold audit

| Experiment | Verdict | Finding |
|---|---|---|
| E1 | **Needs decision record** | “Best Fixed > Random and ≥3/5 folds” is a sensible validity check but is not a v006 threshold. If it is a hard benchmark gate, record why and what happens when headroom is heterogeneous; otherwise label it diagnostic. |
| E2 | **Needs decision record** | `+2.0 pp`, CI lower bound `+0.5 pp`, and natural `−1.0 pp` non-inferiority are new project thresholds. They are not supported by v006 or v07 as facts. They must be frozen in an ADR/preregistration and tied to one primary comparator and estimand. |
| E3 | **Needs decision record** | `+1.0 pp` mechanism margin and CI lower bound 0 are new thresholds; attribution requires a declared primary ablation and multiplicity rule. |
| E4 | **Conditional PASS** | Correctly separates delay and pressure and disallows premature feedback. “Pooled contrast satisfies E2 margin” must not substitute for per-delay scope if the paper claims all delays. |
| E5 | **Needs decision record / possible contradiction** | `selector+update p95 ≤2× Frozen` and `≤100 μs` are not v006 acceptance requirements. They may be useful project constraints, but a candidate can satisfy all v006 benchmark criteria while failing this local performance cap; conversely passing it does not close B12/B13/B15. Kernel/replay timing is explicitly not end-to-end provider latency. |
| E6 | **Conditional PASS** | Frozen one-shot confirmation is correctly described. The natural `−1.5 pp` safety and controlled CI `>0` thresholds are new, and “or same-equivalent unread set” weakens the effective current slice unless the alternate set is approved before freeze. |

## 5. Mapping and wording defects

1. **Gate labels are overloaded.** Section 1 calls the four Gate-B subitems “B1–B4” while section 5 uses B1–B15. Use `GateB-1…GateB-4` or the v006 names to avoid confusing Gate-B scores with B1–B4 rubric rows.
2. **“完成后即可关闭” overstates authority.** The opening and closing sentence say satisfying v08 allows closure. Correct wording is: satisfying v08 produces a candidate evidence package that must be scored against v006; only the signed score record can set CLOSED.
3. **E6 scope is ambiguous.** The table says “157 molecules (or an equivalent unread set).” Current contract names the 157 scaffold-disjoint fresh slice. An alternative requires an explicit scope decision before freeze and a new manifest/hash.
4. **Main-text mapping is prospective.** Table 9 is described as “new or equivalent,” but the current main manuscript has Tables 3–8. A closure package needs an exact final artifact ID; “or equivalent” is not a reproducibility pointer.
5. **Public baseline provenance is not bound per row.** Section 3 says public numbers are motivation only, but B10 requires source/commit/license and adapter evidence. Add source IDs and adapter paths to the row-level receipt, not just a directory note.
6. **Natural non-inferiority is not a universal requirement in v006.** v08's E2/E6 natural margins are acceptable only if the paper explicitly claims a safety trade-off. They must not become a hidden condition for closing a benchmark whose primary claim is controlled recovery.
7. **Current method names are not fully canonical.** “MIRA-Relay candidate” and “MIRA” shorthand need an exact implementation commit and policy identifier; otherwise evidence may be attached to different variants.

## 6. Decisive closure tests

- **Score-sheet test:** produce one signed v006 sheet with H1–H10 statuses, B1–B15 scores/evidence paths, Gate A/B/C subscore and total. Without it, status is HOLD even if every experiment appears to pass.
- **Contract replay test:** independent checker rejects any truth/full-panel/premature-feedback trace.
- **Baseline row test:** each primary baseline has one canonical implementation, same-prior contract, raw JSONL, config/source/checkpoint hash, adapter test and recovery output.
- **Stage separation test:** Stage A E1–E5 is scored before labels are opened; Stage B E6 freeze/receipt is separately scored. No E6 label can alter code or thresholds.
- **Threshold provenance test:** ADR/preregistration names `m_ctrl`, natural margin, E3 margin, E5 cost cap, E6 margins, primary comparator, CI method and multiplicity before execution.
- **Artifact mapping test:** every number in Tables 3–9/figures maps to one experiment ID, comparator, unit, raw JSONL and hash; no “equivalent” artifact pointer.

## 7. Final PASS/FAIL answer

**Question: If real experiments satisfy v08 item by item, is that sufficient to close the effective Benchmark+Baseline acceptance?**

**Answer: FAIL as currently written.** It is sufficient to assemble a strong closure package, but not sufficient to close because:

1. v006 closure requires an independent re-score and signed evidence record, which v08 does not itself produce;
2. several numerical margins and timing caps are new, unapproved project constraints with no provenance record;
3. Gate and rubric identifiers are overloaded, and some baseline/method rows are not canonical;
4. E6 stage/scope and final artifact mapping remain ambiguous.

After recording the threshold decisions, splitting ambiguous identifiers, fixing exact E6 scope/artifact mapping, and obtaining the independent v006 score with all gates ≥12/16 and hard conditions PASS, the evidence would be **PASS / CLOSED** under the effective standard.
