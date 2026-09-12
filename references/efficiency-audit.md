# Delegation efficiency audit

Read this reference only when the user explicitly requests token consumption, agent efficiency, orchestration analysis, or a retrospective.

## Evidence order

Use evidence in this order:

1. Usage information exposed directly by the current Codex interface or tools.
2. Subagent completion metadata and thread summaries.
3. Local session records only when the user explicitly wants forensic accounting, permissions allow access, and no stable surfaced metric answers the question.

Do not present internal database fields or rollout formats as stable Codex APIs. State the source, timestamp, and limitations of every usage figure.

## Per-session record

Collect when available:

- Agent role and owned scope.
- Effective model and reasoning effort.
- Input, cached input, output, reasoning, and total tokens.
- Start/end window or elapsed time.
- Number of correction or resumed turns.
- Files and executable LOC changed.
- Validation commands and results.
- Acceptance criteria satisfied or missed.

If a session was resumed and a surfaced cumulative counter resets, sum reliable per-call records only when those records are available and clearly belong to the session. Otherwise report that exact per-session consumption is unavailable.

## Metrics

Keep these concepts separate:

- **Raw tokens:** the usage reported by Codex or its available records.
- **Cached input:** repeated context that may have different cost characteristics but still represents processed context.
- **Uncached input plus output:** an optional heuristic for newly introduced context and generated work. Never call it billing, exact compute, or official efficiency.
- **Wall-clock time:** may overlap across parallel agents and must not be summed as elapsed project time.

Do not infer efficiency from token volume alone. A high-cost agent may prevent a production defect; a low-cost agent may create expensive rework.

## Assessment dimensions

Score separately:

1. **Outcome effectiveness** — correctness, completeness, acceptance coverage, and validation quality.
2. **Delegation effectiveness** — decomposition, ownership clarity, safe parallelism, and coordinator review.
3. **Model efficiency** — whether model and reasoning matched the actual difficulty.
4. **Token efficiency** — duplicated context, unnecessary agents, repeated resumes, and remediation.
5. **Time efficiency** — useful parallelism versus waiting, conflicts, and redundant checks.

## Required report shape

Return:

1. Executive conclusion.
2. Per-session usage table with totals and percentages.
3. Methodology and data limitations.
4. Outcome, delegation, model, token, and time assessments.
5. Specific waste supported by observed evidence.
6. Recommendations tied to that evidence.

Do not claim exact cost unless Codex exposes authoritative billing data for the relevant account and run.
