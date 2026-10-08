# Demo

This folder demonstrates the input contract expected by the automation.

## Files

- `sample-data.csv` — example Google Sheets data

## Importing the demo

Create a Google Sheet with a `Data` tab and import the CSV contents.

The headers should remain:

```text
URL,Title,Status
```

The sample uses placeholder URLs intentionally.

For an actual test, replace the placeholder with a source URL you are authorized to download/process.

## Expected result

An eligible row should move through:

```text
URL
 ↓
MP3 generation
 ↓
Cover generation
 ↓
Google Drive uploads
 ↓
Gmail
 ↓
Status = Sent
```

No live media source is included in this demo.
