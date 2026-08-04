# Agent-ready issue template

Use this structure as a quality contract, not as filler. Omit a section only when it genuinely does not apply.

```markdown
## Context

Explain why this work exists, the relevant current behavior, and where it fits in the larger initiative. Include repository evidence such as paths, modules, symbols, APIs, or existing issue links. Distinguish facts from assumptions.

## Objective

State the single observable outcome this issue must deliver.

## Scope

- Describe the behavior and components that must change.
- Name likely implementation locations when discovery established them.
- Include compatibility, migration, rollout, or documentation work that belongs in this ticket.

## Non-goals

- Name adjacent work that is intentionally excluded.
- Point to separate issues when appropriate.

## Implementation notes

- Record established constraints, repository conventions, and useful starting points.
- Describe interfaces or invariants that must remain stable.
- Provide guidance without prescribing unsupported implementation details.

## Acceptance criteria

- [ ] Write observable, independently verifiable completion conditions.
- [ ] Cover important success, failure, and edge-case behavior.
- [ ] Include required tests, documentation, migration, or telemetry outcomes.

## Validation

- List exact existing test, lint, build, or manual verification commands when known.
- State what evidence demonstrates success.

## Dependencies

- `None`, or list prerequisite issue links with a short explanation.

## Risks and open questions

- Record only unresolved items that materially affect implementation.
- If an answer is required before work can begin, make the blocker explicit.
```

## Quality checks

Before proposing an issue, verify that:

- A new agent can understand the motivation without reading the parent conversation.
- The issue has one coherent outcome and explicit boundaries.
- Acceptance criteria describe behavior rather than vague activity such as “update code.”
- Validation is realistic for this repository and does not invent commands.
- Dependencies are necessary, directed, and free of cycles.
- Unknowns are either resolved, bounded within the issue, or isolated in a concrete discovery spike.
- The issue does not duplicate an existing open or recently completed ticket.
- No private reasoning, credentials, transient local paths, or irrelevant investigation logs appear in the body.
