# Token-cost comparison

Use this guide when optional delegation or model selection has a meaningful cost tradeoff. Quality and required validation are constraints, not costs to minimize away. Compare only workflows and models that can reasonably meet them.

The supplied relative rates are the same across all token categories: Terra costs 2.5 times Luna, and Sol costs 5 times Luna (2 times Terra). The listed rates are:

| Model | Uncached input | Cached input | Cache writes | Output |
|---|---:|---:|---:|---:|
| Luna | $1.00 | $0.10 | $1.25 | $6.00 |
| Terra | $2.50 | $0.25 | $3.125 | $15.00 |
| Sol | $5.00 | $0.50 | $6.25 | $30.00 |

Use current runtime pricing instead if known. Apply all rates on the same billing-unit basis. Estimate each option as the sum of uncached input, cached input, cache writes, and output multiplied by their respective rates across the parent and every subagent. Include dispatch, repeated context, scoped validation, integration review, and likely correction turns. Do not compare raw token counts or subagent cost alone.

Use rough ranges when usage is uncertain; do not invent precise savings. If a lower-priced model is likely to fail or need a stronger-model retry, include that expected rework and choose the model more likely to finish correctly. A more expensive workflow is justified when its credible quality benefit matters for the task; state that tradeoff rather than claiming cost savings.
