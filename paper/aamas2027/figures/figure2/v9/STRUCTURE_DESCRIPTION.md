# Figure 2 v9 structure description

A single vertical timeline shows decision t, one selected provider call, a terminal record of output and runtime status, an independent frozen endpoint judge after termination, the pending delay queue, release at `τ_t = t + 1 + D`, and the first subsequent action that can see the released state. The unselected output is unavailable and never enters the queue.

The direct lifecycle does not include post-call verification, escalation, refusal, or a second call; those remain E5 extensions.
