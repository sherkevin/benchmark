# AGENTS.md

## Agent skills

### Project docs

Read in this order: `README.md` (what the project is, current state, next step) → `CONTEXT.md` (glossary + directory responsibilities) → `docs/README.md` (document map with authority levels) → `docs/任务清单.md` (the open work, one checkable item per ticket) → `TODO_LOG.md` (recent execution history, result links, and immediate next step).

`docs/目标.md` holds the goal and one undecided conflict in the training objective; `docs/计划_全工具远程跑批与结果库.md` holds the execution plan and the four stop conditions. When two documents disagree, the higher authority level in `docs/README.md` wins, and between equals the newer one wins.

### Work log and result persistence

Every work session must start by reading the latest entries in `TODO_LOG.md` and end by appending the session status, conclusions, artifact paths, unfinished items, and next step. `TODO_LOG.md` is a handoff log, not the canonical result store. Full reusable results must be persisted in the corresponding `task/<task>/report.md`, `docs/`, `research/`, `data/`, or `docs/adr/` document before the session ends. Do not leave reusable results only in chat.

### Research iteration

For the current AAMAS pharma research, follow `docs/研究迭代规范.md`: story → candidate contribution → coherent method → scientific review → small real-data validation. Each experiment must identify a claim, nearest published comparator, and a falsification condition before execution. Preserve negative results and held-out confirmation data. Engineering checks serve this loop; do not wait for all infrastructure to propose the story or infer novelty from a winning score. Existing explicit experiment authorization limits still apply.

### Issue tracker

Issues live as local markdown under `.scratch/<feature-slug>/` (one spec + one file per ticket). See `docs/agents/issue-tracker.md`.

### Domain docs

Single-context layout: one `CONTEXT.md` at repo root + `docs/adr/`. See `docs/agents/domain.md`.
