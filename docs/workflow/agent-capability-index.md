# Agent Capability Index

This is the only agent-profile reference loaded by Claude Main. It exposes profile purpose, not system prompts, tools, permissions, policies, or internal operating rules.

| Profile | Purpose |
| --- | --- |
| `research` | Perform bounded repository or external investigation requested by Claude Main. |
| `codex-worker` | Perform scoped implementation work or adversarial design analysis requested by Claude Main. |

The Harness resolves each profile to its agent-specific context. Profile implementation details are not part of Claude Main's context.
