# Figure 2 v12 — decision-to-feedback terminal flow

- Source file: `/Users/jingwu/Downloads/决策到反馈的终端流程图.png`
- Paper asset: `figure2_v12.png`
- Dimensions: 2120 × 742 px, RGB PNG
- SHA-256: `acf39f29495cb1e1e14af6ffb4db8ebae1963293489672b8c2d47c834b59de98`
- Role: full-width terminal lifecycle and information-boundary figure
- Supersedes in the working paper: `figure2/v9/figure2_v9.pdf` (retained)

## Semantic contract

The solid event path is $S_t+A_t\rightarrow$ selector $\rightarrow$ one selected provider $\rightarrow$ call $\rightarrow$ terminal record. The terminal record contains output and runtime status. A frozen endpoint judge produces chemical correctness only after termination; runtime failure and chemical correctness converge to a terminal outcome, which becomes a selected-only queued event. The event remains hidden until $\tau_t=t+1+D$, then updates the relay and the released state before the first action at or after $\tau_t$.

The diagram explicitly marks unselected output as unobserved and shows the $D=0$ boundary: the released event is visible before the next action, never before the action that generated it.
