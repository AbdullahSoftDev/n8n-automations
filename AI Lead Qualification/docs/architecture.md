# Architecture

```mermaid
flowchart TD
  A[Browser form / website] --> B[POST /new-lead-ai]
  B --> C[Validate and normalize]
  C --> D{Valid?}
  D -->|No| E[400 JSON]
  D -->|Yes| F[Baseline intent and rule scoring]
  F --> G[Optional Gemini enrichment]
  G --> H[Apply intent caps and final score]
  H --> I[Google Sheets]
  I --> J{Spam?}
  J -->|Yes| K[Quarantine / log only]
  J -->|No| L{Hot threshold?}
  L -->|Yes| M[Slack + priority acknowledgement]
  L -->|No| N[Standard acknowledgement]
```

The included JSON implements webhook intake, field validation, deterministic baseline intent detection, scoring, and a JSON response. It does not include AI provider credentials or external side-effect nodes.

Suggested production extensions: add an LLM node with structured output; validate the output and clamp scores in code; append records to Google Sheets; route spam away from all outbound messaging; add Slack/Gmail nodes; and add idempotency plus error handling.
