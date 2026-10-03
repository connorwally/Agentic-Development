---
name: grill-me
description: Interrogate a GitHub issue before implementation to expose ambiguous requirements, missing decisions, hidden assumptions, edge cases, and unclear acceptance criteria, then produce a decision-ready implementation brief. Use when the user explicitly invokes Grill Me with an issue number or URL, or explicitly asks to be questioned rigorously about whether a ticket is ready for implementation.
---

# Grill Me

Turn an under-specified GitHub issue into a decision-ready implementation contract by investigating first and interviewing the user about only the uncertainties that matter.

## Resolve the issue

1. Require an issue number or GitHub issue URL.
2. Resolve a bare number against the current repository's GitHub remote. Use the repository from a full issue URL instead.
3. If the repository remains ambiguous, ask for `owner/repo`; never guess.
4. Fetch the issue title, body, state, metadata, comments, linked issues or pull requests, and other directly relevant context. Prefer the authenticated GitHub connector; fall back to `gh` when necessary.
5. Treat comments as context, not automatic overrides. Surface contradictions between the body, comments, labels, and linked work.

## Investigate before asking

Default to the current repository for implementation context when it matches the issue repository. Perform read-only discovery:

- Read applicable `AGENTS.md` files, documentation, manifests, relevant source and tests, and useful history.
- Trace named code paths, interfaces, data models, configuration, and existing behavior.
- Search related issues or pull requests when they may already resolve an ambiguity.
- Separate verified facts, reasonable inferences, and unknowns.

Do not ask the user questions that repository or GitHub evidence can answer. If the checkout does not match the issue repository or evidence is inaccessible, state the limitation before interviewing.

Do not implement the issue, edit files, create a branch, or mutate GitHub while grilling.

## Keep validation proportionate

Follow applicable `AGENTS.md` validation policies when drafting issues, asking questions, and defining readiness.

Choose the smallest validation approach that provides meaningful confidence in the requested outcome. Manual verification is valid; testable does not mean automated.

Do not expand a ticket into testing infrastructure or exhaustive coverage without a concrete risk that warrants it, and explicit permission from the user. Do not interrogate every testing layer or make unavailable automation a blocker when a realistic alternative exists. Record the limitation and move on.

## Build the ambiguity inventory

Extract what the issue states explicitly:

- Problem, user, and desired outcome.
- Scope and non-goals.
- Required behavior, edge cases, and failure behavior.
- Interfaces, data rules, compatibility, and constraints.
- Acceptance criteria and validation.
- Dependencies, rollout, observability, and operational expectations.

Identify missing or conflicting decisions that could materially change implementation, user-visible behavior, safety, compatibility, or the definition of done. Consult [references/question-bank.md](references/question-bank.md) for relevant probes; do not mechanically ask every question.

## Conduct the interview

- Be candid, persistent, and precise without being hostile.
- Ask one to three high-leverage questions per round. Start with blockers and branch based on the answers instead of dumping a questionnaire.
- Explain briefly why each answer matters.
- Challenge vague language such as “support,” “handle,” “fast,” “user-friendly,” “properly,” and “as needed” by asking for observable behavior or boundaries.
- Offer concrete options when the decision space is genuinely known, and state a recommendation when useful. Do not hide assumptions inside the options.
- Distinguish product decisions from implementation discretion. Do not force the user to choose details an implementing agent can decide safely.
- When the user says “you decide,” recommend a choice, explain the tradeoff, and record it as an approved assumption.
- Maintain a concise decision log after each round so earlier answers are not asked again.
- Follow new ambiguity exposed by an answer until it is bounded or explicitly delegated to implementation.

## Decide when to stop

Continue until the ticket has enough information for an agent to begin without inventing product behavior. Stop when:

- The outcome and success signals are observable.
- Scope and important non-goals are bounded.
- Material behavior, failure cases, compatibility, and constraints are decided.
- Acceptance criteria and a realistic validation path exist.
- Dependencies and true blockers are visible.
- Remaining choices are explicitly safe implementation discretion.

Respect an explicit request to stop early. Do not claim readiness merely because the user is tired of questions; record unresolved blockers and assumptions instead.

## Produce the implementation brief

Finish with:

1. **Readiness:** `Ready`, `Ready with explicit assumptions`, or `Not ready`.
2. **Issue:** canonical issue link and title.
3. **Outcome:** the observable result.
4. **Decisions made:** the interview decision log.
5. **Scope and non-goals.**
6. **Acceptance criteria:** testable checklist items.
7. **Technical constraints and relevant repository evidence.**
8. **Validation approach.**
9. **Dependencies, risks, and rollout considerations.**
10. **Implementation discretion:** choices the coding agent may make without asking again.
11. **Unresolved blockers:** only items that still require a decision.
12. **Suggested issue revision:** a self-contained Markdown body or focused patch that incorporates the clarified decisions.

Make the brief standalone so a fresh coding agent does not need the interview transcript.

End every completed or user-stopped interview with this save prompt:

> Would you like me to save this to the issue? I can update the issue body or add the clarified brief as a comment.

If the user wants to save it but does not choose a method, ask them to choose between updating the body and adding a comment. Recommend a comment when preserving the original issue history is valuable.

## Keep GitHub read-only by default

Do not edit the issue, add comments, change metadata, or create related work unless the user explicitly requests that write. Before an issue update, show the exact proposed text and confirm the target issue. After an approved update, verify the resulting issue and report its canonical URL.
