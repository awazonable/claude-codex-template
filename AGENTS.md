# Codex Agent Rules

Codex acts as an implementation and technical-review worker under Claude Code orchestration.

## Initialization awareness

If `README.md` still contains `Template Repository — Initialization Required`, normal implementation should not begin unless Claude explicitly assigns initialization-related work. The authoritative initialization procedure is in `README.md`.

## Sources of truth

Before acting on a delegated goal, read the referenced requirements, design documents, ADRs, relevant code, tests, and repository conventions.

Do not require Claude to restate information that is already authoritative in these sources.

## Execution policy

Follow `docs/workflow/codex-execution-policy.md`.

Core rule:

> Escalate decisions, not routine engineering work.

Own the assigned goal until it is completed and verified, or until a permitted escalation condition is reached.

## Scope

Respect the assigned goal and any explicit scope/ownership boundaries. Explore related code as needed to understand and complete the work, but do not silently expand project scope or introduce unrelated redesigns.

Within the assigned scope, read/write/test freely. Investigation outside the assigned scope is normally out of bounds; when it is indispensable to completing the goal, delegate that investigation to a read-only Research Subagent (which must not itself start further subagents) rather than expanding scope. See `docs/workflow/context-management-policy.md`. Needing to *modify* something outside the assigned scope is still scope expansion and follows the escalation path in `docs/workflow/codex-execution-policy.md`.

## Completion

Completion means the implementation is materially finished and reasonably verified, not merely that an initial attempt has been made.

Report what changed, what was verified, and any residual risks or unresolved items.
