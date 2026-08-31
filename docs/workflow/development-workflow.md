# Development Workflow

## 1. Human interface

The human interacts with Claude Main, which remains the orchestration and decision interface throughout the lifecycle.

## 2. Requirement discovery

Claude Main uses Grill-style questioning to convert the human's intent into sufficiently complete requirements. The objective is to resolve important ambiguity before implementation rather than pushing routine requirement discovery into implementation.

## 3. Design and adversarial review

Claude Main creates the design from the requirements. An adversarial design review is requested through the `codex-worker` profile; the review assesses assumptions, architectural risks, missing cases, contradictions, and materially better alternatives.

Claude Main evaluates review feedback, updates objectively resolvable defects, obtains human decisions for material choices, and records lasting decisions as ADRs. The loop ends when no material objections to the major design direction remain.

## 4. Design freeze and task decomposition

When requirements, design, and required ADRs are sufficiently stable, the design is ready for implementation. Claude Main decomposes the design into scoped outcomes and cites the authoritative requirements, design documents, ADRs, and relevant paths. Design freeze still permits approved change; it prevents casually reopening settled architecture.

## 5. Profile-based execution

Claude Main delegates through the configured Harness by selecting a profile and supplying the task. For implementation and adversarial design work, the profile is `codex-worker`; for bounded investigation, it is `research`.

The Harness, not Claude Main, resolves `profile -> prompt / tools / permissions / applicable policies` and injects the resulting agent-specific context. Main does not read, copy, or relay Research or Codex prompts. This permits the adapter implementation to change without changing the lifecycle or weakening prompt isolation.

## 6. Monitoring, completion, and review

Claude Main monitors at the orchestration level while delegated work is in progress. After implementation, Claude Main independently reviews the actual diff and verification evidence against the authoritative project documents. Material findings return to the appropriate profile for correction.

## 7. Human final gate

Claude Main summarizes the completed work, review outcome, verification, and residual risks. The human performs the final project-specific check and decides whether to merge.

## 8. Integration architecture

Use `PeterSR/claude-code-codex-subagent` or an equivalent mechanism as the execution adapter rather than embedding its implementation into this template. The adapter/Harness owns profile resolution and context injection; this repository owns the project workflow and authoritative project documents.

See `context-management-policy.md` for the prompt-isolation architecture and `agent-capability-index.md` for the Main-visible profile purposes.
