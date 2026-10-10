# Setup

## Requirements
- A running n8n instance.
- A browser to open the demo.
- Optional Gemini/LLM, Google Sheets, Slack, and Gmail credentials for a production extension.

## Import
1. Open n8n.
2. Choose Workflows → Import from File (wording varies by version).
3. Select `workflow/ai-lead-qualification-starter.json`.
4. Save the workflow.

This starter is designed to run without credentials. It does not call external services.

## Webhook
The path is `new-lead-ai`, accepting POST JSON:

```json
{
  "name": "Daniel Carter",
  "email": "daniel@example.com",
  "company": "Northstar Operations",
  "message": "Please send pricing and arrange a demo next week."
}
```

For local development, click Listen for Test Event / Execute workflow and use the Test URL, commonly `http://localhost:5678/webhook-test/new-lead-ai`.

After publishing/activating, use the Production URL shown by the node, commonly `http://localhost:5678/webhook/new-lead-ai`.

## PowerShell test

```powershell
$body = @{
  name = "Daniel Carter"
  email = "daniel@example.com"
  company = "Northstar Operations"
  message = "Our team needs annual pricing for 120 employees and would like a product demonstration next week."
} | ConvertTo-Json

Invoke-RestMethod `
  -Method Post `
  -Uri "http://localhost:5678/webhook-test/new-lead-ai" `
  -ContentType "application/json" `
  -Body $body
```

A successful starter response confirms intake and baseline processing only—not AI, Sheets, Slack, or Gmail delivery.

## Production extensions
- Ask Gemini/another model for structured `intent`, `urgency`, `fit_score`, `summary`, and `suggested_reply`.
- Treat the enquiry as untrusted data; validate the returned schema and clamp scores in deterministic code.
- Create a `Leads` sheet and map all lead fields.
- Add Slack only on the non-spam hot-lead branch.
- Add Gmail acknowledgement branches with reviewed templates.
- Restrict CORS, add authentication/rate limits, idempotency, and error monitoring.
- Never commit secrets or credentials to Git.
