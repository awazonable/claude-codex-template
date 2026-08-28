# Design

This directory contains the implementation-facing system design derived from the requirements.

Claude/Opus authors and evolves the design, using Codex/Sol as an adversarial adviser before design freeze.

Design documentation should make major engineering direction clear while leaving routine local implementation choices to the implementing agent.

Include as relevant:

- architecture and component boundaries;
- key data/control flows;
- interfaces and ownership boundaries;
- persistence/state model;
- failure handling and operational behavior;
- security or safety-relevant constraints;
- migration/compatibility concerns;
- testing strategy;
- references to applicable ADRs.

Avoid over-specifying reversible local implementation details unless they matter to requirements or architecture.
