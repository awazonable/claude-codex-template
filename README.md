# Template Repository — Initialization Required

> **This README belongs to the template repository.**
> If you can read this section in a newly created repository, the repository is **not initialized yet**.
> During initialization, replace this file completely with a project-specific `README.md`.

This repository is a development template for a Claude Main–orchestrated workflow in which Claude Main owns requirements, design, task decomposition, orchestration, and independent review, while the Harness starts isolated agent profiles for delegated work.

## Human setup

Create a repository from this template, for example:

```bash
gh repo create <owner>/<repo> --template <owner>/<template-repo> --clone
```

Then open the new repository with Claude Code and ask it to initialize the project.

## Initialization procedure

Claude Main must perform the following before normal development work. Track progress with `docs/workflow/initialization-checklist.md`.

1. Confirm this template README is still present. If it is, treat the repository as **uninitialized**.
2. Grill the human for the project goal, requirements, constraints, non-goals, important tradeoffs, and unresolved decisions.
3. Create or update the project requirements under `docs/requirements/`.
4. Create or update the system/design documentation under `docs/design/`.
5. Record architectural decisions that require explicit human choice as ADRs under `docs/adr/`.
6. Request adversarial design review through the `codex-worker` profile and iterate until there are no material objections to the major design direction.
7. Replace this README completely with a project-specific README describing the actual project, setup, operation, and contributor-facing information.
8. Verify that no template-only initialization instructions remain in the project README.

Once step 7 is complete, the repository is considered initialized.

## Template workflow

The intended lifecycle is:

```text
Human
  ↓
Claude Main
  ├─ requirement discovery / Grill
  ├─ requirements
  ├─ design
  ├─ task decomposition
  └─ select profile + task boundary
        ↓
Harness
  └─ resolve prompt / tools / permissions / policies
        ↓
codex-worker profile
  └─ adversarial design advice
        ↓
Claude Main / Human when needed
  └─ decision + ADR
        ↓
Design freeze
        ↓
Claude Main selects `codex-worker` + scoped goal
        ↓
Harness injects the worker-specific context
        ↓
Codex Worker implements autonomously
        ↓
Claude Main independently reviews
        ↓
Human final check
        ↓
PR merge
```

See:

- `CLAUDE.md` — Claude Main operating rules; the only root agent policy loaded by Claude Main
- `AGENTS.md` — Codex Worker rules, injected only for the `codex-worker` profile
- `docs/workflow/agent-capability-index.md` — Main-visible profile names and purposes only
- `docs/workflow/development-workflow.md` — end-to-end lifecycle and Harness-based delegation
- `docs/workflow/codex-execution-policy.md` — Codex Worker policy, injected only for the `codex-worker` profile
- `docs/workflow/design-review-policy.md` — design review loop
- `docs/workflow/code-review-policy.md` — independent implementation review
- `docs/workflow/pr-merge-policy.md` — final integration gate
- `docs/workflow/context-management-policy.md` — prompt-isolation architecture and context boundaries
- `docs/workflow/initialization-checklist.md` — tracks the initialization procedure above to completion

## License

This template, including the generated project's initial `LICENSE` file, is released under the [BSD Zero Clause License](https://opensource.org/license/0bsd) (`LICENSE`): free to reuse and modify, with no attribution required. Projects created from this template inherit it; replace `LICENSE` during initialization if the project needs different terms.

## Codex integration

Use `PeterSR/claude-code-codex-subagent` (or an equivalent adapter) as a dependency/integration layer rather than forking it into this template. The template owns the project workflow; the adapter/Harness owns Claude↔Codex execution plumbing and profile-specific prompt, tool, permission, and policy injection.

Claude Main selects only a profile (for example `research` or `codex-worker`) and supplies its task. It does not read or forward agent-specific prompts. The integration should remain replaceable so native Codex Goal/App Server control can be adopted later without redesigning the workflow.
