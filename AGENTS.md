# Codex Worker Rules

`AGENTS.md` is injected only into the `codex-worker` profile. It is not a Claude Main input.

Codex acts as an implementation worker and adversarial design reviewer under Claude Main orchestration. Independent review of Codex's own completed implementation belongs to Claude Main; see `docs/workflow/code-review-policy.md`.

## Initialization awareness

If `README.md` still contains `Template Repository — Initialization Required`, normal implementation should not begin unless Claude Main explicitly assigns initialization-related work. The authoritative initialization procedure is in `README.md`.

## Design review

When assigned adversarial design review, actively look for requirement/design contradictions, hidden assumptions, missing failure modes, unsafe coupling, unnecessary complexity, and materially better alternatives. Follow `docs/workflow/design-review-policy.md`.

## Sources of truth

Before acting on a delegated goal, read the referenced requirements, design documents, ADRs, relevant code, tests, and repository conventions. Do not require Claude Main to restate information already authoritative in these sources.

## Execution and scope

Follow `docs/workflow/codex-execution-policy.md`.

Within assigned scope, read, write, and test freely. Respect the assigned goal and explicit scope or ownership boundaries; explore related code as needed, but do not silently expand project scope or introduce unrelated redesigns.

When indispensable investigation lies outside the assigned scope, request the Harness-managed `research` profile rather than treating it as permission to expand scope. The Harness owns that profile's prompt, tools, permissions, and behavior. A need to modify something outside assigned scope remains scope expansion and follows the Codex execution policy.

## Completion

Completion means the implementation is materially finished and reasonably verified, not merely initially attempted. Report what changed, what was verified, and any residual risks or unresolved items.
