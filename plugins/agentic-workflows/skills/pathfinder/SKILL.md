---
name: pathfinder
description: Turn a large, ambiguous initiative into an evidence-based execution path and create agent-ready GitHub issues in a user-selected GitHub Project. Use when the user wants to investigate a repository, decompose an outcome, build a backlog or roadmap, plan a substantial change, or create a dependency-aware series of GitHub tickets from current state to desired state.
---

# Pathfinder

Transform an uncertain body of work into a reviewed, dependency-aware set of GitHub issues that another coding agent can execute with minimal rediscovery.

## Establish the destination

1. Treat the current repository as the default source repository and implementation context.
2. Require a desired outcome and a target GitHub Project. Accept a project URL, or an unambiguous owner plus project number/title.
3. If the project is missing, list accessible projects when tooling allows and ask the user to select one. Never choose a project silently.
4. Resolve the issue repository separately from the Project. Default to the current repository; confirm before using another repository.
5. Ask one concise, consolidated question only when the outcome, project, repository, or a material constraint cannot be discovered safely.

## Investigate the current state

Perform read-only discovery before proposing issues:

- Read applicable `AGENTS.md` files, the README, architecture and product documentation, manifests, tests, and relevant implementation paths.
- Inspect repository history and existing GitHub issues when they clarify prior decisions, active work, or likely duplication.
- Trace the real code paths and name relevant files, modules, symbols, APIs, and commands.
- Separate verified facts, reasonable inferences, and unresolved questions. Do not invent repository behavior.
- Search open and recently closed issues for overlapping work before drafting new ones.

Restate the target outcome in observable terms. Identify the gap between current and desired behavior, constraints, workstreams, dependencies, integration points, migration or rollout needs, and validation strategy.

## Build the execution path

Create the smallest coherent set of issues that fully spans the gap:

- Make each issue independently understandable and deliver one testable outcome.
- Size each issue for one coding agent and one bounded working context. Split tickets that require unrelated changes or have multiple independent success conditions.
- Preserve meaningful vertical slices when they reduce integration risk; do not create microscopic file-by-file chores.
- Put prerequisites before dependants and organize independent work into parallel waves.
- Create a discovery spike only when an important unknown cannot be resolved from available evidence. Give the spike a concrete artifact or decision as its deliverable.
- Include implementation, tests, documentation, compatibility, observability, migration, and cleanup work only where the initiative actually requires them.
- Avoid estimates, assignees, priority, dates, labels, milestones, or Project field values unless the user supplies them or explicitly asks for recommendations.

Draft every issue using [references/issue-template.md](references/issue-template.md). Use temporary IDs such as `PF-01` while reviewing the plan. Express dependencies by temporary ID until GitHub issue numbers exist.

## Review before writing

Present the proposed backlog before creating anything. Include:

- The resolved repository and GitHub Project.
- A brief current-state and target-state summary.
- A table of temporary ID, issue title, type, dependencies, and execution wave.
- The complete proposed issue bodies, or offer them in manageable batches when the backlog is large.
- Existing issues that may replace or overlap proposed work.
- Material assumptions, unresolved risks, and any proposed metadata.

Ask for explicit approval to create the reviewed issues and add them to the resolved Project. Treat edits as approval only after presenting the revised plan. Approval to create tickets does not authorize source-code changes, new labels, a new Project, assignments, or other repository mutations.

## Create and attach the issues

After approval:

1. Prefer an authenticated GitHub connector that supports issue and Project operations. Otherwise use the `gh` CLI and verify authentication first.
2. Resolve the Project to its exact owner, number, title, and URL before writing.
3. Recheck likely duplicates immediately before creation.
4. Create issues in topological order so each dependant can reference the real issue numbers of its prerequisites.
5. Add every created issue to the selected GitHub Project.
6. Set labels, milestones, assignees, status, iteration, priority, or other Project fields only when the user approved those exact values. Do not create missing metadata implicitly.
7. Verify each issue URL and its Project membership after creation.

When using `gh`, prefer non-interactive commands and body files or safely quoted input. Typical operations are:

```text
gh repo view --json nameWithOwner,url
gh project view <number> --owner <owner> --format json
gh issue list --repo <owner/repo> --state all --search <terms>
gh issue create --repo <owner/repo> --title <title> --body-file <file>
gh project item-add <number> --owner <owner> --url <issue-url>
```

Adapt commands to the installed `gh` version and prefer connector operations when available.

## Preserve idempotency and recover safely

- Never issue a second create call blindly after a timeout or ambiguous failure. Search for the title and inspect recent issues first.
- Track created issue URLs during the run.
- If issue creation succeeds but Project attachment fails, keep the issue, retry only the attachment when safe, and report the partial state precisely.
- Do not delete or close successfully created issues to hide a partial failure.
- Stop before further writes if the resolved repository or Project differs from what the user approved.

## Report the result

Return the Project URL and all issue links in recommended execution order. Show dependency waves, note reused existing issues, and identify any partial failures or fields left unset. Keep the summary concise because each issue must contain its own complete execution context.
