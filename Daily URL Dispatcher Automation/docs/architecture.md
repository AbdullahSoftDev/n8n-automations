# Architecture

## High-Level Design

The workflow is a scheduled queue processor.

```text
Google Sheets
     │
     ▼
Select first unsent item
     │
     ├───────────────┐
     ▼               ▼
 yt-dlp          Cloudflare AI
     │               │
     ▼               ▼
 MP3 binary      Image binary
     │               │
     ▼               ▼
 Google Drive    Google Drive
     └───────┬───────┘
             ▼
        Merge / Wait
             │
             ▼
           Gmail
             │
             ▼
       Status = Sent
```

## Queue State

The current workflow uses a deliberately simple state model:

```text
empty → processing → Sent
```

There is no explicit `Processing` write in the supplied workflow. Operationally, the item is selected while its Status remains empty and is changed to `Sent` after successful Gmail delivery.

## Branching

After `Pick Unsent URL`, two branches run:

### Audio branch

```text
Pick Unsent URL
→ YouTube to MP3
→ Read MP3 File
→ Name MP3
→ Upload MP3 to Drive
```

### Cover branch

```text
Pick Unsent URL
→ Generate Image (Cloudflare)
→ Image to Binary
→ Upload Cover to Drive
```

The branches converge at:

```text
Wait for Both Uploads
```

Only after both branches reach the merge does the email step run.

## Data Dependencies

The selected row supplies:

```text
URL
Title
row_number
```

`Title` is used by:

- cover-art prompt
- MP3 filename
- cover filename
- Gmail subject
- Gmail message

`URL` is used by:

- yt-dlp
- Gmail message

`row_number` is used by:

- Mark as Sent

## Production Upgrade Path

For larger workloads, replace the spreadsheet queue with a database/job queue and add:

- unique IDs
- retries
- failure states
- dead-letter handling
- structured logs
- concurrency limits
- idempotency
- cleanup/rollback
