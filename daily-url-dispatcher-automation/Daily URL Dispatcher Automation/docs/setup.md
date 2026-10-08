# Setup Guide

## 1. Requirements

- n8n
- Google Sheets OAuth credential
- Google Drive OAuth credential
- Gmail OAuth credential
- Cloudflare account with Workers AI access
- `yt-dlp`
- Node.js runtime for yt-dlp JavaScript support
- FFmpeg/audio conversion support
- An n8n deployment where `Execute Command` is available

## 2. Google Sheet

Create a spreadsheet with a tab named `Data`.

Recommended header row:

```text
URL | Title | Status
```

Example:

```text
https://example.com/test | Test Song |
https://example.com/test-2 | Another Song | Sent
```

Only rows with a non-empty URL and an empty/`empty` Status are eligible.

## 3. Import

Import:

```text
workflow/daily-url-dispatcher.json
```

The repository version is sanitized for public GitHub use. Configure your own credentials and replace the placeholders.

## 4. Google Sheets

Configure `Get All Rows` and `Mark as Sent`.

Set the same spreadsheet and the `Data` sheet.

## 5. Gmail

Configure the Gmail credential and set the destination address in `Send Gmail`.

## 6. Cloudflare

Configure the HTTP Request node:

```text
POST https://api.cloudflare.com/client/v4/accounts/YOUR_ACCOUNT_ID/ai/run/@cf/black-forest-labs/flux-2-klein-4b
```

Use a secure credential/header mechanism for the Cloudflare token rather than committing the token into the workflow.

## 7. yt-dlp

The Execute Command node expects `yt-dlp` to be available to the n8n runtime.

Verify:

```bash
yt-dlp --version
```

The workflow writes temporary output to:

```text
/home/node/.n8n-files/song.mp3
```

## 8. Google Drive

Configure both Drive upload nodes and select the destination folder.

## 9. Schedule

The supplied cron is:

```text
0 9 * * *
```

This runs every day at 09:00 according to the n8n timezone.

## 10. Test

Use one eligible row and execute the workflow manually before activating the schedule.

Expected lifecycle:

```text
Sheet → Select → MP3 + Cover → Drive → Gmail → Status Sent
```
