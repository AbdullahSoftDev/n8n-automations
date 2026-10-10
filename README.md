<div align="center">

# n8n Automations

### Reusable n8n workflows for lead generation, booking reminders, and business process automation.

Build practical automations that connect web forms, APIs, Google Workspace, Slack, email, and AI services.

<p>
  <a href="https://github.com/AbdullahSoftDev/n8n-automations"><img src="https://img.shields.io/badge/n8n-Workflow%20Automation-EA4B71?logo=n8n&logoColor=white" alt="n8n workflow automation"></a>
  <img src="https://img.shields.io/badge/Reusable-Workflows-2563EB" alt="Reusable workflows">
  <img src="https://img.shields.io/badge/Focus-Business%20Automation-0F766E" alt="Business automation">
</p>

<p>
  <a href="#available-workflows">Explore Workflows</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#contributing">Contribute</a>
</p>

</div>

---

## About This Repository

**n8n Automations** is a growing library of practical, reusable [n8n](https://n8n.io/) workflows for automating everyday business processes. The projects demonstrate workflow automation patterns for capturing and qualifying leads, managing appointment bookings, sending reminders, moving files, and connecting common SaaS tools.

Each automation is organized as a separate project with its workflow JSON, documentation, demo assets, and setup instructions where available. The goal is to make each workflow easier to understand, import, configure, test, and adapt.

**Keywords:** n8n automation, n8n workflows, workflow templates, business process automation, lead capture automation, lead scoring, booking reminders, Google Sheets automation, Google Calendar integration, Slack notifications.

## Available Workflows

| Workflow | What it does | Links |
|---|---|---|
| **Lead Capture, Scoring & Follow-up** | Captures website inquiries, calculates a transparent lead score, logs leads in Google Sheets, alerts sales through Slack, and sends an email response. | [README](./Lead%20Capture,%20Scoring%20and%20Follow-up/) · [Workflow files](./Lead%20Capture,%20Scoring%20and%20Follow-up/workflow/) · [Demo](./Lead%20Capture,%20Scoring%20and%20Follow-up/demo/) |
| **Booking Confirmation & Automated Reminders** | Validates booking submissions, creates Google Calendar events, stores booking records, emails confirmations, and sends scheduled reminders. | [README](./Booking%20and%20Reminder%20Automation/) · [Workflow files](./Booking%20and%20Reminder%20Automation/workflow/) · [Demo](./Booking%20and%20Reminder%20Automation/demo/) |
| **Daily URL Dispatcher** | Reads the next URL from a Google Sheet, processes media files, uploads outputs to Google Drive, sends a notification email, and updates the row status. | [README](./Daily%20URL%20Dispatcher/) |

## Why Use These n8n Workflows?

- **Practical use cases:** workflows are built around repeatable tasks and business processes.
- **Readable structure:** separate folders make workflow files and documentation easier to find.
- **Adaptable integrations:** configure supported services and credentials for your own environment.
- **Documented setup:** each workflow's README explains its purpose, required services, and configuration.
- **Learning by example:** explore triggers, conditional routing, data transformation, API integrations, and scheduled automation.

## Quick Start

1. Open the workflow directory you want to try.
2. Read that workflow's README and setup documentation.
3. Download or open the relevant JSON file in the `workflow/` directory.
4. In your n8n instance, choose **Import from File** and select the workflow JSON.
5. Configure the required credentials, IDs, URLs, and environment-specific values.
6. Test with sample data before activating the workflow.

Requirements vary by project. Workflows that use Google Sheets, Google Calendar, Gmail, or Slack require the corresponding accounts and n8n credentials. Importing a workflow does not automatically connect your accounts.

## Repository Structure

```text
n8n-automations/
├── Lead Capture, Scoring and Follow-up/
│   ├── workflow/
│   ├── demo/
│   ├── docs/
│   └── README.md
├── Booking and Reminder Automation/
│   ├── workflow/
│   ├── demo/
│   ├── docs/
│   └── README.md
├── Daily URL Dispatcher/
│   ├── workflow/
│   ├── demo/
│   ├── docs/
│   └── README.md
└── README.md
```

## Technology and Integrations

The collection uses **n8n**, webhooks, REST APIs, JavaScript, and integrations with tools such as Google Sheets, Google Calendar, Gmail, Slack, and Google Drive. The exact requirements depend on the individual workflow; consult its documentation before importing.

## Security Notes

- Never commit API keys, passwords, OAuth secrets, webhook secrets, or private credentials.
- Use n8n's credential system for authentication.
- Review imported nodes and expressions before running a workflow.
- Add authentication, validation, rate limiting, and error handling before exposing webhooks to public or production traffic.
- Test with non-sensitive sample data first.

These projects are reusable examples and starting points. Review and harden each workflow for your own environment before production use.

## Roadmap

- [x] Lead capture, scoring, and follow-up
- [x] Booking confirmation and calendar integration
- [x] Automated appointment reminders
- [x] Scheduled URL and file processing
- [ ] CRM synchronization workflows
- [ ] WhatsApp follow-up automation
- [ ] AI-assisted customer support
- [ ] Reporting and monitoring workflows
- [ ] Error recovery and alerting patterns

## Contributing

Suggestions, bug reports, and improvements are welcome. When proposing a new workflow, include its business use case, required integrations, setup instructions, sample inputs, expected outputs, and known limitations. Remove secrets and personal data before sharing workflow exports.

## Author

**Muhammad Abdullah** · Full-Stack Developer | AI Applications & Workflow Automation

- GitHub: [@AbdullahSoftDev](https://github.com/AbdullahSoftDev)
- LinkedIn: [abdullahsoftdev](https://linkedin.com/in/abdullahsoftdev)

If this workflow library helps you, consider starring the repository and checking back as more n8n automation examples are added.
