# Delegation efficiency audit

Read this reference only after the user requests token consumption, agent efficiency, orchestration analysis, or a retrospective, including consent to the end-of-run offer. For the routine end-of-run report, return only the brief usage and relative-cost summary below; include the broader assessment sections only when the user asks for an efficiency audit or retrospective.

## Evidence order

Use evidence in this order:

1. Usage snapshots retained from runtime metadata as each session completed.
2. Preconfigured OpenTelemetry records supplied by the user or available from readable collector storage.
3. Usage information still exposed directly by the current Codex interface or tools.
4. Subagent completion metadata and thread summaries.
5. Local session records only when the user explicitly wants forensic accounting, permissions allow access, and no stable surfaced metric answers the question.

Do not present internal database fields or rollout formats as stable Codex APIs. State the source, timestamp, and limitations of every usage figure.

OpenTelemetry identifies conversations and records model and token data, but desktop subagent attribution is not guaranteed. Correlate only when conversation identifiers and timestamps support it; otherwise report aggregate or unattributed usage. Telemetry cannot be enabled retroactively for a completed run. Because export is asynchronous, treat missing recent events as pending or unavailable rather than zero and do not claim completeness.

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

## Relative-cost calculation

Use this user-provided Standard / Batch / Flex / Fast price schedule in USD per 1M tokens. Select the short- or long-context columns from surfaced runtime information. If the context class is unavailable, show both estimates as a range instead of guessing.

| Model | Short input | Short cached input | Short cache writes | Short output | Long input | Long cached input | Long cache writes | Long output |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| `gpt-6-astra` | $10.00 | $1.00 | $12.50 | $50.00 | $20.00 | $2.00 | $25.00 | $75.00 |
| `gpt-5.6-sol` | $4.00 | $0.40 | $5.00 | $20.00 | $8.00 | $0.80 | $10.00 | $30.00 |
| `gpt-5.6-terra` | $2.00 | $0.20 | $2.50 | $12.00 | $4.00 | $0.40 | $5.00 | $18.00 |
| `gpt-5.6-luna` | $0.20 | $0.02 | $0.25 | $1.20 | $0.40 | $0.04 | $0.50 | $1.80 |

For each session, calculate:

```text
relative cost = (input tokens × input rate
               + cached-input tokens × cached-input rate
               + cache-write tokens × cache-write rate
               + output tokens × output rate) / 1,000,000
```

Do not double-count cached input if the surfaced input count already includes it: subtract cached input from total input before applying the ordinary input rate. Do not add reasoning tokens separately when they are already included in output tokens. Treat a missing category as unavailable, not zero, unless the runtime explicitly reports zero. Label the result a relative-cost estimate based on the user-provided schedule, not an authoritative bill.

## Brief end-of-run report

Return a compact table with one row for the main session and one for each subagent. Include the effective model, input, cached input, cache writes, output, total tokens, share of known total tokens, and relative-cost estimate. Add a totals row, then one short methodology note naming missing data, context-class handling, and the calculation timestamp. Omit efficiency scores and recommendations unless requested.

## Assessment dimensions

Score separately:

1. **Outcome effectiveness** — correctness, completeness, acceptance coverage, and validation quality.
2. **Delegation effectiveness** — decomposition, ownership clarity, safe parallelism, and coordinator review.
3. **Model efficiency** — whether model and reasoning matched the actual difficulty.
4. **Token efficiency** — duplicated context, unnecessary agents, repeated resumes, and remediation.
5. **Time efficiency** — useful parallelism versus waiting, conflicts, and redundant checks.

## Full audit report shape

Return:

1. Executive conclusion.
2. Per-session usage table with totals and percentages.
3. Methodology and data limitations.
4. Outcome, delegation, model, token, and time assessments.
5. Specific waste supported by observed evidence.
6. Recommendations tied to that evidence.

Do not claim exact cost unless Codex exposes authoritative billing data for the relevant account and run.
