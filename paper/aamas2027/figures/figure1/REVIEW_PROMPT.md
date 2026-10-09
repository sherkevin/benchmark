# Figure 1 external-review prompt

Use this prompt for each independent visual/method reviewer. Do not edit the source while reviewing.

1. At the actual AAMAS `figure*` width, can a non-specialist identify the six stages within 30 seconds?
2. Is the selected-only information boundary unambiguous? In particular, can the reader tell that unselected outputs and unreleased labels are hidden?
3. Is MIRA-Relay visually separable from the frozen fallback and runtime components without relying on color alone?
4. Does the loop show how a released event changes the next action without implying that feedback is immediate?
5. Are any labels, arrows, or boxes clipped, too small, or semantically overloaded?
6. Does the caption state the paper's contribution without claiming an empirical improvement?
7. Which one element should be removed if the figure is too dense?

Return: `PASS`, `REVISE`, or `REJECT`; a 1–5 score for readability, semantic fidelity, visual hierarchy, and AAMAS fit; and no more than three concrete edits.
