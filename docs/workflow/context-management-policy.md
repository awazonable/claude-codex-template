# Prompt Isolation and Context Architecture

## Objective

The system separates agent contexts so that instructions intended for one agent are not treated as instructions by another. Role labels in a shared document are not the isolation mechanism; the Harness constructs isolated contexts for each profile.

Context economy remains a design goal: exploratory work occurs in disposable agent contexts instead of repeatedly enlarging Claude Main's context.

## Architecture

```text
Claude Main
  receives: CLAUDE.md, project requirements/design/ADRs, current task,
            agent capability index
  selects: profile + task boundary
          |
          v
Harness
  resolves: profile -> system prompt, tools, permissions, applicable policies
          |
          +--> research profile
          |      receives: Research-specific prompt, task, required repository context
          |
          +--> codex-worker profile
                 receives: AGENTS.md, Codex-specific policies, task,
                           required repository context
```

The Harness is the only component that retrieves, combines, and injects agent-specific prompts. Claude Main chooses a profile based on its published purpose; it neither reads nor forwards the profile's implementation details.

## Context boundaries

| Context | Includes | Excludes |
| --- | --- | --- |
| Claude Main | `CLAUDE.md`, project sources of truth, current task, capability index | `AGENTS.md`, Codex policies, Research prompts, and other profile internals |
| `research` profile | Harness-managed Research prompt, assigned investigation, required repository context | Claude Main and Codex Worker prompts |
| `codex-worker` profile | `AGENTS.md`, Harness-selected Codex policies, assigned goal, required repository context | Claude Main and Research prompts |

Research-specific instructions, including tool use, permissions, read-only constraints, subagent limits, and reporting format, are Harness-owned. They are not maintained in this shared workflow document or supplied by Claude Main.

Codex-specific instructions, including autonomous execution and escalation rules, are injected only when the Harness starts the `codex-worker` profile. `AGENTS.md` and `docs/workflow/codex-execution-policy.md` are Codex profile material, not Main startup material.

## Shared workflow information

The profile names have stable purposes, published in `agent-capability-index.md`. Authoritative project documents remain the requirements, design documents, and ADRs. The workflow documents describe lifecycle relationships and integration architecture; they do not serve as a single combined instruction set for every agent.
