---
name: delegator
description: Coordinate explicitly requested Codex subagents with strict disjoint ownership, deliberate model and reasoning selection, scoped validation, and final integration review. Use only when invoked as $delegator.
---

# Delegator

Coordinate the smallest effective set of Codex subagents. The user's explicit instructions override this skill, including whether to delegate, agent count, model, reasoning effort, ownership, sequence, and validation.

This skill is explicit-only. Invocation starts with a delegation decision; it does not imply delegation is worthwhile. If the user requires subagents, delegate. Otherwise, keep the task in the main session when coordination and repeated context cost more than the expected benefit. Do not delegate trivial edits, formatting, imports, or small fixes.

Use Codex subagents for work owned by the current request. Do not create user-visible tasks or threads unless the user explicitly asks for separate tasks.

## 1. Preflight and decomposition

Before dispatch:

- Read applicable `AGENTS.md` and repository instructions.
- Inspect current changes so user work is not overwritten or silently absorbed.
- Identify shared interfaces, generated files, lockfiles, fixtures, servers, databases, build outputs, and mutable test resources.
- Check the parent permission and sandbox mode; subagents inherit these unless the runtime supports an override.

Delegate only when the user requires it, when substantial investigation should be isolated from implementation context, when risky implementation benefits from separate ownership, or when substantial workstreams are totally disjoint.

Use no more than three active subagents by default. Increase this only at the user's request or when every additional workstream is independently useful and totally disjoint.

### Announce the delegation plan

Before spawning any subagent, briefly explain the delegation plan to the user. For each planned subagent, state:

- its task name or role;
- its assigned work and read/write scope;
- its model and reasoning effort;
- whether it runs sequentially or in parallel, with a brief reason for that ordering.

Runtime agent IDs do not exist until spawn, so the pre-dispatch explanation identifies agents by planned task name or role. If the plan changes after dispatch, disclose the revised assignment and model choice before spawning any replacement or additional subagent.

## 2. Use Codex subagents

Do not depend on personal custom agents. Use the standard subagent capability exposed by the current Codex surface. When that surface supports selecting named built-ins, use `explorer` for read-only discovery, `worker` for implementation and fixes, and `default` only when neither role fits. When it does not expose role selection, state the role and read/write boundary in the task contract instead of inventing a custom agent.

Keep final integration ownership in the main session.

Do not dispatch a subagent under permissions that make its implementation or validation knowingly impossible.

## 3. Prefer Terra by default and use Luna for simple work

Inspect the models and reasoning levels available in the current Codex runtime. Set both model and reasoning effort explicitly for every spawn; do not rely on parent inheritance. Verify effective settings after spawn when Codex exposes them.

Honor an exact user selection as an override. If the selected model or reasoning level is unavailable, explain the limitation and proposed substitution before dispatch; never substitute silently.

Personal defaults when available:

| Work | Model | Reasoning |
|---|---|---|
| Simple, narrow, low-risk exploration, log analysis, implementation, fixes, and scoped validation | `gpt-5.6-luna` | `medium` |
| Default eligible non-simple subagent work: broad or high-risk implementation, complex debugging, concurrency, cross-boundary changes, or recovery after a failed Luna attempt | `gpt-5.6-terra` | `medium` or `high` |
| Rare, unusually difficult planning or architectural analysis | `gpt-5.6-sol` | `high` |

Terra is the default for assignments that are demonstrably unsuitable for Luna. Use Luna when the assignment is simple, narrow, and low risk, or whenever there is no concrete evidence that Luna is inadequate. Choose Terra only when material complexity, breadth, ambiguity, or risk rules Luna out; a failed Luna attempt is sufficient but not required evidence. Before dispatch, state the observable task characteristics that disqualify Luna rather than merely labeling the work complex or risky.

Do not use Sol for ordinary implementation, debugging, exploration, or validation. Reserve it for rare planning or architectural-analysis assignments whose ambiguity and impact justify the additional capability, and state that justification before dispatch. An explicit user model choice may override these defaults.

Use Astra only when the user explicitly selects it. Use `xhigh`, `max`, or `ultra` only when supported and justified by concrete difficulty. Improve a vague contract instead of compensating with more reasoning. If the user did not require a model and a default is unavailable, choose the nearest role-equivalent and disclose the substitution before dispatch.

## 4. Prove parallel work is totally disjoint

Parallel execution is allowed only when all conditions hold:

- Write sets do not overlap.
- No agent changes an interface consumed by another active agent.
- No shared generated file, lockfile, fixture, snapshot, or build artifact can change.
- Neither workstream depends on the other's decisions or output.
- Validation does not contend for a mutable database, server, browser session, test account, port, or output directory.

If any condition is uncertain, sequence the agents. Default to sequential writers. Read-only agents may run in parallel only when their scopes and tool use cannot interfere. Before dispatch, state the ownership map and why each parallel pair is disjoint.

## 5. Give each subagent a compact contract

Use this shape:

```text
Goal:
Context:
Scope:
Negative constraints:
Done when:
Return:
```

- **Goal:** one observable result.
- **Context:** only relevant paths, behavior, project rules, and prior decisions.
- **Scope:** owned files or subsystem and whether editing is allowed.
- **Negative constraints:** forbidden changes, abstractions, dependencies, files, and behavior.
- **Done when:** measurable acceptance criteria and required validation.
- **Return:** changed files, criteria checklist, exact commands and results, unverified items, and residual risks.

Do not paste the full conversation or large documents when paths and a short summary suffice. Do not ask a subagent to improve unrelated code.

## 6. Let subagents validate their scope

Allow every instrument available under current permissions that can validate the assigned scope: repository search, shell commands, type checking, linting, tests, browser tools, screenshots, logs, or documentation lookup.

Each implementation subagent must review its diff against `Done when` and `Negative constraints`, run focused tests for owned behavior, run any broader check needed to establish scoped correctness, and return exact evidence. Do not restrict it to cheap checks when stronger scoped validation is available.

A subagent must not validate against another active writer's incomplete changes. If agents need the same mutable test resource, sequence their validation or the agents themselves.

## 7. Coordinate and remediate

Use Codex's native wait mechanism instead of repeated polling. Wait for all agents needed by the next dependency gate. Request concise completion packets, steer only when new information materially changes a contract, and stop obsolete or incorrectly scoped work.

Allow one correction turn after a major criterion is missed. If the corrected result still violates a major criterion, stop that agent for the workstream. The coordinator then fixes it directly or rewrites the contract before a fresh dispatch. Keep mechanical integration fixes in the main session unless the user assigned all edits to subagents.

## 8. Review total integration

Subagent completion is evidence, not approval. The coordinator must review the combined diff and high-risk paths, verify every acceptance and negative constraint, check cross-boundary composition, and run repository-required integrated type checks, lint, tests, and builds. Run deterministic checks before credentialed, external, or flaky E2E checks. Do not rerun an unchanged external test without evidence of a transient failure.

Finish with agents used, effective model and reasoning, model-selection rationale, owned scopes, proof of any parallel disjointness, subagent validation, coordinator integration checks, and remaining uncertainty. For every Terra agent, prove that Luna was not a viable option by citing concrete characteristics or evidence from the assignment. A generic claim that the work was complex, broad, or risky is insufficient. If the report cannot establish why Luna was unsuitable, use Luna instead.
