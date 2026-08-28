# Template Repository — Initialization Required

> **This README belongs to the template repository.**
> If you can read this section in a newly created repository, the repository is **not initialized yet**.
> During initialization, replace this file completely with a project-specific `README.md`.

This repository is a development template for a Claude Code–orchestrated workflow in which Claude/Opus owns requirements, design, task decomposition, orchestration, and independent review, while Codex performs delegated implementation work autonomously.

## Human setup

Create a repository from this template, for example:

```bash
gh repo create <owner>/<repo> --template <owner>/<template-repo> --clone
```

Then open the new repository with Claude Code and ask it to initialize the project.

## Initialization procedure

Claude must perform the following before normal development work. Track progress with `docs/workflow/initialization-checklist.md`.

1. Confirm this template README is still present. If it is, treat the repository as **uninitialized**.
2. Grill the human for the project goal, requirements, constraints, non-goals, important tradeoffs, and unresolved decisions.
3. Create or update the project requirements under `docs/requirements/`.
4. Create or update the system/design documentation under `docs/design/`.
5. Record architectural decisions that require explicit human choice as ADRs under `docs/adr/`.
6. Run adversarial design review with Codex and iterate until there are no material objections to the major design direction.
7. Replace this README completely with a project-specific README describing the actual project, setup, operation, and contributor-facing information.
8. Verify that no template-only initialization instructions remain in the project README.

Once step 7 is complete, the repository is considered initialized.

## Template workflow

The intended lifecycle is:

```text
Human
  ↓
Claude Code / Opus
  ├─ requirement discovery / Grill
  ├─ requirements
  ├─ design
  ├─ task decomposition
  └─ orchestration
        ↓
Codex / Sol
  └─ adversarial design advice
        ↓
Claude / Human when needed
  └─ decision + ADR
        ↓
Design freeze
        ↓
Claude delegates scoped goals
        ↓
Codex implements autonomously
        ↓
Claude independently reviews
        ↓
Human final check
        ↓
PR merge
```

See:

- `CLAUDE.md` — Claude Code operating rules
- `AGENTS.md` — Codex operating rules
- `docs/workflow/development-workflow.md` — end-to-end lifecycle
- `docs/workflow/codex-execution-policy.md` — autonomous implementation policy
- `docs/workflow/design-review-policy.md` — design review loop
- `docs/workflow/code-review-policy.md` — independent implementation review
- `docs/workflow/pr-merge-policy.md` — final integration gate
- `docs/workflow/context-management-policy.md` — Main/Research Subagent/Codex Worker delegation hierarchy and context economy
- `docs/workflow/initialization-checklist.md` — tracks the initialization procedure above to completion

## License

This template, including the generated project's initial `LICENSE` file, is released under the [BSD Zero Clause License](https://opensource.org/license/0bsd) (`LICENSE`): free to reuse and modify, with no attribution required. Projects created from this template inherit it; replace `LICENSE` during initialization if the project needs different terms.

## Codex integration

Use `PeterSR/claude-code-codex-subagent` (or an equivalent adapter) as a dependency/integration layer rather than forking it into this template. The template owns the development policy; the adapter owns Claude↔Codex execution plumbing.

The integration should remain replaceable so native Codex Goal/App Server control can be adopted later without redesigning the workflow.
