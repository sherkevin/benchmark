# Paper claim-scope audit v1

**Date:** 2026-10-09 (Asia/Shanghai)  
**Status:** `PASS_WITH_EXPLICIT_BOUNDARIES` for the locked narrow submission slice  
**Scope:** public TDC BBB truth; four checkpoint-bound fresh providers; 157 scaffold-disjoint confirmation molecules; one-call selected-only routing; delays $D\in\{0,1,4\}$; natural outcomes plus evaluator-side controlled correctness-drift stress.

## Purpose

This is a text audit, not a scientific result and not a replacement for the three gate documents. It checks whether the current manuscript says more than the receipt-joined baseline matrix can support. The audit covers the title/abstract, introduction, problem and method, experiment contract, results, limitations, conclusion, captions and table-facing prose.

## Evidence

| Item | Path | SHA-256 |
|---|---|---|
| Manuscript source | `paper/aamas2027/main.tex` | recorded in `audit.json` |
| Locked matrix | `task/task49-pharma-benchmark-baseline-freeze/baseline_scope_lock_and_receipt_join_v1/matrix.json` | recorded in `audit.json` |
| Independent check | `independent_verification.json` | `verified_independently` |

The audit was run with:

```bash
python3 src/probe_headless/build_paper_claim_scope_audit_v1.py \
  --output task/task49-pharma-benchmark-baseline-freeze/paper_claim_scope_audit_v1/audit.json
python3 src/probe_headless/verify_paper_claim_scope_audit_v1.py \
  --audit task/task49-pharma-benchmark-baseline-freeze/paper_claim_scope_audit_v1/audit.json \
  --output task/task49-pharma-benchmark-baseline-freeze/paper_claim_scope_audit_v1/independent_verification.json
```

The verifier re-read the manuscript and matrix, recomputed the source hashes and line-addressed boundary witnesses, and did not import the builder.

## Findings

1. All required scope anchors are present: public TDC BBB truth, four fresh checkpoint-bound providers, the 157-molecule confirmation boundary, selected-only feedback, one call, the three delays, and the natural/controlled split.
2. The manuscript explicitly says that the wider 33-provider pool is development evidence, not a model-fresh claim, and that recent routers with incompatible semantics are audit-only.
3. The natural result is stated as a null and explicitly does not support generic online improvement. The controlled label flip is called evaluator-side and explicitly does not establish natural provider nonstationarity.
4. The arrival-order result is conditioned on the previously inspected fixed cohort and explicitly does not establish new-scaffold or deployment generalization.
5. Post-call verification/refusal, hERG, AMES, lifecycle events and integrated multi-call agent evaluation are marked as separate or outside the measured primary scope.
6. The wording “scientific agents” appears as motivation and task context. It is not used to claim an end-to-end agent-level gain.

The scanner found seven broad-looking phrases. All are in explicit limitation or boundary sentences; none is a positive broad performance claim. This is why the result is a scoped pass rather than a claim that the paper proves broad scientific-agent generalization.

## Decision

For the current narrow manuscript, no source edit is required. The main PDF remains unchanged and must continue to be promoted only through the master-PDF procedure. Any future edit that changes “four providers”, “157”, “one-call”, “selected-only”, the delay set, or the natural/controlled qualification must rerun this audit before promotion.

This receipt does **not** close the broad project gate. The broad score remains Gate A 11/16 HOLD, Gate B 11/16 HOLD, Gate C 10/16 HOLD. The narrow score remains Gate A 15/16, Gate B 13/16, Gate C 14/16, conditional on preserving this claim surface.
