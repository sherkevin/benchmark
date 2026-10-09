# Main-paper receipt-closure binding — 2026-10-08

## Purpose

Bind the completed `fresh_receipt_closure_v1` audit to the paper text without turning a policy-replay audit into a new empirical estimate. The paper-level master remains the only promoted entrypoint; iteration builds stay under `paper/aamas2027/build/`.

## Text change

`paper/aamas2027/main.tex` now states three facts explicitly:

1. The routing contract checks visible reads, actions, release queues, state fields, and schema/counts.
2. The v3 closure artifact materializes canonical state bytes from the sealed replay and independently checks scores, lifecycle metrics, checkpoint suffixes, logical-clock reorder, mutations, terminal commitments, and final state.
3. The closure is scoped to the declared policy replay. It is not a second provider forward pass, offline-fit reimplementation, new effect estimate, or evidence for natural drift and agent claims.

The change removes the stale sentence that the older receipt could not be independently rehashed, while preserving the distinction between the original result receipt and the v3 closure artifact.

## Build evidence

- Build: `paper/aamas2027/build/submission_20261008_044332/main.pdf`
- Source SHA256: `12087fd6541f15a6dc88e5d42ab4f81333189c28c7af64d7b6d619334bc430b6`
- PDF SHA256: `f6c0b6ca473d9b6a65d8ef3e24fad12ce32b5aa6fc0a9beee9e9d19b308cd3d0`
- Total pages: 9; body pages: 8; references start: page 9.
- LaTeX warning/fatal/undefined-reference gate: pass.
- Natural and controlled contract audits: 270/270 and 150/150 verified.
- Page inspection: rendered pages 8 and 9 at 140 dpi; no truncation or overflow observed.

## Scope decision

This binding closes the receipt wording issue for the declared replay only. The broader method gate remains HOLD until the separate scope limits are addressed. The master PDF was promoted only after the complete submission build passed and the previous master was archived.
