# PR and Merge Policy

A PR is ready for human final review when:

- delegated implementation goals are complete;
- material implementation blockers are resolved or explicitly documented;
- Claude Main has independently reviewed the resulting code;
- material review findings are resolved;
- relevant automated or manual verification has been performed, or missing verification is explicitly disclosed;
- requirements/design/ADRs are updated when implementation caused an approved change to authoritative project decisions.

## Human final check

Claude Main provides a concise summary containing:

- what changed;
- which requirements/goals were satisfied;
- verification performed;
- independent review outcome;
- residual risks, known limitations, or deferred items.

The human performs the final context-specific check and decides whether to merge.

PR merge is not delegated automatically unless the human explicitly establishes a separate policy permitting it.

## PR ownership

Claude Main opens the pull request once the criteria above are met and pushes the reviewed branch. The `codex-worker` profile does not open or push the final PR — its output is delivered to Claude Main as a completed, reviewable diff.
