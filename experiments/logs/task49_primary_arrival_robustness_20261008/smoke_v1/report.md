# Smoke v1 — interrupted engineering attempt

All 480 policy streams completed and their compressed chunk is preserved. The original-receipt reproduction loop repeatedly indexed the compressed NpzFile, decompressing a full array at every event. After observing this avoidable I/O cost, the coordinator-approved repair interrupted the process using SIGINT; failure.json contains the actual KeyboardInterrupt traceback and elapsed time. This run is incomplete and is not a reproduction pass.

The repair loads every receipt array once before the comparison/decoder loop. Policies, order generation, inputs, thresholds, and statistics are unchanged. The next immutable attempt is smoke_v2. No random-order full execution occurred in this attempt.
