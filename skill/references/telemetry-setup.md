# Token telemetry setup

Read this reference only when the user asks to configure or troubleshoot durable token telemetry. Telemetry must be enabled before the run; there is no prompt or command that can recreate missing telemetry afterward.

## Configure export

Use the user-level `~/.codex/config.toml`. Project `.codex/config.toml` files cannot override telemetry routing. Do not change user configuration without explicit permission.

```toml
[otel]
environment = "development"
log_user_prompt = false

exporter = { otlp-http = {
  endpoint = "http://localhost:4318/v1/logs",
  protocol = "binary"
} }
```

An OTLP collector must listen at the configured endpoint and persist or forward the logs to storage that can later be queried. Fully restart Codex after changing the configuration. Export then happens automatically; exporters batch asynchronously and flush on shutdown.

Relevant log events include:

- `codex.conversation_starts` for model and reasoning settings.
- `codex.sse_event` with `response.completed` for token counts.
- `codex.tool_result` for tool execution information.

See the official [configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference) and [observability documentation](https://learn.chatgpt.com/docs/config-file/config-advanced#observability-and-telemetry).

## Reporting limits

The collector output must be supplied by the user or readable under current permissions. Desktop OTel metadata includes conversation identifiers, but a stable subagent identifier is not guaranteed; attribute subagents only when conversation identifiers and timestamps provide reliable correlation.

For API-managed workflows, Agents API sessions and subagent turns expose best-effort structured `usage` objects. See [Agents API session usage](https://developers.openai.com/api/reference/typescript/resources/beta/subresources/agents/subresources/sessions/methods/retrieve).

The `[analytics] enabled` setting controls separate anonymous product analytics. It does not make OTel records available for a Delegator report.
