# Harmony Spine Appointment Reminder

**Version 1.0** | n8n Workflow for Automated Patient Appointment Reminders

## Overview

Automate appointment reminders for your medical practice. This workflow fetches upcoming appointments from Airtable, converts times to the correct timezone, and sends Telegram reminders to patients.

**What it does:**

1. **Airtable query** – Fetches appointments where reminder hasn't been sent
2. **Timezone conversion** – Converts UTC to America/New_York time
3. **Batch processing** – Handles multiple appointments in queue
4. **Telegram alerts** – Sends personalized reminders to patients
5. **Status tracking** – Updates Airtable to mark reminder as sent

## Requirements

- n8n instance (self-hosted or cloud)
- Airtable account with Personal Access Token
- Telegram bot with API access
- Airtable base with Appointments table

## Airtable Schema

Your Appointments table should have these fields:

- `patient_name` (Single line text)
- `appointment_time` (Date with time)
- `reminder_sent` (Checkbox)

## Setup Instructions

### 1. Import the Workflow

Import `harmony-spine-appointment-reminder.json` into your n8n instance.

### 2. Configure Airtable

Update the **Get Patient Appointments** node:
- Replace `YOUR_AIRTABLE_BASE_ID` with your base ID
- Replace `YOUR_TABLE_ID` with your table ID
- Add your Airtable Personal Access Token credential

### 3. Configure Telegram

Update the **Appointment Telegram Alert** node:
- Replace `YOUR_TELEGRAM_CHAT_ID` with the patient's chat ID
- Add your Telegram bot API credential

### 4. Customize Message

Edit the reminder message in the Telegram node to match your practice's voice.

## License

MIT License - Feel free to modify and share!
