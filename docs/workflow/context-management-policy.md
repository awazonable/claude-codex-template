# Context Management Policy

## Objective

The optimization target is **not** the total number of tool calls. It is the number of times the large Main/Orchestrator context has to be re-evaluated. Every additional read/grep/search performed directly in Main forces the whole accumulated context to be reprocessed on the next turn. Pushing exploratory work into small, disposable subagent contexts keeps Main cheap to run for many turns.

## Roles

```text
Claude Main / Orchestrator
├─ Research Subagent
│   └─ investigation / exploration / analysis
│
└─ Codex Worker
    ├─ read / write / test within assigned scope
    └─ Research Subagent (only when unavoidable)
        └─ investigation strictly outside the assigned scope
```

### Claude Main / Orchestrator

- At startup only, read `CLAUDE.md` / `AGENTS.md`, `README.md`, the ADR index and any directly relevant ADRs, the requirements/design documents relevant to the current work, and the current task spec.
- After startup, focus on design, decision-making, task decomposition, `write`/`edit`, delegation, and final integration.
- After startup, Main must not perform open-ended exploratory `read` / `grep` / `search` / log inspection / repository exploration itself. Delegate it.
- Two things are Main's own work, not delegable exploration, and do not need a Research Subagent: re-reading the requirements/design documents Main itself authors and evolves, and reading the diff/report under review during independent code review (see `code-review-policy.md`). If reviewing that diff raises a question that requires widening beyond it — repo-wide search, history, log or test-failure digging — that widening is delegated to a Research Subagent, whose compressed report Main then judges.

### Research Subagent

- Owns `read` / `grep` / `search` / repository exploration / log inspection / test-failure investigation.
- Read-only. Does not modify repository state and does not spawn further subagents.
- Returns to its caller a compressed report: conclusions, supporting evidence with `file:line` references, and open/unresolved questions. Raw logs or large search dumps are not passed back.

### Codex Worker

- Started by Claude with an explicit assigned scope, the relevant ADRs, pointers to the governing constraints (not restatements — see `docs/requirements/` and `docs/design/`), implementation direction, and the expected report contract.
- Within its assigned scope, may freely `read` / `write` / `test`.
- **Assigned scope** is the file/module set named in the delegation, plus whatever is directly reachable from the code under change: its callers, callees, tests, and configuration. Reading within that reach is ordinary work, not out-of-scope investigation.
- Investigation of subsystems the goal does not touch — beyond that reach — is normally out of bounds.
- When information outside the assigned scope is indispensable to completing the goal, the Codex Worker may start a Research Subagent for that investigation only. That Research Subagent is read-only and must not itself start further subagents.
- If the Claude↔Codex execution adapter in use provides no mechanism for a Codex Worker to start a subagent directly, the Codex Worker instead sends Claude a bounded, non-blocking research request (what needs confirming, and why it is indispensable) and continues other assigned work where possible; Claude runs the Research Subagent and relays the compressed report back. This is a research request, not a scope-expansion escalation under `codex-execution-policy.md` — it becomes scope expansion only when the goal requires *modifying* something outside the assigned scope.

## Delegation contract

- Main does not micromanage subagents.
- Before delegating, state up front: investigation purpose, what must be confirmed, scope, pointers to governing constraints, and the expected response contract (format/content of the report).
- Prefer **one delegation → one final report**. Avoid repeated small round trips of "investigate → Main judges → investigate again" where a single, well-scoped delegation would do.

## Relationship to other policies

- This policy governs *how* Claude Main and Codex Workers use subagents while operating under `development-workflow.md`, `codex-execution-policy.md`, `design-review-policy.md`, and `code-review-policy.md`. It does not change what those policies require — it changes how the investigative work behind them is carried out.
- A Codex Worker's use of a Research Subagent for indispensable out-of-scope investigation is not, by itself, scope expansion under `codex-execution-policy.md`; it becomes a scope-expansion escalation only if the Codex Worker needs to *modify* something outside its assigned scope.
