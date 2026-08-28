# Development Workflow

## 1. Human interface

The human interacts with Claude Code. Claude remains the orchestration and decision interface throughout the lifecycle.

## 2. Requirement discovery

Claude/Opus uses Grill-style questioning to convert the human's intent into sufficiently complete requirements.

The objective is to resolve important ambiguity before implementation rather than pushing routine requirement discovery into the implementation phase.

## 3. Design

Claude/Opus creates the design from the requirements.

Codex/Sol is then asked for adversarial design advice. Codex should challenge assumptions, identify architectural risks, missing cases, contradictions, and better alternatives rather than merely affirm the proposal.

## 4. Decision loop

Claude evaluates review feedback.

- If feedback exposes an objectively resolvable defect, update the design.
- If feedback exposes a material choice requiring human judgment, ask the human.
- Record material lasting decisions as ADRs.

Repeat the design review loop until there are no significant objections to the major design direction. Minor implementation details need not be exhausted before implementation.

## 5. Design freeze

When requirements, design, and required ADRs are sufficiently stable, declare the design ready for implementation.

Design freeze does not prohibit all change. It means implementation should not casually reopen settled architecture.

## 6. Task decomposition

Claude decomposes the design into goals suitable for Codex execution.

A delegated task normally needs:

- a clear goal;
- references to authoritative requirements/design/ADRs;
- scope or ownership boundaries when necessary to avoid ambiguity or conflicting concurrent work.

Do not mechanically restate constraints already present in authoritative documents.

## 7. Codex implementation

Claude delegates the goal to Codex through the configured Claude↔Codex adapter.

Codex owns execution and should autonomously:

- inspect relevant repository state;
- choose routine implementation details;
- implement;
- test/check;
- diagnose failures;
- revise and retest;
- continue until completion or a permitted escalation condition.

See `codex-execution-policy.md`.

## 8. Claude monitoring

Claude monitors at orchestration level, not line-by-line implementation level.

Intervention is reserved for:

- explicit permitted escalation;
- material scope or requirement divergence;
- obvious stalls/loops;
- cross-task coordination issues.

## 9. Completion and independent review

When Codex completes a goal, Claude reviews the actual resulting code/diff independently.

Claude checks requirement/design conformance, correctness risks, maintainability, and verification evidence. Material findings are returned for correction.

## 10. Human final gate

Claude summarizes the implementation and review results to the human. The human performs the final project-specific check and approves PR merge.

## 11. Context economy

Throughout the lifecycle, Claude Main delegates exploratory investigation (read/grep/search/log inspection/repository exploration) to a read-only Research Subagent rather than performing it directly after startup, and Codex Workers do the same for investigation outside their assigned scope. See `context-management-policy.md` for the full hierarchy and delegation contract.

## 12. Integration architecture

Use `PeterSR/claude-code-codex-subagent` or an equivalent mechanism as the execution adapter rather than embedding its implementation into this template.

Keep orchestration policy independent from the adapter so the project can later adopt native Codex Goal/App Server control without rewriting the development process.
