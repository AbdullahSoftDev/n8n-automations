# Setup Guide

## 1. Requirements

- Running n8n instance
- Google account
- Google Calendar
- Google Sheets
- Gmail
- A spreadsheet tab named `Bookings`

## 2. Import

Import:

- `workflow/booking-confirmation-calendar.json`
- `workflow/booking-reminders.json`

## 3. Google Sheets

Create a `Bookings` tab with:

```text
Booking ID | Created At | Name | Email | Phone | Service | Start | Status | Reminder 24h | Reminder 2h | Calendar Event ID | Notes
```

## 4. Credentials

Connect:

- Google Calendar credential to `Create Calendar Event`
- Google Sheets credential to `Save to Bookings Sheet`, `Read Bookings`, and `Mark Reminder in Sheet`
- Gmail credential to `Send Confirmation Email` and `Send Reminder Email`

## 5. Business settings

Update the email footer:

```text
YOUR BUSINESS NAME
```

The current Code nodes use:

```text
Asia/Karachi
UTC+5
```

If your business uses another timezone, update the date parsing and display logic in both workflows.

## 6. Webhook

The booking workflow exposes:

```text
POST /new-booking
```

The local demo uses:

```text
http://localhost:5678/webhook/new-booking
```

Use the production webhook URL after activating the workflow.

## 7. Reminder scheduler

The reminder workflow starts automatically every 15 minutes. It reads confirmed bookings and updates the two reminder columns when a reminder is sent or skipped.
