# 7-inch QA — Figure 1 v33 + Figure 2 v26

This build checks the current Figure 1 candidate and the current Figure 2 candidate at the intended seven-inch figure width. It is not the paper master and does not change `figures/qa/main.pdf`.

- Figure 1: `generated/figure1/v33_ai.png`, prompt v43, central computation architecture.
- Figure 2: `generated/figure2/v26_ai.png`, prompt v27, single-event delayed-feedback timeline.
- The build must be compiled twice with no fatal, overfull, missing-font, or undefined-reference warnings.
- Independent review and user selection are still required before master synchronization.

The Codex LaTeX compiler could not read project images from its isolated temporary directory; that failed attempt is retained in the turn log. The same source was then compiled twice with local `pdflatex` using the absolute project image paths. Both runs completed without fatal, overfull, undefined-reference, or missing-font warnings.

- PDF SHA-256: `b8dcd23df681f39993d21e84912d8766fd46f9ff5c19eb4688cefc869319d29d`
- Figure 1 page SHA-256: `d66d2f1b76dee7ce81740a256c933f40792a0436c36a167198b61039d4644e7b`
- Figure 2 page SHA-256: `814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa`
