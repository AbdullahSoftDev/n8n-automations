<div align="center">

# 🎵 Daily URL Dispatcher — YouTube to MP3 + AI Cover + Google Drive + Gmail

### Automate scheduled URL-to-MP3 media processing with n8n, Google Sheets, yt-dlp, AI-generated cover art, Google Drive uploads, and Gmail delivery.

**Keywords:** n8n media automation, YouTube to MP3 workflow, scheduled URL processing, Google Sheets automation, AI cover image generation, Google Drive file upload, and automated Gmail delivery.

<p>
  <img src="https://img.shields.io/badge/n8n-Automation-EA4B71?logo=n8n&logoColor=white" alt="n8n">
  <img src="https://img.shields.io/badge/Google%20Sheets-Input-34A853?logo=googlesheets&logoColor=white" alt="Google Sheets">
  <img src="https://img.shields.io/badge/Google%20Drive-Storage-4285F4?logo=googledrive&logoColor=white" alt="Google Drive">
  <img src="https://img.shields.io/badge/Gmail-Delivery-EA4335?logo=gmail&logoColor=white" alt="Gmail">
  <img src="https://img.shields.io/badge/Cloudflare%20Workers%20AI-Cover%20Art-F38020?logo=cloudflare&logoColor=white" alt="Cloudflare">
  <img src="https://img.shields.io/badge/yt--dlp-Media%20Processing-111111" alt="yt-dlp">
</p>

<p>
  <a href="https://github.com/AbdullahSoftDev/n8n-automations">Automation Library</a> ·
  <a href="https://n8n.io/">n8n</a> ·
  <a href="https://github.com/AbdullahSoftDev">GitHub Profile</a>
</p>

</div>

---

## ⚡ Overview

**Daily URL Dispatcher** is a scheduled media-processing automation built with **n8n**.

Every day at **9:00 AM**, the workflow:

1. Reads all rows from a Google Sheet.
2. Finds the first row containing a URL whose `Status` is empty.
3. Downloads the referenced media as an MP3 using `yt-dlp`.
4. Generates square AI cover artwork with **Cloudflare Workers AI / FLUX**.
5. Converts the returned base64 image into an n8n binary file.
6. Uploads the cover image and MP3 to Google Drive.
7. Waits until both uploads are available.
8. Sends a Gmail containing links to both Drive files.
9. Marks the processed spreadsheet row as `Sent`.

> **Automation principle:** keep the queue in a simple spreadsheet, let n8n process one item on schedule, and maintain a clear `Sent` state.

---

## 🤖 What Problem Does It Solve?

Manually processing a queue of URLs can involve the same repetitive sequence every day:

**Find URL → download audio → create artwork → rename files → upload → copy links → send email → update spreadsheet**

This workflow turns that sequence into one scheduled pipeline.

### Before

```text
Google Sheet
    ↓
Manually choose URL
    ↓
Download audio
    ↓
Create artwork
    ↓
Rename files
    ↓
Upload to Drive
    ↓
Send links
    ↓
Update spreadsheet
```

### After

```text
              ┌──────────────────────┐
              │ Every Day at 9:00 AM │
              └──────────┬───────────┘
                         ↓
                ┌─────────────────┐
                │  Google Sheets  │
                │   Find queue    │
                └────────┬────────┘
                         ↓
                ┌─────────────────┐
                │ Pick first      │
                │ unsent URL      │
                └───────┬─────────┘
                        │
             ┌──────────┴──────────┐
             ↓                     ↓
      ┌──────────────┐      ┌────────────────┐
      │   yt-dlp     │      │ Cloudflare AI  │
      │ YouTube → MP3│      │ Generate Cover │
      └──────┬───────┘      └───────┬────────┘
             ↓                      ↓
      ┌──────────────┐      ┌────────────────┐
      │ Read / Name  │      │ Image → Binary │
      │     MP3      │      └───────┬────────┘
      └──────┬───────┘              ↓
             ↓               ┌────────────────┐
      ┌──────────────┐       │ Google Drive   │
      │ Google Drive │       │ Upload Cover   │
      │ Upload MP3   │       └───────┬────────┘
      └──────┬───────┘               │
             └──────────────┬────────┘
                            ↓
                   ┌─────────────────┐
                   │ Wait for Both   │
                   │ Uploads         │
                   └────────┬────────┘
                            ↓
                   ┌─────────────────┐
                   │      Gmail      │
                   │ Send both links│
                   └────────┬────────┘
                            ↓
                   ┌─────────────────┐
                   │ Google Sheets   │
                   │ Status = Sent   │
                   └─────────────────┘
```

---

## 🧩 Workflow Architecture

The automation is intentionally split into three logical branches after the queue item is selected.

```mermaid
flowchart TD
    A["⏰ Every Day at 9 AM"] --> B["📊 Get All Rows"]
    B --> C["🔎 Pick Unsent URL"]

    C --> D["🎵 YouTube to MP3<br/>yt-dlp"]
    D --> E["📁 Read MP3 File"]
    E --> F["🏷️ Name MP3"]
    F --> G["☁️ Upload MP3 to Drive"]

    C --> H["🎨 Generate Image<br/>Cloudflare Workers AI"]
    H --> I["🧬 Image to Binary"]
    I --> J["☁️ Upload Cover to Drive"]

    G --> K["🔀 Wait for Both Uploads"]
    J --> K

    K --> L["📧 Send Gmail"]
    L --> M["✅ Mark as Sent"]
```

---

## 🔄 Execution Flow

| # | Node | Responsibility |
|---|---|---|
| 01 | ⏰ **Every Day at 9 AM** | Starts the workflow on a daily cron schedule |
| 02 | 📊 **Get All Rows** | Reads the `Data` sheet |
| 03 | 🔎 **Pick Unsent URL** | Selects the first URL whose status is empty |
| 04 | 🎵 **YouTube to MP3 (yt-dlp)** | Extracts/downloads audio as MP3 |
| 05 | 📁 **Read MP3 File** | Loads the generated MP3 into n8n binary data |
| 06 | 🏷️ **Name MP3** | Sanitizes the title and creates the final filename |
| 07 | 🎨 **Generate Image (Cloudflare)** | Generates 1024×1024 cover art |
| 08 | 🧬 **Image to Binary** | Converts Cloudflare base64 output into an image file |
| 09 | ☁️ **Upload Cover to Drive** | Stores the generated cover in Google Drive |
| 10 | ☁️ **Upload MP3 to Drive** | Stores the MP3 in Google Drive |
| 11 | 🔀 **Wait for Both Uploads** | Synchronizes both branches |
| 12 | 📧 **Send Gmail** | Emails the Drive links |
| 13 | ✅ **Mark as Sent** | Updates the source row to `Sent` |

---

## 📊 Input Data Model

The workflow expects a Google Sheet named **`Data`**.

### Recommended columns

| Column | Required | Example | Purpose |
|---|---:|---|---|
| `URL` | ✅ | `https://...` | Source URL to process |
| `Title` | ✅ | `Example Song` | Used for filenames and email |
| `Status` | ❌ | `Sent` | Queue state |

### Example queue

| URL | Title | Status |
|---|---|---|
| `https://example.com/video-1` | Morning Drive | |
| `https://example.com/video-2` | Evening Session | |
| `https://example.com/video-3` | Weekend Mix | Sent |

The workflow processes **only the first eligible unsent row** on each daily execution.

---

## 🧠 Queue Logic

The `Pick Unsent URL` Code node applies this logic:

```javascript
const rows = $input.all().filter(item => {
  const status = String(item.json.Status || '').trim().toLowerCase();
  const url = String(item.json.URL || '').trim();

  return url !== '' && (status === '' || status === 'empty');
});

if (rows.length === 0) {
  return [];
}

return [rows[0]];
```

### In plain English

```text
URL exists?
   │
   ├── No  → Ignore row
   │
   └── Yes
        │
        ▼
Status empty?
   │
   ├── No  → Ignore row
   │
   └── Yes
        │
        ▼
Take first matching row
```

This makes the Google Sheet act as a simple processing queue.

---

## 🎵 MP3 Processing

The workflow uses `yt-dlp` through n8n's **Execute Command** node.

The configured command:

```bash
timeout 900 yt-dlp \
  --no-progress \
  --socket-timeout 30 \
  --js-runtimes node \
  --remote-components ejs:github \
  -x \
  --audio-format mp3 \
  --audio-quality 0 \
  --no-playlist \
  --force-overwrites \
  -o "/home/node/.n8n-files/song.%(ext)s" \
  "<URL>"
```

### Processing behavior

- Uses a 15-minute command timeout.
- Uses a 30-second socket timeout.
- Extracts audio with `-x`.
- Produces MP3 output.
- Uses highest available audio quality through `--audio-quality 0`.
- Prevents playlist processing.
- Overwrites the temporary output file.
- Writes the temporary file to:

```text
/home/node/.n8n-files/song.mp3
```

> **Important:** this requires an n8n environment where the Execute Command node is available and `yt-dlp` plus its required runtime/dependencies are installed.

---

## 🏷️ Automatic MP3 Naming

After the MP3 is read, the workflow uses the spreadsheet title to generate a clean filename.

Unsafe filename characters are removed:

```javascript
const title = $('Pick Unsent URL').first().json.Title || 'song';

const safe = String(title)
  .replace(/[\\/:*?"<>|]/g, '')
  .trim() || 'song';

item.binary.audio.fileName = safe + '.mp3';
item.binary.audio.mimeType = 'audio/mpeg';
```

Example:

```text
Spreadsheet Title:
My Song: Live/2026?

Generated File:
My Song Live2026.mp3
```

---

## 🎨 AI Cover Generation

The cover-art branch uses a Cloudflare Workers AI HTTP request.

The supplied workflow targets:

```text
@cf/black-forest-labs/flux-2-klein-4b
```

The prompt is dynamically generated from the selected title:

```text
Album cover art for the song <TITLE>.
Artistic, cinematic, vibrant colors, no text or letters.
```

### Image settings

| Setting | Value |
|---|---|
| Model | FLUX 2 Klein 4B |
| Width | 1024 |
| Height | 1024 |
| Format handling | Base64 → binary |
| Output filename | `<Title> - cover.jpg` |

---

## ☁️ Google Drive Output

The workflow uploads two assets to Google Drive:

### 1. Cover

```text
<Title> - cover.jpg
```

### 2. MP3

```text
<Title>.mp3
```

Both are currently configured for the Google Drive **My Drive / root folder** in the supplied workflow.

You can change the destination folder from the corresponding Google Drive nodes.

---

## 📧 Gmail Delivery

After both Drive uploads finish, the workflow sends an email.

### Email subject

```text
✅ Files ready: <Title>
```

### Email includes

- Song title
- Original URL
- MP3 Google Drive link
- Cover image Google Drive link

Example structure:

```text
Hi,

Your files are ready in Google Drive.

Song: Example Song
YouTube: <source URL>

MP3: <Google Drive link>
Cover image: <Google Drive link>

— Your n8n automation
```

---

## 🔀 Why the Merge Node Matters

The MP3 and cover-art branches execute independently.

```text
                    Pick Unsent URL
                    /             \
                   /               \
                  ↓                 ↓
             MP3 branch       Cover branch
                  ↓                 ↓
             Drive MP3         Drive Cover
                  \               /
                   \             /
                    ↓           ↓
                  Wait for Both Uploads
                           ↓
                         Gmail
```

The **Wait for Both Uploads** merge ensures Gmail is not sent until both files have been uploaded.

This is an important synchronization point in the automation.

---

## 📋 Status Tracking

Once Gmail completes successfully, the workflow updates the original spreadsheet row:

```text
Status = Sent
```

The matching is performed using the row number captured by n8n.

This creates a simple state machine:

```text
┌─────────────┐
│ Empty       │
│ Status      │
└──────┬──────┘
       │
       │ selected
       ▼
┌─────────────┐
│ Processing  │
│ MP3 + Cover │
└──────┬──────┘
       │
       │ email sent
       ▼
┌─────────────┐
│ Sent        │
└─────────────┘
```

---

## ⏰ Schedule

The trigger is configured with:

```text
0 9 * * *
```

This means:

> **Run every day at 9:00 AM according to the n8n instance timezone.**

If your n8n server uses a different timezone, configure the instance/workflow timezone accordingly.

---

## 🛠️ Setup

<details>
<summary><strong>01 — Prepare Google Sheets</strong></summary>

Create a Google Sheet with a tab named:

```text
Data
```

Add:

```text
URL | Title | Status
```

Populate `URL` and `Title`.

Leave `Status` empty for items that should be processed.

</details>

<details>
<summary><strong>02 — Import the Workflow</strong></summary>

1. Open your n8n instance.
2. Create/open a workflow.
3. Import:

```text
workflow/daily-url-dispatcher.json
```

4. Save the workflow.
5. Configure the credentials described below.

The repository version intentionally removes exported credential bindings and replaces private configuration with placeholders for safer GitHub publication.

</details>

<details>
<summary><strong>03 — Configure Google Sheets</strong></summary>

Update the Google Sheets nodes:

- `Get All Rows`
- `Mark as Sent`

Set:

```text
Document ID → Your Google Sheet
Sheet → Data
```

Connect a Google Sheets OAuth credential.

</details>

<details>
<summary><strong>04 — Configure Gmail</strong></summary>

Configure the Gmail credential.

Then change:

```text
YOUR_EMAIL@example.com
```

to the destination address you want to receive the generated files.

</details>

<details>
<summary><strong>05 — Configure Cloudflare Workers AI</strong></summary>

The HTTP Request node expects:

```text
POST
https://api.cloudflare.com/client/v4/accounts/YOUR_ACCOUNT_ID/ai/run/@cf/black-forest-labs/flux-2-klein-4b
```

Configure:

```text
YOUR_ACCOUNT_ID
YOUR_CLOUDFLARE_API_TOKEN
```

The authorization header should be:

```text
Authorization: Bearer YOUR_CLOUDFLARE_API_TOKEN
```

Do **not** hard-code real API tokens into a public workflow export.

</details>

<details>
<summary><strong>06 — Configure yt-dlp</strong></summary>

The Execute Command node requires:

```text
yt-dlp
Node.js runtime
required yt-dlp JavaScript components
FFmpeg / audio conversion support
```

Verify your n8n runtime can execute:

```bash
yt-dlp --version
```

and that MP3 extraction works before activating the workflow.

</details>

<details>
<summary><strong>07 — Configure Google Drive</strong></summary>

Connect a Google Drive OAuth credential to:

- `Upload Cover to Drive`
- `Upload MP3 to Drive`

Choose the desired destination folder instead of the root if required.

</details>

---

## 🧪 Testing

Before activating the daily schedule, test each stage individually.

### Test 1 — Spreadsheet

Add one test row:

```text
URL: <test URL>
Title: Test Song
Status:
```

Expected:

```text
Pick Unsent URL → returns the test row
```

### Test 2 — MP3

Execute the MP3 branch.

Expected:

```text
/home/node/.n8n-files/song.mp3
```

### Test 3 — Cover

Execute the Cloudflare branch.

Expected:

```text
1024 × 1024 image
```

### Test 4 — Google Drive

Confirm both assets appear in Drive:

```text
Test Song.mp3
Test Song - cover.jpg
```

### Test 5 — Gmail

Confirm the email contains:

```text
MP3 link
Cover link
Original URL
Title
```

### Test 6 — Status

Confirm the source row changes to:

```text
Sent
```

---

## 🧰 Demo

The `demo/` directory contains a sample queue you can use to understand the expected input format before connecting a real production sheet.

### Demo files

```text
demo/
├── sample-data.csv
└── README.md
```

The demo intentionally uses placeholder URLs rather than real media sources.

> Use only URLs and media that you are legally permitted to download and process. Respect the terms of the source platform and applicable copyright/licensing rules.

---

## 📁 Project Structure

```text
Daily URL Dispatcher Automation/
│
├── workflow/
│   └── daily-url-dispatcher.json
│
├── demo/
│   ├── sample-data.csv
│   └── README.md
│
├── docs/
│   ├── setup.md
│   └── architecture.md
│
├── .gitignore
└── README.md
```

---

## 🔐 Security & Production Considerations

### Credentials

Never commit:

```text
API tokens
OAuth secrets
private keys
passwords
personal email addresses
```

Use n8n credentials or environment variables.

### Google Sheet

The workflow contains a spreadsheet identifier in the original exported configuration. The GitHub-safe version replaces it with:

```text
YOUR_GOOGLE_SHEET_ID
```

### Cloudflare

The workflow uses placeholders:

```text
YOUR_ACCOUNT_ID
YOUR_CLOUDFLARE_API_TOKEN
```

### Gmail

The public version uses:

```text
YOUR_EMAIL@example.com
```

instead of a personal recipient address.

### Execute Command

Because the automation uses `Execute Command`, it should only run in an environment where command execution is intentionally enabled and trusted.

---

## ⚠️ Known Limitations

| Limitation | Explanation |
|---|---|
| One item per day | The queue intentionally selects the first unsent URL only |
| No retry branch | A failed execution does not automatically retry the item |
| Simple status model | The sheet currently uses an empty/`Sent` state |
| Root Drive destination | The supplied workflow defaults to My Drive root |
| External media dependency | `yt-dlp` depends on the source URL remaining accessible |
| Command execution | Requires an n8n environment supporting Execute Command |
| Cloudflare dependency | Cover generation depends on a valid Workers AI configuration |
| No explicit duplicate detection | Duplicate URLs/titles are not automatically detected |
| No failure status | Failed jobs do not currently write a `Failed` status |
| No transactional rollback | A partial upload can leave files behind if a later node fails |

---

## 🚀 Possible Improvements

### Queue improvements

- [ ] Add `Processing` status before starting.
- [ ] Add `Failed` status and error message.
- [ ] Add retry count.
- [ ] Add processed timestamp.
- [ ] Add unique item IDs.
- [ ] Process multiple rows per execution.

### Reliability

- [ ] Add automatic retry handling.
- [ ] Add an error workflow.
- [ ] Add execution monitoring.
- [ ] Add cleanup for partial uploads.
- [ ] Add timeout/failure notifications.

### Google Drive

- [ ] Create a dedicated folder per title.
- [ ] Organize outputs by date.
- [ ] Set sharing permissions automatically.
- [ ] Store Drive file IDs back in Sheets.

### Gmail

- [ ] Use an HTML email template.
- [ ] Include richer metadata.
- [ ] Add processing timestamps.
- [ ] Send failure notifications to an admin address.

### AI cover generation

- [ ] Add configurable visual styles.
- [ ] Add artist/genre context.
- [ ] Generate multiple cover candidates.
- [ ] Add image moderation/validation.
- [ ] Store generation metadata.

### Scaling

- [ ] Replace Google Sheets with a database.
- [ ] Introduce a real job queue.
- [ ] Add concurrency control.
- [ ] Add idempotency keys.
- [ ] Add centralized logging.

---

## 📈 Automation Value

This workflow demonstrates several production-oriented automation patterns:

| Pattern | Implementation |
|---|---|
| ⏰ Scheduled execution | n8n Schedule Trigger |
| 📋 Queue processing | Google Sheets + status |
| 🧠 Business logic | JavaScript Code nodes |
| 🎵 Media processing | yt-dlp + Execute Command |
| 🤖 AI integration | Cloudflare Workers AI |
| 📁 Binary processing | n8n binary data |
| ☁️ Cloud storage | Google Drive |
| 🔀 Synchronization | Merge node |
| 📧 Communication | Gmail |
| 🔄 State tracking | Google Sheets |
| 🧩 Modular branches | Parallel MP3 + cover pipelines |

---

## 📚 Documentation

- 📘 [Setup Guide](./docs/setup.md)
- 🏗️ [Architecture](./docs/architecture.md)
- 🧪 [Demo Guide](./demo/README.md)
- ⚙️ [n8n Workflow](./workflow/daily-url-dispatcher.json)

---

## 👨‍💻 Author

<div align="center">

### Muhammad Abdullah

Full-Stack Developer · AI Applications · Automation

<a href="https://github.com/AbdullahSoftDev">GitHub</a> ·
<a href="https://linkedin.com/in/abdullahsoftdev">LinkedIn</a>

⭐ Explore the workflows, reuse what helps, and follow the repository as new automation agents are added.

</div>

---

<div align="center">

**Built with n8n · Automated with AI · Delivered through Google Workspace**

</div>
