---
name: grill
description: Grill a GitHub issue against repository docs, domain vocabulary, ADRs, and code before implementation. Resolve material ambiguities and produce a decision-ready implementation brief. Use when the user invokes /grill or explicitly asks for a rigorous ticket-readiness interview.
---

# Grill

Turn an under-specified GitHub issue into a decision-ready implementation brief. Investigate first, then interview the user about only the decisions the evidence cannot settle.

## Resolve the issue

1. Require an issue number or GitHub issue URL.
2. Resolve a bare number against the current repository's GitHub remote. Use the repository from a full issue URL instead.
3. If the repository remains ambiguous, ask for `owner/repo`; never guess.
4. Fetch the issue title, body, state, metadata, comments, linked issues or pull requests, and other directly relevant context. Prefer the authenticated GitHub connector; fall back to `gh` when necessary.
5. Treat comments as context, not automatic overrides. Surface contradictions between the body, comments, labels, and linked work.
6. As soon as the issue is resolved, show its title and canonical GitHub issue URL before starting the interview. Put the bare URL on its own line inside a fenced `text` code block so it is easy to copy into an implementing session. Do not use a Markdown hyperlink or clickable autolink. Use this same non-hyperlinked format whenever returning issue or comment URLs, including in the implementation brief and save confirmation. Tell the user that the session brief will be saved as an issue comment by default and that they can opt out by saying “don't save” or “keep this local.”

## Investigate before asking

Default to the current repository for implementation context when it matches the issue repository. Perform read-only discovery:

- Read applicable `AGENTS.md` files, documentation, manifests, relevant source and tests, and useful history.
- Trace named code paths, interfaces, data models, configuration, and existing behavior.
- Search related issues or pull requests when they may already resolve an ambiguity.
- Separate verified facts, reasonable inferences, and unknowns.

Do not ask the user questions that repository or GitHub evidence can answer. If the checkout does not match the issue repository or evidence is inaccessible, state the limitation before interviewing.

Do not implement the issue or create a branch. Keep the working tree read-only unless the user explicitly approves a documentation change. The default session-comment save at the end is the only automatic GitHub write.

## Check ADRs and project vocabulary

Before the first interview round, look in the matching checkout for architecture decision record (ADR) locations. Check repository instructions and documentation indexes for declared paths, and search for case-insensitive `adr` and `adrs` directory names (e.g. `Docs/ADR`, `docs/adrs`, or a component's `docs/adr`). Skip unrelated dependency and build directories.

- Consider every ADR directory found, not just the first. Inventory records recursively, including nested folders and records stored as one directory per decision. Use indexes or titles to triage, then read each potentially relevant record and follow links to related or superseding records. Ignore placeholders such as `.gitkeep`. If no directories or records exist, say so briefly and continue with other evidence.
- Note each relevant decision's scope and status. Treat applicable accepted decisions as constraints, but do not treat proposed, rejected, or superseded records as current policy. If status or scope is unclear, do not assume it. Cite repository-relative paths when relying on decisions.
- Read the relevant design docs and any glossary or context docs (e.g. `GLOSSARY.md` or `CONTEXT.md`). Use the project's terms, and flag when the ticket uses an established term differently or gives two concepts the same name.
- When the issue, code, and docs disagree, cite the conflicting evidence and ask whether to keep the existing decision, fix drift, or deliberately change it. Do not silently override a decision or re-ask questions the docs already answer. Revisit the relevant records if the interview shifts to another component or decision.

## Record lasting decisions

When a domain term is settled, propose a short glossary change in the relevant context if it adds or corrects shared vocabulary. Propose a new or superseding ADR only when the decision is hard to reverse, would surprise a future maintainer without context, and reflects a real trade-off. Follow the repository's existing location and format; don't rewrite a historical decision to hide the change. Apply documentation edits only with explicit user approval. Otherwise, record proposed updates in the brief. Don't create empty docs just because no ADRs or glossary exist.

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
7. **Technical constraints and relevant repository evidence:** cite applicable ADRs and docs; note conflicts, decisions to supersede, and proposed or approved documentation updates.
8. **Validation approach.**
9. **Dependencies, risks, and rollout considerations.**
10. **Implementation discretion:** choices the coding agent may make without asking again.
11. **Unresolved blockers:** only items that still require a decision.
12. **Suggested issue revision:** a self-contained Markdown body or focused patch that incorporates the clarified decisions.

Make the brief standalone so a fresh coding agent does not need the interview transcript.

## Save the session as a comment by default

At the end of every completed or explicitly user-stopped interview, automatically add the standalone implementation brief to the resolved issue as a comment. Do not ask for an additional save confirmation or a choice of save method. An interrupted session is not a completed interview; do not claim it was saved.

- Honor any request not to save or to keep the session local. If the user opts out, return the brief in chat without a GitHub write.
- Use a clear heading such as `## Grill session brief`. Save the decisions, approved assumptions, readiness, and unresolved blockers, including when the user stops early. Save a useful session summary, not the raw transcript; exclude credentials and unrelated private information.
- Show the brief in chat before posting it. Preserve the original issue body and metadata; the suggested issue revision remains a proposal inside the comment.
- Prefer the authenticated GitHub connector; fall back to `gh issue comment <issue-url> --body-file <file>` when necessary.
- Verify the saved comment and return its direct URL along with the canonical issue link. Never claim a successful save without verification.
- If posting fails or permissions are unavailable, say the brief was not saved and provide the Markdown in chat for manual posting. If the result of a write is uncertain, check existing comments before retrying to avoid duplicates. On resume, do not repost an already-saved unchanged brief.

## Keep other GitHub changes opt-in

The default session comment is the only automatically authorized GitHub mutation. Do not edit the issue body, change metadata, or create related work unless the user explicitly requests that write. Before an issue-body update, show the exact proposed text and confirm the target issue. After an approved update, verify the resulting issue and report its canonical URL.
