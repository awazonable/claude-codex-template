# Code Review Policy

## Roles

- **Implementation author:** normally the `codex-worker` profile
- **Independent technical reviewer:** Claude Main
- **Final project-specific gate:** Human

The reviewer should provide a meaningfully different perspective from the implementation author.

## Review inputs

Review the actual repository state and diff, not only the implementation agent's summary.

Use the authoritative requirements, design, and ADRs as review criteria.

## Review focus

Prioritize:

- correctness and requirement conformance;
- architecture/design conformance;
- regressions and edge cases;
- unsafe assumptions;
- error/failure behavior;
- test adequacy;
- maintainability issues that materially affect future work;
- unintended scope expansion.

Avoid blocking on subjective style preferences unless they violate repository standards or materially reduce maintainability.

## Findings

Material findings should be returned to the implementation agent with enough context to correct them.

After corrections, re-review the affected areas as needed.

## Completion

Claude Main may recommend human final approval when no material unresolved review findings remain and verification evidence is adequate for the task.
