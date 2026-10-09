# Fresh MIRA context-state replay

## Result

The import-free NumPy replay verified both existing fresh receipts:

| receipt | MIRA streams | decisions | state transitions | max score error |
|---|---:|---:|---:|---:|
| natural fresh v2 | 30 | 4,710 | 4,710 | 0 |
| controlled drift v2 | 30 | 4,710 | 4,710 | 0 |

For every stream, the replay recovered the provider-local posterior residual,
the selected provider, all delayed releases, and the final dynamic state. The
canonical traces contain 14,130 event states per receipt and are compressed in
the corresponding experiment log directory.

## Integrity checks

- Changed one MIRA reward in a copied receipt: verifier rejected the next
  decision with a score mismatch.
- Changed one MIRA score by `0.001`: verifier rejected the decision.
- The checker imports only Python standard library and NumPy; it does not
  import the production runner, policy, model loader, or stream implementation.
- Source: `src/probe_headless/verify_fresh_context_replay_v1.py`, SHA256
  `eeb616fa31f8d4c93ee229518ce8398b0c9094b069c0337f433bcc3fea5d7e81`.

## Evidence hashes

- Natural verification JSON: `b4ba8e180167bec89f8aeb070d560113e33c7e9980a2cf8a72f726aab7226b24`.
- Controlled verification JSON: `6b7272cb674a8ca991d2e31e8b26b06f776058695f242a15ba71b051f27a5463`.
- Natural canonical trace: `2841fd08c5e5bb3ccf4b0a055b685e16153d2c53945740255d471a32e924572e`.
- Controlled canonical trace: `6b1c78e5fc59f828910987023aa2a546321a05abf1e1db91498dd5c44859cdae`.

## Boundary

This closes the dynamic learned-state and transition replay gap for the
current fresh receipts. It does not independently rerun provider inference or
the offline fit, and it does not rehash the historical production state hash's
immutable weight array. The paired Frozen score surface is the sealed static
input to the replay. A future fresh run should write the initial weight bytes,
feature bytes, and lifecycle queue state directly into its receipt.
