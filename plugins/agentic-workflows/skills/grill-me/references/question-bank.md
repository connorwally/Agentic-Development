# Ambiguity question bank

Use this as a selective probe library. Ask only questions whose answers could materially change the implementation or definition of done.

## Outcome and users

- What observable user or system outcome must change?
- Who experiences the problem, and are different actors expected to behave differently?
- What evidence would prove the problem is solved rather than merely moved?
- Which stated goal is primary when goals conflict?

## Scope and boundaries

- What is the smallest acceptable scope for this ticket?
- Which adjacent workflows must remain unchanged?
- Are there explicit non-goals or follow-up issues?
- Does the behavior apply globally, per tenant, per account, per environment, or only to new data?

## Behavior and edge cases

- What should happen on empty, malformed, duplicate, stale, partial, or conflicting input?
- What is the expected failure behavior: reject, retry, degrade, queue, roll back, or alert?
- Must the operation be idempotent, atomic, reversible, or safe to resume?
- What wins when configuration, stored state, and user input disagree?
- Are concurrency, ordering, time zones, localization, or accessibility material?

## Interfaces and data

- Which API, event, schema, CLI, configuration, or UI contract is authoritative?
- Is backward compatibility required, and for which consumers or versions?
- Are data migration, backfill, retention, deletion, or audit requirements involved?
- What permissions, authentication, privacy, or security boundaries apply?
- Can interfaces change, or must the work be additive?

## Quality attributes

- Is there a measurable latency, throughput, reliability, or resource target?
- What scale should the implementation safely handle?
- What logging, metrics, tracing, or alerting is required?
- What abuse cases or threat boundaries need explicit treatment?

## Delivery and operations

- Is a feature flag, staged rollout, migration order, or rollback path required?
- Must old and new behavior coexist during deployment?
- Are documentation, support, release-note, or operational-runbook changes part of done?
- Which other issues, teams, services, or external decisions block delivery?

## Acceptance and validation

- Which examples must pass, and which examples must be rejected?
- What automated tests belong at unit, integration, contract, or end-to-end level?
- What manual verification is necessary and in which environment?
- What must remain unchanged to demonstrate no regression?
- Who or what is the final source of acceptance?

## Signals that merit follow-up

Probe further when the issue contains:

- Adjectives without measures: “fast,” “simple,” “secure,” or “intuitive.”
- Verbs without results: “support,” “handle,” “improve,” or “refactor.”
- Unbounded nouns: “users,” “data,” “errors,” or “integrations.”
- Acceptance criteria that only describe implementation activity.
- A proposed solution without a stated problem or user outcome.
- Multiple outcomes with no priority or dependency order.
- Hidden compatibility, migration, permission, or rollout implications.
- Comments that contradict the issue body or each other.
