# Design Review Policy

## Roles

- **Author:** Claude Main
- **Adversarial adviser:** the `codex-worker` profile
- **Decision authority:** Human, when an unresolved material choice requires judgment

## Purpose

Design review exists to challenge the proposed architecture before implementation, not to produce consensus for its own sake.

Adversarial design review examines:

- requirement/design contradictions;
- hidden assumptions;
- missing failure modes;
- unsafe coupling or ownership boundaries;
- unnecessary complexity;
- scalability/performance/operability risks where relevant;
- alternatives that materially improve the design;
- decisions that should be explicit ADRs.

## Review loop

1. Claude Main produces or updates the design.
2. The `codex-worker` profile reviews it adversarially through the Harness.
3. Claude Main classifies findings:
   - fix directly;
   - reject with rationale;
   - escalate to human decision.
4. Human decisions with lasting architectural significance are recorded as ADRs.
5. Repeat as needed.

## Exit criterion

Design review is complete when additional review no longer produces material objections to the **major design direction**.

Do not continue cycling merely to eliminate minor implementation questions that can safely be delegated to the implementation agent.
